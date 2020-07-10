---
layout: post
title:  "Notes from Coursera - Front End Web Development with React"
date:   2019-06-01 12:30:00 +1000
categories: programming react
excerpt_separator: <!--more-->
---

Front End Web Development with React is a great introductory course on React. Of all the courses I found online, this one is the best for a beginner with no prior knowledge of React.

<!--more-->

### Advice on Yarn Vs npm
- Both are package managers. 
- npm comes installed with node. 
- Start with npm until you reach a point where you believe there is something that npm is struggling with; at which time try out yarn and see if it offers a solution


### Setting up your system for React

Start with installing node which would in turn, automatically install npm.
```
brew install node
```

Install create-react-app (and not react-create-app. Its CRA not RCA). Don't forget the -g which stands for “global” mode, so that packages are installed into the prefix folder instead of the current working directory. 

If --global flag is used, 
- packages are installed into the {prefix}/lib/node_modules folder, instead of the current working directory.
- bin files are linked to {prefix}/bin
- man pages are linked to {prefix}/share/man 

[npm cli flags reference](https://docs.npmjs.com/using-npm/config.html)

#### Installing Create-React-App 
Generates the scaffold for the react application

```
npm install -g create-react-app
``` 

### Scaffolding a blank React app and launching it 
Just like `rails new app-name` in Ruby on Rails, the `create-react-app` generates the folder structure and the necessary files you need to begin your react project.

```
create-react-app application-name
```

In order to launch this application and see it in the browser you'll simply need to get inside the application-name folder and run `npm start`

### React App Overview

#### Deconstructing React folder structure
```
\application-name
	\node_modules
	\public
		favicon.ico
		index.html
		manifest.json
		robots.txt
	\src
		App.css
		App.js
		App.test.js
		index.css
		index.js
		logo.svg
		serviceWorker.js
		setupTests.js
	.gitignore
	package-lock.json
	package.json
	README.md
```

### ReactStrap - Bootstrap for React

The first step is to install Bootstrap, followed by installing reactstrap and react-popper. 

```
npm install bootstrap
npm install reactstrap
npm install react-transition-group
npm install react-popper
```

Once we have bootstrap, it needs to added to `index.js` in the src folder. 

```
import 'bootstrap/dist/css/bootstrap.min.css';
``` 

#### Styling with Bootstrap in React

_src/App.js_
```
import { Navbar, NavbarBrand } from 'reactstrap'
```

### Creating Components in React

- User defined components must always start with a capital letter. These compile to React.createElement(...) 
- Tags that start with a lower case letter are treated as DOM tags like h1 or p

> While this is not a requisite, it makes sense to create a folder called components in the src folder. This folder would contain all of your components

To create a component, start with  adding a file in the components folder
```
src/
	components/
		MenuComponent.js
```

The next step is to import the React libraries and create the class for our component. Notice that this class extends Component and has a constructor defined in it. 

```javascript
	import React, { Component } from 'react'

	Class Menu extends Component {
		constructor(props) {
			super(props);
		}
	}
```

Every Component class must execute the `render()` function within which it returns the JSX powered HTML code. This JSX code is executed and then rendered as part of that component. 

```javascript
  import React, { Component } from 'react'

  class Menu extends Component{

  	constructor(props) {
  		super(props);
  	}

  	render() {
  		return (
  			<div className="container">
  				<div className="row">
  					Menu starts here
  				</div>
  			</div>
  		);
  	}
  }
```

This component is now available for us to use in our `App.js` file. Here we have added the Menu component within the Navbar and it will automatically show.

```javascript
import React from 'react'
import Navbar, NavbarBrand from 'reactstrap'
import Menu from 'MenuComponent'

function App(){
	return(
		<div className='container'>
			<Navbar>
				<Manu />
			</Navbar>
		</div>
	)
}
``` 

### State and Props

- Components which are defined as class 

    

---
    
    
### Troubleshooting

#### package.json not found
```
$ npm start
npm ERR! code ENOENT
npm ERR! syscall open
npm ERR! path /Users/xxx/xxx/xxx/package.json
npm ERR! errno -2
npm ERR! enoent ENOENT: no such file or directory, open '/Users/xxx/xxx/xxx/package.json'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent 
```

This occured because I installed packages in the parent directory and not the React application directory. 