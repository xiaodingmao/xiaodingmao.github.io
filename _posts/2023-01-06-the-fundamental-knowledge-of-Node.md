---
layout: post
title: the fundamental knowledge of node.js for beginners
image: 
  path: /assets/img/blog/Nodejs.png
description: >
  node.js is an open-source, cross-platform JavaScript runtime environment.
  Node.js runtime provides all the necessary components so as to use and run JavaScript code outside the browser.
sitemap: false
---

the fundamental knowledge of node.js for beginners
{:.lead}

## What's node.js?
As we can see on the official website, node.js is an open-source, cross-platform JavaScript runtime environment.
*open-source* means that source code is publicly available for sharing and modification.
*cross-platform* means that node.js is available on Mac, Windows and Linux.
### What does the JavaScript runtime mean?

Firstly, we should know the meaning of the JavaScript engine, which is a program that converts JavaScript code into machine code that allows a computer to perform specific tasks.  
V8 engine is one of the JavaScript engines, which is open-source and developed by Google for Chrome.V8 is written in C++ and not in JavaScript. it can run standalone or can be embedded into any C++ application. the important thing is that the **V8 engine sits at the core of Node.js.** Simple speaking, JavaScript runtime is an environment that provides all the necessary components to use and run a JavaScript program.JavaScript engine is one component in the JavaScript runtime. The following picture shows how js codes are executed in the **browser runtime** which consists of the js engine, web APIs, task queues and event loop.
![browser js runtime](./BrowserRuntime.png)
>For example,V8 engine in chrome is used to execute js codes. It comprises a call stack where js codes get executed and a heap storing all the variables. Web APIs refer to DOM, timers such as setTimeout and setInterval, promise,web storage etc. these are extra functionality added to the V8 engine. Queues are where asynchronous tasks wait before they are executed. Event loop ensures async tasks get executed in the right order.  

What we can highlight from the above picture is that the js engine itself can only perform *ECMAScript*, whereas JavaScript extends the core language ECMAScript by supplying objects that need to run in the JavaScript runtime.  
*So what's the Node.js runtime?*   
Node.js runtime provides all the necessary components so as to use and run JavaScript codes **outside** the browser**. As I have mentioned before, the V8 engine is one of the most important dependencies, which can help Node.js understand js codes. Libuv is another major dependency that can help Node.js handle asynchronous I/O such as file system and network operation.C++ features backing Node.js are the source codes that provide Node.js accessing network or file system. on top of Node.js, JS library enables developers to get access to C++ features easily without understanding C++. For example, when we write js codes to access the file system, the code internally calls the corresponding C++ feature which further relies on Libuv to access the file system.
![node.js runtime](./nodeRuntime.png)  
**one important difference between Node.js runtime and the browser runtime is that Node.js runtime doesn't have access to web APIs like DOM or window.**   
To sum up, Node.js is capable of executing JavaScript codes outside a browser which can not only execute the standard ECMAScript but also new features, like reading from the network and accessing a database or filesystem, which actually are developed by C++ but exposed common utilities by JavaScript.  
## Why do we use Node.js?  
Node.js has a unique advantage in that frontend developers who write JavaScript for the browser are able to write node.js for the server side without the need to learn a complete language. It means we can run JavaScript on a server. For example, node.js allows an application to communicate with a database, perform file manipulation or create a web server to respond to a custom request from the browser.  
## How to use Node.js?  
The first thing we need to know is that Node.js supports both CommonJS and ES module systems(since Node.js v12), which means we can use both require() and import() in Node.js while we are limited to import() in the browser. In Node.js, each file can be treated as a separate module.
There are three types of modules, which are local modules that we create in our own application, built-in modules that Node.js ships with out of the box and third-party modules that we can import and use but are written by other developers.     
Every local module in node.js gets wrapped in an IFFE before being loaded, which can avoid conflict with the same variable in modules.IFFE helps keep top-level variables scoped to the module rather than the global object. Before we execute a node.js module, it will be wrapped with an IFFE function internally which contains five parameters as follows.
```javascript
(function(exports,require,module,__filename,__dirname){

})
```   
Next, I will introduce some built-in modules which are very useful when we build applications.  
*path module* The path module provides utilities for working with files and directory paths.
```javascript
const path=require("node:path") //import path
console.log(__filename) // for example:/user/node/index.js
path.basename(__filename) // return index.js
path.extname(__filename) // return .js
path.join('/foo', 'bar', 'baz/asdf') // return /foo/bar/baz/asdf
path.resolve('/foo/bar', './baz');
// return /foo/bar/baz  
// this method resolves a sequence of paths or path segments into an absolute path.
```
*Events Module*   
The events module allows us to work with events in Node.js.We can dispatch our own custom events and respond to those custom events in a non-blocking manner.
```javascript
const EventEmitter = require("node:events");
const emitter = new EventEmitter();
emitter.on('checkList',(a,b) => {
  console.log(a,b)
});  // this is used to register a listen
emitter.emit('checkList','talk','writting'); // this is used to trigger the event
```
we can also write custom objects that extend from EventEmitter so that we can emit an event and react to it.*Most of the built-in modules such as HTTP and FS modules extend from EvenEmitter as well.*
````javascript
const EventEmitter = require('node:events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
  console.log('an event occurred!');
});
myEmitter.emit('event');
````
*FS module*  
 The file system module enables us to work with the file system on our computer. All file system operations have synchronous, callback and promise-based forms. 
