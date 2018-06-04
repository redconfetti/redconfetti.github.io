---
layout: page
title: Node.js
---

# Intro

Allows you to build scalable network applications using JavaScript on the
server-side. Uses V8 JavaScript Runtime, which powers Chrome / Chromium. NodeJS is
a wrapper for this engine.

## What can you build with NodeJS?

* Websocket Server (e.g. Chat Room)
* Fast File Upload client
* Ad Server
* Any Real-Time Data Apps

NodeJS is not a framework, it is very low level, and it is not multi-threaded. It
is a single threaded server.

## Blocking vs Non-Blocking Code

With blocking code the previous command must be completed before the next command
can run. Non-blocking code is structured so that it uses callbacks to determine the
next step after something is completed. This allows the code to continue running if
one item is not yet complete.

NodeJS is single threaded, however it’s able to run JavaScript Asynchronously. It
is build upon libuv, a cross-platform library that abstracts apis/syscalls for
asynchronous (non-blocking) input/output provided by the supported OSes (Unix, OS X
and Windows).

In this model, open/read/write operation on devices and resources (sockets, file
system, etc) managed by the file-system do not block the calling thread like they
do with C programs. They mark the process to be notified when new data or events
are available.

Node uses an event loop to allow this, which invokes the next callback/function
that was scheduled for execution. “Everything runs in parallel except your code”,
meaning that node allows your code to handle requests from hundreds and thousands
of open sockets with a single thread concurrently by multiplexing and sequencing
all your js logic in a single stream of execution.

```javascript
var http = require("http")
http
  .createServer(function(request, response) {
    response.writeHead(200) // status code in header
    response.write("Hello, this is dog.") // response body
    response.end() // close the connection
  })
  .listen(8080) // listen for connections on this port
```

This code uses an event loop that checks for an HTTP request until one is
encountered.

## Why JavaScript?

> "Javascript has certain characteristics that make it very different than other
> dynamic languages, namely that it has no concept of threads. Its model of
> concurrency is completely based around events." - Ryan Dahl

Known Events:

* request
* connection
* close

When these events are encountered, the associated callback functions are called. You could consider these callback functions our Event Queue.

```javascript
var http = require("http")
http
  .createServer(function(request, response) {
    response.writeHead(200) // status code in header
    response.write("Hello, this is dog.") // response body
    setTimeout(function() {
      // represents long running process
      response.write("Dog is done.")
      response.end()
    }, 5000) // 5000ms = 5 seconds
  })
  .listen(8080) // listen for connections on this port
```

## Blocking Call to the File System

```javascript
var fs = require("fs")
var contents = fs.readFileSync("index.html")
console.log(contents)
```

## Non-Blocking Call to the File System

```javascript
var fs = require("fs")
fs.readFile("index.html", function(error, contents) {
  console.log(contents)
})
```

## Read File and Serve as HTML

```javascript
var http = require("http")
var fs = require("fs")
http
  .createServer(function(request, response) {
    response.writeHead(200, { "Content-Type": "text/html" })
    fs.readFile("index.html", function(error, contents) {
      response.write(contents)
      response.end()
    })
  })
  .listen(8080)
```

# Events

The DOM triggers events such as click, hover, or submit. You can register callbacks
via jQuery when these events occur.

Many objects in NodeJS emit events. The net.Server object inherits from the
EventEmitter class, and emits the ‘request’ event. fs.readStream also inherits from
EventEmitter, and emits the ‘data’ event as data is read from the file.

## Custom Event Emitters

We can register our own emitter to do something like log errors, warnings, or info
events.

```javascript
var EventEmitter = require("events").EventEmitter
var logger = new EventEmitter()

logger.on("error", function(message) {
  console.log("ERR: " + message)
})

logger.emit("error", "Spilled Milk")
logger.emit("error", "Eggs Cracked")

// Chat Emitter
var events = require("events")
var EventEmitter = events.EventEmitter
var chat = new EventEmitter()
chat.on("message", function(message) {
  console.log(message)
})
// emit call to 'message' callback
chat.emit("message", "hello, how are you doing")
```

## Multiple Events

It is possible to register multiple callbacks for a single request emitter. When
the request is received, the response will be sent to the requestor, and the
console log will occur.

