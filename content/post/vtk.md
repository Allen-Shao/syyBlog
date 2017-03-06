---
title: "Play with VTK"
date: 2017-03-06
categories:
- Tutorial
tags:
- vtk
- 3D
- mesh decimation
keywords:
- 3D models
- mesh decimation
- mesh subdivision

autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://res.cloudinary.com/tinysyy/image/upload/v1488783052/vtk-logo_ffxmkk.gif
comments: false
showTags: false
---
Recently, I was dealing with a task that to average the mesh densities of millions of 3D models.
After some research, I found the VTK tool is quite good at it. So I will share some experience of using VTK as a first time user.
<!--more-->

## Objective
This task is assigned to me by Autodesk, where I am doing my internship at. They are using some algorithm similar to bigram to generate descriptors for each 3D model. 
Before doing that, they need to process all the 3D models with mesh density normalization, which make each 3D model has similar number of meshes. 
The original method they use is called Qslim. It does not perform well because sometimes it cannot reduce the mesh number significantly to desired level.  

So I took this task and researched out the VTK tool, which is able to set a specific target number of meshes for mesh decimation.  

## VTK
[The Visualization Toolkit (VTK)](http://www.vtk.org/) is an open-source, freely available software system for 3D computer graphics, image processing, and visualization. 
VTK has many visualization algorithms built in within a C++ library. 
I mainly made use of the polygon reduction and mesh subdivision algorithms to average our mesh density of each 3D part. 
The download and installation guide can be found [here](http://www.vtk.org/Wiki/VTK/Configure_and_Build). (OpenGL is required).  

My first impression of vtk is the documentation. everything is explained in detail and most useful things are plenty of example codes are provided, which makes my understanding extremely quick and intuitive.  

The main advantage of vtk is that a specific reduction rate can be set. 
Therefore, i can reduce the number of meshes to a desired target to make sure all the 3d parts have similar mesh density. 
Moreover, vtk mesh decimation also preserves the topology and the visual shape of the original geometry very well.  

## What did I do
I have uploaded my code on [Github](https://github.com/Allen-Shao/Mesh-Simplification) and feel free to check it out.  

### Input and Output
Because the original algorithm qslim uses [SMF file format](http://people.sc.fsu.edu/~jburkardt/data/smf/smf.txt) for both input and output, 
my version kept it same for easy integration into original system. 
VTK algorithms usually require vtkPolyData as input and output, so custom reader and writer classes are implemented. 
If other file formats besides SMF(eg. OFF file) are needed, simply modify current reader and writer will do. It's extremely easy.

### Decimation
My first approach is only use decimation algorithm provided by VTK. There are several classes can be used for this task. My choice is [vtkDecimatePro](http://www.vtk.org/doc/nightly/html/classvtkDecimatePro.html) Class.  

The decimation process is separated into three steps: Vertex classification, Decimation criterion and Triangulation.  

* **Vertex Classification**: It characterizes the local geometry and topology for each vertex by classifying them into different types according to their neighborhoods.  
* **Decimation Criterion**: This step measures the error of each vertex if removed.  
* **Triangulation**: The resulting hole polygon after removing vertex needs to be triangulated.  

### Subdivision
The problem with vtkDecimatePro class is that it can only reduce the number of meshes. What if a 3D model has fewer meshes than the target numebr. 
So I use subdivision to increase the number of meshes. There are different [subdivision schemes](http://graphics.stanford.edu/courses/cs468-10-fall/LectureSlides/10_Subdivision.pdf) available, including [linear scheme](http://www.icm2006.org/proceedings/Vol_III/contents/ICM_Vol_3_58.pdf), [loop scheme](https://graphics.stanford.edu/~mdfisher/subdivision.html) and [butterfly scheme](http://www.math.tau.ac.il/~niradyn/papers/butterfly.pdf), etc. I chose to use linear scheme([vtkLinearSubdivisionFilter](http://www.vtk.org/doc/nightly/html/classvtkLinearSubdivisionFilter.html)) because it preserves geometry relatively better for most of cases.  
Due to the design of subdivision algorithm, the number of triangles can only increase by approximately a factor of 4 each time.  

### Result and Conclusion
Decimation result of three typical parts is listed below with the comparison of visual geometries and mesh numbers before and after the mesh decimation using this algorithm.  
![3D_1](https://res.cloudinary.com/tinysyy/image/upload/v1488793605/Visualization_Toolkit_-_OpenGL_001_gmfcp2.png) ![result1](https://res.cloudinary.com/tinysyy/image/upload/v1488793605/Selection_002_z9amsd.png)  
![3D_2](https://res.cloudinary.com/tinysyy/image/upload/v1488793605/Visualization_Toolkit_-_OpenGL_003_y4jcmg.png) ![result2](https://res.cloudinary.com/tinysyy/image/upload/v1488793605/Selection_004_omf6aw.png)  
![3D_3](https://res.cloudinary.com/tinysyy/image/upload/v1488793605/Visualization_Toolkit_-_OpenGL_005_ql7wam.png) ![result3](https://res.cloudinary.com/tinysyy/image/upload/v1488793605/Selection_006_j0r8hc.png)  
From the testing result, it is seen that this algorithm is effective to reduce mesh density to a desired target (1000 polygons in this testing case). It is able to solve the problem of current qslim that 3D parts may have various mesh densities after mesh simplification.


