# Javascript Popular Libraries
# =======================================================
**NPM**: Node JS package manager, helps you to manage all the libraries your software relays on. You would define your needs in a file called package.json and run npm install in the command line... BANG your packages are downloaded and are ready to use. Could be used both for front-end and back-end.

**Bower**: Front-end package manager. Not that much good and is not useful these days since most of the devs are relaying on NPM.

**Grunt**: You can create automation for your development environment to pre process codes or create build scripts with a not very simple config file. It was great back there in 2013 but not much these days.

**Gulp**: Automation just like Grunt but instead of configurations you can write JavaScript with streams like it's a node application. Much better than Grunt.

**Webpack**: The coolest kid in the town. It will bundle your app into other bundling patters so you can use all the libraries available in NPM right into your front-end code, load different modules and do many other things. It is amazingly flexible and can create strong development environments. Very trendy these days.

**Browserify**: Similar to Webpack but less powerful.

**Slush and Yeoman**: Project scaffolding systems. You can create starter projects with them. Not good that much, use a Github boilerplate instead.

**Express**: Node JS web application framework. Could be used for routing and anything else needed is cover through middle wares. Very popular and beautifully designed so if you want to create a web application project with node, you are probably using it.


# What is "use strict"

order of CSS and order of JS

# async function
https://ponyfoo.com/articles/understanding-javascript-async-await
gulp.task("clean", async (cb) => {
  await del([".tmp", "build/*"], {dot: true});
  await mkdirp(destination.logs);
  await mkdirp(destination.public);
  return cb;
});

What are callbacks
Why to return callbacks
Passing functions to other functions
using the fat arrow instead of defining new functions
What is javascript V8


gulp.task("bundle", ["clean"], cb => {
  // change these for production and development
  webpackServerBundler.run();
  webpackClientBundler.run();
  return cb();
});

is equal to

gulp.task("bundle", ["clean"], function(cb) {
  // change these for production and development
  webpackServerBundler.run();
  webpackClientBundler.run();
  return cb();
});



# Importing from modules
###### Defining multiple exports in a module

```
// There are 4 exports
export const MyFunction = () => {}
export const MyFunction2 = () => {}
const Var = 1;
const Var2 = 2;
export {
   Var, Var2
}

// Then import it this way
import {MyFunction, MyFunction2, Var, Var2 } from './foo-bar-baz';
```

# Window Object
Multiple initializing of function
```
  window.onGoogleSignIn = Login.onGoogleSignIn = function (googleUser) {
      Login.sendAccessToken(googleUser.getAuthResponse().id_token, "google");
  }
```

EventEmitter

Dispatcher

Flux

EventEmitter
Facebook's EventEmitter is a simple emitter implementation that prioritizes speed and simplicity. It is conceptually similar to other emitters like Node's EventEmitter, but the precise APIs differ.


Explicitly calling / Passing a pointer
For things that are done when event is triggered we just pass a pointer.


Document onLoad vs Window onLoad

The 2 syntaaxes
export default class Some extends AnotherClass{
}

export class Some extends AnotherClass{
}
export default Some;


Defining functions and Classes
var destroyTypeahead = function(){
    $("#university-input").typeahead("destroy");
};


var SectionFeatured = class SectionFeatured extends React.Component{
    render(){
      
    }
};

###### Importing multiple exports from a module
A module can have multiple imports under different name. There is also a defined default export too. When importing, we can export different members using the specific export statements.
For example the following
```
import React, { Component, PropTypes } from 'react'
```
This will grab the exported { **Component**, **PropTypes** } members from the 'react' module and assign them to Component and **PropTypes**, respectively. **React** will be equal to the module's default export.


# Javascript Flow Control
# =======================================================

# Javascript Sync and Async
# =======================================================

# Javascript Events and States
# =======================================================

# Javascript Classes and Objects
# =======================================================