```javascript
var http = require("http")

var server = http.createServer()
server.on("request", function(request, response) {
  response.writeHead(200)
  response.write("Hello, this is dog")
  response.end()
})
server.on("request", function(request, response) {
  console.log("New request coming in...")
})
server.listen(8080)
```

```bash
# terminal 1
$ node server.js
New request coming in...
```

```bash
# terminal 2
$ curl http://localhost:8080/
Hello, this is dog
```

## HTTP Echo Server

In the last lesson, we registered an event for http server to respond to requests,
using the request event callback.

`http.createServer` returns a new web server object. It allows you to pass a
requestListener function, which it uses to respond to requests.

```javascript
http.createServer(function(request, response) { ... });
```

The ‘request’ event makes a call to this function, passing the two parameters into
the function.

### Alternative Syntax

This alternative syntax is typically how you add event listeners in Node.

```javascript
// create server with no parameters
var server = http.createServer();

// register request event callback
server.on('request', function(request, response) { … });

// register event callback when server is closed
server.on('close', function() { … });
```

# Streams

For efficiency, we need to be able to access data chunk-by-chunk, piece-by-piece,
and sending the data as it receives each chunk. By processing and sending each
chunk, less memory is used.

Streams can be readable, writeable, or both. The API described here is for streams
in Node version 0.10.x (the streams2 API).

With our server example, the request is a readable stream, and the response is a
writable stream.

```javascript
http
  .createServer(function(request, response) {
    response.writeHead(200)
    response.write("<p>Dog is running.</p>")
    setTimeout(function() {
      response.write("<p>Dog is done.</p>")
      response.end()
    }, 5000)
  })
  .listen(8080)
```

The browser immediately sends the response with the first write to the response
stream. 5 seconds later we write the “Dog is done” string to the response, then
close the response object, ending the stream.

How might we read from the request? The request object is a readable stream, which
also inherits from EventEmitter. It can communicate with other objects through
events, such as ‘readable’ which is fired when the object is ready to consume data,
and the ‘end’ event which is fired when it is done.

```javascript
http
  .createServer(function(request, response) {
    response.writeHead(200)
    request.on("readable", function() {
      var chunk = null
      while (null !== (chunk = request.read())) {
        console.log(chunk.toString())
      }
    })
    request.on("end", function() {
      response.end()
    })
  })
  .listen(8080)
```

We have to use toString() to convert the chunk, because it
provides a buffer where binary data might be present.

In this case we’re logging the data we receive from the client to
the console. Instead of doing this we can provide the same data
back to the client that we’ve received.

```javascript
http
  .createServer(function(request, response) {
    response.writeHead(200)
    request.on("readable", function() {
      var chunk = null
      while (null !== (chunk = request.read())) {
        response.write(chunk)
      }
    })
    request.on("end", function() {
      response.end()
    })
  })
  .listen(8080)
```

In this scenario, response.write converts the chunk to a string for us. When we want to redirect the request stream back to the response stream we can instead use request.pipe(response). This same code above can be refactored with the following:

```javascript
http
  .createServer(function(request, response) {
    response.writeHead(200)
    request.pipe(response)
  })
  .listen(8080)
```

```bash
$ curl -d 'hello' http://localhost:8080
Hello
```

This is similar to the pipe operator used on the Bash command line, used to stream
the output from one operation into the next one.

When you can, use pipe instead of listening to the readable event and manually
reading the chunks. This helps protect your application from future breaking
changes to the streams API provided by Node, which may change in the future given
that NodeJS is still very young (v0.10.0). In the NodeJS documentation, it is
reported how stable the API is, meaning how likely it is to change and thus break
your existing functionality that depends on the API.

For instance, the File System has a Stability rating of 3 - Stable, with the Stream
API rated as 2 - Unstable.

## Reading and Writing a File

```javascript
var fs = require("fs")
var file = fs.createReadStream("readme.md")
var newFile = fs.createWriteStream("readme_copy.md")
file.pipe(newFile)
```

Streaming is so powerful, so simple to use with the pipe function, that there are
3rd party libraries that depend on it. For instance the Gulp.js build system, which
exposes the pipe function as its public API so you can do many sorts of
manipulations on its assets with very few lines of code.

