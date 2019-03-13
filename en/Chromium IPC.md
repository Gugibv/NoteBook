Chromium has a multi-process architecture, which means that we have a lot of processes communicating with each other. 

> Chromium采用了多进程的架构，这也就意味着，程序中会有很多的跨进程通信。

Our main inter-process communication primitive is the named-pipe.

> 为了完成进程间通信，我们主要依赖的基本手段是命名管道。

A named pipe is allocated for each renderer process for communications with the browser process.

> 每个渲染器进程都会分配到一个命名管道。渲染器进程通过命名管道和浏览器进程进行通信。

The pipes are used in asynchronous mode to ensure that neither end is blocked waiting for the other.

> 为了确保管道两端在等待对方消息时不被阻塞，管道被设置成异步模式。

Within the browser, communication with the renderers is done in a separate I/O thread.

> 浏览器进程中，和渲染器的通信是在一个独立的I/O线程中完成的。

Messages to and from the views then have to be proxied over to the main thread using a ChannelProxy.

> 传递到视图区或者从视图区传出的消息，都需要用ChannelProxy代理后，交给主线程。

The advantage of this scheme is that resource requests, which are the most common and performance-critical messages, can be handled entirely on the I/O thread and not block the user interface.

> 这种模式的好处在于，网络资源请求——这种最常见，也是对性能影响最大的消息——可以完全地由I/O线程去处理，而不会影响到用户界面的渲染。

Some messages should be synchronous from the renderer's perspective.

> 从渲染器的角度来看，有一些消息应该是同步的。

This happens mostly when there is a WebKit call to us that is supposed to return something, but that we must do in the browser.

> 这种情况经常发生在WebKit发起了一个期待返回值的函数调用，而我们又必须在浏览器中完成这个函数逻辑的时候。

Examples of this type of messages are spell-checking and getting the cookies for JavaScript.

> 举例来说，“拼写检查”以及“为Javascript获取cookie”，都属于这样的情况。

Synchronous browser-to-renderer IPC is disallowed to prevent blocking the user-interface on a potentially flaky renderer.

> 为了防止本身可能有缺陷的渲染器阻塞住用户界面，从浏览器到渲染器的跨进程同步通信是被禁止的。	

ulti-process architecture	多进程架构

mean /min/	意思是

main /men/	adj. 主要的

major /ˈmedʒɚ/	adj. 主要的

primitive /ˈprɪmɪtɪv/	n. 原生

named-pipe	n. 命名管道

allocate /ˈæləˌket/	分配

browser /ˈbraʊzɚ/	浏览器

asynchronous /e'sɪŋkrənəs/	异步的

synchronous /ˈsɪŋkrənəs/	同步的

ensure /ɛnˈʃʊr/	确保

within /wɪðˈɪn/	在……里面

separate /'sepəreɪt/	独立的

to and from	往返，来回

proxy /ˈprɑ:ksi/	代理

advantage /ədˈvæntɪdʒ/	好处；优势

scheme /skim/	体制，模式

handle /ˈhændl/	处理

perspective /pərˈspektɪv/	角度

be supposed to do sth.	应该，理应做某事

type /taɪp/	类型

spell-checking	拼写检查

browser-to-renderer	从浏览器到渲染器的

disallow /ˌdɪsəˈlaʊ/	驳回；不准

prevent /prɪˈvɛnt/	预防，阻止

block /blɑ:k/	阻止

potentially /pəˈtɛnʃəlɪ/	潜在的

flaky /ˈfleki/	ss有缺陷的

本文选自[Inter-process Communication (IPC)](https://www.chromium.org/developers/design-documents/inter-process-communication)