# Javascript Modules (Modular Javascript)
# =======================================================
Loading modules asyncly.
https://www.airpair.com/javascript/posts/the-mind-boggling-universe-of-javascript-modules

http://www.redairship.com/2015/05/making-sense-difference-amd-commonjs-requirejs-browserify/

Getting started with gulp and browserify
justinjohnson.org/javascript/getting-started-with-gulp-and-browserify/

Browserify is just CommonJS module format in the browser, emulating NodeJS.
Webpack can handle CommonJS or RequireJS, but it also adds a lot more. Webpack can handle the entire build process for you if you so please. This includes optimizing images, compiling SCSS, etc.
I'd recommend Webpack, use it for as much or as little as you need. It also has a development server which will serve assets and supports hot reloading. It's really nice not having to refresh the page when changing code.

https://auth0.com/blog/javascript-module-systems-showdown/
https://addyosmani.com/writing-modular-js/
https://www.leanpanda.com/blog/2015/06/28/amd-requirejs-commonjs-browserify/
**Creating a npm module**
https://github.com/Jam3/jam3-lesson-module-basics

There are different module systems: AMD (RequireJS), CommonJS (Node) and the new ES6 module syntax (and the old ES5 Global system of course).
However if you want to use those in your browser you still need to load and wire those modules with some module loader library because browsers still do not support that. For that you could use a module loader like RequireJS, Browserify, SystemJS or es6-module-loader.
SystemJS is my personal favorite because it allows you to load any module system (AMD, CommonJS, ES6) and even use them interchangably in 1 app.
**Update**: In the mean time Webpack has become available and could be considered as a module loader as well.

JSPM
https://www.leanpanda.com/blog/2015/06/28/amd-requirejs-commonjs-browserify/
https://gist.github.com/substack/68f8d502be42d5cd4942

###### Require
Require is used to require javascript modules and not directories
THis is wrong
app.set('views', require('./views'));
This is right
app.use(require('./routes.js'))

require("views")
checks for views.js and if thats not present then views/index.js
Directoreis not accepted

Passing Path
__dirname prints the path
app.set('views', __dirname + '/views');



# Isomophism and Universal Code
Isomorphism is the functional aspect of seamlessly switching between client- and server-side rendering without losing state. Universal is a term used to emphasize the fact that a particular piece of JavaScript code is able to run in multiple environments.
http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/
https://blog.twitter.com/2012/improving-performance-twittercom
https://medium.com/@ghengeveld/isomorphism-vs-universal-javascript-4b47fb481beb#.vkh5zxfj0


# Javascript Versions (ECMAScript or ES)
# =======================================================
ECMAScript is the language, whereas JavaScript, JScript, and even ActionScript 3 are called "dialects". Wikipedia sheds some light on this.
JavaScript has a strange naming history. For its initial release in 1995 as part of Netscape Navigator, Netscape labeled their new language LiveScript, before renaming it to JavaScript a year later, hoping to capitalize on Java's popularity at the time (JavaScript has no actual relationship to Java). In 1996 Netscape submitted JavaScript to ECMA International for standardization. This eventually resulted in a new language standard, labeled ECMAScript. All major JavaScript implementations since have actually been implementations of the ECMAScript standard, but the term JavaScript has stuck for historical and marketing reasons 1. In the real world ECMAScript is usually used to refer to the standard while JavaScript is used when talking about the language in practice.

This has mostly been trivia for JavaScript developers, because ECMAScript didn't change much for the first 15 years of its existence, and real world implementations often differed significantly from the standard. After the initial version of ECMAScript, work on the language continued and two more versions were quickly published. But after ECMASCript 3 came out in 1999, there were no changes made to the official standard for a decade. Instead various browser vendors made their own custom extensions to the language, and web developers were left to try and support multiple APIs. Even after ECMAScript 5 was published in 2009, it took several years for wide browser support of the new spec, and most developers continued to write code in ECMAScript 3 style, without necessarily being aware of the standard.

