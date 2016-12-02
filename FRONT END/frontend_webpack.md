# About
# ============================================
Today’s websites are evolving into web apps:

    More and more JavaScript is being used.
    Modern browsers are offering a wider range of interfaces.
    Fewer full page reloads → even more code in a page.

As a result there is a lot of code on the client side!
A big code base needs to be organized. Module systems offer the option to split your code base into modules.

To understand Webpack, it can often be a good idea to talk about Grunt and Gulp first. The input to a Grunt task or a Gulp pipeline is filepaths (globs). The matching files can be run through different processes. Typically transpile, concat, minify, etc. This is a really great concept, but neither Grunt or Gulp understands the structure of your project. If we compare this to Webpack, you could say that Gulp and Grunt handle files, while Webpack handles projects.

With Webpack, you give a single path. The path to your entry point. This is typically index.js or main.js. Webpack will now investigate your application. It will figure out how everything is connected through require, import, etc. statements, url values in your CSS, href values in image tags, etc. It creates a complete dependency graph of all the assets your application needs to run. All of this just pointing to one single file.

An asset is a file. It being an image, css, less, json, js, jsx etc. And this file is a node in the dependency graph created by Webpack.

When Webpack investigates your app, it will hook on new nodes to the dependency graph. When a new node is found, it will check the file extension. If the extension matches your configuration, it will run a process on it. This process is called a loader. An example of this would be to transform the content of a .js file from ES6 to ES5. Babel is a project that does this and it has a Webpack loader. Install it with npm install babel-loader.

###### Server and Client Side javascript
Now with nodeJs there is javascript on the server side and client side. Both of these have javascript files with require present in it and dependencies that are to be built.
On server all the files are buit first (using something like gulp) that transpiles files in coffee-script, applies babel transformation if needed etc and then throws them into the build directory where we run it using 
npm server.js 
where server.js is barebones javascript (with all the require.js fully loaded)

Same is there on the client side as well.

###### Module system styles
There are multiple standards for how to define dependencies and export values:

    <script>-tag style (without a module system)
    CommonJS
    AMD and some dialects of it
    ES6 modules
    and more…

<script>-tag style

This is how you would handle a modularized code base if you didn’t use a module system.

```
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libraryA.js"></script>
<script src="module3.js"></script>
```

Modules export an interface to the global object, i. e. the window object. Modules can access the interface of dependencies over the global object.
Common problems

    Conflicts in the global object.
    Order of loading is important.
    Developers have to resolve dependencies of modules/libraries.
    In big projects the list can get really long and difficult to manage.

Once you understand what Webpack does that’s most likely the second question that will come to mind: what possible benefits could this approach have? “Images and CSS? In my JS? What the hell man?”. Well consider this: for a long while we’ve been taught and trained to concatenate all the things into one single file; to be very preserving about our HTTP requests, yada yada.

This has led to one big downside which is that most people nowadays bundle all their assets into one single app.js file that is then included in all the pages. Which means most of the time on any given page you’re loading a ton of assets that aren’t required. And if you aren’t doing that, then you’re most likely including assets by hand on certain pages but not on others, which leads to a big mess of dependency trees to maintain and keep track of: on which pages is this dependency needed already? Which pages do the stylesheets A and B affect?

Neither approaches are right, nor wrong. Consider Webpack a middleground– it’s not just a build system or a bundler, it’s a wicked smart module packing system. Once properly configured, it’ll know more about your stack then even you do, and it’ll know better than you how to best optimize it.

# Existing solutions
###### CommonJs: synchronous require
This style uses a synchronous require method to load a dependency and return an exported interface. A module can specify exports by adding properties to the exports object or setting the value of module.exports.

```
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```

It’s used server-side by node.js.
**Pros**
- Server-side modules can be reused.
- There are already many modules written in this style (npm).
- Very simple and easy to use.

**Cons**
- Blocking calls do not apply well on networks. Network requests are asynchronous.
- No parallel require of multiple modules

###### Implementations
    node.js - server-side
    browserify
    modules-webmake - compile to one bundle
    wreq - client-side

AMD: asynchronous require

Asynchronous Module Definition

Other module systems (for the browser) had problems with the synchronous require (CommonJS) and introduced an asynchronous version (and a way to define modules and exporting values):

require(["module", "../file"], function(module, file) { /* ... */ });
define("mymodule", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});

Pros

    Fits the asynchronous request style in networks.
    Parallel loading of multiple modules.

Cons

    Coding overhead. More difficult to read and write.
    Seems to be some kind of workaround.

Implementations

    require.js - client-side
    curl - client-side

Read more about CommonJS and AMD.
ES6 Modules

ECMAScript 2015 (6th Edition), adds some language constructs to JavaScript, which form another module system.

import "jquery";
export function doStuff() {}
module "localModule" {}

Pros

    Static analysis is easy.
    Future-proof as ES standard.

Cons

    Native browser support will take time.
    Very few modules in this style.



# Chunked Transferring
Modules should be executed on the client, so they must be transferred from the server to the browser.
There are two extremes when transferring modules:
    1 request per module
    All modules in one request

