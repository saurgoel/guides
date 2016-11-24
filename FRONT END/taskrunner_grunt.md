Grunt is the workhorse of the Node world. You need tasks run, then you put Grunt to work. If you are from the Rails world, Grunt is much like make or Rake. Grunt, simply defined, is a task runner built over Node.js that can be used to automate certain tasks in almost any project, in any language. Grunt and Grunt plugins are installed and managed via npm.
In one word? Automation.
Grunt helps you in performing repetitive tasks like:
    Minification
    Compilation
    Unit testing
    Linting and more

###### Install grunt and run globally
```
npm install grunt -g --save
```

In order to get more productive with Grunt, you will need to install Grunt's command line interface (CLI) globally. Run the following command:
```
npm install -g grunt-cli
```
This will put the grunt command in your system path, allowing it to be run from any directory. Note that installing grunt-cli does not install the Grunt task runner! The job of the Grunt CLI is to run the version of Grunt which has been installed next to a Gruntfile.

###### gruntfile (gruntfile.js)
The Gruntfile is a valid JavaScript or CoffeeScript file that belongs in the root directory of your project next to the package.json file, and should be committed with your project source. A Gruntfile is comprised of the following parts:

    The "wrapper" function
    Project and task configuration
    Loading Grunt plugins and tasks
    Custom tasks
    
###### Wrapper Function
The wrapper function is nothing but a function assigned to your module exports that encapsulates all the Grunt code. This function is called by the Grunt engine and serves as the entry point for Grunt configurations. All gruntcode must be specified in this.
```
module.exports = function(grunt) {
  // Do grunt-related things in here
};
```

###### Project and task configuration
Most Grunt tasks rely on configuration data defined in an object passed to the grunt.initConfig method. That’s why we need to specify configurations as desired by the plugins we use. Grunt also have several helper methods; one of them is reading files. Hence we used grunt.file.readJSON to read our package.json file and store its parsed object in the pkg key.
```
module.exports = function(grunt) {
  // Project configuration.
  grunt.initConfig({
    //Lets read the project package.json
    pkg: grunt.file.readJSON('package.json'),
    //Some other configs
    someKey: 'Some Value',
    author: 'Modulus.io'
  });

};
```
###### Loading grunt plugins and tasks
Many commonly used tasks like concatenation, minification and linting are available as Grunt plugins.

```
module.exports = function (grunt) {
  // Project configuration.
  grunt.initConfig({
    //Lets read the project package.json
    pkg: grunt.file.readJSON('package.json'),
    //Some other configs
    someKey: 'Some Value',
    author: 'Modulus.io',
    filePostfix: '-<%= pkg.name %>-<%= pkg.version %>',
    greetingMessage: 'Running Grunt for project: <%= pkg.name %>, Version: <%= pkg.version %>, Author: <%= author %>',

    //Configure Clean Module
    clean: ['.tmp', 'dist', 'npm-debug.log']
  });

  // Load the plugin that provides the "clean" task.
  grunt.loadNpmTasks('grunt-contrib-clean');

};
```

###### Installing a plugin
Let’s install a simple plugin, grunt-clean, into our project and try to configure that in our Gruntfile. Install the module via npm by running the command:
```
> npm install grunt-contrib-clean --save-dev
```
and now we **load that plugin**
Now we’ll load the task in our Gruntfile via the grunt.loadNpmTask('pluginName') method and do some configurations as required. Now our Gruntfile reads as shown below:
```
module.exports = function (grunt) {

  // Project configuration.
  grunt.initConfig({
    //Lets read the project package.json
    pkg: grunt.file.readJSON('package.json'),
    //Some other configs
    someKey: 'Some Value',
    author: 'Modulus.io',
    filePostfix: '-<%= pkg.name %>-<%= pkg.version %>',
    greetingMessage: 'Running Grunt for project: <%= pkg.name %>, Version: <%= pkg.version %>, Author: <%= author %>',

    //Configure Clean Module
    clean: ['.tmp', 'dist', 'npm-debug.log']
  });

  // Load the plugin that provides the "clean" task.
  grunt.loadNpmTasks('grunt-contrib-clean');

};
```

###### Adding Subtasks
You can also configure sub tasks as they are supported by most Plugins. To understand this concept, let’s review the following example:

```
module.exports = function (grunt) {
 ***javascript***
 // Project configuration.
  grunt.initConfig({
    //Lets read the project package.json
    pkg: grunt.file.readJSON('package.json'),
    //Some other configs
    someKey: 'Some Value',
    author: 'Modulus.io',
    filePostfix: '-<%= pkg.name %>-<%= pkg.version %>',
    greetingMessage: 'Running Grunt for project: <%= pkg.name %>, Version: <%= pkg.version %>, Author: <%= author %>',

    //Configure Clean Module
    clean: {
      npm: 'npm-debug.log',
      temp: ['temp', '.tmp'],
      dist: ['dist', 'out/dist']
    }
  });

  // Load the plugin that provides the "clean" task.
  grunt.loadNpmTasks('grunt-contrib-clean');

};
```
Here during configurations we specified an object with different keys including npm, temp and dist. These are random names we came up with in order to divide the clean task into subtasks. Now we can run subtasks to delete all mentioned files/dirs or delete a subgroup. This is demonstrated below:
#This will delete all files, npm-debug.log, temp, .tmp, dist, out/dist > grunt clean
#This will delete only npm-debug.log > grunt clean:npm
#This will delete only temp, .tmp > grunt clean:temp
#This will delete only dist, out/dist > grunt clean:dist

just like rake db:migrate

In the previous example, we specified the key we provided in the config to run the subtask. The syntax to use for writing the key is:

```
> grunt TaskName:SubTaskKeySpecifiedInConfig
```

###### Custom Tasks
You can define tasks with custom names, which can be a combination of existing tasks or purely your own implementation. If you are implementing custom tasks, you must implement them in JavaScript.
```
module.exports = function (grunt) {
  // Project configuration.
  grunt.initConfig({
    //Lets read the project package.json
    pkg: grunt.file.readJSON('package.json'),
    //Some other configs
    someKey: 'Some Value',
    author: 'Modulus.io',
    filePostfix: '-<%= pkg.name %>-<%= pkg.version %>',
    greetingMessage: 'Running Grunt for project: <%= pkg.name %>, Version: <%= pkg.version %>, Author: <%= author %>. ',

    //Configure Clean Module
    clean: {
      npm: 'npm-debug.log',
      temp: ['temp', '.tmp'],
      dist: ['dist', 'out/dist']
    }
  });

  // Load the plugin that provides the "clean" task.
  grunt.loadNpmTasks('grunt-contrib-clean');

  //Lets register a basic task.
  grunt.registerTask('print-info', 'Lets print some info about the project.', function() {
    grunt.log.write(grunt.config('greetingMessage')).ok();
  });

  //Specify a custom task which is combination of tasks.
  grunt.registerTask('my-clean', ['print-info', 'clean:dist', 'clean:npm']);

};
```
In this latest example above, we created a custom task print-info, and executed some JavaScript in it. We used the grunt.registerTask() method to do so. We also defined a custom task my-clean which is combination of other tasks. Because you can run any JavaScript in your tasks, the possibilities are endless.

Run the following command to see what tasks are available:

```
> grunt --help
```

You should see the two custom tasks specified. Now you can run your my-clean task:

```
> grunt my-clean
```

###### Default Task
When you just run grunt in your project root without any tasks specified, then it looks for the default task and runs it. You can specify the default task by simply issuing the command:

```
> grunt
```

To specify a default task we simply register a task named default. Let’s do that; now our Gruntfile looks like this:

```
module.exports = function (grunt) {
 // Project configuration.
  grunt.initConfig({
    //Lets read the project package.json
    pkg: grunt.file.readJSON('package.json'),
    //Some other configs
    someKey: 'Some Value',
    author: 'Modulus.io',
    filePostfix: '-<%= pkg.name %>-<%= pkg.version %>',
    greetingMessage: 'Running Grunt for project: <%= pkg.name %>, Version: <%= pkg.version %>, Author: <%= author %>. ',

    //Configure Clean Module
    clean: {
      npm: 'npm-debug.log',
      temp: ['temp', '.tmp'],
      dist: ['dist', 'out/dist']
    }
  });

  // Load the plugin that provides the "clean" task.
  grunt.loadNpmTasks('grunt-contrib-clean');

  //Lets register a basic task.
  grunt.registerTask('print-info', 'Lets print some info about the project.', function() {
    grunt.log.write(grunt.config('greetingMessage')).ok();
  });

  //Specify a custom task which is combination of tasks.
  grunt.registerTask('my-clean', ['print-info', 'clean:dist', 'clean:npm']);

  //Specify a default task
  grunt.registerTask('default', ['my-clean', 'clean:temp']);
};
```

###### Setup sass
