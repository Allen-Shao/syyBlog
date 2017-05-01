---
title: "Simple HTTP Load Testing"
date: 2017-04-03
categories:
- Tutorial
tags:
- Testing
- apache
- benchmark
- jmeter
keywords:
- Testing
- apache
- benchmark
- jmeter
autoThumbnailImage: false
thumbnailImagePosition: left
thumbnailImage: https://res.cloudinary.com/tinysyy/image/upload/v1491209670/testng-selenium-load-test_tafwq6.png
metaAlignment: center
comments: false
showTags: false
---
Recently, I need to do some load testing for a http service I created. In this post, I will briefly introduce _Apache Bench_ and _Jmeter_.  
<!--more-->

## Apache Benchmark
ApacheBench is a very lightweight and simple tool to do some concurrency load testing. The official documentation can be found [here](https://httpd.apache.org/docs/2.4/programs/ab.html).  

### Install
```bash
apt-get install apache2-utils
```
### Simple Examples
```bash
ab -n 100 -c 10 http://www.google.com/
```

Sample output:

```
Concurrency Level:      10
Time taken for tests:   1.889 seconds
Complete requests:      100
Failed requests:        0
Write errors:           0
Total transferred:      1003100 bytes
HTML transferred:       949000 bytes
Requests per second:    52.94 [#/sec] (mean)
Time per request:       188.883 [ms] (mean)
Time per request:       18.888 [ms] (mean, across all concurrent requests)
Transfer rate:          518.62 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
              Connect:       57   59   1.7     59      64
              Processing:   117  126   7.5    124     162
              Waiting:       57   62   7.0     60      98
              Total:        175  186   8.0    184     224

              Percentage of the requests served within a certain time (ms)
                50%    184
                66%    186
                75%    187
                80%    188
                90%    192
                95%    203
                98%    216
                99%    224
                100%    224 (longest request)
```

Several useful parameters  
 -n Total number of requests  
 -c Concurrency level: How many requests to perform at same time  
 -m HTTP Methods  
 -p POST file  
 -T Content Type  
Full list can be found using ```ab -h```

## Jmeter
Jmeter is a relatively complicated tool for load testing integrated with many functions. I just tried some simple ones.  
### Dependency
Java SDK
### Install
Jmeter can be downloaded [here](http://jmeter.apache.org/download_jmeter.cgi). You can choose binary or build from source.
I simply use the binary directly.  
### How to use
Jmeter provides a GUI which is very useful to configure your test cases. When starting the real test, make sure to use the non-GUI mode.  
When configuring test plan, simply added a thread group and added HTTP Request under the thread group.  
It also supports CSV Data which can configure variable values, which ApacheBench is not capable.  
After configuration, simply save the test plan and use the command ```jmeter -n -t xxx.jmx```  
The return results will show in detail.

