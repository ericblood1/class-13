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
* be sure to test a non existent page e.g. page-x.html

##Basic Express Website

* Jade and indentation (tabs or spaces)
```
<div class="container">
	<p></p>
	<a href="#">Link</a>
</div>

.container
	p
		a(href='#')

```
In a new directory
* install express globally: $ npm install -g express
* install generator: $ npm install -g express-generator
* generate website: $ express express_website
* examine package.json for dependancies
* add latest version of node mailer "nodemailer":"*"   see: http://adilapapaya.com/docs/nodemailer/
* $ npm install

* add to app.js

* var nodemailer = require('nodemailer');

run
$ npm start

Convert to Jade
* with bootstrap starter template
* html2jade

##Routes and jade views

* in app.js 
```
var routes = require('./routes/index');
```
and
```
app.use('/', routes);
```
* can remove users route - not using
* note routes folder and index.js
* index.js renders the jade files and includes an example of an object 'title' that is passed to the template (examine index.jade)
```
extends layout

block content
  h1= title
  // no need for concatination
  p Welcome to #{title}
  // concatination
```

* edit the ttitle attribute in layout.jade
```
doctype html
html
  head
    title #{title}: Express Website
    meta( charset='utf-8' )
```

##Implementing the Jade Templates

* examine index.jade in views (folder number 4)
* note addition of jumbotron and 3 columns below (inside container > row)
* try introducing an error by combining tabs and spaces
* refresh browser and note highlighted Home link
* in layout.jade there is a condition:
```
ul.nav.navbar-nav.navbar-right
         li(class=(title === 'Home' ? 'active' : ''))
```

## routing in app.js
* we've added routing
```
var routes = require('./routes/index');
var about = require('./routes/about');
var contact = require('./routes/contact');
```
and
```
app.use('/', routes);
app.use('/about',about);
app.use('/contact',contact);
```
* and we've added about.js and contact.js
* no databases here so the files are simple
* and we've add about.jade and contact.jade

##Node Mailer Module

* examine contact.jade
* examine contact route (contact.js)
```
var nodemailer = require('nodemailer');
```
* transporter variable provides credentials
* main options for the standard email options including plain text and html versions
* within the versions the variables are coming from contact.jade (name, email, etc)
* need to be logged into gmail in order for it to work










































