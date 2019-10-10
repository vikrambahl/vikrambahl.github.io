---
layout: post
title:  "Introduction to props in Reactjs"
date:   2019-06-04 19:30:00 +1000
categories: react
---

Notes from the react course by TylerMcginnis

Props are used in React for the purpose of data flow. We can pass data to a certain React component using props. Or in other words, the data passed to a component is stored in its `props` 


### Passing data through direct values in props hash

When we instantiate a component by calling its tag `<User />` we can also pass data to this tag in this way - 

```
ReactDOM.render(
	<User 
	name='Robert Stark'
	address='Stark Tower, New York, USA'
	username='ironman'
	avatar='https://s.gravatar.com/avatar/7d1bcf8c2871726ce6fe48e245fcfa9c?s=80
' />,
	Document.getElementById('app')
);
```

Now if in our component definition we can use these variables
```
class User extends React.component{
	return(
		<div>
			<img src={this.props.img} />
			<h2>Name: {this.props.name}</h2>
			<h3>Username: {this.props.username}</h3>
			<p>Address: {this.props.address}</p>
		</div>
	)
}
```

### Passing data through a static hash
There are other ways to pass data to the component as well. You could define static data and refer to it.

```
ReactDOM.render(
	<User 
	user=[USER_DATA]/>,
	Document.getElementById('app')
);
```

```
var USER_DATA= {
	name: 'Robert Stark',
	address: 'Stark Tower, New York, USA',
	username: 'ironman',
	avatar: 'https://s.gravatar.com/avatar/7d1bcf8c2871726ce6fe48e245fcfa9c?s=80'
}


class User extends React.component{
	return(
		<div>
			<img src={this.props.user.img} />
			<h2>Name: {this.props.user.name}</h2>
			<h3>Username: {this.props.user.username}</h3>
			<p>Address: {this.props.user.address}</p>
		</div>
	)
}
```

### Passing data through nested components

Another way would be to pass data to the nested components in a definition. That means within the definition of a component, we are instantiating other components

```
class Name extends React.component{
	return(
		<div>
			Name: {this.props.name}
		</div>
	)
}


class User extends React.component{
	return(
		<div>
			<Name name={this.props.name} />
			<Address address={this.props.address}/>
			<Username username={this.props.username}/>
		</div>
	)
}

ReactDOM.render(
	<User 
	name='Robert Stark'
	address='Stark Tower, New York, USA'
	username='ironman'
	avatar='https://s.gravatar.com/avatar/7d1bcf8c2871726ce6fe48e245fcfa9c?s=80
' />,
	Document.getElementById('app')
);
```

In the above example, I have not put the definitions of Address and Username components. But I hope we get the gist. 
