# About
grunt and gulp are task runners to automate everything that can be automated (i.e. compile CSS/Sass, optimize images, make a bundle and minify/transpile it).
Provides minification and concatenation.
Uses pure JavaScript code.
Converts LESS or SASS to CSS compilation.
Manages file manipulation in the memory and enhances speed by using the Node.js platform.
Huge speed advantage over any other task runner
Easy to code and understand.
Easy to test the web applications..
Plugins are simple to use and they are designed to do one thing at a time.
Performs repetitive tasks repeatedly such as minifying stylesheets, compressing images, etc.

# Installation
# ===============================================
Install gulp globally
```
npm install -g gulp
```

Check gulp installation
```
gulp -v
```

Then install gulp locally
The only thing different here is we used the --save-dev flag which instructs npm to add the dependency to our devDependencies list in our package.json file that we created earlier.
```
npm install --save-dev gulp
```

# With Babel (ES6)
# ===============================================
Install babel core and presets and add them to the project
```
npm install babel-core babel-preset-es2015 --save-dev
```
Once this has finished, we need to create a .babelrc config file to enable the es2015 preset:
```
touch .babelrc
```
And add the following to the file:
```
{
  "presets": ["es2015"]
}
```
We then need to instruct gulp to use Babel. To do this, we need to rename the gulpfile.js to gulpfile.babel.js:
```
mv "gulpfile.js" "gulpfile.babel.js"
```


# Setting Up
# ===============================================
###### Setting Up Our Gulpfile & Running Gulp
Following are the some example tasks that we will accomplish with sass
    Lint our JavaScript. (Seriously. Do it.)
    Compile our Sass files. (Browsers can’t read that stuff...)
    Concatenate our JavaScript. (Reduce HTTP Requests!)
    Minify and rename concatenated files. (Every little bit counts!)

###### Install the required plugins
```
npm install gulp-jshint gulp-sass gulp-concat gulp-uglify gulp-rename --save-dev
```

###### Create GulpFile
Now that our plugins are available for us to use, we can start writing our gulpfile and instructing gulp to perform the tasks our boss assigned to us.
In the root directory of your project create a new file and name it gulpfile.js and paste the following code inside.

Our **lint task** checks any JavaScript file in our js/ directory and makes sure there are no errors in our code.

The **sass task** compiles any of our Sass files in our scss/ directory into CSS and saves the compiled CSS file in our dist/css directory.

The **scripts task** concatenates all JavaScript files in our js/ directory and saves the ouput to our dist/js directory. Then gulp takes that concatenated file, minifies it, renames it and saves it to the dist/js directory alongside the concatenated file.

###### Creating a task
gulp.task('task-name', function() {
   //do stuff here
});
Where ‘task-name’ is a string name and ‘function()’ performs your task. The ‘gulp.task’ registers the function as a task within name and specifies the dependencies on other tasks.

###### Cominging multiple tasks
You can run multiple tasks at a time by creating default task in the configuration file as shown in the following code −

gulp.task('default', ['imagemin', 'styles'], function() {

});

###### Gulp Rungs task async
While rewriting the build process for one of my projects with gulp.js, a strange behavior was driving me mad. My rough setup looked something like this:

```
var gulp = require('gulp'),
    concat = require('gulp-concat'),
    rm = require('gulp-rimraf');

gulp.task('clean', function() {
    gulp.src('dist/*').pipe(rm());
});

gulp.task('concat', function() {
    gulp.src('app/**/*.js').pipe(concat('main.js')).pipe(gulp.dest('dist'));
});

gulp.task('build', ['clean', 'concat']);
```

When I was running $ gulp build I did expect that the dist folder would be cleaned and after that all javascript files would be concatenated into dist/main.js. And in fact it worked at the first try, but sometimes I ended up with an empty dist folder. What was going on?

After a little research I noticed gulp runs all its tasks in parallel. That means sometimes the clean task and sometimes the concat tasks runs first. So I needed a way to run those tasks in their correct order.

In the official gulp docs on Github they recommend using callback functions to run tasks synchronously. Using their solution my code looked something like the following. Unfortunately it still didn't work.

gulp.task('clean', function(cb) {
    gulp.src('dist/*').pipe(rm());
    cb();
});

gulp.task('concat', ['clean'], function() {
    gulp.src('app/**/*.js').pipe(concat('main.js')).pipe(gulp.dest('dist'));
});
The cb() function seemed to be triggered before rm() had finished. So I tried another way which finally worked. Here is the solution I ended up with.

gulp.task('clean', function() {
    return gulp.src('dist/*').pipe(rm());
});

gulp.task('concat', ['clean'], function() {
    return gulp.src('app/**/*.js').pipe(concat('main.js')).pipe(gulp.dest('dist'));
});

gulp.task('build', ['clean', 'concat']);
The difference is, that you need to return the actual task and gulp knows on its own when it's done. To specify the running order of the tasks, you need to add the dependent tasks, which should be finished first, as the second parameter to a task. In my case it's pretty obvious, that the directory needs to be cleaned first, before the concat task may run.



###### Pipe
Gulp.js makes use of pipes for streaming data that needs to be processed.
```
   var gulp = require('gulp');
    var gutil = require('gulp-util');

    // require sass
    var sass = require('gulp-ruby-sass');

    gulp.task('sass', function () {
      gulp.src('./assets/styles/**/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./assets/styles'));
    });

    gulp.task('default', ['sass']);
```
We’ve just created a task called ‘sass’ and inside of its anonymous function, told it what to do. We grab the src file(s) we want to process, which in this case is ./assets/styles/*/.scss—notice the */. Like Grunt, the double ** is saying, “Look in all the subfolders within styles and for all of the .scss files.” We then pipe the file(s) to the Sass function, then pipe what that returns to a Gulp destination directory that we want that final file to live. In this case, it’s gulp.dest(’./assets/styles’)


