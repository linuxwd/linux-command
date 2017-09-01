ab
===

Apache服务器的性能测试工具

## 补充说明

**ab命令** 是apachebench命令的缩写。是Apache的Web服务器的性能测试工具，它可以测试安装Web服务器每秒种处理的HTTP请求。

## ab的原理
&#160; &#160; &#160; &#160;	ab命令会创建多个并发访问线程，模拟多个访问者同时对某一URL地址进行访问。它的测试目标是基于URL的，因此，它既可以用来测试apache的负载压力，也可以测试nginx、lighthttp、tomcat、IIS等其它Web服务器的压力。

&#160; &#160; &#160; &#160;	ab命令对发出负载的计算机要求很低，它既不会占用很高CPU，也不会占用很多内存。但却会给目标服务器造成巨大的负载，其原理类似CC攻击。自己测试使用也需要注意，否则一次上太多的负载。可能造成目标服务器资源耗完，严重时甚至导致死机。

### 语法

```
ab(选项)(参数)
```

### 选项

```
-A：auth-username:password
	对服务器提供BASIC认证信任。 用户名和密码由一个:隔开，并以base64编码形式发送。 无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。

-B:
-b:
	即windowsize，TCP发送/接收的缓冲大小(单位：字节)。
    
-c：concurrency
	一次产生的请求个数。默认是一次一个。指定一次向服务器发出请求数；
    
-C：cookie-name=value
	对请求附加一个Cookie:行。 其典型形式是name=value的一个参数对。 此参数可以重复。
    
-d:
	不显示"percentage served within XX [ms] table"的消息(为以前的版本提供支持)。
    
-e:csv-file
    产生一个以逗号分隔的(CSV)文件， 其中包含了处理每个相应百分比的请求所需要(从1%到100%)的相应百分比的(以微妙为单位)时间。 由于这种格式已经“二进制化”，所以比'gnuplot'格式更有用。
    
-f：
-g：gnuplot-file
    把所有测试结果写入一个'gnuplot'或者TSV (以Tab分隔的)文件。 此文件可以方便地导入到Gnuplot, IDL, Mathematica, Igor甚至Excel中。 其中的第一行为标题。
    
-h：显示帮助信息；
-H：custom-header
    对请求附加额外的头信息。 此参数的典型形式是一个有效的头信息行，其中包含了以冒号分隔的字段和值的对 (如, "Accept-Encoding: zip/zop;8bit")。
    
-i：执行HEAD请求，而不是GET；

-k：启用HTTP KeepAlive功能，即, 在一个HTTP会话中执行多个请求。 默认时，不启用KeepAlive功能；

-n：requests
    在测试会话中所执行的请求个数。 默认时，仅执行一个请求，但通常其结果不具有代表意义。
-P:proxy-auth-username:password
    对一个中转代理提供BASIC认证信任。 用户名和密码由一个:隔开，并以base64编码形式发送。 无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。
-p：POST-file
    包含了需要POST的数据的文件。此外还必须设置-T参数。
-q：
	如果处理的请求数大于150， ab每处理大约10%或者100个请求时，会在stderr输出一个进度计数。 此-q标记可以抑制这些信息；
-r:
	指定接收到错误信息时不退出程序。
-S:
	不显示中值和标准背离值， 而且在均值和中值为标准背离值的1到2倍时，也不显示警告或出错信息。 默认时，会显示 最小值/均值/最大值等数值。(为以前的版本提供支持)。
-s：
	用于编译中(ab -h会显示相关信息)使用了SSL的受保护的https， 而不是http协议的时候。此功能是实验性的，也是很简陋的。最好不要用。
-t：timelimit
    测试所进行的最大秒数。其内部隐含值是-n 50000。 它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。
-T：content-type
    POST数据所使用的Content-type头信息。
-u:
	即putfile，发送PUT请求时需要上传的文件，此外还必须设置-T参数。
-V:
	显示版本号并退出。
-v：verbosity
    设置显示信息的详细程度 - 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。
-w：以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。
-x：<table>-attributes
    设置<table>属性的字符串。 此属性被填入<table 这里 >。
-X：proxy[:port]
    对请求使用代理服务器。
-y：<tr>-attributes
    设置<tr>属性的字符串.
-z：<td>-attributes
    设置<td>属性的字符串.
-Z：
```


### 参数

主机：被测试主机。


<!-- Linux命令行搜索引擎：https://jaywcjlove.github.io/linux-command/ -->