Both are used in the wild, but both are suboptimal:

    1 request per module
        Pro: only required modules are transferred
        Con: many requests means much overhead
        Con: slow application startup, because of request latency
    All modules in one request
        Pro: less request overhead, less latency
        Con: not (yet) required modules are transferred too

###### Chunked transferring
A more flexible transferring would be better. A compromise between the extremes is better in most cases.
→ While compiling all modules: Split the set of modules into multiple smaller batches (chunks).
This allows for multiple smaller, faster requests. The chunks with modules that are not required initially can be loaded on demand. This speeds up the initial load but still lets you grab more code when it will actually be used.
The “split points” are up to the developer.
→ A big code base is possible!
Note: The idea is from Google’s GWT.
Read more about Code Splitting.


# Why only JavaScript?
Why should a module system only help the developer with JavaScript? There are many other resources that need to be handled:

    stylesheets
    images
    webfonts
    html for templating
    etc.

Or translated/processed:

    coffeescript → javascript
    elm → javascript
    less stylesheets → css stylesheets
    jade templates → javascript which generates html
    i18n files → something
    etc.

This should be as easy as:

require("./style.css");

require("./style.less");
require("./template.jade");
require("./image.png");


# Indepth
Webpack is a module bundler which takes modules with dependencies and generates static assets by bundling them together based on some configuration.
The support of loaders in Webpack makes it a perfect fit for using it along with React and we will discuss it later in this post with more details.
Webpack is a build tool that puts all of your assets, including Javascript, images, fonts, and CSS, in a dependency graph. Webpack lets you use require() in your source code to point to local files, like images, and decide how they're processed in your final Javascript bundle, like replacing the path with a URL pointing to a CDN.
If you're building a complex Front End™ application with many non-code static assets such as CSS, images, fonts, etc, then yes, Webpack will give you great benefits.
If your application is fairly small, and you don't have many static assets and you only need to build one Javascript file to serve to the client, then Webpack might be more overhead than you need.

Use webpack to bundle the backend and frontend parts of the application
Example of the code splitting feature offered by webpack

Process
    Split the dependency tree into chunks loaded on demand
    Keep initial loading time low
    Every static asset should be able to be a module
    Ability to integrate 3rd-party libraries as modules
    Ability to customize nearly every part of the module bundler
    Suited for big projects

In the early days, we "managed" Javascript dependencies by including files in a specific order:
```
<script src="jquery.min.js"></script>
<script src="jquery.some.plugin.js"></script>
<script src="main.js"></script>
```
# All assets are basically treated as modules
What this means more precisely is that instead of building all your Sass files, and optimizing all your images, and including them on one side, then bundling all your modules, and including them on your page on another, you have this:

```
import stylesheet from 'styles/my-styles.scss';
import logo from 'img/my-logo.svg';
import someTemplate from 'html/some-template.html';

console.log(stylesheet); // "body{font-size:12px}"
console.log(logo); // "data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5[...]"
console.log(someTemplate) // "<html><body><h1>Hello</h1></body></html>"
```
All your assets are considered modules themselves, that can be imported, modified, manipulated, and that ultimately can be packed into your final bundle.

# Requiring Javascript Files
We graduated to concatenating and minifying our scripts in a build step:
**Old Method of Combining (like we did in rails)**
```
// build-script.js
var scripts = [  
    'jquery.min.js',
    'jquery.some.plugin.js',
    'main.js'
].concat().uglify().writeTo('bundle.js');

// Everything our app needs!
<script src="bundle.js"></script>
```
This still relied on the order of concatenated files. Even worse, the code could only communicate through global variables! The first script declared a global jQuery variable, then jquery.some.plugin.js created a new global, or modified the global jQuery object. Yuck.
Now we use CommonJS or ES6 modules to put our Javascript in a true dependency graph.
The browser doesn't support require(), so we use a build tool to transform the above files into a "bundled" file that the browser can execute properly.

Server Side Javascript

Client Side Javascript

# Features
# ============================================
**ES6 module support**
Webpack supports ES6 modules and their import and export methods without having to compile them to 
**Supports source maps for easier debugging**
Source maps allow for easier debugging, because they allow you to find the problems within the origin files 
**Built-in hot reloading webserver**
**Supports both CommonJS (npm packages) and AMD**
Webpack supports AMD and CommonJS module styles. It performs clever static analysis on the AST of your code.
**Hot module replacement drastically increases development productivity**
Can create a single bundle or multiple chunks loaded on demand, to reduce initial loading time.
Webpack allows you to split your codebase into **multiple chunks**. Chunks are loaded on demand. This reduces the  initial loading time.
**Rich and flexible plugin infrastructure**
Plugins and loaders are easy to write and allow you to control each step of the build, from loading and compiling 
**Tap into npm's huge module ecosystem**
Using Webpack opens you up to npm, that has over 80k modules of which a great amount work both client-side 
**Mix ES6 AMD and CommonJS**
Webpack supports using all three module types, even in the same file. 
Webpack allows you to require your CSS and images at the top of your JS files. This makes your dependencies clear and reduces HTTP requests by inlining.

# Installing webpack
# ============================================
```
npm i webpack -S
```

