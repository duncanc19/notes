# Node.js

Javascript which runs outside the browser using the v8 engine.

```js
node script.js // running a file
node // running interactive node in terminal
```
node does not have a document or window object, global variables are stored in global and process
```js
process.exit()
```
### globalThis 

In a browser globalThis references the window object

In node, it references global

This allows us to have global variables which can be accessed from anywhere

### Modules

Old import export syntax(commonJS):
```js
module.exports = {
  largeNumber: largeNumber
}

const script2 = require('./script2');

const a = script2.largeNumber;
```

New import export syntax(ES6):

You need to specify that you're using ES6 modules by renaming files .mjs or by adding 'type': 'module' to the package.json file.

```js
export largeNumber = 254;
import { largeNumber } from 'script2.mjs'
```
Use npm init to set up npm in project and create package.json file. Add 'type': 'module' to package to avoid changing file names.


```js
const http = require('http');
require('fs');
```
http and fs are two of the most important modules you can import - http for http requests and fs for reading files.

Adding Node packages 

Nodemon is a package which reruns the js file whenever there's a change, you can use it by installing it and then adding it to the scripts section of the package.json.
```js
npm init -y 
npm install nodemon --save-dev / adding a dev dependency

"scripts": {
  "start": "nodemon"
}
```

### Building a Server

```js
const http = require('http');

const server = http.createServer((request, response) => {
    console.log(`Headers: ${request.headers}`);
    console.log(`Method: ${request.method}`);
    console.log(`url: ${request.url}`);

    const user = {
        name: "Darren",
        hobby: "Eating"
    }

    response.setHeader('Content-Type', 'text/html');
    response.setHeader('Content-Type', 'application/json');

    // response.end('<h1>Hello!</h1>');
    response.end(JSON.stringify(user));
});

server.listen(3000); // listen on port 3000
```

### Express

Express is a package which simplifies creating an API/setting up a server

Install with: npm install express

```js
const express = require('express');

const app = express();

const user = {
    name: "Bob",
    hobby: "Footie"
}

app.get('/', (req, res) => {
    res.send(user); // automatically does JSON.stringify(), don't need to specify content type
});

app.get('/hello', (req, res) => {
    res.send('Hello!'); // automatically does JSON.stringify(), don't need to specify content type
});

app.post('/profile', (req, res) => {
    res.send(user);
});

app.listen(3000);
```

#### Middleware

Express has middleware with the use function which allows you to modify requests before they get passed through to the routes  

```js
app.use((req, res, next) => {
    console.log("hello!");
    next();
});

app.use(express.urlencoded({extended: false})); // middleware which converts form data
app.use(express.json()); // middleware to parse json
```

### RESTful APIs

An API which follows the rules so that we have compatibility across different systems.

GET to receive a resource, PUT to change a resource, POST to create a resource and DELETE to remove it.

URL routes should make sense - the noun of what we want to receive.

REST APIs are stateless - each request has enough information for a proper response, doesn't need to remember information

```js
app.get('/:id', (req, res) => {
    req.query // localhost:3000/?name=dom&age=15    you'd get { name: 'dom', age: '15' }
    req.body
    req.headers
    req.params // parameters of the URL - localhost:3000/1234   you'd get id: '1234'

    res.status(404).send("Not found");
    res.send(user); // automatically does JSON.stringify(), don't need to specify content type
});
```

#### Serving static files(HTML, CSS)

This is in a file system that has a public folder with an index.html file.

```js
const express = require('express');

const app = express();

app.use(express.static(__dirname + '/public'));

app.listen(3000);
```
