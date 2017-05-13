---
title: "Post local images to Facebook via Graph API"
date: 2017-05-01
categories:
- Tutorial
tags:
- Facebook
- Graph API
keywords:
- Facebook
- automation
- api
- images
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://res.cloudinary.com/tinysyy/image/upload/v1493649801/lvqssrhcBZ0_nra5gj.png
metaAlignment: center
comments: false
showTags: false
---
Facebook provides an API called __Graph API__ for developers to post images onto Facebook from the application. 
However, the API can only support image url. In this post, I will discuss an easy way to upload local image files using Graph API.
<!--more-->
## Graph API
Facebook [Graph API](https://developers.facebook.com/docs/graph-api) has various functions. 
And it provides an easy to use [GUI Explorer](https://developers.facebook.com/tools/explorer/) for trying.  
We are going to use the specific one: **/me/photos**. The detail document can be found [here](https://developers.facebook.com/docs/graph-api/photo-uploads).  
**Note**: You need to require an access token and pay attention to the permission granted to the token.

## Python HTTP Server
Because the Facebook Graph API only support url as image input, the next step we are going to do is to convert local image file into a url.  
First, we need to have a simple server running locally. I choose to use Python for this simple task.  
Just use the command below in the cmd or terminal will start a local http server:
```bash
python -m http.server
```
Use this command under the folder containing only your image file, because we will do the port forwarding to make this folder public.  
Now, you should be able to access your image in your browser by the url "http://localhost:8000/{your-image.jpg}"  

## ngrok Port Forwarding
After having a local server running, we need to make this public so that Facebook API can also access.  
**[ngrok](https://ngrok.com/)** is a handy tool to do this. Download the tool from the link provided. Then run the command in cmd or terminal: 
```bash
ngrok http 8000
```
After running successfully, you should see the url mapping to your localhost:8000.  
Now you can use a url looks like "http://demo.ngrok.io/{your-image.jpg}". Use this url to upload images via Facebook Graph API will be easy.