# Working
# ============================================
###### Basic Working
Let’s create some modules in JavaScript, using the CommonJs syntax:
**cats.js**
```
var cats = ['dave', 'henry', 'martha'];
module.exports = cats;
```
**app.js (Entry Point)**
```
cats = require('./cats.js');
console.log(cats);
```
Give webpack the entry point (app.js) and specify an output file (app.bundle.js):
```
webpack ./app.js app.bundle.js
```
Webpack will read and analyze the entry point and its dependencies (including transitive dependencies). Then it will bundle them all into app.bundle.js.
**Run the javascript**
```
node app.bundle.js
["dave", "henry", "martha"]
```

###### Configuration
Webpack is a very flexible module bundler. It offers a lot of advanced features, but not all features are exposed via the command-line interface. To gain full access to webpack’s flexibility, we need to create a “configuration file”.
Webpack takes an entry module, reads the entire dependency tree, and bundles it all together as a single file (assuming a simple configuration). We are going to do this for the backend as well. Let's start with this simple config, which tells it to take the entry point src/index.js and generate a file at build/bundle.js.

**Source Code (/src) --> Webpack with Config ---> Bundled Destination (/dist)**

**Create Config File**
```
touch webpack.config.js
```
**webpack.config.js**
```
var webpack = require('webpack');
var path = require('path');

var BUILD_DIR = path.resolve(__dirname, 'src/client/public');
var APP_DIR = path.resolve(__dirname, 'src/client/app');

var config = {
  // Here, entry tells Webpack which files are the entry points of your application. 
  entry: APP_DIR + '/index.jsx',
  output: {
    path: BUILD_DIR,
    filename: 'bundle.js'
  }
};

module.exports = config;
```
**APP_DIR:** holds the directory path of the React application's codebase
**BUILD_DIR:** represents the directory path of the bundle file output.
Instead of the above we can directly use the global ___dir.

**entry** — name of the top level file or set of files that we want to include in our build, can be a single file or an array of files. In our build, we only pass in our main file (app.js).
If you need multiple bundles for multiple HTML pages you can use the “multiple entry points” feature. It will build multiple bundles at once. Additional chunks can be shared between these entry chunks and modules are only built once.
Entry tells the Webpack where the root module or the starting point is. This can be a String, an Array or an Object. This could confuse you but the different types are used for different purposes. If you have a single starting point (like most apps), you can use any format and the result will be the same.
But, if you want to append multiple files that are NOT dependent on each other, you can use the Array format.
```
// webpack.config.js
module.exports = {
  entry: [ './profile.js', './feed.js' ],
  output: {
    path: 'build',
    filename: '[name].js' // Template based on keys in entry above
  }
};
```

Now, let’s say you have true multi-page application, not a SPA w/ multi-views, but with multiple HTML files (index.html and profile.html). You can then tell Webpack to generate multiple bundles at once by using entry object.
```
// webpack.config.js
module.exports = {
  entry: {
    Profile: './profile.js',
    Feed: './feed.js'
  },
  output: {
    path: 'build',
    filename: '[name].js' // Template based on keys in entry above
  }
};
```

You can also use the Array type entries inside an entry object. For example the below config will generate 3 files: vendor.js that contains three vendor files, an index.js and a profile.js.
```
// webpack.config.js
module.exports = {
  entry: { 
	vendor: ["jquery", "some"],
	Index: './profile.js',
    	Profile: './feed.js'

},
  output: {
    path: 'build',
    filename: '[name].js' // Template based on keys in entry above
  }
};
```


**output** — an object containing your output configuration. In our build, we only specify the filename key (bundle.js) for the name of the file we want Webpack to build.
**publicPath**: 
```
// Development: Both Server and the image are on localhost
.image { 
  background-image: url(‘./test.png’);
 }

// Production: Server is on Heroku but the image is on a CDN
.image { 
  background-image: url(‘https://someCDN/test.png’);
 }
```

How to specify a directory and pack all the files present in it??????
name-bundle.js



# Code Splitting
# ============================================
Webpack has two types of dependencies in its dependency tree: sync and async. Async dependencies act as split points and form a new chunk. After the chunk tree is optimized, a file is emitted for each chunk.

# Loaders
# ============================================
Webpack can only process JavaScript natively, but loaders are used to transform other resources into JavaScript. By doing so, every resource forms a module. 
* Loaders can be chained. They are applied in a pipeline to the resource. The final loader is expected to return JavaScript; each other loader can return source in arbitrary format, which is passed to the next loader.
* Loaders can be synchronous or asynchronous.
* Loaders run in Node.js and can do everything that’s possible there.
* Loaders accept query parameters. This can be used to pass configuration to the loader.
* Loaders can be bound to extensions / RegExps in the configuration.
* Loaders can be published / installed through npm.
* Normal modules can export a loader in addition to the normal main via package.json loader.
* Loaders can access the configuration.
* Plugins can give loaders more features.
* Loaders can emit additional arbitrary files.


**Options**
test: A condition that must be met
exclude: A condition that must not be met
include: An array of paths or files where the imported files will be transformed by the loader
loader: A string of “!” separated loaders
loaders: An array of loaders as string


