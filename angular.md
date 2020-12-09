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

### Binding data to templates

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

## Directives

Directives are added to the HTML and start with a star.

**Structural Directives** - create or remove DOM elements 

### ngFor Directive

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

### ngIf Directive

Checks if variable is truthy and only renders if evaluates to true.

```html
<p 
  *ngIf="query">I can access the query! Here it is: {{ query }}</p>
```

### Directives using ng-template

The above examples are '*syntactic sugar*'(syntax used to make code easier to read) which is converted into the ng-template:

```js
<ng-template [ngIf]="mediaItem.watchedOn">
  <div>Watched on {{ mediaItem.watchedOn }}</div>
</ng-template>
```

The ng-template can still be useful if you want to use the same directive for several html tags.

**Attribute Directives** - change the appearance or behaviour of DOM elements

**ngClass Directive**

```js
<mw-media-item
  *ngFor="let mediaItem of mediaItems"
  [mediaItem]="mediaItem"
  (delete)="onMediaItemDelete($event)"
  [ngClass]="{ 'medium-movies': mediaItem.medium === 'Movies', 'medium-series': mediaItem.medium === 'Series'"></mw-media-item>
```

### Creating your own directive

You can create your own directive. In the file, you need a Directive decorator to specify the selector and a HostBinding in the class to link it to a property(in this case a CSS class).
```js
import { Directive, HostBinding } from '@angular/core';

@Directive({
  selector: '[mwFavorite]'
})
export class FavoriteDirective {
  @HostBinding('class.is-favorite') isFavorite = true;
}
```
```html
<svg
    mwFavorite
    class="favorite" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
```

If you want to set values using the directive, you need to put the selector in the HTML in square brackets to let angular know you want to do binding. In the directive, you need to add the input package and add a setter method so that it can be updated.
```html
<svg
    [mwFavorite]="mediaItem.isFavorite"
    class="favorite" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
```
```js
export class FavoriteDirective {
  @HostBinding('class.is-favorite') isFavorite = true;
  @Input() set mwFavorite(value) {
    this.isFavorite = value;
  }
}
```

#### Events in directives

In this example, the directive is bound to the CSS class *is-favorite-hovering* and is toggled by mouse entering and leaving the property's area.

```js
@HostBinding('class.is-favorite-hovering') hovering = false;

@HostListener('mouseenter') onMouseEnter() {
  this.hovering = true;
}
@HostListener('mouseleave') onMouseLeave() {
  this.hovering = false;
}
```

### Events

Add event listener to an HTML tag:
```html
(click)="showArtist($event, artist)"

<li 
  *ngFor="let artist of artists"
  (click)="showArtist($event, artist)">
  <ul>{{ artist.name }}: 
    <li *ngFor="let song of artist.songs">
      <ol>{{ song }}</ol>
    </li>
  </ul>
</li>
```

Functions in a component are declared like this, no need for function or ()=>:
```js
showArtist(e, artist) {
    console.log(e);
    this.query = artist.name;
    console.log(artist.songs);
  }
```

### Properties

```js
[ PROP ]
```
Properties mean that the tag will only be rendered when the expression it is set to has been evaluated.
```html
<img 
  style="width:70px;"
  [src]="'./assets/images' + artist.shortname + '_tn.jpg'"
  alt="{{ 'Photo of ' + artist.name }}">
```

Adding highlighting toggle using property:
```html
<li 
  *ngFor="let artist of artists"
  (click)="showArtist($event, artist)"
  [style.backgroundColor]="artist.highlight ? '#EEE' : '#FFF'">
  <ul>{{ artist.name }}: 
    <li *ngFor="let song of artist.songs">
      <ol>{{ song }}</ol>
    </li>
  </ul>
</li>
```

```js
showArtist(artist) {
    this.query = artist.name;
    artist.highlight = !artist.highlight;
  }
```

### Two-way Binding

Useful for tracking changes in form elements.

