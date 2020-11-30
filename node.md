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