```javascript
var fs = require("fs")
var http = require("http")

http
  .createServer(function(request, response) {
    var newFile = fs.createWriteStream("readme_copy.md")
    request.pipe(newFile)

    request.on("end", function() {
      response.end("uploaded!")
    })
  })
  .listen(8080)
```

```shell
$ curl --upload-file readme.md http://localhost:8080
```

We can pipe any read stream into any write stream. In this example we can read from
a request instead of from a file. We listen to the ‘end’ event for the request so
that we can close the response stream once completed.

We are streaming pieces of the file from the client to the server, then the server
is streaming those pieces to the disk as they are being read from the request.
Because Node is non-blocking, if we try to upload two files to the same server,
Node can handle them simultaneously.

Ryan Dahl created NodeJS was to deal with the issue of file uploads. Many web
applications try to receive the entire file into memory before writing it to the
disk, which can cause issues on the server side, which can also block other users
of the same web application.

It’s also not possible to provide file upload progress to the user as it’s being
uploaded.

## File Uploading Progress

```bash
$ curl --upload-file file.jpg http://localhost:8080
progress: 3%
progress: 6%
...
progress: 99%
progress: 100%
```

```javascript
var fs = require("fs")
var http = require("http")

http
  .createServer(function(request, response) {
    var newFile = fs.createWriteStream("readme_copy.md")
    var fileBytes = request.headers["content-length"]
    var uploadedBytes = 0
    request.on("readable", function() {
      var chunk = null
      while (null !== (chunk = request.read())) {
        uploadedBytes += chunk.length
        var progress = uploadedBytes / fileBytes * 100
        response.write("progress: " + parseInt(progress, 10) + "%\n")
      }
    })

    request.pipe(newFile)

    request.on("end", function() {
      response.end("uploaded!")
    })
  })
  .listen(8080)
```

Because the stream of request contents to the newFile object (writeable file stream)
is being established immediately after the request object has registered the
‘readable’ function callback, the pipe is set up immediately to feed into the file.
As soon as the request object is ready to read chunks of data from, it starts to
process each chunk and provide the progress back to the client making the request.

## Output to Standard Output

```javascript
var fs = require("fs")
var file = fs.createReadStream("fruits.txt")
file.pipe(process.stdout)
```

# Modules

We’ve loaded modules using ‘require’ in past lessons.

```javascript
var http = require("http")
var fs = require("fs")
```

How does ‘require’ return the libraries?
How does it find these files?

_custom_hello.js_

```javascript
var hello = function() {
  console.log("hello!")
}
module.exports = hello
```

_custom_goodbye.js_

```javascript
exports.goodbye = function() {
  console.log("bye!")
}
```

_app.js_

```javascript
var hello = require('./custom_hello');
Var gb = require('./custom_goodbye');
hello();
gb.goodbye();
```

With our hello module, we’re only making a single method public by assigning it to
module.exports. With the goodbye module we could assign more than just this single
function to the module.

We can optionally require the module and then call the method directly.

```javascript
require("./custom_goodbye").goodbye()
```

## Export Multiple Functions

_my_module.js_

```javascript
var foo = function() { … }
var bar = function() { … }
var baz = function() { … }

exports.foo = foo
exports.bar = bar
```

_app.js_

```javascript
var myMod = require("./my_module")
myMod.foo()
myMod.bar()
```

Because we did not export the ‘baz’ function, it is private, and only accessible
from within the module.

## Making HTTP Requests

_app.js_

```javascript
var http = require("http")

var message = "Here's looking at you, kid."
var options = {
  host: "localhost",
  port: 8080,
  path: "/",
  method: "POST"
}

var request = http.request(options, function(response) {
  response.on("data", function(data) {
    console.log(data) // logs response body
  })
})
request.write(message) // begins request
request.end() // finishes request
```

## Encapsulating the Function

We can make this simpler by wrapping it in a function call.

_make_request.js_

```javascript
var http = require("http")
var makeRequest = function(message) {
  var options = {
    host: "localhost",
    port: 8080,
    path: "/",
    method: "POST"
  }

  var request = http.request(options, function(response) {
    response.on("data", function(data) {
      console.log(data) // logs response body
    })
  })
  request.write(message) // begins request
  request.end() // finishes request
}
module.exports = makeRequest
```