Around 2012 things started to change. There was more of a push to stop supporting old Internet Explorer versions, and writing code in ECMAScript 5 (ES5) style became much more feasible. At the same time work was underway on a new ECMAScript standard, at which point it became much more common to start referring to JavaScript implementations in terms of their support for different ECMAScript standards. The new standard was originally named ES.Harmony, before eventually being referred to as ECMAScript 6th Edition (ES6). In 2015 TC39, the committee responsible for drafting the ECMAScript specifications, made the decision to move to a yearly model for defining new standards, where new features would be added as they were approved, rather than drafting complete planned out specs that would only be finalized when all features were ready. As a result ECMAScript 6th edition was renamed ECMAScript 2015 (ES2015) before it was published in June.

Currently there are several proposals for new features or syntax to be added to JavaScript. These include decorators, async-await, and static class properties. These are often refered to as ES7, ES2016, or ES.Next features, but should realistically be called proposals or possibilities, since the ECMAScript 2016 specification hasn't been written yet, and might include all or none of those features. TC39 divides proposals into 4 stages. You can see the current state of various proposals on Babel's website.

So where does that leave us in terms of terminology? The following list might be helpful:

    ECMAScript: A language standardized by ECMA International and overseen by the TC39 committee. This term is usually used to refer to the standard itself.
    JavaScript: The commonly used name for implementations of the ECMAScript standard. This term isn't tied to a particular version of the ECMAScript standard, and may be used to refer to implementations that implement all or part of any particular ECMASCript edition.
    ECMAScript 5 (ES5): The 5th edition of ECMAScript, standardized in 2009. This standard has been implemented fairly completely in all modern browsers
    ECMAScript 6 (ES6)/ ECMAScript 2015 (ES2015): The 6th edition of ECMAScript, standardized in 2015. This standard has been partially implemented in most modern browsers. To see the state of implementation by different browsers and tools, check out these compatibility tables.
    ECMAScript 2016: The expected 7th edition of ECMAScript. This is scheduled to be released next summer. The details of what the spec will contain have not been finalized yet
    ECMAScript Proposals: Proposed features or syntax that are being considered for future versions of the ECMAScript standard. These move through a process of five stages: Strawman, Proposal, Draft, Candidate and Finished.

Going forward in this blog, I'll be referring to the recent ECMAScript version as ES6 (since that is how it is best known by most developers), next years spec as ES2016 (since that will be what it is called the whole way through its standardization process, unlike ES6/ES2015) and future language ideas that are not yet part of a draft or finalized spec as ECMAScript proposals or JavaScript proposals. I'll do my best to point back to this post in any cases that might be confusing. 


**1995**: JavaScript is born as LiveScript
**1997**: ECMAScript standard is established
**1999**: ES3 comes out and IE5 is all the rage
**2000–2005**: XMLHttpRequest, a.k.a. AJAX, gains popularity in app such as Outlook Web Access (2000) and Oddpost (2002), Gmail (2004) and Google Maps (2005).
**2009**: ES5 comes out (this is what most of us use now) with forEach, Object.keys, Object.create (specially for Douglas Crockford), and standard JSON
**2015**: ES6/ECMAScript2015 comes out; it has mostly syntactic sugar, because people weren’t able to agree on anything more ground breaking (ES7?)



# ES6 or EcmaScript 2015
# =======================================================
Here are the list of top features of ES6

    Default Parameters in ES6
    Template Literals in ES6
    Multi-line Strings in ES6
    Destructuring Assignment in ES6
    Enhanced Object Literals in ES6
    Arrow Functions in ES6
    Promises in ES6
    Block-Scoped Constructs Let and Const
    Classes in ES6
    Modules in ES6
    New Math, Number, String, Array and Object methods
    Binary and octal number types
    Default rest spread
    For of comprehensions (hello again mighty CoffeeScript!)
    Symbols
    Tail calls
    Generators
    New data structures like Map and Set



