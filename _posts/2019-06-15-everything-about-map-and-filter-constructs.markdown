---
layout: post
title:  "A continuous exploration of .map and .filter in programming languages"
date:   2019-06-15 19:30:00 +1000
categories: programming react
excerpt_separator: <!--more-->
---

In a world of do-while and for-loops, Map and Filter provide a declarative way to traverse through collections. As a Ruby, Python and Javascript programmer, this is your quick reference guide to understanding the nuances of these amazing programing constructs.
<!--more-->


### What are Map and Filter
Map and filter are two functions that allow you to loop over the contents of an array or a hash. They are both in addition to traditional looping constructs like do-while-loops and for-loops. 

#### .map 
Map iterates through an array picking up one element at a time and doing the necessary action on them.

```
var cities = ['Melbourne', 'Sydney', 'Brisbane', 'Perth', 'Darwin']
var cities_with_country = cities.map(function(city){
	return city+" ,Australia"
});
console.log(cities_with_country);
```

This yields `['Melbourne, Australia', 'Sydney, Australia', 'Brisbane, Australia', 'Perth, Australia', 'Darwin, Australia']`. The loop iterates through each element in the cities array, which is then passed as an argument (city) to the callback function, which inturn does whatever computation (adds the country name to the city) it needs on the element and returns the same. The function therefore iterates through the cities array and adds the country to each of the entries 

The returned values are stored in a new array and the original array is retained as is.  

What's important to notice here is that we did not have to specify the starting point or the iteration counter or the condition to terminate the loop. All of those are implicitly understood by the map construct. 

#### .filter 
Filter iterates through the array or hash but returns only those elements which fulfil the condition. 

```
var cities = ['Melbourne', 'Sydney', 'Brisbane', 'Perth', 'Darwin']
var cities_with_country = cities.map(function(city){
	return (city.length > 6)
});
console.log(cities_with_country);
```

#### Code Examples

### Declarative looping constructs
Map and Filter, as opposed to do-while loops or for-loops are declarative in nature. What that means is, that with for-loops and while-do loops, we have to define exactly how the traversal would happen. We have to provide the increment and we have to check conditions. In case of declarative loops, 


#### Code Examples
```
class Users extends React.Component {
  render() {
    return (
      <ul>
        {
          this.props.ulist.map(function(elem){ 
      		  return <li>{elem}</li>
          })
        }
      </ul>
    )
  }
}

ReactDOM.render(
  <Users ulist={['Tyler', 'Mikenzi', 'Ryan', 'Michael']} />,
  document.getElementById('app')
);
```

### Using .map and .filter together

There are several scenarios where you need to use both map and filter together. A quick example is when you need to map through a filtered array. 

```
class Users extends React.Component {
  render() {
    return (
      <h2> Friends List </h2>
      <ul>
        { this.props.list.filter(function(friend){
           return friend.friend === 'true' 
        })
        map(function(elem){ 
            return <li>{elem}</li>
          })
        }
      </ul>
    )
  }
}

ReactDOM.render(
  <Users list={[
    { name: 'Tyler', friend: true },
    { name: 'Ryan', friend: true },
    { name: 'Michael', friend: false },
    { name: 'Mikenzi', friend: false },
    { name: 'Jessica', friend: true },
    { name: 'Dan', friend: false } ]} 
  />,
  document.getElementById('app')
);

```