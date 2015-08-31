#Session 13

## Node Basic HTTP Server

```
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

$ npm start

and check in browser @ localhost

## Serving Pages

* remove all lines from server.js except the first and add modules
```
var http = require('http');
var url = require('url');
var path = require('path');
var fs = require('fs');
```
* create an array with available mime types
```
var mimeTypes = {
	"html" : "text/html",
	"jpeg" : "image/jpeg",
	"jpg" : "image/jpeg",
	"png" : "image/png",
	"js" : "text/javascript",
	"css" : "text/css"
};
```
* create server function (like the code from the Node.js web site but expanded)
```
// note request and response variables
http.createServer(function(req, res){
	var uri = url.parse(req.url).pathname;
	var fileName = path.join(process.cwd(),unescape(uri));
	console.log('Loading '+ uri);
	var stats;
// look for the file eg. page.html 
	try{
		stats = fs.lstatSync(fileName);
// if its not there send 404 error
	} catch(e) {
		res.writeHead(404, {'Content-type': 'text/plain'});
		res.write('404 Not Found\n');
		res.end();
		return;
	}

// Check for file/directory
	if(stats.isFile()){
// get the mime type return 200 - everything ok
		var mimeType = mimeTypes[path.extname(fileName).split(".").reverse()[0]];
		res.writeHead(200, {'Content-type': mimeType});
// fs file system module method createReadStream
		var fileStream = fs.createReadStream(fileName);
		fileStream.pipe(res);
// if its a directory redirect - 302 - to index page 
	} else if(stats.isDirectory()){
		res.writeHead(302,{
			'Location' : 'index.html'
		});
		res.end();
// if its not a file or directory do 500 internal error
	} else {
		res.writeHead(500, {'Content-Type' : 'text/plain'});
		res.write('500 Internal Error\n');
		res.end();
	}
}).listen(3000);
```
* create new files index.html etc.
* test with $ npm start and open localhost:3000
* be sure to test a non existant page

##Basic Express Website


















































