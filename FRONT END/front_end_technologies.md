NPM: Node JS package manager, helps you to manage all the libraries your software relays on. You would define your needs in a file called package.json and run npm install in the command line... BANG your packages are downloaded and are ready to use. Could be used both for front-end and back-end.

Bower: Front-end package manager. Not that much good and is not useful these days since most of the devs are relaying on NPM.

Grunt: You can create automation for your development environment to pre process codes or create build scripts with a not very simple config file. It was great back there in 2013 but not much these days.

Gulp: Automation just like Grunt but instead of configurations you can write JavaScript with streams like it's a node application. Much better than Grunt.

Webpack: The coolest kid in the town. It will bundle your app into other bundling patters so you can use all the libraries available in NPM right into your front-end code, load different modules and do many other things. It is amazingly flexible and can create strong development environments. Very trendy these days.

Browserify: Similar to Webpack but less powerful.

Slush and Yeoman: Project scaffolding systems. You can create starter projects with them. Not good that much, use a Github boilerplate instead.

Express: Node JS web application framework. Could be used for routing and anything else needed is cover through middle wares. Very popular and beautifully designed so if you want to create a web application project with node, you are probably using it.


Javascript Modules
https://www.airpair.com/javascript/posts/the-mind-boggling-universe-of-javascript-modules#ok-modules-are-cool-but-why-should-i-use-them-


###### Require
Require is used to require javascript modules and not directories
THis is wrong
app.set('views', require('./views'));
This is right
app.use(require('./routes.js'))

Passing Path
__dirname prints the path
app.set('views', __dirname + '/views');