###### Default Parameters in ES6
Remember we had to do these statements to define default parameters:
```
var link = function (height, color, url) {
    var height = height || 50
    var color = color || 'red'
    var url = url || 'http://azat.co'
    ...
}
```
They were okay until the value was 0 and because 0 is falsy in JavaScript it would default to the hard-coded value instead of becoming the value itself. Of course, who needs 0 as a value (#sarcasmfont), so we just ignored this flaw and used the logic OR anyway… No more! In ES6, we can put the default values right in the signature of the functions:

```
var link = function(height = 50, color = 'red', url = 'http://azat.co') {
  ...
}
```
By the way, this syntax is similar to Ruby!

###### Template Literals in ES6
Template literals or interpolation in other languages is a way to output variables in the string. So in ES5 we had to break the string like this:
```
var name = 'Your name is ' + first + ' ' + last + '.'
var url = 'http://localhost:3000/api/messages/' + id
```
Luckily, in ES6 we can use a new syntax ${NAME} inside of the back-ticked string:
```
var name = `Your name is ${first} ${last}.`
var url = `http://localhost:3000/api/messages/${id}`
```

###### Multi-line Strings in ES6
Another yummy syntactic sugar is multi-line string. In ES5, we had to use one of these approaches:
```
var roadPoem = 'Then took the other, as just as fair,\n\t'
    + 'And having perhaps the better claim\n\t'
    + 'Because it was grassy and wanted wear,\n\t'
    + 'Though as for that the passing there\n\t'
    + 'Had worn them really about the same,\n\t'

var fourAgreements = 'You have the right to be you.\n\
    You can only be you when you do your best.'
```
While in ES6, simply utilize the backticks:
```
var roadPoem = `Then took the other, as just as fair,
    And having perhaps the better claim
    Because it was grassy and wanted wear,
    Though as for that the passing there
    Had worn them really about the same,`

var fourAgreements = `You have the right to be you.
    You can only be you when you do your best.`
```

###### Destructuring Assignment in ES6
Destructuring can be a harder concept to grasp, because there’s some magic going on… let’s say you have simple assignments where keys house and mouse are variables house and mouse:
```
var data = $('body').data(), // data has properties house and mouse
  house = data.house,
  mouse = data.mouse
```
Other examples of destructuring assignments (from Node.js):
```
var jsonMiddleware = require('body-parser').json
var body = req.body, // body has username and password
  username = body.username,
  password = body.password  
```
In ES6, we can replace the ES5 code above with these statements:
```
var { house, mouse} = $('body').data() // we'll get house and mouse variables
var {jsonMiddleware} = require('body-parser')
var {username, password} = req.body
```
This also works with arrays. Crazy!
```
var [col1, col2]  = $('.column'),
  [line1, line2, line3, , line5] = file.split('\n')
```
It might take some time to get use to the destructuring assignment syntax, but it’s a sweet sugarcoating.

###### Enhanced Object Literals in ES6
What you can do with object literals now is mind blowing! We went from a glorified version of JSON in ES5 to something closely resembling classes in ES6.
Here’s a typical ES5 object literal with some methods and attributes/properties:
```
var serviceBase = {port: 3000, url: 'azat.co'},
    getAccounts = function(){return [1,2,3]}

var accountServiceES5 = {
  port: serviceBase.port,
  url: serviceBase.url,
  getAccounts: getAccounts,
  toString: function() {
    return JSON.stringify(this.valueOf())
  },
  getUrl: function() {return "http://" + this.url + ':' + this.port},
  valueOf_1_2_3: getAccounts()
}
```
If we want to be fancy, we can inherit from serviceBase by making it the prototype with the Object.create method:
```
var accountServiceES5ObjectCreate = Object.create(serviceBase)
var accountServiceES5ObjectCreate = {
  getAccounts: getAccounts,
  toString: function() {
    return JSON.stringify(this.valueOf())
  },
  getUrl: function() {return "http://" + this.url + ':' + this.port},
  valueOf_1_2_3: getAccounts()
}
```
I know, accountServiceES5ObjectCreate and accountServiceES5 are NOT totally identical, because one object (accountServiceES5) will have the properties in the ***proto*** object as shown below:
But for the sake of the example, we’ll consider them similar. So in ES6 object literal, there are shorthands for assignment getAccounts: getAccounts, becomes just getAccounts,. Also, we set the prototype right there in the ***proto***`` property which makes sense (not‘proto’` though:

```
var serviceBase = {port: 3000, url: 'azat.co'},
    getAccounts = function(){return [1,2,3]}
var accountService = {
    __proto__: serviceBase,
    getAccounts,
```
Also, we can invoke super and have dynamic keys (valueOf_1_2_3):
```
    toString() {
     return JSON.stringify((super.valueOf()))
    },
    getUrl() {return "http://" + this.url + ':' + this.port},
    [ 'valueOf_' + getAccounts().join('_') ]: getAccounts()
};
console.log(accountService)
```
This is a great enhancement to good old object literals!

###### Arrow Functions in ES6
This is probably one feature I waited the most. I love CoffeeScript for its fat arrows. Now we have them in ES6. The fat arrows are amazing because they would make your this behave properly, i.e., this will have the same value as in the context of the function—it won’t mutate. The mutation typically happens each time you create a closure.

Using arrows functions in ES6 allows us to stop using that = this or self = this or _this = this or .bind(this). For example, this code in ES5 is ugly:

```
var _this = this
$('.btn').click(function(event){
  _this.sendData()
})
```
This is the ES6 code without _this = this:
```
$('.btn').click((event) =>{
  this.sendData()
})
```
Sadly, the ES6 committee decided that having skinny arrows is too much of a good thing for us and they left us with a verbose old function instead. (Skinny arrow in CoffeeScript works like regular function in ES5 and ES6).
Here’s another example in which we use call to pass the context to the logUpperCase() function in ES5:

```
var logUpperCase = function() {
  var _this = this

  this.string = this.string.toUpperCase()
  return function () {
    return console.log(_this.string)
  }
}
logUpperCase.call({ string: 'es6 rocks' })()
```
While in ES6, we don’t need to mess around with _this:
```
var logUpperCase = function() {
  this.string = this.string.toUpperCase()
  return () => console.log(this.string)
}
logUpperCase.call({ string: 'es6 rocks' })()
```
Note that you can mix and match old function with => in ES6 as you see fit. And when an arrow function is used with one line statement, it becomes an expression, i.e,. it will implicitly return the result of that single statement. If you have more than one line, then you’ll need to use return explicitly.
This ES5 code is creating an array from the messages array:

```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9']
var messages = ids.map(function (value) {
  // explicit return
  return "ID is " + value 
});
```
Will become this in ES6:
```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9']
// implicit return
var messages = ids.map(value => `ID is ${value}`)
```
Notice that I used the string templates? Another feature from CoffeeScript… I love them! The parenthesis () are optional for single params in an arrow function signature. You need them when you use more than one param.
In ES5 the code has function with explicit return:
```
var ids = ['5632953c4e345e145fdf2df8', '563295464e345e145fdf2df9'];
var messages = ids.map(function (value, index, list) {
  // explicit return
  return 'ID of ' + index + ' element is ' + value + ' '
});
```
And more eloquent version of the code in ES6 with parenthesis around params and implicit return:
```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9']
// implicit return
var messages = ids.map((value, index, list) => `ID of ${index} element is ${value} `) 
```

###### Promises in ES6
Promises have been a controversial topic. There were a lot of promise implementations with slightly different syntax. q, bluebird, deferred.js, vow, avow, jquery deferred to name just a few. Others said we don’t need promises and can just use async, generators, callbacks, etc. Gladly, there’s a standard Promise implementation in ES6 now!

Let’s consider a rather trivial example of a delayed asynchronous execution with setTimeout():
```
setTimeout(function(){
  console.log('Yay!')
}, 1000)
```

We can re-write the code in ES6 with Promise:
```
var wait1000 =  new Promise(function(resolve, reject) {
  setTimeout(resolve, 1000)
}).then(function() {
  console.log('Yay!')
})
```

Or with ES6 arrow functions:
```
var wait1000 =  new Promise((resolve, reject)=> {
  setTimeout(resolve, 1000)
}).then(()=> {
  console.log('Yay!')
})
```
So far, we’ve increased the number of lines of code from three to five without any obvious benefit. That’s right. The benefit will come if we have more nested logic inside of the setTimeout() callback:
```
setTimeout(function(){
  console.log('Yay!')
  setTimeout(function(){
    console.log('Wheeyee!')
  }, 1000)
}, 1000)
```
Can be re-written with ES6 promises:
```
var wait1000 =  ()=> new Promise((resolve, reject)=> {setTimeout(resolve, 1000)})
wait1000()
    .then(function() {
        console.log('Yay!')
        return wait1000()
    })
    .then(function() {
        console.log('Wheeyee!')
    });
```

Still not convinced that Promises are better than regular callbacks? Me neither. I think once you got the idea of callbacks and wrap your head around them, then there’s no need for additional complexity of promises.

Nevertheless, ES6 has Promises for those of you who adore them. Promises have a fail-and-catch-all callback as well which is a nice feature. Take a look at this post for more info on promises: Introduction to ES6 Promises.

###### Block-Scoped Constructs Let and Const
You might have already seen the weird sounding let in ES6 code. I remember the first time I was in London, I was confused by all those TO LET signs. The ES6 let has nothing to do with renting. This is not a sugarcoating feature. It’s more intricate. **let** is a new var which allows to scope the variable to the blocks. We define blocks by the curly braces. In ES5, the blocks did NOTHING to the vars:
```
function calculateTotalAmount (vip) {
  var amount = 0
  if (vip) {
    var amount = 1
  }
  { 
    // more crazy blocks!
    var amount = 100
    {
      var amount = 1000
      }
  }  
  return amount
}
console.log(calculateTotalAmount(true))
```
The result will be 1000. Wow! That’s a really bad bug. In ES6, we use let to restrict the scope to the blocks. Vars are function scoped.
```
function calculateTotalAmount (vip) {
  var amount = 0 // probably should also be let, but you can mix var and let
  if (vip) {
    let amount = 1 // first amount is still 0
  } 
  { // more crazy blocks!
    let amount = 100 // first amount is still 0
    {
      let amount = 1000 // first amount is still 0
      }
  }  
  return amount
}
console.log(calculateTotalAmount(true))
```
The value is 0, because the if block also has let. If it had nothing (amount=1), then the expression would have been 1.
When it comes to const, things are easier; it’s just an immutable, and it’s also block-scoped like let. Just to demonstrate, here are a bunch of constants and they all are okay because they belong to different blocks:
```
function calculateTotalAmount (vip) {
  const amount = 0  
  if (vip) {
    const amount = 1 
  } 
  { // more crazy blocks!
    const amount = 100 
    {
      const amount = 1000
      }
  }  
  return amount
}

console.log(calculateTotalAmount(true))
```
In my humble opinion, let and const overcomplicate the language. Without them we had only one behavior, now there are multiple scenarios to consider. ;-(

###### Classes in ES6
If you love object-oriented programming (OOP), then you’ll love this feature. It makes writing classes and inheriting from them as easy as liking a comment on Facebook.

Classes creation and usage in ES5 was a pain in the rear, because there wasn’t a keyword class (it was reserved but did nothing). In addition to that, lots of inheritance patterns like pseudo classical, classical, functional just added to the confusion, pouring gasoline on the fire of religious JavaScript wars.

I won’t show you how to write a class (yes, yes, there are classes, objects inherit from objects) in ES5, because there are many flavors. Let’s take a look at the ES6 example right away. I can tell you that the ES6 class will use prototypes, not the function factory approach. We have a class baseModel in which we can define a constructor and a getName() method:
```
class baseModel {
  constructor(options = {}, data = []) { // class constructor
        this.name = 'Base'
	   this.url = 'http://azat.co/api'
        this.data = data
	   this.options = options
    }

    getName() { // class method
        console.log(`Class name: ${this.name}`)
    }
}
```
Notice that I’m using default parameter values for options and data. Also, method names don’t need to have the word function or the colon (:) anymore. The other big difference is that you can’t assign properties this.NAME the same way as methods, i.e., you can’t say name at the same indentation level as a method. To set the value of a property, simply assign a value in the constructor.

The AccountModel inherits from baseModel with class NAME extends PARENT_NAME:
```
class AccountModel extends baseModel {
    constructor(options, data) {
       // To call the parent constructor, effortlessly invoke super() with params:
        super({private: true}, ['32113123123', '524214691']) 
    	   //call the parent method with super
        this.name = 'Account Model'
	   this.url +='/accounts/'
    }
```
If you want to be really fancy, you can set up a getter like this and accountsData will be a property:
```
    get accountsData() { //calculated attribute getter
    // ... make XHR
        return this.data
    }
}
```
So how do you actually use this abracadabra? It’s as easy as tricking a three-year old into thinking Santa Claus is real:
```
let accounts = new AccountModel(5)
accounts.getName()
console.log('Data is %s', accounts.accountsData)
```
In case you’re wondering, the output is:
```
Class name: Account Model
Data is %s 32113123123,524214691
```

###### Modules in ES6
As you might now, there were no native modules support in JavaScript before ES6. People came up with AMD, RequireJS, CommonJS and other workarounds. Now there are modules with import and export operands.
In ES5 you would use <script> tags with IIFE, or some library like AMD, while in ES6 you can expose your class with export. I am a Node.js guy, so I’ll use CommonJS which is also a Node.js syntax. It’s straightforward to use CommonJS on the browser with the Browserify bunder. Let’s say we have port variable and getAccounts method in ES5 module.js:

```
module.exports = {
  port: 3000,
  getAccounts: function() {
    ...
  }
}
```
In ES5 main.js, we would require('module') that dependency:
```
var service = require('module.js')
console.log(service.port) // 3000
```
In ES6, we would use export and import. For example, this is our library in the ES6 module.js file:
```
export var port = 3000
export function getAccounts(url) {
  ...
}
```
In the importer ES6 file main.js, we use import {name} from 'my-module' syntax. For example,
```
import {port, getAccounts} from 'module'
console.log(port) // 3000
```
Or we can import everything as a variable service in main.js:
```
import * as service from 'module'
console.log(service.port) // 3000
```
Personally, I find the ES6 modules confusing. Yes, they are more eloquent, but Node.js modules won’t change anytime soon. It’s better to have only one style for browser and server JavaScript, so I’ll stick with CommonJS/Node.js style for now. The support for ES6 modules in the browsers are not coming anytime soon (as of this writing), so you’ll need something like jspm to use ES6 modules.
No matter what, write modular JavaScript!

###### How to Use ES6 Today (Babel)
ES6 is finalized, but not fully supported by all browsers (e.g., ES6 Firefox support). To use ES6 today, get a compiler like Babel. You can run it as a standalone tool or use with your build system. There are Babel plugins for Grunt, Gulp and Webpack.

Here’s a Gulp example. Install the plugin:

```
$ npm install --save-dev gulp-babel
```

In gulpfile.js, define a task build that takes src/app.js and compiles it into the build folder:

```
var gulp = require('gulp'),
  babel = require('gulp-babel')

gulp.task('build', function () {
  return gulp.src('src/app.js')
    .pipe(babel())
    .pipe(gulp.dest('build'))
})
```