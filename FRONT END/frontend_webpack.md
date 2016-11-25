# https://www.devcasts.io/p/what-is-webpack-and-why-do-you-need-it/
# https://web-design-weekly.com/2014/09/24/diving-webpack/


# About
Webpack is a module bundler which takes modules with dependencies and generates static assets by bundling them together based on some configuration.
The support of loaders in Webpack makes it a perfect fit for using it along with React and we will discuss it later in this post with more details.
Webpack is a build tool that puts all of your assets, including Javascript, images, fonts, and CSS, in a dependency graph. Webpack lets you use require() in your source code to point to local files, like images, and decide how they're processed in your final Javascript bundle, like replacing the path with a URL pointing to a CDN.
If you're building a complex Front End™ application with many non-code static assets such as CSS, images, fonts, etc, then yes, Webpack will give you great benefits.
If your application is fairly small, and you don't have many static assets and you only need to build one Javascript file to serve to the client, then Webpack might be more overhead than you need.

In the early days, we "managed" Javascript dependencies by including files in a specific order:
```
<script src="jquery.min.js"></script>
<script src="jquery.some.plugin.js"></script>
<script src="main.js"></script>
```


# Installing webpack
```
npm i webpack -S
```

# Configuration
Webpack requires some configuration settings to carry out its work and the best practice is doing it via a config file called webpack.config.js.

```
touch webpack.config.js

var webpack = require('webpack');
var path = require('path');

var BUILD_DIR = path.resolve(__dirname, 'src/client/public');
var APP_DIR = path.resolve(__dirname, 'src/client/app');

var config = {
  entry: APP_DIR + '/index.jsx',
  output: {
    path: BUILD_DIR,
    filename: 'bundle.js'
  }
};

module.exports = config;
```

The minimalist requirement of a Webpack config file is the presence of entry and output properties.
The APP_DIR holds the directory path of the React application's codebase and the BUILD_DIR represents the directory path of the bundle file output.
As the name suggests, entry specifies the entry file using which the bundling process starts. If you are coming from C# or Java, it's similar to the class that contains main method. Webpack supports multiple entry points too. Here the index.jsx in the src/client/app directory is the starting point of the application
The output instructs Webpack what to do after the bundling process has been completed. Here, we are instructing it to use the src/client/public directory to output the bundled file with the name bundle.js
```
console.log('Hello World!');

./node_modules/.bin/webpack -d
```
The above command runs the webpack in the development mode and generates the bundle.js file and its associated map file bundle.js.map in the src/client/public directory.

To make it more interactive, create an index.html file in the src/client directory and modify it to use this bundle.js file

```
<html>
<head>
<meta charset="utf-8">
<title>React.js using NPM, Babel6 and Webpack</title>
</head>
<body>
<div id="app" />
<script src="public/bundle.js" type="text/javascript"></script>
</body>
</html>
```


# Requiring Javascript Files
We graduated to concatenating and minifying our scripts in a build step:
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

# Version.js
module.exports = { version: 1.0 };  

# App.js
var config = require('./Version.js');  
console.log('App Version:', config.version);  


# Comparing with grunt and gulp
Webpack puts your static assets (and source code) in a true dependency graph. Grunt and Gulp are only tools for working with files, and have no concept of a depdency graph.

# Comparing with browserify
Browserify is mainly a tool to transform require() calls that work in Node.js into calls that work in the browser. It's a dependency graph for your source code only. There's some plugins like Parcelify to manage some static assets, but you have go to out of your way to make it work.
Webpack's core idea of a dependency graph is what makes it so powerful and useful.


Traditional Front End™ programming relies mainly on global variables. CSS rules all exist in a global namespace. Applying CSS rules to elements relies on manually lining up the contents of global strings (selectors) correctly. A hard coded image path is a global, and you can't statically analyze your codebase to find outdated, moved, or deleted images.


html-loader is also present in babel
Running the webpack command every time when you change the file is not a productive workflow. We can easily change this behavior by using the following command