Index.jsx ---- Webpack (with babel loader) ---- Index.js
As we have seen in the beginning, by using JSX and ES6 we can be more productive while working with React. But the JSX syntax and ES6, are not supported in all the browsers.
Hence, if we are using them in the React code, we need to use a tool which translates them to the format that has been supported by the browsers. Its where babel comes into the picture.
Webpack uses loaders to translate the file before bundling them

```
npm i babel-loader babel-preset-es2015 babel-preset-react -S
```
The babel-preset-es2015 and babel-preset-react are plugins being used by the babel-loader to translate ES6 and JSX syntax respectively.

As we did for Webpack, babel-loader also requires some configuration. Here we need to tell it to use the ES6 and JSX plugins.
The next step is telling Webpack (webpack.config.js) to use the babel-loader while bundling the files

```
var config = {
// Existing Code ....
  module : {
    loaders : [
      {
        test : /.jsx?/,
        include : APP_DIR,
        loader : 'babel'
      }
    ]
  }
}
```
The loaders property takes an array of loaders, here we are just using babel-loader. Each loader property should specify what are the file extension it has to process via the test property. Here we have configured it to process both .js and .jsx files using the regular expression. The include property specifies what is the directory to be used to look for these file extensions. The loader property represents the name of the loader.