````javascript
const fs = require("node:fs");
// async form
fs.readFile("/index.txt",(err,data) => {
    if(err) throw err
    console.log(data)
})
//sync form
fs.readFileSync("/index.txt")
//promise form
const {readFile} = require("node:fs/promises")
try {
  const contents = await readFile("index.txt",{encoding:'utf8' });
  console.log(contents);
} catch (err) {
  console.error(err.message);
}
````
*Stream module*  
A stream is a sequence of data that is being moved from one point to another over time. Working with data in chunks instead of waiting for the entire data to be available at once can prevent unnecessary memory usage. For example, when you're transferring file contents from fileA to fileB, you don't wait for the entire fileA content to be saved in temporary memory before moving it into fileB.In Node.js, a stream is an abstract interface for working with streaming data. There are many stream objects used in other node.js modules like FS or HTTP. Streams can be readable, writable, or both. All streams are instances of EventEmitter.In other words, the Stream module is a built-in module that inherits from EventEmitter class.
````javascript
const fs = require("node:fs");
const readableStream = fs.createReadStream("./file1.txt",{ encoding:"utf-8"});
const writableStream = fs.createWriteStream("./file2.txt");

readableStream.on("data",(chunk) => {
  console.log(chunk);
  writableStream.write(chunk);
});
````
*HTTP Module*  
HTTP(hypertext transfer protocol) is a protocol that defines a format for clients and servers to speak to each other. The client sends an HTTP request and the server responds with an HTTP response. HTTP module can support many features of the protocol, especially transferring the large,chunk-encoded message. When we use the HTTP server or client via the HTTP module, the HTTP interface never buffer entire requests or response so that we are able to stream, which can save memory a lot.
````javascript
const http = require("node:http");
const server = http.createServer((req,res) => {
  res.writeHead(200,{"Content-Type":"text/plain"});
  res.end("Hello world");
});
server.listen(3000,() => {
  console.log("server running on port 3000");
})
````
There are more modules and details about every module which need to investigate. We can take a deep look at the [official documentation](https://nodejs.dev/en/api/v19). As far as I'm concerned, one of the most important points from Node.js is that it provides us the capability to run JavaScript codes outside
the browser and perform file or network operations like other back-end languages, which is easier for front-end developers to master. But we still have a lot to learn, such as Node.js framework Express.js,Next.js etc.
