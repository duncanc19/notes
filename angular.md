# Angular

### Angular CLI

Install: npm install -g @angular/cli

Command Line Interface which scaffolds an application similar to create-react-app

Common commands:
```js
ng version
ng help
ng new filename // create a new project
ng serve // uses webpack to run a temporary live server which updates as you make changes
ng serve -o // opens app in browser
ng build // process files and get them ready for deployment
ng g type name // generates different features in your app, e.g. commenting system
```

[Useful page for Angular CLI commands](https://www.freecodecamp.org/news/how-to-install-angular-on-windows-a-guide-to-angular-cli-node-js-and-build-tools/)

### Angular Project File System

The *ng new* command creates a lot of folders and files.

The **angular.json** is one of the most important, as it's where you can change the config, add scripts and change how the CLI runs the app.

**.editorconfig** file gives styling conventions, so all developers can follow the style guide. You can add a plug-in to your IDE to automatically conform to this config.

The **environment** files specify whether to run in production or development mode - they are used when using *ng serve* and *ng build*.

The **main.ts file** is where the modules are loaded which will be rendered on the page.  

**Import statements** in files are separated into local imports and external imports, don't need extensions at end.

When there's a new project, AppModule is imported in main.ts from the app.module file

The AppModule file is where you set up parameters for the entire application as well as the application logic. 

Uses Decorator pattern, which gives an object describing the program and shows what components are used.

The @Component section here is an example of a decorator - it gives the selector(new HTML tag), templateUrls is the component's HTML content and styleUrls its style.
```js
import { Component } from '@angular/core';

@Component({
  selector: 'app-root', // creates custom html tag which can be used in application
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'learnangular';
}
```

Inside the class export, you can create variables which will be accessible in other files. You can access it in an html page with {{ }}.
```html
<span>{{ title }} app is running!</span>
```

You can use npm to install packages in the directory, using --save-dev stops it from being used in production.

You then need to add them to the angular.json file. 

```js
"styles": [
   "../node_modules/bootstrap/dist/css/bootstrap.css"
 ],
"scripts": [
   "../node_modules/jquery/dist/jquery.js",
   "../node_modules/bootstrap/dist/js/bootstrap.bundle.js"]
]
```

### Templates

You can have a templateUrl in the decorator which just points to an HTML file, or you can change it to template and write ES6 syntax in the decorator itself(wrap html in backticks).

```js
@Component({
  selector: 'app-root',
  template: `
    <h1>Welcome to my website!</h1>
    <p>Hello there</p>
  `,
  styleUrls: ['./app.component.css']
})
```

However, it's often easier and cleaner to keep it in the html file. You can do the same with styles in the decorator.

#### Binding data to templates

**Expressions** {{ }} making variables declared in your components available inside your templates.

**Directives** - commands used in your HTML which look like attributes with a few extra characters.

You can set up variables using the constructor when the class is created.

Angular is regularly used with Typescript, where you declare the variable's type.

```js
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'the Learn Angular app';
  query: string;

  constructor() {
    this.query = 'Bonjour';
  }
}
```
```html
<h1>Welcome to my website: {{ title }}</h1>
<p>I can access the query! Here it is: {{ query }}</p>
```

### Directives

This example shows a variable which is initialised to an array of objects. Two directives(ngFor - like a foreach) are used in the html file to loop through the artists and then the songs in the artist.
```js
export class AppComponent {
  title = 'the Learn Angular app';
  query: string;
  artists: any[];
  something: any;

  constructor() {
    this.query = 'Bonjour';
    this.artists = [
      {
        "name": "Michael Jackson",
        "songs": ["Billie Jean", "Smooth Criminal", "You Rock My World"]
      },
      {
        "name": "Justin Bieber",
        "songs": ["Sorry", "What Do You Mean", "Beauty and a Beat"]
      }
    ]
  }
}
```

```html
<li *ngFor="let artist of artists">
  <ul>{{ artist.name }}: 
    <li *ngFor="let song of artist.songs">
      <ol>{{ song }}</ol>
    </li>
  </ul>
</li>
```
