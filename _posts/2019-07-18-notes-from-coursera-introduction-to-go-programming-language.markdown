---
layout: post
title:  "Notes from Coursera - Introduction to Go"
date:   2019-07-18 21:25:00 +1000
categories: programming go
excerpt_separator: <!--more-->
---

Introduction to Go is an intermidiary level course which introduces programmers to Go language. 

<!--more-->

## Go Vs the rest of the programming languages

In contrast to several other scripting languages, Go provides 

1. Faster execution
2. Better Garbage collection
3. Simpler implementation of Object Oriented
4. Efficient concurrency built into the language

### Faster Execution because of better Garbage collection

Go is essentially a compiled language and not  interpreted. This is the reason Go executes faster than interpreted languages. 

However, since interpreters are efficient at memory management, compiled languages usually suffer from memory challenges. Go has efficient Garbage collection built into it which adds the garbage collection code into the compiled code at the time of compilation. Now, this means that Go might be a little slower than other pureplay compiled languages like C or C++, it is still more efficient in managing memory and thus less prone to such runtime errors.

### Simpler/Weaker Object Oriented

- Go doesn't have the concept of a class. Instead, we use structs (from C). These structs define data and methods on that data just like a call.   
- Go doesn't have inheritance. 

### Concurrency

Go programming code allows parallelism, where different tasks can run on different cores. Go provides channels, threads and all the necessary primitives needed for concurrent programming.

## Workspace

Since Go is built with an underlying doctrine that everybody should be able to share their packages, it prescribes (not enforce) a directory structure. 

- There are three directories with the go workspace
	- /src - Source code files
	- /pkg - contains packages or libraries
	- /bin - executables
- We typically put multiple projects in the same workspace. The three directories are therefore shared among different projects.
- GOPATH is the path variable that refers to your workspace. By default Go sets up `/Users/username/go` as the workspace and puts it into the default.

### To install Go on MacOs and setup the GoPath 

#### Install Go using homebrew
```
brew update & brew install golang
```

#### Set Up the workspace
```
mkdir -p $HOME/go/{bin,src}
```

#### Setup the environment
Add the gopath to your bash file


```
nano ~/.bashrc
export GOPATH=$HOME/go
export GOROOT="$(brew --prefix golang)/libexec"
export PATH="$PATH:${GOPATH}/bin:${GOROOT}/bin"
```

All the packages that we `import` in our go programs should be available in the GOROOT or GOPATH directories. That's where GO tool would look for these libraries when its trying to search for these packages

### Packages

- There should be one package called `Main`
- Its only the Main package that gets compiled and built into an executable
- The `main` function in the Main package is where the execution starts from.

### Go Tool 

- `go build` 
- `go doc` : prints documentation from your go packages
- `go fmt` : it'll just indent the program automatically
- `go get packagename` : gets and install packages
- `go list` : lists all the installed packages
- `go run` : runs the executable
- `go test` : executes the tests

### Variables

- variables are case sensitive
- must start with a letter
- can't be keywords
- must have a declaration that specifies its name and a type. eg `var x int` Here var is the keyword used to declare x as an integer
- can do `var x,y,z int`

#### Data Types
- integer
- float
- Strings 

#### Data Type Aliasing with Type declaration

- We can create an alias for a datatype and use that alias in our variable declarations
`type Celsius float64` and then `var temp_c Celsius`, which makes 

#### Initialising is necessary 
- At the time of declaration  `var x int = 100`
- After the declaration `x = 200`
- if you don't initialise it just takes the zero value for that type. That means integer variables will be autoinitialsed with the value 0; Strings will be autoinitialised with value "" and floats with 0.0

#### Short Variable Declaration 
-  `x := 0` where `:=` is the declaration and initialisation. Here the variable is automatically typecasted to whatever value we assign in the right side of the operator. If its 100, the variables would typecasted as integer and so on and so forth.
- Short variable declaration can only happen within a function

