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

### Composing Components

```js
function City({name}) {
    return (
        <div>
            <h1>Welcome to {name}</h1>
            <p>It's a beautiful city</p>
        </div>
    )
}

function App() {
    return (
        <div>
            <City name="New York" />
            <City name="Brasilia" />
            <City name="Rome" />
        </div>
    )
}

ReactDOM.render(
    <App />, 
    document.getElementById('root')
);
```

### Rendering Lists

You can render props dynamically using map so you don't have to worry about how many elements are passed in e.g. the length of an array

```js
const lakeList = ["Lake Titicaca", "Lake Baikal", "Lake Windermere"]

function App({lakes}) {
    return (
        <div>
            <h1>My Favourite Lakes!</h1>
            <ul>
                {lakes.map(lake => <li>{lake}</li>)}
            </ul>
        </div>
    )
}

ReactDOM.render(
    <App lakes={lakeList}/>, 
    document.getElementById('root')
);
```

### Rendering Lists of Objects

```js
const lakeList = [
    {name: "Lake Titicaca",location: "Peru"},
    {name: "Lake Baikal",location: "Russia"},
    {name: "Lake Windermere",location: "The UK"}
]

function App({lakes}) {
    return (
        <div>
            <h1>My Favourite Lakes!</h1>
            <ul>
                {lakes.map(lake => <li>{lake.name} is in {lake.location}</li>)}
            </ul>
        </div>
    )
}
```

### Adding Keys

Keys help to keep track of which items have changed, added or removed. 
It's an identifier for a dynamically created element.

```js
function App({lakes}) {
    return (
        <div>
            <h1>My Favourite Lakes!</h1>
            <ul>
                {lakes.map(lake => <li key={lake.name}>{lake.name} is in {lake.location}</li>)}
            </ul>
        </div>
    )
}
```

### Conditional Rendering

```js
function Lake({name}) {
    return <h1>Visit {name}!</h1>;
}

function SkiResort({name}) {
return <h1>Visit {name}!</h1>;
}

function App(props) {
    if (props.season == "summer") {
        return <Lake name="Lake Windermere"/>;
    } else if (props.season == "winter") {
        return <SkiResort name="a ski slope"/>
    } else {
        return <h1>Come back in winter or summer!</h1>;
    }
    
}

ReactDOM.render(
    <App season="summer"/>, 
    document.getElementById('root')
);
```

### React Fragments

There must always be one parent element which is returned by a component, you can't pass in h1 and p for example, they'd have to be wrapped in a div.

You can end up with loads of divs though on the HTML page, to combat this you can use React.Fragment, which doesn't show up as a div. The shorthand for this is an empty tag.

```js
function App() {
    return (
        <> // or <React.Fragment>
            <Lake name="Lake Malawi" />
            <SkiResort name="a ski resort" />
        </> // </React.Fragment>
    )
}
```

### Array Destructuring - JS feature

```js
const [food, snack, fruit] = ["popcorn", "pretzels", "pineapple"]
const [,,fruit] = ["popcorn", "pretzels", "pineapple"]
// you can just put commas if you don't need to access other variables
console.log(fruit) // prints pineapple
```

### useState

A hook is a function which allows you to add functionality to a component. React has several hooks to handle different use cases, useState is a hook.

The first value(status) that is returned from the useState function is the state value and the second value is a function to change that state value.

You can add destructuring to the React import statement to access useState without writing React.useState.

```js
import React, { useState } from 'react';

function App() {
    const [status, setStatus] = useState("Open");
    return (
        <>
            <h1>Status: {status}</h1>
            <button onClick={() => setStatus("Closed")}>Close Shop</button>
            <button onClick={() => setStatus("Open")}>Open Shop</button>
            <button onClick={() => setStatus("Back in 5")}>Take a Break</button>
        </>
    )
}

ReactDOM.render(
    <App />, 
    document.getElementById('root')
);
```

Example with several state variables:

```js
function App() {
    const [status, setStatus] = useState("Open");
    const [manager, setManager] = useState("Alex");
    const [year, setYear] = useState(2020);
    return (
        <>
            <div>
                <h1>Status: {status}</h1>
                <h3>Manager: {manager}</h3>
                <button onClick={() => setStatus("Closed")}>Close Shop</button>
                <button onClick={() => setStatus("Open")}>Open Shop</button>
                <button onClick={() => setStatus("Back in 5")}>Take a Break</button>
                <button onClick={() => setManager("Michelle")}>Change Manager</button>
            </div>
            <div>
                <h2>It's {year}!</h2>
                <button onClick={() => setYear(year + 1)}>Happy New Year!</button>
            </div>        
        </>
    )
}
```

### useEffect

The things that we want the component to do other than the UI are called effects. useEffect lets us add side effects to changes in the DOM

```js
import React, { useState, useEffect } from 'react';
import ReactDOM from 'react-dom';
import './index.css';

function Checkbox() {
    const [checked, setChecked] = useState(false);
    useEffect(() => {
        alert(`checked: ${checked.toString()}`);
    });
    return (
        <>
            <input 
                type="checkbox" 
                value={checked} 
                onChange={() => 
                    setChecked(checked => !checked)
                }
            />  
            {checked ? "checked" : "not checked"}
        </>
       
    )
}

ReactDOM.render(
    <Checkbox />, 
    document.getElementById('root')
);
```

### Updating the useEffect dependency array

useEffect takes a second argument called the dependency array, so that you can specify the state variable you want to listen to changes in. 

It allows for smart rendering, so it won't trigger unnecessary rerenders and focus in on the element that has changed.

In this example, without the dependency array argument in useEffect, every time a change is made in either of the input boxes, both would be console logged.

```js
function App() {
    const [val, setVal] = useState("");
    const [val2, setVal2] = useState("");

    useEffect(() => {
        console.log(`Your favourite food: ${val}`);
    }, [val]);

    useEffect(() => {
        console.log(`Your second favourite food: ${val2}`);
    }, [val2]);
    
    return (
        <>
            <label>
                Favourite food: 
                <input value = {val} onChange={e => setVal(e.target.value)} />
            </label>
            <br />
            <label>
                Second Favourite food: 
                <input value = {val2} onChange={e => setVal2(e.target.value)} />
            </label>
              
        </>
       
    )
}

ReactDOM.render(
    <App />, 
    document.getElementById('root')
);
```

### Fetching data with useEffect

This is an example of fetching info from an API(my Github info) inside useEffect and then displaying the info, passing the account name in as props. 

```js
import React, { useState, useEffect } from 'react';
import ReactDOM from 'react-dom';
import './index.css';

function GithubUser({login}) {
    const [data, setData] = useState(null);
    useEffect(() => {
        fetch(`https://api.github.com/users/${login}`)
            .then(res => res.json())
            .then(setData)
            .catch(console.error);
    }, []);

    if (data) {
        return (
            <div>
                <h1>Say hello to {data.login}!</h1>
                <h3>You can find {data.login} at {data.url}</h3>
                <img source={data.avatar_url} width={100} />
            </div>
        );
    }
    return null;
}


function App() {
       return (
            <GithubUser login="duncanc19" />
       );
}

ReactDOM.render(
    <App />, 
    document.getElementById('root')
);
```