```js
[(ngModel)] // for input fields
[(ngControl)] // form elements that are not an input field
```

You need to import the Forms module to use them. When you add a new import you need to add it to the app.module imports section and the import statement to the module it's used in.

```html
<input type="text"
[(ngModel)]="query">
```
This means that the query variable will be updated to whatever is in the input field.

```js
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

### Lifecycle Methods

onInit - requires additional imports, need to subscribe to observable

In app.module.ts file:
```js
import { HttpClientModule } from '@angular/common/http';

imports: [ BrowserModule, AppRoutingModule, FormsModule, HttpClientModule ]
```

The class/module needs to implement OnInit to use it. This is using an HTTP get request to get the information from a json file in the assets folder, instead of having the artists info in the class.

```js
export class AppComponent implements OnInit {
  title = 'the Learn Angular app';
  query: string;
  artists: any[];
  something: any;

  showArtist(artist) {
    this.query = artist.name;
    artist.highlight = !artist.highlight;
  }

  constructor(private http: HttpClient) {
    this.query = '';
  }

  ngOnInit(): void {
    this.http.get<Object>('../assets/data.json').subscribe(
      data => {
        this.artists = data;
      }
    );
  }
}
```


### Generating Components

Easiest to use the CLI - *ng generate component artist-items*. This generates a sub-folder inside app with four files - component, html, css and test. It automatically generates import statements, the decorator and an export of the class. It also updates the app module, bringing in an import statement and adding it to declarations.

You need to import the component into where you want to use it. The inputs section of the decorator is where you name variables which are passed into it through props.

ArtistItemsComponent
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-artist-items',
  templateUrl: './artist-items.component.html',
  styleUrls: ['./artist-items.component.css'],
  inputs: ['artist']
})
export class ArtistItemsComponent implements OnInit {

  constructor() { }

  ngOnInit(): void {
  }
}
```
Artist prop being passed into the artist-items component: <app-artist-items [artist]="artist"></app-artist-items>
```html
<li 
  *ngFor="let artist of artists"
  (click)="showArtist(artist)"
  [style.backgroundColor]="artist.highlight ? '#EE3' : '#1FF'">
  <app-artist-items [artist]="artist"></app-artist-items>
</li>
```

It makes your code easier to update and easier to test.

## Pipes

There are prebuilt pipes such as the uppercase pipe shown below. You can also generate pipes with the CLI and it is automatically imported in the app-module file - *ng generate pipe search-heroes*. The pipe class implements PipeTransform which gives you the transform method.

transform(pipeData, pipeModifier) - the pipeData is what you want to pass in and the pipeModifier is what you want to base your filtering on.


```html
<h2>{{hero.name | uppercase}} Details</h2>
<div>Watched on {{ mediaItem.watchedOn | date: 'shortDate' }}</div>
<div>Watched on {{ mediaItem.watchedOn | date }}</div>
<h2>{{ mediaItem.name | slice: 0: 10 }}</h2>

<li 
  *ngFor="let artist of (artists | searchArtists: query)"
  (click)="showArtist(artist)"
  [style.backgroundColor]="artist.highlight ? '#EE3' : '#1FF'">
  <app-artist-items [artist]="artist"></app-artist-items>
</li>
```
In this artist example which filters the list based on the query from the input box, artists is piped in to the searchArtists pipe(the name is set by decorator in searchArtistsPipe class) and then query is given as the second parameter to the transform method. The transform method just filters the artists based on their names.

```js
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'searchArtists'
})
export class SearchArtistsPipe implements PipeTransform {

  transform(pipeData, pipeModifier): any {
    return pipeData.filter(item => {
      return item['name'].toLowerCase().includes(pipeModifier.toLowerCase());
    })
  }
}
```

