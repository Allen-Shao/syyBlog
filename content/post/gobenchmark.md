---
title: "Easy Benchmarking in Golang"
date: 2017-04-04
categories:
- Tutorial
tags:
- Golang
- Benchmark
keywords:
- Golang
- Testing
- Benchmark
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://res.cloudinary.com/tinysyy/image/upload/v1491276724/stopwatch_405_iurni1.jpg
comments: false
showTags: false
---

I am doing some Golang development in recent weeks. In this post, I will talk about how to benchmark a function easily in Golang.
<!--more-->

Golang provides a useful package called "testing". We can make use of it for both testing and benchmarking.  

## Test files
The first thing is to create a test file named xxx_test.go for xxx.go which is going to be tested.  

## Testing functions
For example, in file foo.go, we have a function called Bar. The definition is as below.  
{{< codeblock "" "go">}}
package main

func Bar(n int) int {
    return n*2
}
{{< /codeblock >}}

So, in the file foo_test.go, we will create a testing function has a prefix "Test" in the name.  
{{< codeblock "" "go">}}
package main

import "testing"

func TestBar(t \*testing.T) {
    t.Run(...)    //add test cases here
}
{{< /codeblock >}}

## Benchmark functions
Benchmark functions are similar to testing functions. They have a prefix "Benchmark" in the name.  
{{< codeblock "" "go">}}
package main

import "testing"

func BenchmarkBar(b \*testing.B) {
    // run the Bar function b.N times
    for n := 0; n < b.N; n++ {
        Bar(10)
    }
}
{{< /codeblock >}}

### Running benchmark
```
% go test -bench=.
```
If everything is successfully completed, a PASS result will be displayed. Also, you will get a benchmark with ns/operation unit to show how fast a function runs in average.  

## Reference
* [Golang Document](https://golang.org/pkg/testing/)
* [How to write benchmarks in Go](https://dave.cheney.net/2013/06/30/how-to-write-benchmarks-in-go)
* [https://dave.cheney.net/2013/06/30/how-to-write-benchmarks-in-go](https://dave.cheney.net/2013/06/30/how-to-write-benchmarks-in-go)