_app.js_

```javascript
var makeRequest = require("./make_request")

makeRequest("Here's looking at you, kid")
makeRequest("Hello, this is dog")
```

## Node Package Manager (NPM)

Where does require look for modules?

```javascript
var make_request = require("./make_request") // looks in same directory
var make_request = require("../make_request") // looks in parent directory

// looks at absolute path
var make_request = require("/Users/eric/nodes/make_request")

// looks for it inside 'node_modules' directory
var make_request = require("make_request")
```

Looks for node_modules directory:

* In the current directory
* In the parent directory
* In the parent’s parent directory
* Etc.

Each directory inside of ‘node_modules’ is a package that represents a module.

Packages come from NPM (Node Package Manager).

NPM comes with node, there is a module repository, and it handles dependencies
automatically, and makes it easy to public modules.

http://www.npmjs.org/

## Installing a NPM Module

Inside of /home/my_app:

```bash
$ npm install request
```

This will install the ‘request’ package inside of /home/my_app/node_modules/request

```javascript
// - /home/my_app/app.js
// loads from local node_modules directory
var request = require("request")
```

## Local vs Global

Sometimes you may want to install packages globally, instead of only within a
specific application.

```bash
$ npm install coffee-script -g
```

This package comes with an executable that we can use from the command line:

```bash
$ coffee app.coffee
```

A globally installed NPM module cannot be required.

```javascript
// will not work
var coffee = require("coffee-script")
```

We still have to install the coffee-script module locally for the application to
require it into our program.

## Finding Modules

You can find libraries that are useful in the NPM registry website, in Github, or you can search from the command line:

```bash
$ npm search request
```

## Defining Your Dependencies

_my_app/package.json_

```javascript
{
  "name": "My App",
  "version": "1",
  "dependencies": {
    "connect": "1.8.7"
  }
}
```

```bash
$ npm install
```

When you get a node project, the node_modules folder won’t be present. You’ll have
to run ‘npm install’ to install the needed packages.

Inside of my_app/node_modules/connect, you'll notice that each package has it's own
set of dependencies as well. NPM installs those dependencies as well.

## Semantic Versioning

`"connect": "1.8.7"`

Major version is 1, Minor is 8, Patch is 7.

A major version change may completely change the API. A minor version is less likely, and a patch shouldn’t.

* “connect”: “~1” - Will fetch any version greater to or equal to 1.0.0, yet less than 2.0.0
* “connect”: “~1.8” - Will fetch versions greater than 1.8.0, less than 1.9.0
* “connect”: “~1.8.7” - Will fetch versions greater than 1.8.7, less than 1.9.0

See http://semver.org/ for more information

# Express

Node is very low level. You’ll want to build a web framework if you’re working on a very large web application. Or you can use an existing framework such as Express.

"Sinatra inspired web development framework for Node.js -- insanely fast, flexible, and simple"

* Easy route URLs to callbacks
* Middleware (from Connect)
* Environment based configuration
* Redirection helpers
* File Uploads

```bash
# install module and add to package.json
$ npm install --save express
```

```javascript
var express = require("express")
var app = express()

// configure root route
app.get("/", function(request, response) {
  // serve file from current directory
  response.sendFile(__dirname + "/index.html")
})
app.listen(8080)
```

## Express Routes

We want to create an endpoint that receives a certain twitter users name, obtains that users tweets from Twitter, and returns them.

_app.js_

```javascript
var request = require("request")
var url = require("url")

app.get("/tweets/:username", function(req, response) {
  var username = req.params.username
  options = {
    protocol: "http:",
    host: "api.twitter.com",
    pathname: "/1/statuses/user_timeline.json",
    query: { screen_name: username, count: 10 }
  }

  var twitterUrl = url.format(options)
  request(twitterUrl).pipe(response)
})
```

The Twitter API requires users to authenticate, so there will be more code required
to do this.

```bash
$ curl -s http://localhost:8080/tweets/eallam/

$ npm install prettyjson -g
$ curl -s http://localhost:8080/tweets/eallam/ | prettyjson
```

## Express Templates

```bash
$ npm install --save ejs
```

_my_app/package.json_

```bash
"dependencies": {
  "express": "4.9.6",
  "ejs": "1.0.0"
}
```