This example creates a category list which doesn't add duplicates.
```js
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'categoryList'
})
export class CategoryListPipe implements PipeTransform {
  transform(mediaItems) {
    const categories = [];
    mediaItems.forEach(mediaItem => {
      if (categories.indexOf(mediaItem.category) <= -1) {
        categories.push(mediaItem.category);
      }
    });
    return categories.join(', ');
  }
}
```
```html
<div *ngIf="mediaItems">{{ mediaItems | categoryList }}</div>
```

## Forms

### Template-driven Forms

Template-driven forms are forms written in the HTML file. They're easy to set up, but give you less control and are harder to unit test than model-driven forms. 

ngModel is a directive which binds input, select and textarea, and stores the required user value in a variable and we can use that variable whenever we require that value. You need to add it to each field that you want to be passed through on submit. 

```html
<form
  #mediaItemForm="ngForm"
  (ngSubmit)="onSubmit(mediaItemForm.value)">
  <label for="name">Name</label>
  <input type="text" name="name" id="name" ngModel>
</form>
```

```js
export class MediaItemFormComponent {
  onSubmit(mediaItem) {
    console.log(mediaItem);
  }
}
```

### Model-driven Forms

Uses the ReactiveFormsModule instead of the normal FormsModule.
```html
<form
  [formGroup]="form"
  (ngSubmit)="onSubmit(form.value)">
  <select name="medium" id="medium" formControlName="medium">
      <option value="Movies">Movies</option>
      <option value="Series">Series</option>
  </select>
</form>
```
```js
import { Component, OnInit } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';

export class MediaItemFormComponent implements OnInit {
  form: FormGroup;

  ngOnInit() {
    this.form = new FormGroup({
      medium: new FormControl('Movies'),
      name: new FormControl(''),
      category: new FormControl(''),
      year: new FormControl(''),
    });
  }

  onSubmit(mediaItem) {
    console.log(mediaItem);
  }
}
```

### Form Validation

You can add form validation into a model driven form using the Validators package.

```js
import { FormGroup, FormControl, Validators } from '@angular/forms';

export class MediaItemFormComponent implements OnInit {
  form: FormGroup;

  ngOnInit() {
    this.form = new FormGroup({
      medium: new FormControl('Movies'),
      name: new FormControl('', Validators.compose([
        Validators.required,
        Validators.pattern('[\\w\\-\\s\\/]+')
      ])),
      category: new FormControl(''),
      year: new FormControl(''),
    });
  }
}

Validators.pattern('[\\w\\-\\s\\/]+')
```
You can disable the button based on whether the form input is valid. You can use just the pattern if you only want to add the CSS class of invalid to the form field but it will not prevent the form being submitted. 

```html
<button type="submit" [disabled]="!form.valid">Save</button>
```

#### Custom Validator

You can add a method to your class which is passed in a form control object to do your own validation. You add it as the second argument to the FormControl object without brackets.
```js
year: new FormControl('', this.yearValidator)

yearValidator(control: FormControl) {
    if (control.value.trim().length === 0) {
      return null;
    }
    const year = parseInt(control.value, 10);
    if (year >= 1900 && year <= 2100) {
      return null;
    }
    return { year: true };
}
```

### Error Handling

This shows how you can do error handling, using the .hasError method or with your custom validation you can set the error object to a variable to get messages from it. 
```html
<input type="text" name="name" id="name" formControlName="name">
<div *ngIf="form.get('name').hasError('pattern')" class="error">
  Name has invalid characters
</div>

<input type="text" name="year" id="year" maxlength="4" formControlName="year">
<div *ngIf="form.get('year').errors as yearErrors" class="error">
  Must be between {{yearErrors.year.min}} and {{yearErrors.year.max}}
</div>
```
```js
 yearValidator(control: FormControl) {
    if (control.value.trim().length === 0) {
      return null;
    }
    const year = parseInt(control.value, 10);
    if (year >= 1900 && year <= 2100) {
      return null;
    } else {
      return {
        year: { min: minYear, max: maxYear }
      };
    }
  }
```
