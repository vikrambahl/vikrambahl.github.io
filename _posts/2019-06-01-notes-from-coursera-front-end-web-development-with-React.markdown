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
npm install react-popper @popperjs/core
npm install jquery
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

Every Component class must execute the `render()` function within which it returns the JSX powered HTML code. render() is the most used method for any React powered component which returns a JSX with backend data. It is seen as a normal function but render() function has to return something whether it is null. If the DOM Node already contains content rendered by React, this content will be updated to reflect the new React Element.


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
import { Navbar, NavbarBrand } from 'reactstrap'
import Menu from 'MenuComponent'

function App(){
	return(
		<div className='container'>
			<Navbar>
				<Menu />
			</Navbar>
		</div>
	)
}
``` 

### State and Props

- Only those components which are defined as class can have a state. This is because the state is defined in the constructor only


#### How to pass data or information between parent component and child component

- The prevalent practice is to lift the `state` (of the app or components) up the chain to parent components so that any changes in state can be shared across the entire app
- The state is then passed down to child components using `props`
- Another prevalent practice demonstrated here is to use render to build a pseudo component and use it in he final  

#### Storing the state in App.js
 
- Notice that state is setup as a hash with dishes as key and the constant DISHES (which has been defined in dishes.js file) as the value
- Further, the expression `<Menu  dishes:{ this.state.dishes } />` helps us pass this information down to the Menu component by making `dishes` available as props to the Menu component 

_App.js_
```
import { DISHES } from './shared/dishes';

...


class App extends Component {

    constructor(props) {

        super(props);

        this.state = {
            dishes: DISHES
        };

    }

 ...

    return(
      <Menu dishes={ this.state.dishes } />
    );
}
```

We refer to the variable `dishes=` in the called component through the use of props `this.props.dishes`  

_MenuComponent.js_
```
...
render() {
        const menu = this.props.dishes.map((dish) => {
            return (
...
```

#### Capturing events that may modify state of a component 

Now for the component, there are several events that can happen like clicks, scroll, drag etc. The usual practice is to capture these events in the state of the component. 

We start with defining a state-variable `selectedDish` in which we can capture this event. We initiate it with the value `null`. 

_MenuComponent.js_
```
...
    contructor(props){
        super(props);

        this.state = {
            selectedDish = null
        }
    }
...
```

Further, we add a function that can capture the event and be invoked when it happens

_MenuComponent.js_
```
...
<Card onClick={() => this.onDishSelect(dish)}>
...
```

We further create a function in the class called `onDishSelect()`. In this function we change the state by setting selectedDish to the dish that we have just clicked 
   
#### Changing the State with setState

State of a component should not be changed using a `=`. That means `this.state.selectedDish = 'Pie'` is not a valid method. Instead we need to use the `setState` method. 

```javascript
...
constructor(props){
...    
}

onDishSelect(dish){
    this.setState( { selectedDish : dish} );
}

````

### Flow of control within a component class

When a component is instantiated in JSX, the first part that is executed is the constructor. After that the control flows to render() function that executes. Within the render function ofcourse, we can call other functions (which would render something else).

### Lifecycle of a react component

Every React Component goes through the following stages in its lifecycle 
- Mount
- Update
- Unmount

Several lifecycle methods are available at each stage and these act as hooks to perform an action at that specific stage.

#### Methods invoked at the time of Mounting

- constructor()
- getDerivedStateFromProps()
- render()
- componentDidMount() 
- componentWillMount() is now deprecated

The order in which they are executed is also as stated above. In fact `componentDidMount()` is executed after the component has been added.


### Component Types

#### Presentational Vs Container Components
 
Presentational containers do not store any state and therefore don't even trigger change of state. They are purely for presentation purposes only. Container components are purely to contain/track states and then pass them to other presentational containers. They are usually used to fetch data from servers and pass it to presentational containers.


#### Skinny Vs Fat Components

Is also another name to the categories provided above. Skinny components do not carry any business logic (usually in the form of states). They are primarily concerned with the view logic. Fat components do have a lot of functional logic in the form of change of states etc.

This is used to define the hierarchy of components. 

#### Implementing Presentational and Container components

#### App.js to Container component 

We usually build a pseudo-container component called `MainComponent.js` and move mostly everything from App component to Main component. That means, all the presentation containers `Menu` and `DishDetail` are moved to Main. Even state information is now moved to MainComponent. 

Since Main component is the container component, we would therefore move the `selectedDish` state and the onDishSelect() function out of `MenuComponent` and add it to `MainComponent`. 


MainComponent.js
```
class Main extends Component {

    constructor(props){
        super(props);
    

        this.state = {
            dishes: DISHES,
            selectedDish: null
        };
    }

    onDishSelect(dish){
        this.setState(selectedDish: dish)
    }

...

    <Menu dishes={this.state.dishes} />
    <DishDetail dish={this.state.selectedDish} /> 
}
```

#### Passing state from Presentation Component to Container Component

In the example above, even though we have moved the state into MainComponent, we still need to have the clicked dish from MenuComponent passed back to the MainComponent. 

This can be achieved by making the entire `<Menu />` component have an `onClick` function associated with it. This onClick on a component actually acts as a props and connects the `OnClick` function within the Component with the Component Tag wherever its called. Also note that the term in the brackets of `onClick={(dish.id) ... }` which is the dish.id would be used to receive the props back from onClick within the component. 

In this specific case, instead of getting the entire dish passed through at onClick, we simply get the id of the dish passed back. Further, the `dish_id` that is passed back as props is then passed using `=>` to invoke the function `this.state.onDishSelect(dish_id)`

_MainComponent.js_
```
    <Menu dishes={this.state.dishes} onClick={(dish_id) => this.state.onDishSelect(dish_id)}
```

We also make the chage to pass the onClick event back to a props OnClick function. 

_MenuComponent.js_
```
    <Card onClick={() => this.props.onClick(dish.id)}
```

We also change the On Dish Select function to get the dish_id. The reason we have changed everything to Ids is because we already have the dishes array and we can always pull out the dish by using the `filter` on dishes array.

```
    onDishSelect(dish_id){
        this.setState(selectedDishId: dish_id);
    }

 ...
 
       
```



---
    
    
### Troubleshooting


#### Your render method should have return statement
```
./src/components/MenuComponent.js
  Line 13:3:  Your render method should have return statement  react/require-render-return
```




#### Incorrect import from a library

```
./src/App.js
Attempted import error: 'reactstrap' does not contain a default export (imported as 'Navbar').
```
```
 ~~ import Navbar, { NavbarBrand } from 'reactstrap'; ~~ #Incorrect
 import { Navbar, NavbarBrand } from 'reactstrap'
```

#### Import Modules without the npm_modules in path

index.js
```
~~ import './mode_modules/bootstrap/dist/css/bootstrap.min.css'; ~~
import 'bootstrap/dist/css/bootstrap.min.css'; 
```

You don't need to have `./node_modules` in the path since NPM would by default look within the folder.


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