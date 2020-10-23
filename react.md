# React.js

### Set up React

You need node installed - npm comes with node

```js
npx create-react-app hello-react --use-npm
```

yarn will be used as default if not given flag

if you are cloning a repo, you may need to run npm install to install all the dependencies of the project before you can run npm start to open the app in localhost

### index.js file


ReactDOM.render() takes in two arguments - the first is the element we want to create, the second is where to put it
React.createElement takes 3 arguments - first is the element, second is any properties we want the element to have, third is children

```js
ReactDOM.render(<App />, document.getElementById('root'));
ReactDOM.render(
    React.createElement("h1", null, "Hello there!"), 
    document.getElementById('root')
);
React.createElement("h1", { style: { color: 'red' } }, "¡Hola!") // example of passing in property

React.createElement("div", { style: { color: 'red' } }, React.createElement("h1", null, "¡Hola chicos!"))
// you can nest createElements but gets unmanageable so it's best to use JSX
```

In the second example, the element is an h1 tag with Hello there inside it, and it is passed into the root element of the web page

### JSX - Javascript as XML

A language extension which lets you write tags in JS

React uses Babel to convert JSX syntax back into the standard JS, which means we don't have to worry about it
```js
ReactDOM.render(
    <ul>
      <li>Pizza</li>
      <li>Sushi</li>
    </ul>, 
    document.getElementById('root')
);
// this would be converted in Babel to lots of createElements like above
```

JSX is mostly similar to HTML, but there are some differences 
It can use string interpollation with variables
```js
let city = {name: "Madrid", country: "Spain"}

ReactDOM.render(
    <h1 id="heading" className="cool-text">{city.name} is in {city.country}</h1>, 
    document.getElementById('root')
); // you can use id like in html, but class changes to className(to avoid confusion with JS classes)
```

### React Components

Components are collections of react elements, a function which returns some kind of UI

When components are called in render, self closing tags are often used

```js
function Hello() {
    return (
        <div>
            <h1>Welcome to my website</h1>
            <p>It's pretty nice right?</p>
        </div>
    )
}

ReactDOM.render(
    <Hello />, 
    document.getElementById('root')
);
```

Components should always be capitalised

### Properties

You can pass information in when you call your component with props

```js
function Hello(props) {
    Console.log(Object.keys(props));
    return (
        <div>
            <h1>Welcome to {props.library}</h1>
            <p>{props.message}</p>
            <p>There are {props.number} properties</p>
        </div>
    )
}

ReactDOM.render(
    <Hello library="React" message="Isn't it great?!" number={3} />, 
    document.getElementById('root')
);
```

You can also use destructuring for the props argument to make it easier to access them
```js
function Hello({library, message, number}) {
    return (
        <div>
            <h1>Welcome to {library}</h1>
            <p>{message}</p>
            <p>There are {number} properties</p>
        </div>
    )
}
```
