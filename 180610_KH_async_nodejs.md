# Synchronous & Asynchronous

### I/O models and Sync-Async
- Four types of I/O models
    * ![Four types of I/O models](https://www.ibm.com/developerworks/library/l-async/figure1.gif)


1. Synchronous blocking I/O
    * ![Synchronous blocking I/O](https://www.ibm.com/developerworks/library/l-async/figure2.gif)
    * The application blocks until the system call is complete (data transferred or error).

2. Synchronous non-blocking I/O
    * ![Synchronous non-blocking I/O](https://www.ibm.com/developerworks/library/l-async/figure3.gif)
    * In this model, a device is opened as non-blocking. This means that instead of completing an I/O immediately, a read may return an error code indicating that the command could not be immediately satisfied (EAGAIN or EWOULDBLOCK)
    * Application may call numerous times to await the read completion.
    * This can be extremely inefficient.
        
3. Asynchronous blocking I/O
    * ![Asynchronous blocking I/O](https://www.ibm.com/developerworks/library/l-async/figure4.gif)
    * In this model, non-blocking I/O is configured, and then the blocking select system call is used to determine when there's any activity for an I/O descriptor.
    * So, IO is non-blocking, notification(kernel to application) is blocking
    * select is not very efficient. Not advisable for high performance I/O.

4. Asynchronous non-blocking I/O (AIO) **Our interest**
    * ![Asynchronous non-blocking I/O (AIO)](https://www.ibm.com/developerworks/library/l-async/figure5.gif)
    * The read request returns immediately, indicating that the read was successfully initiated.
    * The application can then perform other processing while the background read operation completes.
    * When the read response arrives, *a signal or a thread-based callback* can be generated to complete the I/O transaction.
    * When many slow I/O call happens, threads can be fully operational regardless of I/O blocking.  

> ref : [Boost application performance using asynchronous I/O](https://www.ibm.com/developerworks/library/l-async)
> ref(translated in Korean) : [Asynchronous IO 개념 정리](https://djkeh.github.io/articles/Boost-application-performance-using-asynchronous-IO-kor/)

### Asynchronous programming
- Asynchrony, in computer programming, refers to the occurrence of events independent of the main program flow and ways to deal with such events.
- 메인 프로그램을 blocking(wait) 하지 않고 수행 -> 병렬 컴퓨팅 가능(parallel computing)
- 일반적으로 subroutine 을 통해 처리
    * ex) procedure, function(callback), method, routin, subprogram 등으로 불림
- **future or promise**
    * [Java Future article](http://hamait.tistory.com/748)
    * [JS Promise article](http://programmingsummaries.tistory.com/325) 

> ref : [Wikipedia : Asynchrony](https://en.wikipedia.org/wiki/Asynchrony_(computer_programming))


# node.js

### node.js architecture
![nodejs architecture](http://biznomy.com/images/diagram/nodejs-arch-land.jpg)
> ref : http://biznomy.com/nodejs-development-company

### key points
- single threaded event loop
- worker thread pool (to enable it, node.js uses libuv)
    * libuv : libuv (Unicorn Velociraptor Library) is a multi-platform C library that provides support for asynchronous I/O based on event loops. 
        + It supports epoll(4), kqueue(2), Windows IOCP, and Solaris event ports.
- V8 javascript engine
    * V8 compiles JavaScript directly to native machine code before executing it
    