
## 01 - Introduction to State Management

#### State?
- Server response data
- User Information
- User Input
- UI State
- Router/Location state

- [!] State typically lives inside the store.
- [i] Store is just a container that holds our state in a centralized location.

#### State Management Libraries
- Model our app state
- Read/Update state
- Monitor changes to state


## 02 - Redux: Three Principles
 - [!] *Important*
1. Single source of truth
2. State is read-only
3. Pure functions update state

#### Single Source of truth
- One state tree inside Store
- It is Predictable and Maintainable
- Easy to Test and Debug.
- Easy to create Server Side Rendered Apps **( Angular Universal )**

#### State is read-only
- Derive properties(values) from state
- Dispatch **actions** to change the state
- Immutable update patterns

#### Pure functions update state
- Pure functions are **reducers**
- Reducers respond to **action** types
- Reducers return new state


## 03 - Redux: Core Concepts

#### Core Concepts
- [!] *Important*
1. Single state tree
2. Actions
3. Reducers
4. Store
5. One-way dataflow

#### Single State Tree (1/5)
- Plain JavaScript Object
- Created by Reducers

``` Javascript
// Initial State
const state = {
	todos: []
}
```


#### Actions (2/5)
- Dispatch actions to reducers i.e. trigger/invoke reducers
- Two Properties:
	1. **type** : string, describe event (unique)
	2. **payload?** : data (optional)

``` Javascript
// Action
const action = {
	type : 'ADD_TODO', // always keep them unique
	payload: {
		label: 'Eat_Something',
		complete: false
	}
}
```


#### Reducers (3/5)
- Pure functions
- Given dispatched action they:
	1. Respond to **action.type**
	2. Has access to **action.payload**
	3. Compose new **State**
	4. Returns new **State**

``` Javascript
// reducer
function reducer ( state , action ) {
	switch( action.type ) {
		case 'ADD_TODO': {
			const todo = action.payload;
			const todos = [...state.todos, todo ];
			return { todos };
		}
	}
	return state;
}
```

``` Javascript
// new state
const state = {
	todos: [
		{ label: 'Eat Something', complete: false }
	]
}
```


#### Store (4/5)
- State container
- Components interact with the Store:
	1. **Subscribe** to slices of State
	2. Dispatch **Actions** to the Store
	3. Store invokes **Reducers** with previous State and Action
	4. Reducers compose new **State**
	5. Store is updated, notifies subscribers


#### One-Way Dataflow (5/5)

![One Way Dataflow](02-one-way-data-flow.png)



## 04 - Immutable and Mutable Javascript

#### Understanding Immutability?
An immutable object is an object whose state cannot be modified after creation.

#### Why Immutable?
- State is Predictable hence Debugging is easy
- Explicit state changes
- Can Undo state changes
- Provides better Performance
- Mutation(Changes) can be Tracked

#### Mutability in JavaScript
- Functions
- Objects
- Arrays

``` Javascript
// Mutable by Design
const character = { name: 'Han Solo' };
character.role = 'Captain';
console.log(character); 
// character --> { name: 'Han Solo', role:'Captain' }
```

``` Javascript
// Mutable by Design
const names = ['Han Solo', 'Darth Vader'];
names.push('R2-D2');
console.log(names);
// names --> ['Han Solo', 'Darth Vader', 'R2-D2']
```


#### Immutability in JavaScript
Data Structures that are immutable in JavaScript:
- Strings
- Numbers

``` JavaScript
// Immutable by Design
const name = 'Han Solo';
const uppercaseName = name.toUpperCase();
console.log(name,uppercaseName);
// name -> 'Han Solo'; uppercaseName --> 'HAN SOLO'
```

``` JavaScript
// Immutable Operations
const character = { name:'Han Solo' };

// Object.assign( {}, character, { role: 'Captain' });
const updatedCharacter = { ...character, role: 'Captain' };

// { name: 'Han Solo' };
console.log( character );

// { name: 'Han Solo', role: 'Captain' };
console.log( updatedCharacter );
```

``` JavaScript
// Immutable Operations
const names = ['Han Solo', 'Darth Vader'];
const newNames = [ ...names, 'R2-D2' ];
console.log(names);
// names --> ['Han Solo', 'Darth Vader']

console.log(newNames);
// newNames --> ['Han Solo', 'Darth Vader', 'R2-D2']
```



