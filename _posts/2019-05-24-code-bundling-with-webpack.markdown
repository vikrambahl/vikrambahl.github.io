---
layout: post
title:  "Code bundling with webpack - Developer Notes"
date:   2018-01-04 10:46:44 +0530
categories: developer-notes react
excerpt_separator: <!--more-->
---

Webpack, at its core, is a code bundler. It takes your code, transforms and bundles it, then returns a brand new version of your code.

<!--more-->

### Why does webpack exist? 

Webpack, at its core, is a code bundler. It takes your code, transforms and bundles it, then returns a brand new version of your code.

### What problem does webpack solve? 

If you are using Coffeescript, you'd need to compile that into javascript. If you are using LESS, you'd need a CSS preprocessor to convert LESS into CSS. Webpack helps you define which all transformations does your project need. It doesn't carry out those transformations per se, which means it won't compile LESS into css but it'll help execute the processes to achieve all transformations.

### The file structures and commands of Webpack?

The webpack configuration is specified in a file called webpack.config.js. Its usually located in the base directory of the project. 

### Understanding webpack.config.js

```   
// In webpack.config.js
   module.exports = {
     entry: './app/index.js',
   }
```

The `entry` tag identifies the starting point or the root file of our javascript app. 

```
// In webpack.config.js
module.exports = {
  entry: './app/index.js',
  module: {
    rules: [
      { test: /\.coffee$/, use: "coffee-loader" }
    ]
  },
}
```

Next we need to define the modules and how they need to be transformed. This is done using the `module` and `rules` directives. In the above example, anything with .coffee extension should be loaded using the coffee-leader module. It is imperetive that the coffee-loader module is already present in our packages.

This can be done using npm. In your terminal you would run `npm install --save-dev coffee-loader` which would then save coffee-loader as a dev dependency in your `package.json` file.

```
// In webpack.config.js
module.exports = {
  entry: './app/index.js',
  module: {
    rules: [
      { test: /\.coffee$/, use: "coffee-loader" }
    ]
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'index_bundle.js'
  }
}
```

Finally, the output tag defines the name of the transformed consolidated output file. In this case, it'll sit in the `dist` folder with the name `index_bundle.js`

Since we have placed the `index_bundle.js` in the dist folder, there is one additional task that needs to be done. We must copy our original index.html to folder that we've specified in the output path. This is done so that we can access the `index_bundle.js` file in our `index.html` file. This requires us to use a webpack plugin which copies the index.html from the root folder into the folder with output path. 

```
// In webpack.config.js
module.exports = {
  entry: './app/index.js',
  module: {
    rules: [
      { test: /\.coffee$/, use: "coffee-loader" }
    ]
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'app/index.html'
    })
  ],
  mode: "development"
}
```

Webpack also allows us to operate in different modes, namely development and production. And when we are ready to execute webpack, we do that using two modules - webpack & webpack-cli. We therefore must install it using npm

`npm install --save-dev webpack-cli webpack`

The `--save` ensures that not only is webpack installed but its also added to `package.json`. So the nexttime somebody wants to install all dependencies of the project, they can do that using `npm`and `package.json`

### Understanding babel.js

While we use webpack to define which all transformations need to be done, we depend upon babel.js to transform our reactjs code into javascript which can be executed on the browser.

## Building an App in React (means building multiple React Components)


### Coding the React Component

With React, the way to build an app is by creating components. Every component would have these 3 elements/parts 

1. State: This is usually the data associated with the component
2. LifeCycle Events: Different times in the lifecyle like OnLoad or OnFocus etc
3. UI: 

While a component may not have a State or a Lifecycle Event, it will always have a UI.

The UI is defined in the component is through the `render` method. To be able to build a simple "Hello World" app in react, we'll need to first build the component

*index.js*
```
var React = require('react');

class App extends React.Component{
  render(){
    return (
      <div>
        Hello React
      </div>
    )
  }
}
```

Now that we have the React Component defined, we need to render it to DOM. This can be done using the ReactDOM.render function. This function takes in two parameters 
1. Which component to render
2. Where to render this component

We therefore simply add the code to render the ReactComponent to the DOM using ReactDOM. The arguements that we introduce to this function are, as expected, the react component and the location where we need to render this component

*index.js*
```
var React = require('react');
var ReactDom = require('react-dom');

class App extends React.Component{
  render(){
    return (
      <div>
        Hello React
      </div>
    )
  }
}

ReactDom.render(
  <App />,Document.getElementById('app')
  )
```

I would also add my css to this app as well. This can be done by simply using the `require` function on the css file. The reason `require` works is because we will use webpack's css loader will automatically convert this require css into a link tag. 

