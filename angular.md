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
