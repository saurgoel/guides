# About
grunt and gulp are task runners to automate everything that can be automated (i.e. compile CSS/Sass, optimize images, make a bundle and minify/transpile it).

# Installation
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

# Setting Up
###### Setting Up Our Gulpfile & Running Gulp
Following are the some example tasks that we will accomplish with sass
    Lint our JavaScript. (Seriously. Do it.)
    Compile our Sass files. (Browsers can’t read that stuff...)
    Concatenate our JavaScript. (Reduce HTTP Requests!)
    Minify and rename concatenated files. (Every little bit counts!)

###### Install the required plugins
npm install gulp-jshint gulp-sass gulp-concat gulp-uglify gulp-rename --save-dev

###### Create GulpFile
Now that our plugins are available for us to use, we can start writing our gulpfile and instructing gulp to perform the tasks our boss assigned to us.
In the root directory of your project create a new file and name it gulpfile.js and paste the following code inside.

Our **lint task** checks any JavaScript file in our js/ directory and makes sure there are no errors in our code.

The **sass task** compiles any of our Sass files in our scss/ directory into CSS and saves the compiled CSS file in our dist/css directory.

The **scripts task** concatenates all JavaScript files in our js/ directory and saves the ouput to our dist/js directory. Then gulp takes that concatenated file, minifies it, renames it and saves it to the dist/js directory alongside the concatenated file.

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
