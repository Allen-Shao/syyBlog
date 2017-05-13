---
title: "Gob - Easy Binary Serialization in Golang"
date: 2017-05-13
categories:
- Tutorial
tags:
- Golang
- Gob
- Serialization
keywords:
- Golang
- gob
- serialization
- binary
- programming language
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://res.cloudinary.com/tinysyy/image/upload/v1494652943/Gopher_GoRequest_400x300_uxkxqa.jpg
metaAlignment: center
comments: false
showTags: false
---

I discovered "gob" is a very simple way provided by Golang package to do binary serialization. I will share some experience with gob in this post.  
<!--more-->
## What is Gob
Simply put, Gob is a data structure that can be used across a network or to store it in a file. In Golang, packages for JSON, XML and other universally used formats are all provided well. 
However, in go-specific environment, Gob is a specially designed structure to be easier to use and more efficient.  
### Several Advantages
* Very easy to use. The data structure itself is all the package should need to figure out how to encode and decode it. However, this means that other languages are impossible to make use of it.
* Efficient. Gob is basically a binary stream. 
* Self-describing. Each gob stream, read from the beginning, contains sufficient information that the entire stream can be parsed by an agent that knows nothing a priori about its contents. This property means that you will always be able to decode a gob stream stored in a file, even long after you've forgotten what data it represents. 

## How to use
In this part, I will briefly explain several steps to use gob and then directly show some examples of using gob to show how easy to implement with gob.  
### Steps
* Import the package "encoding/gob"
* Define the type of gob, it can be almost anything(slice of int, string, self-defined struct, etc.).
* Create Encoder/Decoder with targeted io(file, network, stream, etc.).
* Pass the data you want to encode/variable you want decode in.

### Examples

#### Write/Read in network
{{<codeblock "gob_example.go" "go" "" "gob_example.go">}}
package main

import (
    "bytes"
    "encoding/gob"
    "fmt"
    "log"
)

type P struct {
    X, Y, Z int
    Name    string
}

type Q struct {
    X, Y *int32
    Name string
}

func main() {
    // Initialize the encoder and decoder.  Normally enc and dec would be
    // bound to network connections and the encoder and decoder would
    // run in different processes.
    var network bytes.Buffer        // Stand-in for a network connection
    enc := gob.NewEncoder(&network) // Will write to network.
    dec := gob.NewDecoder(&network) // Will read from network.
    // Encode (send) the value.
    err := enc.Encode(P{3, 4, 5, "Pythagoras"})
    if err != nil {
        log.Fatal("encode error:", err)
    }
    // Decode (receive) the value.
    var q Q
    err = dec.Decode(&q)
    if err != nil {
        log.Fatal("decode error:", err)
    }
    fmt.Printf("%q: {%d,%d}\n", q.Name, *q.X, *q.Y)
}
{{</codeblock>}}

### Write/Read to a file
{{<codeblock "gob_savefile.go" "go" "" "gob_savefile.go">}}
package main

import (
    "encoding/gob"
    "fmt"
    "os"
)

func main() {
    data := []int{101, 102, 103}

    // create a file
    dataFile, err := os.Create("integerdata.gob")

    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }

    // serialize the data
    dataEncoder := gob.NewEncoder(dataFile)
    dataEncoder.Encode(data)

    dataFile.Close()
}
{{</codeblock>}}

{{<codeblock "gob_readfile.go" "go" "" "gob_readfile.go">}}
package main

import (
    "encoding/gob"
    "fmt"
    "os"
)

func main() {
    var data []int

    // open data file
    dataFile, err := os.Open("integerdata.gob")

    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }

    dataDecoder := gob.NewDecoder(dataFile)
    err = dataDecoder.Decode(&data)

    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }

    dataFile.Close()

    fmt.Println(data)
}
{{</codeblock>}}

## Reference
* [Official Doc](https://golang.org/pkg/encoding/gob/)
* [Official Blog](https://blog.golang.org/gobs-of-data)
* [Socket Loop](https://www.socketloop.com/tutorials/golang-saving-and-reading-file-with-gob)
