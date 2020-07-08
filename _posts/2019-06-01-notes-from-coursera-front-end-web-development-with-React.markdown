---
layout: post
title:  "Notes from Coursera - Front End Web Development with React"
date:   2019-06-01 12:30:00 +1000
categories: programming react
excerpt_separator: <!--more-->
---

Front End Web Development with React is a great introductory course to React. Of all the courses I found online, this one is the best for a beginner with no prior knowledge of React.

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

```
packages are installed into the {prefix}/lib/node_modules folder, instead of the current working directory.
bin files are linked to {prefix}/bin
man pages are linked to {prefix}/share/man 
```

[npm cli flags reference][https://docs.npmjs.com/using-npm/config.html]

#### Installing Create-React-App 
Generates the scaffold for the react application

```
npm install -g create-react-app
``` 


