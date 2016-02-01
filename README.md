# Node JS

Javascript based platform/framework built on google chrome javascript engine.
Node.js is an open source, cross-platform runtime environment for developing server-side and networking applications.

`Node.js = Runtime Environment + JavaScript Library`

Features:
* async - the server will not wait after API call. The event mechanism gets resp from the previous call
* single threaded - the nodejs server is single threaded and is highly scalable due to non-blocking way of event execution.
* no buffering - they do not buffer data and sends it all at once, the data is sent in chunks 

Additonal points
* nodejs can be typed on any text editor, I use Sublime Text. Atom is also a very good editor.
* node js file extension is *.js

### Exercise 1
Create file main.js
Add the below text to print onto console.

`console.log("Hello");`

Run by using node

`$ node main.js`

### Exercise 2
To import a module in node js application use require keyword.
`var http = require('http');`

The http module is used to create an http web server.
```
http.createServer(function (request, response) {

   // Send the HTTP header 
   // HTTP Status: 200 : OK
   // Content Type: text/plain
   response.writeHead(200, {'Content-Type': 'text/plain'});
   
   // Send the response body as "Hello World"
   response.end('Hello World\n');
}).listen(8081);

// Console will print the message
console.log('Server running at http://127.0.0.1:8081/');
```

add both snippets in a file main.js and run.
Open browser type localhost:8081 you will see helloworld

### Exercise 3
REPL - read eval print loop - it represents a console environment like unix/DOS.
Mainly used for debugging and trying out functions in nodejs
node comes bundled with this envirnoment.
to start this type node
we can try simple math like below:
```
$ node
> 1 + 3
4
> 1 + ( 2 * 3 ) - 4
3
>
> x = 10
10
> console.log(x)
10
>
(^C again to quit)
```

### Exercise 4
 
Node package manager provides 2 manin functionalities:
* Online repo of node.js packages that is searchable
* Command line tool to install node dependencies
```
$npm --version

$ npm install <Module Name>

$ npm install express

var express = require('express'); // use in program

$ npm ls // to display all locally installed packages

$ npm uninstall express
```
Globally installed node packages can only be accessed by REPL, command line.
To install globally use node install express -g
Express is a light-weight Sinatra-inspired web development framework. Express provides several great features such as an intuitive view system, robust routing, an executable for generating applications and much more.

### Exercise 5
Using package.json 
It is located in the root directory of any Node application/module.
It is used to define properties of a package.
Open node_modules/expresss/package.json
```
name - name of the package

version - version of the package

description - description of the package

homepage - homepage of the package

author - author of the package

contributors - name of the contributors to the package

dependencies - list of dependencies. npm automatically installs all the dependencies mentioned here in the node_module folder of the package.

repository - repository type and url of the package

main - entry point of the package

keywords - keywords
```

### Exercise 6
Creating a module
Creation of a package requires package.json to be generated.
```
$ npm init
```
The prompt will ask several properties required by node modules.
Once it is done you can pulish the module and it will be accessable by using npm install.
```
$ npm publish
```

### Exercise 7

Callback- it is like a async function. It is called at the completion of a given task.
Example - If a function reads a file, once the funtion is called the control is returned immidiately to execute the next instruction. Once the file is read the funtion calls a callback funtion with the content of the file as the parameter.

Example of a regular sync call
```
var fs = require("fs"); // fs is a module to read and write to file system 

var data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log("Program Ended");
```

Example of regular call
```
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});

console.log("Program Ended");
```

### Exercise 8
Event driven development
Callback funtions are called when a async functions returns its result.
Event driven programming works on observer pattern. A funtion is declared as an observer or event listener. Node.js has multiple in-built events available through events module and EventEmitter class which is used to bind events and event listeners When an event occurs it starts executing. Below example shows how:

```
// Import events module
var events = require('events');
// Create an eventEmitter object
var eventEmitter = new events.EventEmitter();

// Create an event handler as follows
var connectHandler = function connected() {
   console.log('connection succesful.');
  
   // Fire the data_received event 
   eventEmitter.emit('data_received');
}

// Bind the connection event with the handler
eventEmitter.on('connection', connectHandler);
 
// Bind the data_received event with the anonymous function
eventEmitter.on('data_received', function(){
   console.log('data received succesfully.');
});

// Fire the connection event 
eventEmitter.emit('connection');

console.log("Program Ended.");
```