```
    var gulp = require('gulp'),
        gutil = require('gulp-util'),
        sass = require('gulp-ruby-sass'),
        uglify = require('gulp-uglify'),
        watch = require('gulp-watch'),
        concat = require('gulp-concat'),
        notify = require('gulp-notify');
     
    // sass task
    gulp.task('sass', function () {
      gulp.src('./assets/styles/**/*.scss')
        .pipe(sass({ 
          noCache: true,
          style: "expanded",
          lineNumbers: true,
          loadPath: './assets/styles/*'
        }))
        .pipe(gulp.dest('./assets/styles'))
        .pipe(notify({
          message: "You just got super Sassy!"
        }));;
    });
     
    // uglify task
    gulp.task('js', function() {
      // main app js file
      gulp.src('./assets/js/app.js')
      .pipe(uglify())
      .pipe(concat("app.min.js"))
      .pipe(gulp.dest('./assets/js/'));
     
      // create 1 vendor.js file from all vendor plugin code
      gulp.src('./assets/js/vendor/**/*.js')
      .pipe(uglify())
      .pipe(concat("vendor.js"))
      .pipe(gulp.dest('./assets/js'))
      .pipe( notify({ message: "Javascript is now ugly!"}) );
    });
     
    gulp.task('watch', function() {
      // watch scss files
      gulp.watch('./assets/styles/**/*.scss', function() {
        gulp.run('sass');
      });
     
      gulp.watch('./assets/js/**/*.js', function() {
        gulp.run('js');
      });
    });

gulp.task('default', ['sass', 'js', 'watch']);
```

**gulpfile.js**
```
// Include gulp
var gulp = require('gulp');

// Include Our Core Plugins
var jshint = require('gulp-jshint');
var sass = require('gulp-sass');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var rename = require('gulp-rename');

// Lint Task
gulp.task('lint', function() {
    return gulp.src('js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// Compile Our Sass
gulp.task('sass', function() {
    return gulp.src('scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('dist/css'));
});

// Concatenate & Minify JS
gulp.task('scripts', function() {
    return gulp.src('js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('dist'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('dist/js'));
});

// Watch Files For Changes
gulp.task('watch', function() {
    gulp.watch('js/*.js', ['lint', 'scripts']);
    gulp.watch('scss/*.scss', ['sass']);
});

// Default Task
gulp.task('default', ['lint', 'sass', 'scripts', 'watch']);
```

###### Read and Build all inside a directory tree
The pattern for all files under all directories is usually ./src/less/*/.* or ./src/less/*/, either should work.
Generally speaking, it's better to match specific files extensions — even if they should all be the same — to prevent grabbing system files or other junk. In that case, you can do ./src/less//*.less for just .less files, or something like .src/less/**/*.{less,css} for both .less and .css files.

###### Run specific tasks
For example running the sass task
```
gulp sass
```

###### Default Task
The default task can be specified in the gulpfile. 
```
// Default Task
gulp.task('default', ['lint', 'sass', 'scripts', 'watch']);
```
It can be run via the following too:
```
gulp
```

###### Gulp Plugins website
http://gulpjs.com/plugins/

###### Using for Asset Pipeline
I’ve found myself using Gulp for just about everything involving HTML/CSS/JS these days. It’s super fast, quick to write scripts for and flexible. I’m at a point now where I have a ton of projects I can just cd into, run gulp and be up and running. It’s the best solution I’ve found for delivering static assets whether for local development or production.
One of my favorite parts of Rails has been the Asset Pipeline. The ability to just start throwing assets into the application and have rails spit out CDN-ready assets has been super helpful. 
Gulp is a build system. It’s like Grunt, Make, Rake, and the like. It’s easy to use for the person running it. While it does have a slight learning curve, you’ll find it a super useful tool for all kinds of tasks. It’ll be the fastest weapon in your toolbox for asset compilation

###### Concatenating CSS (SCSS/LESS)
run a gulp watch, make an edit to your file and see gulp recompiling the stylesheets each time. Now we have Gulp compiling our assets each time we touch the file: great! On my projects I like to include livereload to refresh the browser automatically on updated files. 

###### Concatenating Javascript
Now that we’ve mostly reimplemented asset pipeline for CSS, it’s time to look at JavaScript. JavaScript, unlike LESS does not have the convenient @import to support specifying what files to import, so we will just concatenate all of them into one file. (If you want something like that, look into Browserify or webpack, but that’s out of scope for this guide).
gulp-concat is used for this

###### Minification
Minification is easy mode. Just pipe the stream JS through Uglifier (make sure you npm install) and set the compress flag on less:

###### Rev
This is my favorite part of using Gulp. Rev will give you that friendly app-ef62e7.js filename output that Asset Pipeline is famous for. The reason for it is you can cache it forever. New requests will just point to new files. CDNs love this. To get the files to have the hash is pretty easy with Rev.
Here is where a bit of custom code would come into play. When you generate your index.html (or wherever else you reference the CSS/JS) you will have to swap out the URL for the one in the digest file. Should only be a matter of parsing this JSON file in your framework of choice, or having Gulp rewrite your index.html to replace the CSS/JS include with the correct filename.

```
var rev = require('gulp-rev');

gulp.task('rev', ['less', 'scripts'], function() {
  return gulp.src(['dist/**/*.css', 'dist/**/*.js'])
    .pipe(rev())
    .pipe(gulp.dest('dist'))
    .pipe(rev.manifest())
    .pipe(gulp.dest('dist'));
});
```