*index.js*
```
var React = require('react');
var ReactDom = require('react-dom');
require('index.css');

class App extends React.Component{
  render(){
    return (
      <div>
        Hello React
      </div>
    )
  }
}

ReactDom.render(
  <App />,Document.getElementById('app')
  )
```


### Webpack Configuration to run the react application

In order to run the application, we'll need to configure webpack to process all the transformations in the react files and bundle the modules. We start off with loading the path variable in our configuration file

*webpack.config.js*
```
var path = require ('path')
```

The next step is to specify the entry and output locations of the modules. The term `path.resolve(_dirname)` essentially provides us with the project's root directory. The `filename` directive would tell us the name of the final file with all the transformed modules that must be placed in the directory.   

*webpack.config.js*
```
var path = require ('path')

module.exports = {
  entry: './app/index.js',
  output: {
    path: path.resolve(_dirname,'dist'),
    filename: 'index_bundle.js'
  } 
}
```

Once we have defined the input and output paths, we need to specify which all transformations do we need

*webpack.config.js*
```
var path = require ('path')

module.exports = {
  entry: './app/index.js',
  output: {
    path: path.resolve(_dirname,'dist'),
    filename: 'index_bundle.js'
  } 
  module:{
    rules: [
      { test: /\.(js)$/, use: 'babel-loader' },
      { test: /\.css$/, use: [ 'style-loader' , 'css-loader' ] }
    ]
  }
  mode: development
}
```

Here the rules state that for all javascript files (identified by the js extension), webpack should run the babel-loader to transform them. However, in case of css files, webpack should first execute the style-loader, then execute the css-loader to transform the files. 

The CSS loader does a simple thing. It transforms all referrals inside a css like `url('/images/bg.jpg')` to `require(bg.jpg')`. Style loader would copy all the content and inject it into the js file. Hence the entire css is copied inside of the final bundled file. 

*webpack.config.js*
```
var path = require ('path');
var HTMLWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './app/index.js',
  output: {
    path: path.resolve(_dirname,'dist'),
    filename: 'index_bundle.js'
  } 
  module:{
    rules: [
      { test: /\.(js)$/, use: 'babel-loader' },
      { test: /\.css$/, use: [ 'style-loader' , 'css-loader' ] }
    ]
  }
  mode: development,
  plugins: [
    newHTMLWebpackPlugin()
  ]
}
```

Now since we are using `babel-loader`, we need to specify specifically, which babel transformations do we need. 
1. The first transformation is to transform our classes (components) into ES5 or ES6. This is done using @babel-env preset
2. The second transformation that we need is to transform JSX into javascript. This is done by @babel-react preset

We achieve this by adding the babel presets to package.json file

*package.json*
```
...
  "main": "index.js",
  "babel": {
    "presets": [
      "@babel/preset-env",
      "@babel/preset-react"
    ]
  },
...  
```

The final step is to get webpack to execute all of these transformations. That itself will be done by adding the script to execute the webpack

*package.json*
```
...
  "main": "index.js",
  "babel": {
    "presets": [
      "@babel/preset-env",
      "@babel/preset-react"
    ]
  },
...
  "scripts": {    
    "create": "webpack"
  },
  "repository": {
...

```

With this script, all we need to do is `npm run create` and all the webpack transformations would be executed. 

### Errors and debugging the above scripts

1. index.css was not found
```
ERROR in ./app/index.js
Module not found: Error: Can't resolve 'index.css' in '/Users/vikrambahl/Work/tutorials/react-tylermcginnis/app'
 @ ./app/index.js 19:0-20
 ```

 The `require('index.css')` in `index.js` was not working because it couldn't find `index.css` even though it was in the same directory. This was resolved by `require('./index.css')` that could resolve the path for index.css

 2. Incorrect formating in package.json 
```
npm ERR! code EJSONPARSE
npm ERR! JSON.parse Failed to parse json
npm ERR! JSON.parse Unexpected token } in JSON at position 358 while parsing near '...ied\" && exit 1",
npm ERR! JSON.parse   },
npm ERR! JSON.parse   "repository": {...'
npm ERR! JSON.parse Failed to parse package.json data.
npm ERR! JSON.parse package.json must be actual JSON, not just JavaScript.
```

This error was resolved by adding a `,` after a hash in webpack.config.js. The error wasn't even in package.json but it still gave this error.

3. The CSS was not getting compiled

```
ERROR in ./app/index.css
Module build failed (from ./node_modules/css-loader/dist/cjs.js):
CssSyntaxError

(2:1) Unknown word

  1 | 
> 2 | var content = require("!!./index.css");
    | ^

```

This error was resolved by the sequencing of the two loaders. Initially, the sequence was `{ test: /\.css$/, use: ['css-loader' , 'style-loader'] },`  where css-loader was executed before style-loader. However the problem was fixed when we reversed the order `{ test: /\.css$/, use: ['style-loader', 'css-loader'] },`