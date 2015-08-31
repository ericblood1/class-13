#Session 13

## Node Basic HTTP Server

* recall: npm init to create manifest

```
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

$ npm start

and check in browser at localhost:1337

## Serving Pages

* reference folder 2-simpleserver
* removed all lines from server.js except the first and added modules
```
var http = require('http');
var url = require('url');
var path = require('path');
var fs = require('fs');
```
* created an array with available mime types
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
* created a server function (expanded the code from the Node.js web site)
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
* created new files index.html etc.
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
* try http://html2jade.org/ with layout.html

In a new directory:

* install express globally: $ sudo npm install -g express
* install generator: $ npm install -g express-generator
* generate website: $ express express_website
* examine package.json for dependancies
* install dependencies $npm install 

* added latest version of node mailer "nodemailer":"*"   see: http://adilapapaya.com/docs/nodemailer/
* $ npm install
* $ DEBUG=express_website:* npm start


##Routes and Jade Templates

* reference 4-express_website ($ npm install and $ npm start)
* sample from http://getbootstrap.com/getting-started/#examples
* added to app.js var nodemailer = require('nodemailer');
* note routes in app.js 
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
* note routes folder and corresponding elements in index.js: each route has a js file
* index.js renders the jade files and includes an example of an object 'title' that is passed to the template (examine index.jade)

```
<!-- extends layout

block content
  h1= title
  // no need for concatination
  p Welcome to #{title}
  // concatination
``` -->

* change the variable to home and restart the server
```
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Home' });
});
```

* edited the title attribute in views/layout.jade
```
doctype html
html
  head
    title #{title}: Express Website
    meta( charset='utf-8' )
```

* note addition of jumbotron and 3 columns below (inside index.jade)
* try introducing an error by combining tabs and spaces
* refresh browser and note highlighted Home link
* in layout.jade there is a condition:
```
ul.nav.navbar-nav.navbar-right
         li(class=(title === 'Home' ? 'active' : ''))
```

* no databases here so the files are simple
* and we've add about.jade and contact.jade


##Node Mailer Module

* reference 5-nodemailer
* examine contact.jade
* examine contact route (contact.js)
```
var nodemailer = require('nodemailer');
```
* transporter variable provides credentials
* main options for the standard email options including plain text and html versions
* within the versions the variables are coming from contact.jade (name, email, etc)
* need to be logged into gmail in order for it to work

##Push to Heroku
* before pushing make sure you have a working master branch on github
* login to heroku
* download and install Heroku toolbelt
* $ heroku login
* $ git clone https://<pathtoyourapp>.git
* $ cd into clone directory
* $ heroku create - when you create an app, a git remote (called heroku) is also created and associated with your local git repository
* $ git push heroku master
* $ heroku ps:scale web=1 - to ensure one instance of the app is running
* $ heroku open










