### 使用实例
```bash
#对index.php进行，1000次请求，并发用户10的压力测试

[root@localhost ~]# ulimit -n 65535
[root@localhost ~]# ab -k -n 1000 -c 600 http://localhost/index.php
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        Apache/2.4.6
#表示被测试的Web服务器软件名称。
Server Hostname:        localhost
#e表示请求的URL主机名。
Server Port:            80
#表示被测试的Web服务器软件的监听端口。

Document Path:          /index.php
#请求的资源，表示请求的URL中的根绝对路径，通过该文件的后缀名，我们一般可以了解该请求的类型。
Document Length:        207 bytes
#文档返回的长度，不包括相应头
Concurrency Level:      10
#并发量
Time taken for tests:   0.127 seconds
#测试总共耗时，即总请求时间。表示所有这些请求被处理完成所花费的总时间。
Complete requests:      1000
#表示总请求数量即完成的请求。
Failed requests:        0
#失败的请求。这里的失败是指请求在连接服务器、发送数据等环节发生异常，以及无响应后超时的情况。如果接收到的HTTP响应数据的头信息中含有2XX以外的状态码，则会在测试结果中显示另一个名为“Non-2xx responses”的统计项，用于统计这部分请求数，这些请求并不算在失败的请求中。

Write errors:           0
#写入错误
Non-2xx responses:      1000
Total transferred:      422000 bytes
#表示所有请求的响应数据长度总和，包括每个HTTP响应数据的头信息和正文数据的长度。注意这里不包括HTTP请求数据的长度，仅仅为web服务器流向用户PC的应用层数据总长度。
HTML transferred:       207000 bytes
#表示所有请求的响应数据中正文数据的总和，也就是减去了Total transferred中HTTP响应数据中的头信息的长度。
Requests per second:    7865.65 [#/sec] (mean)
#每秒钟的请求量。（仅仅是测试页面的响应速度）。吞吐率，计算公式：Complete requests/Time taken for tests
Time per request:       1.271 [ms] (mean)
#等于 Time taken for tests/(complete requests/concurrency level)即平均请求等待时间（用户等待的时间）
Time per request:       0.127 [ms] (mean, across all concurrent requests)
#等于 Time taken for tests/Complete requests 即服务器平均请求响应时间 在并发量为1时 用户等待时间相同
Transfer rate:          3241.51 [Kbytes/sec] received
#表示这些请求在单位时间内从服务器获取的数据长度，计算公式：Total trnasferred/ Time taken for tests，这个统计很好的说明服务器的处理能力达到极限时，其出口宽带的需求量。

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       1
Processing:     0    1   0.2      1       3
Waiting:        0    1   0.1      1       2
Total:          1    1   0.2      1       3

Percentage of the requests served within a certain time (ms)
  50%      1
#50%的请求都在1ms内完成
  66%      1
  75%      1
  80%      1
  90%      1
  95%      1
  98%      1
  99%      2
 100%      3 (longest request)
[root@localhost ~]#

============================
	由于对于并发请求，cpu实际上并不是同时处理的，而是按照每个请求获得的时间片逐个轮转处理的，所以基本上第一个Time per request时间约等于第二个Time per request时间乘以并发请求数。
```

### 其它注意事项
```bash
1）
ab并发数不能大于请求数，会提示
ab: Cannot use concurrency level greater than total number of requests
2）
请求数默认不能超过1024个，会提示
socket: Too many open files (24)
可用ulimit -n命令修改，例如：ulimit -n 8192 （设置用户可以同时打开的最大文件数）。
3）
并发数默认不能大于20000个，会提示
ab: Invalid Concurrency [Range 0..20000]
需要修改apache源代码support目录下ab.c文件，找到：
#define MAX_CONCURRENCY 20000
将宏定义的值改大，重新编译安装apache。
4）
提示
apr_socket_recv: Connection reset by peer (104)
网上说是apr-util有些问题，不太稳定，多试几次就好了。
5）
还有单独的apache bench源码包，及如何单独安装ab教程
下载：http://apachebench-standalone.googlecode.com/files/ab-standalone-0.1.tar.bz2
安装教程：https://code.google.com/p/apachebench-standalone/wiki/HowToBuild
6）测试时发现有如下错误
 
...
Failed requests:        998
   (Connect: 0, Receive: 0, Length: 998, Exceptions: 0)
...
 

是因为在测试动态页面时，返回的长度和第一次不同，所以ab就判断失败了，对于动态页面这是合理的。
```