_my_app/app.js_

```javascript
app.get('/tweets/:username', function(req, response) {
   …
   request(url, function(err, res, body) {
     var tweets = JSON.parse(body);
     response.locals = {tweets: tweets, name: username};
     response.render('tweets.ejs');
   }
}
```

_my_app/views/tweets.ejs_

```html
<h1>Tweets for @<%= name %></h1>
<ul>
  <% tweets.forEach(function(tweet) { %>
    <li><%= tweet.text %></li>
  <% }); %>
</ul>
```

# Socket IO

Node works very well for providing real time communication, which is perfect for
running a chat server.

Typically the HTTP request/response cycle involves a request, the browser waits for
a response, and then the server responds. That is the end of the connection.

With Websockets, we can transmit information back and forth at the same time. This
is known as a full duplex connection. We cannot rely on every web browser to
support WebSockets, so we have to use a library as a fallback for when the browser
does not support socket connections.

```bash
$ npm install --save socket.io
```

_app.js_

```javascript
var express = require("express")
var app = express()
var server = require("http").createServer(app)
var io = require("socket.io")(server)

io.on("connection", function(client) {
  console.log("Client connected…")
})
app.get("/", function(req, res) {
  res.sendFile(__dirname + "/index.html")
})
server.listen(8080)
```

Here we are passing the server object to the socket.io library so that it can use
the server to listen for requests. We are registering a connection event with
logger, and then also configuring a path for the request to be received by.

## Socket.io for Websockets

```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io.connect('http://localhost:8080');
</script>
```

## Sending Message to Client

_app.js_

```javascript
io.on("connection", function(client) {
  console.log("Client connected…")
  // emit the message event on the client
  client.emit("messages", { hello: "world" })
})
```

_index.html_

```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io.connect('http://localhost:8080');
  socket.on('messages', function(data) {
    alert(data.hello);
  });
</script>
```

```bash
$ node app.js
  Info  - socket.io started
```

http://localhost:8080/

Alert pops up with the hello in the browser, and the console logs that the client
connected.

## Sending Messages to Server

_app.js_

```javascript
io.on("connection", function(client) {
  client.on("messages", function(data) {
    console.log(data)
  })
})
```

_index.html_

```html
<script>
  var socket = io.connect('http://localhost:8080');
  $('#chat_form').submit(function(e) {
    var message = $('#chat_input').val();
    socket.emit('messages', message);
  });
</script>
```

```bash
$ node app.js
  Info  - socket.io started
```

## Broadcasting Messages

We’re trying to create a chat room, not simply send and receive messages. Luckily there is a broadcast method supported by Socket.io

```javascript
socket.broadcast.emit("message", "Hello")
```

This will send the message to all the other connected sockets (chat room clients).

_app.js_

```javascript
io.on("connection", function(client) {
  client.on("messages", function(data) {
    client.broadcast.emit("messages", data)
  })
})
```

_index.html_

```html
<script>
  var socket = io.connect('http://localhost:8080');
  socket.on('messages', function(data) { insertMessage(data) });
</script>
```

## Saving Data On The Socket

We don’t know who is who on this server, so we need to make some possible way of
registering usernames.

```javascript
io.on("connection", function(client) {
  client.on("join", function(name) {
    client.nickname = name // set the nickname associated with this client
  })
})
```

We’ll prompt the user for their nickname when they connect, and then we’ll emit
that to the server via the ‘join’ event.

```html
<script>
  var server = io.connect('http://localhost:8080');
  server.on('connect', function(data) {
    $('#status').html('Connected to chattr');
    nickname = prompt("What is your nickname?");
    server.emit('join', nickname);
  });
</script>
```

This ensures that the name is available both to the server, and to the client. Now
we need to make the messages listener so that before we broadcast the message we
get the nickname of the client and use that when emitting the message.

_app.js_

```javascript
io.on("connection", function(client) {
  client.on("join", function(name) {
    client.nickname = name
  })
  client.on("messages", function(data) {
    var nickname = client.nickname
    client.broadcast.emit("message", nickname + ": " + message) // broadcasts the name and message
    client.emit("messages", nickname + ": " + message) // sends the same message back to our own client
  })
})
```

# Persisting Data

...