Output
```
connection succesful.
data received succesfully.
Program Ended.
```

### Exercise 9

What are Buffers?
Javascript unicode does not have good support for octet streams. Node provides Buffer class which provides instances to store raw data similar to an array of integers but corresponds to a raw memory allocation outside the V8 heap.

What are streams?
Streams are objects that let you read data from a source or write data to a destination in continous fashion.

Important global variables
* __filename - global object that needs not be declared but used directly. It gives the name of the file being executed.
* __dirname - represents the name of the directory that the currently executing script resides in
* setTimeout(cb, ms) - global function is used to run callback cb after at least ms milliseconds.
```
function printHello(){
   console.log( "Hello, World!");
}
// Now call above function after 2 seconds
setTimeout(printHello, 2000);
```

* setInterval(cb, ms) - global function is used to run callback cb repeatedly after at least ms milliseconds.
```
function printHello(){
   console.log( "Hello, World!");
}
// Now call above function after 2 seconds
setInterval(printHello, 2000);
```

### Exercise 10

Web server using Node.

```
var http = require('http');
var fs = require('fs');
var url = require('url');


// Create a server
http.createServer( function (request, response) {  
   // Parse the request containing file name
   var pathname = url.parse(request.url).pathname;
   
   // Print the name of the file for which request is made.
   console.log("Request for " + pathname + " received.");
   
   // Read the requested file content from file system
   fs.readFile(pathname.substr(1), function (err, data) {
      if (err) {
         console.log(err);
         // HTTP Status: 404 : NOT FOUND
         // Content Type: text/plain
         response.writeHead(404, {'Content-Type': 'text/html'});
      }else{    
         //Page found     
         // HTTP Status: 200 : OK
         // Content Type: text/plain
         response.writeHead(200, {'Content-Type': 'text/html'});    
         
         // Write the content of the file to response body
         response.write(data.toString());       
      }
      // Send the response body 
      response.end();
   });   
}).listen(8081);

// Console will print the message
console.log('Server running at http://127.0.0.1:8081/');
```

Web client using node
```
var http = require('http');

// Options to be used by request 
var options = {
   host: 'localhost',
   port: '8081',
   path: '/index.htm'  
};

// Callback function is used to deal with response
var callback = function(response){
   // Continuously update stream with data
   var body = '';
   response.on('data', function(data) {
      body += data;
   });
   
   response.on('end', function() {
      // Data received completely.
      console.log(body);
   });
}
// Make a request to the server
var req = http.request(options, callback);
req.end();
```

Web server with Express
```
var express = require('express');
var app = express();

// This responds with "Hello World" on the homepage
app.get('/', function (req, res) {
   console.log("Got a GET request for the homepage");
   res.send('Hello GET');
})


// This responds a POST request for the homepage
app.post('/', function (req, res) {
   console.log("Got a POST request for the homepage");
   res.send('Hello POST');
})

// This responds a DELETE request for the /del_user page.
app.delete('/del_user', function (req, res) {
   console.log("Got a DELETE request for /del_user");
   res.send('Hello DELETE');
})

// This responds a GET request for the /list_user page.
app.get('/list_user', function (req, res) {
   console.log("Got a GET request for /list_user");
   res.send('Page Listing');
})

// This responds a GET request for abcd, abxcd, ab123cd, and so on
app.get('/ab*cd', function(req, res) {   
   console.log("Got a GET request for /ab*cd");
   res.send('Page Pattern Match');
})


var server = app.listen(8081, function () {

  var host = server.address().address
  var port = server.address().port

  console.log("Example app listening at http://%s:%s", host, port)

})
```

Static file handling with node
```
var express = require('express');
var app = express();

app.use(express.static('public'));

app.get('/', function (req, res) {
   res.send('Hello World');
})

var server = app.listen(8081, function () {

  var host = server.address().address
  var port = server.address().port

  console.log("Example app listening at http://%s:%s", host, port)

})
```

### Exercise 11

`http://www.tutorialspoint.com/nodejs/nodejs_interview_questions.htm`