“When you encounter this kind of file, do this with it”.
**List of Loaders** - http://webpack.github.io/docs/list-of-loaders.html
Webpack only supports JavaScript modules natively, but most people will be using a transpiler for ES2015, CoffeeScript, TypeScript, etc. They can be used in webpack by using loaders. Loaders are special modules webpack uses to ‘load’ other modules (written in another language) into JavaScript (that webpack understands). For example, 
**babel-loader** uses Babel to load ES2015 files. (js(es2015) -> js(strict))
**json-loader** loads JSON files (simply by prepending module.exports = to turn it into a CommonJs module).	
**html-loader** - Exports HTML as string. HTML is minimized when the compiler demands.
**sass-loader** - Use in tandem with the style-loader and css-loader to add the css rules to your document.
**style-loader** - 
**css-loader**
file-loader
url-loader (https://github.com/webpack/url-loader)

Loaders can be chained together. For example yaml-loader converts yaml to json.
**yml -- (yaml-loader) --> json --(json-loader)--> webpack compatible**

```
{
  // When you import a .ts file, parse it with Typescript
  test: /\.ts/,
  loader: 'typescript',
},
{
  // When you encounter images, compress them with image-webpack (wrapper around imagemin)
  // and then inline them as data64 URLs
  test: /\.(png|jpg|svg)/,
  loaders: ['url', 'image-webpack'],
},
{
  // When you encounter SCSS files, parse them with node-sass, then pass autoprefixer on them
  // then return the results as a string of CSS
  test: /\.scss/,
  loaders: ['css', 'autoprefixer', 'sass'],
}
```

###### Transpiling ES205 using Babel Loader
Install babel and the presets
```
 npm install --save-dev babel-core babel-preset-es2015
```
Install **babel-loader**
```
 npm install --save-dev babel-loader
```
Configure Babel to use these presets by adding .babelrc
```
 { "presets": [ "es2015" ] }
```
Modify webpack.config.js to process all .js files using **babel-loader** loader.
```
 module.exports = {
     entry: './src/app.js',
     output: {
         path: './bin',
         filename: 'app.bundle.js',
     },
     module: {
         loaders: [{
             test: /\.js$/,
             exclude: /node_modules/,
             loader: 'babel-loader'
         }]
     }
 }
```
**Install the libraries that are to be used**
Install the libraries you want to use (in this example, jQuery). We are using --save instead of --save-dev this time, as these libraries will be used in runtime. We also use babel-polyfill so that ES2015 APIs are available in older browsers.
```
 npm install --save jquery babel-polyfill
```
**src/app.js**
```
 import 'babel-polyfill';
 import cats from './cats';
 import $ from 'jquery';

 $('<h1>Cats</h1>').appendTo('body');
 const ul = $('<ul></ul>').appendTo('body');
 for (const cat of cats) {
     $('<li></li>').text(cat).appendTo(ul);
 }
```


# Plugins
# ============================================
Plugins are additional node modules that usually work on the resulting bundle.
Webpack features a rich plugin system. Most internal features are based on this plugin system. This allows you to customize webpack for your needs and distribute common plugins as open source.
Plugins work at bundle or chunk level and usually work at the end of the bundle generation process. 

```
const webpack = require('webpack');

module.exports = {
    entry: './src/app.js',
    output: {
        path: './bin',
        filename: 'app.bundle.js',
    },
    module: {
        loaders: [{
            test: /\.js?$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
        }]
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false,
            },
            output: {
                comments: false,
            },
        }),
    ]
}
```
**uglifyJSPlugin** takes the bundle.js and minimizes and obfuscates the contents to decrease the file size.
**extract-text-webpack-plugin** internally uses css-loader and style-loader to gather all the CSS into one place and finally extracts the result into a separate external styles.css file and includes the link to style.css into index.html
**DefinePlugin** allows us to define the NODE_ENV variable as a global variable in the bundling process as if it was defined in one of the scripts. Some modules (e.g. React) relies on it to enable or disable specific features for the current environment (production or development).
**DedupePlugin** removes all the duplicated files (modules imported in more than one module).
**OccurenceOrderPlugin** helps in reducing the file size of the resulting bundle.
**UglifyJsPlugin** minifies and obfuscates the resulting bundle using UglifyJs.


# Babel-Rc
babel-loader uses “presets” configuration to know how to convert ES6 to ES5 and also how to parse React’s JSX to JS. We can pass the configuration via “query” parameter like below:

```
module: {
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /(node_modules|bower_components)/,
      loader: 'babel',
      query: {
        presets: ['react', 'es2015']
      }
    }
  ]
}
```

However in many projects babel’s configuration can become very large. So instead you can keep those them in babel-loader’s configuration file called .babelrc file. babel-loader will automatically load the .babelrc file if it exists.

```
//webpack.config.js 
module: {
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /(node_modules|bower_components)/,
      loader: 'babel'
    }
  ]
}


//.bablerc
{
 “presets”: [“react”, “es2015”]
}
```

# Running with Server
# ============================================
###### Method 1 - Running using CLI and API
The above command runs the webpack in the development mode and generates the bundle.js file and its associated map file bundle.js.map in the src/client/public directory.
```
console.log('Hello World!');

// Run in the development environment
./node_modules/.bin/webpack -d

// Run in the production envionment
./node_modules/.bin/webpack -p
```
You can run webpack by simply using webpack command to bundle at appropriate time. This can be done by watching over files using gulp/grunt and then running the command post that using the API.
Following is the **API.**
This file should export the configuration object:
```
module.exports = {
    // configuration
};
```
devTool: enhance debugging by adding meta info for the browser devtools. source-map most detailed at the expense of build speed.
externals: Don't follow/bundle these modules, but request them at runtime from the environment

And the following is the API endpoint.
```
webpack({
    // configuration
}, callback);
```
Following is the **CLI**. If you use the CLI it will read a file webpack.config.js (or the file passed by the --config option). Use this to run:
```
./nod_modules/.bin/webpack
```
The NODE_ENV environment variable and the -p option are used to generate the bundle in production mode, which will apply a number of additional optimizations, for example removing all the debug code from the React library.
```
NODE_ENV=production node_modules/.bin/webpack -p
```
You can also run webpack --watch to make it automatically watch for changes to your files and recompile as needed.
```
./node_modules/.bin/webpack --watch
```

```
webpack <entry> <output>
webpack ./app.js bundle.js
```

**Custom config file**
```
webpack --config example.config.js
```

Specifies a different configuration file to pick up. Use this if you want to specify something different than webpack.config.js, which is the default.


**various options available:**
https://github.com/webpack/docs/wiki/cli
**devtool** - Choose a developer tool to enhance debugging.
**debug** - Switch loaders to debug mode.
**bail** - Report the first error as a hard error instead of tolerating it
**cache** - Cache generated modules and chunks to improve performance for multiple incremental builds. This is enabled by default in watch mode. You can pass false to disable it.
**devServer** - Can be used to configure the behaviour of webpack-dev-server when the webpack config is passed to webpack-dev-server CLI.
**plugins** - Add additional plugins to the compiler
--progress - Display a compilation progress to stderr.
--json - Write JSON to stdout instead of a human readable format.

**Adding it to the index.html file**
```
<!DOCTYPE html>
 <html>
     <head>
         <meta charset="utf-8">
     </head>
     <body>
         <script src="bin/app.bundle.js" charset="utf-8"></script>
     </body>
 </html>
```

###### Webpack Watch mode
With watchmode, Webpack will watch your files and when one of them changes, it will immediately rerun the build and recreate your output file.
Use it via the command line
```
webpack --watch
```

or add it to the config
```
module.exports = {
  entry: "./app.js",
  output: {
    filename: "bundle.js"
  }, 
  watch: true
}
```

**How to enable live reloading of assets?**
webpack cli (webpack-cli) or webpack api (webpack) at barebones is for running just on the files (converting just one file into output). 

In development environment, generally we make changes to our files and want to recompile the assets as the file changes. In the browser, when we now refresh, ew can see the new assets. This is called **live reloading**.
There is another advanced thing that can be done, called hot reloading. Whenever we change assets, along with the files being changed the browser is also updated without any reloading of page. This is called **hot reloading**.


**Remember**, these are just used for development purposes, as in production all the assets will be precompiled and then served. We don't want any other functionality there. We just want to bundle our assets using the webpack command and then serve them using our normal express server.

If you want to implement hot reloading, you need to use the webpack-hot-middleware.

If you are to use with the server then you need to use the webpack-dev-server package. 

Web pack provides an express **middleware** that you can plug into your app to serve up your fronted assets via webpack-dev-server rather than express.static or express/serve-static.  This is a seperate server of webpack that runs just for the purpose of asset serving.

1. If you are using a task runner like grunt or gulp you'll want to use the barebones **webpack-dev-server** API (without the middleware). You can run the server from one of your gulp/grunt tasks, which is solely meant for asset serving.
2. If you use express to host your site: use **webpack-dev-middleware and webpack-hot-middleware**. It will automatically configure all the configuration and integrate seamlessly with your express server. Internally it uses the webpack-dev-server APIs.

###### Method 2 - webpack-dev-middleware API (single server)
The dev server doesnt build files. It just serves.
There is no CLI, but only API.
The webpack-dev-middleware is a small middleware for a connect-based middleware stack. It uses webpack to compile assets in-memory and serve them. When a compilation is running every request to the served webpack assets is blocked until we have a stable bundle.
It is kind of a webpack function that we can use anywhere (and does not rely on webpack.config.js). The api and process is executed there only. We are not running any webpack commands, just that file only.

**noInfo** - Display no info to console (only warnings and errors) (Default: false)
**quiet** - Display nothing to the console (Default: false)
**lazy** - Switch into lazy mode. (Default: false)
**filename** - In lazy mode: Switch request should trigger the compilation.
In most cases this equals the webpack configuration option output.filename.
**watchOptions.aggregateTimeout** - Delay the rebuilt after the first change. Value is a time in ms. (Default: 300)
**watchOptions.poll** (true: use polling)
**number**: use polling with specified interval (Default: undefined)
**publicPath (required)** - The path where to bind the middleware to the server. In most cases this equals the webpack configuration option output.publicPath.
headers. Add custom headers. i. e. { "X-Custom-Header": "yes" }
**stats** - Output options for the stats. See node.js API.
**middleware.invalidate()** - Manually invalidate the compilation. Useful if stuff of the compiler has changed.
**middleware.fileSystem** - A readable (in-memory) filesystem that can access the compiled data.

Now that you have webpack working (config etc. setted up) how do you integrate with actually serving files.
For this we have middlewares that we put. There are couple of middlewares.
Adding the Middleware to the **server.js** file
```
import path from 'path';  
import express from 'express';  
import webpack from 'webpack';  
import webpackMiddleware from 'webpack-dev-middleware';  
import config from './webpack.config.js';

const app = express();  

// Read from config file
const compiler = webpack(config);

// or specify explicitly
const compiler = webpack({
    // configuration
    output: { path: '/' }
});

// Specify the middleware API Settings
app.use(webpackDevMiddleware(compiler, {
    // options
}));

app.use(express.static(__dirname + '/dist'));  
app.get('*', function response(req, res) {  
  res.sendFile(path.join(__dirname, 'dist/index.html'));
});

app.listen(3000);
```
**In production**
In production you dont want these features. Simple, have a conditional statement checking your environment variable, if you’re in a dev environment use the webpack-dev-middleware, if not, use express.static to serve your static assets. 

###### Method 3 - webpack-dev-server CLI and API (dual servers)
Only meant for development purpose. 
“The dev server uses **Webpack’s watch mode**. It also prevents webpack from emitting the resulting files to disk. Instead it keeps and serves the resulting files from memory.” — This means that you will not see the webpack-dev-server build in bundle.js, to see and run the build, you must still run the webpack command.

Please note that webpack-dev-server runs in memory by design. If you want a real bundle, build through webpack. The files are not written in directory.

With Webpack dev server running, you will notice that if you go back to your app and make a change, the browser will automatically refresh (hot-loading).
There are 2 modes: 
- **Watch Modes** - compiler compiles assets as a file is changed.
- **Lazy Mode** - compiler compiles assets on every request end point.

webpack-dev-server is a wrapper over webpack-dev-middleware and contains some additional server layer on top of the barebones middleware.
The webpack-dev-server is a little Node.js Express server, which uses the webpack-dev-middleware to serve a webpack bundle. It also has a little runtime which is connected to the server via Sock.js. 
The server emits information about the compilation state to the client, which reacts to those events. You can choose between different modes, depending on your needs. 
The development server is even better.

The easiest way to use it is with the CLI. In the directory where your webpack.config.js is, run:

node_modules/.bin/webpack-dev-server

npm install webpack-dev-server -g

webpack-dev-server --progress --colors
webpack-dev-server ./webpack.dev.config.js

OPTION 1:

//Install it globally
npm install webpack-dev-server --save

//Use it at the terminal
$ webpack-dev-server --inline --hot

OPTION 2:

// Add it to package.json's script 

“scripts”: {
 “start”: “webpack-dev-server --inline --hot”,
 ...
 }

// Use it by running 
$ npm start

Open browser at:
http://localhost:8080

This binds a small express server on localhost:8080 which serves your static assets as well as the bundle (compiled automatically). It automatically updates the browser page when a bundle is recompiled (SockJS). Open http://localhost:8080/webpack-dev-server/bundle in your browser. The dev server uses webpack’s watch mode. It also prevents webpack from emitting the resulting files to disk. Instead it keeps and serves the resulting files from memory.

**Installing**
```
npm install webpack-dev-server
```

Taking into account the following settings:
```
var path = require("path");
module.exports = {
  entry: {
    app: ["./app/main.js"]
  },
  output: {
    path: path.resolve(__dirname, "build"),
    publicPath: "/assets/",
    filename: "bundle.js"
  }
};
```
You have an app folder with your initial entry point that webpack will bundle into a bundle.js file in the build folder. As we have specified the /assets as the publicPath the assets will be served at this endpoint.

**Running the server using CLI**
This reads the configuration from the webpack.config.js file and starts the server.
```
./node_modules/.bin/webpack-dev-server
```
Options for the server:
    --content-base <file/directory/url/port>: base path for the content.
    --quiet: don’t output anything to the console.
    --no-info: suppress boring information.
    --colors: add some colors to the output.
    --no-colors: don’t use colors in the output.
    --compress: use gzip compression.
    --host <hostname/ip>: hostname or IP. 0.0.0.0 binds to all hosts.
    --port <number>: port.
    --inline: embed the webpack-dev-server runtime into the bundle.
    --hot: adds the HotModuleReplacementPlugin and switch the server to hot mode. Note: make sure you don’t add HotModuleReplacementPlugin twice.
    --hot --inline also adds the webpack/hot/dev-server entry.
    --public: overrides the host and port used in --inline mode for the client (useful for a VM or Docker).
    --lazy: no watching, compiles on request (cannot be combined with --hot).
    --https: serves webpack-dev-server over HTTPS Protocol. Includes a self-signed certificate that is used when serving the requests.
    --cert, --cacert, --key: Paths the certificate files.
    --open: opens the url in default browser (for webpack-dev-server versions > 2.0).
    --history-api-fallback: enables support for history API fallback.
    --client-log-level: controls the console log messages shown in the browser. Use error, warning, info or none.

**Running server using API (using a file)**
Instead of using a command like we executed earlier, we can just run the following server.js file as "node server.js" and this will do the stuff for us.

**server.js**
```
var WebpackDevServer = require("webpack-dev-server");
var webpack = require("webpack");

var compiler = webpack({
  // configuration
});
var server = new WebpackDevServer(compiler, {
  // webpack-dev-server options

  contentBase: "/path/to/directory",
  // Can also be an array, or: contentBase: "http://localhost/",

  hot: true,
  // Enable special support for Hot Module Replacement
  // Page is no longer updated, but a "webpackHotUpdate" message is sent to the content
  // Use "webpack/hot/dev-server" as additional module in your entry point
  // Note: this does _not_ add the `HotModuleReplacementPlugin` like the CLI option does. 

  historyApiFallback: false,
  // Set this as true if you want to access dev server from arbitrary url.
  // This is handy if you are using a html5 router.

  compress: true,
  // Set this if you want to enable gzip compression for assets

  proxy: {
    "**": "http://localhost:9090"
  },
  // Set this if you want webpack-dev-server to delegate a single path to an arbitrary server.
  // Use "**" to proxy all paths to the specified server.
  // This is useful if you want to get rid of 'http://localhost:8080/' in script[src],
  // and has many other use cases (see https://github.com/webpack/webpack-dev-server/pull/127 ).

  setup: function(app) {
    // Here you can access the Express app object and add your own custom middleware to it.
    // For example, to define custom handlers for some paths:
    // app.get('/some/path', function(req, res) {
    //   res.json({ custom: 'response' });
    // });
  },

  // pass [static options](http://expressjs.com/en/4x/api.html#express.static) to inner express server
  staticOptions: {
  },

  clientLogLevel: "info",
  // Control the console log messages shown in the browser when using inline mode. Can be `error`, `warning`, `info` or `none`.

  // webpack-dev-middleware options
  quiet: false,
  noInfo: false,
  lazy: true,
  filename: "bundle.js",
  watchOptions: {
    aggregateTimeout: 300,
    poll: 1000
  },
  // It's a required option.
  publicPath: "/assets/",
  headers: { "X-Custom-Header": "yes" },
  stats: { colors: true }
});
server.listen(8080, "localhost", function() {});
// server.close();
```

**Summary**:
You should not use the webpack-dev-server as the backend server.
You can run two servers side-by-side: The webpack-dev-server and your backend server. In this case you need to teach the webpack-generated assets to make requests to the webpack-dev-server even when running on a HTML-page sent by the backend server. On the other side you need to teach your backend server to generate HTML pages that include script tags that point to assets on the webpack-dev-server. In addition to that you need a connection between the webpack-dev-server and the webpack-dev-server runtime to trigger reloads on recompilation.
* Webpack-dev-server on port 9090
* Backend server on port 8080. This is a separate server.
* generate HTML pages with <script src="/assets/bundle.js">, no need for hardcoding hostnames.
* webpack-dev-server publicPath: '/assets/'
* webpack-dev-server contentBase: {target: 'http://localhost:8080'} // an object with key target
* open http://localhost:9090 on the machine that starts the server, or visit http://<server-ip>:9090 on other machines like mobile phones.


###### webpack-hot-middleware
http://andrewhfarmer.com/webpack-hmr-tutorial/
Hot loading code is a great concept. It makes your workflow a lot smoother. Normally you have to refresh the application and sometimes click your way back to the same state. We spend a lot of time on this, and we should not do that. As I mentioned, Webpack can do some pretty amazing things with its loaders. 

Its is different than Live Reload. **Live reload** is when the browser automatically refreshes the page whenever you make a code change. HMR is faster because it updates code in-place without refreshing.

Hot loading styles is the first we will look at, but before that we have to make our Webpack workflow allow hot loading:
```
npm install webpack-hot-middleware
```
Add to **server.js**
```
import path from 'path';  
import express from 'express';  
import webpack from 'webpack';  
import webpackMiddleware from 'webpack-dev-middleware';  
import webpackHotMiddleware from 'webpack-hot-middleware'; // This line  
import config from './webpack.config.js';

const app = express();  
const compiler = webpack(config);

app.use(express.static(__dirname + '/dist'));  
app.use(webpackMiddleware(compiler);  
app.use(webpackHotMiddleware(compiler)); // And this line  
app.get('*', function response(req, res) {  
  res.sendFile(path.join(__dirname, 'dist/index.html'));
});

app.listen(3000);  
```

# Handling Stylesheets
# ============================================
###### Basic Setup (Inline css Stylesheets)
In the basic setup you can just embed css directly into the JS and dont need to make a separate bundled file. The styles would get rendered easily.
**app/main.css**
```
body {
  background: cornsilk;
}
```

Require it so that webpack can find it.
**app/index.js**
```
require('./main.css');
```

**css**
```
loaders: [
        {
          test: /\.css$/,
          loaders: ['style', 'css'],
          include: paths
        }
]
```
In this case, css-loader gets evaluated first, then style-loader. css-loader will resolve @import and url statements in our CSS files. style-loader deals with require statements in our JavaScript. A similar approach works with CSS preprocessors, like Sass and Less, and their loaders.

###### Basic Setup (Inline scss Stylesheets)
For support of scss add the scss loader and it will start to work
```
{
	test: /\.scss$/,
	loaders: [ 'style', 'css', 'sass' ]
}
```

###### Separate CSS Bundle/Files
Even though we have a nice build set up now, where did all the CSS go? As per our configuration, it has been inlined to JavaScript! Even though this can be convenient during development, it doesn't sound ideal. The current solution doesn't allow us to cache CSS. In some cases we might suffer from a Flash of Unstyled Content (FOUC).
Webpack provides a means to generate a separate CSS bundle. We can achieve this using the ExtractTextPlugin. It comes with overhead during the compilation phase, and it won't work with Hot Module Replacement (HMR) by design. Given we are using it only for production, that won't be a problem.

###### Modular and Contained CSS
Let’s now write a small Button component, it’ll have some SCSS styles, an HTML template, and some behavior. So we’ll install the things we need. We’ll need loaders for Sass and HTML files. Also, as results are piped from one loader to another, we’ll need a CSS loader to handle the results of the Sass loader. Now, once we have our CSS, there are multiple ways to handle them, for now we’ll use a loader called the style-loader which takes a piece of CSS and injects it into the page dynamically.
A short description of CSS Modules is that each CSS file you create has a local scope. Just like a JavaScript module has its local scope. The way it works is:

**App.css**
```
.header {
  color: red;
}
```
**App.js**
```
import styles from './App.css';

export default function (props) {

  return <h1 className={styles.header}>Hello world!</h1>;

};
```
You also have to update the config:
```
import path from 'path';
const config = {  
  ...
  module: {
    loaders: [{
      test: /.js?$/,
      exclude: /node_modules/,
      loader: 'babel'
    }, {
      test: /.css?$/,
      loader: 'style!css?modules&localIdentName=[name]---[local]---[hash:base64:5]'
    }]
  }
};
```

So you only use classes and those classes can be referenced by name when you import the css file. The thing here now is that this .header class is not global. It will only work on JavaScript modules importing the file. This is fantastic news because now you get the power of CSS. :hover, [disabled], media queries, etc. but you reference the rules with JavaScript.

###### CSS loaders with Hot Replacement
First we add a new loader to our project. This makes Webpack understand what CSS is. Specifically it will understand what a url means. It will treat this as any other require, import, etc. statement. But we do not just want to understand CSS, we also want to add it to our page. With npm install style-loader we can add behavior to our CSS loading.
```
import path from 'path';

const config = {

  devtool: 'eval-source-map',

  // We add an entry to connect to the hot loading middleware from
  // the page
  entry: [
    'webpack-hot-middleware/client',
    path.join(__dirname, 'app/main.js')
  ],
  output: {
    path: path.join(__dirname, '/dist/'),
    filename: '[name].js',
    publicPath: '/'
  },

  // This plugin activates hot loading
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
  ],
  module: {
    loaders: [{
      test: /\.js?$/,
      exclude: /node_modules/,
      loader: 'babel'
    }, {
      test: /\.css?$/,
      loader: 'style!css' // This are the loaders
    }]
  }
};
```

In our config we tell Webpack to first run the css-loader and then the style-loader, it reads right to left. The css-loader makes any urls within it part of our dependency graph and the style-loader puts a style tag for the CSS in our HTML.
So now you see that we do not only process files with Webpack, we can create side effects like creating style tags. With the HOT middleware, we can even run these side effects as we change the code of the app. That means every time you change some CSS Webpack will just update the existing style tag on the page, without a refresh.


# Long Term Caching


# Comparison
# ============================================
###### Grunt and Gulp
Webpack puts your static assets (and source code) in a true dependency graph. Grunt and Gulp are only tools for working with files, and have no concept of a depdency graph.
Whereas grunt and gulp are task runners.

###### Comparing with browserify
Browserify is mainly a tool to transform require() calls that work in Node.js into calls that work in the browser. It's a dependency graph for your source code only. There's some plugins like Parcelify to manage some static assets, but you have go to out of your way to make it work.
Webpack's core idea of a dependency graph is what makes it so powerful and useful.
Traditional Front End programming relies mainly on global variables. CSS rules all exist in a global namespace. Applying CSS rules to elements relies on manually lining up the contents of global strings (selectors) correctly. A hard coded image path is a global, and you can't statically analyze your codebase to find outdated, moved, or deleted images.

# Dev Tools
# ============================================
http://webpack.github.io/docs/webpack-dev-server.html
http://webpack.github.io/docs/webpack-dev-middleware.html

# Resources
# ============================================
https://www.devcasts.io/p/what-is-webpack-and-why-do-you-need-it/
https://web-design-weekly.com/2014/09/24/diving-webpack/
https://blog.madewithlove.be/post/webpack-your-bags/
http://jlongster.com/Backend-Apps-with-Webpack--Part-I
http://dontkry.com/posts/code/single-page-modules-with-webpack.html
https://medium.com/@okonetchnikov/long-term-caching-of-static-assets-with-webpack-1ecb139adb95#.fq5yzka8z
http://blog.xebia.fr/2016/03/08/lazy-loading-avec-webpack-angularjs/
https://blog.risingstack.com/using-react-with-webpack-tutorial/
http://dontkry.com/posts/code/single-page-modules-with-webpack.html