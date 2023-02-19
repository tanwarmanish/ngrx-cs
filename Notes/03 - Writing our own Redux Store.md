
## 01 - Project Setup

- [b] [Project Link](https://github.com/UltimateAngular/redux-store)

#### Installation
1. yarn install
2. yarn start

## 02 - Store Creation & Initial State

``` JavaScript
// store/store.ts
export class Store {
	private state: { [key: string]: any };

	constructor(reducers = {}, initialState = {}){
		this.state = initialState;
	}
	
	// value --> store.value
	get value(){
		return this.state;
	}
}
```

``` JavaScript
// store/index.ts
export * from './store';
```

``` Javascript
// app.ts
import * as fromStore from './store';

const reducers = {};
const initialState = {
	todos: [ { label: 'Eat Something', complete: false } ]
};
const store = new fromStore.Store( reducers, initialState);[]
// value --> store.value
```


## 03 - Dispatching Actions

``` JavaScript
// store/store.ts

// inside the Store Class
dispatch( action: { type:string, payload:any } ) {
	this.state = {
		...this.state,
		todos: [ ...this.state.todos, action.payload ]
	};
}
```

``` JavaScript
// app.ts
store.dispatch({
	type: 'ADD_TODO',
	payload: { ... }
});
```


## 04 - All about Reducers

``` JavaScript
// store/reducers.ts
type Action = { type:string , payload: any };

export const initialState = {
	loaded: false,
	loading: false,
	data: [ { label: 'Eat Something', complete: false }]
};
export function reducer(state = initialState, action: Action ){
	switch(action.type){
		case 'ADD_TODO':{
			// add new todo
			const todo = action.payload;
			const data = [ ...state.data, todo ];
			return {
				...state,
				data
			}
		}
	}
	return state;
}
```

``` JavaScript
// store/store
private state: { [key: string]: any };
private reducers: { [key: string]: Function };

constructor(reducers = {}){
	this.reducers = reducers;
	const [ initialState, action ] = [ {}, {} ];
	this.state = this.reduce( initialState, action );
}

dispatch(action:Action){
	this.state = this.reduce( this.state, action );
}

private reduce(state, action){
	const newState = {};
	for (const prop in this.reducers){
		newState[prop] = this.reducers[prop](state[prop],action);
	}
	return newState;
}
```

``` JavaScript
// store/index.ts
export * from './store'
export * from './reducers';
```


``` Javascript
// app.ts
const reducers = {
	todos:  fromStore.reducer
}
const store = new fromStore.Store(reducers);
```


## 05 - Store Subscriptions

``` JavaScript
// store/store.ts
private subscribers: Function[];

constuctor(...args){
	this.subcribers = [];
}

subscribe(fn){
	this.subscribers = [...this.subscribers, fn];
	this.notify();
	return ()=>{
		this.subscribers = this.subscribers.filter(sub => sub != fn);
	};
}

dispatch(action){
	...
	this.notify();
}

private notify(){
	this.subscribers.forEach(fn => fn(this.value));
}
```

``` Javascript
// app.ts
const unsubscribe = store.subscribe(state => console.log({state}));

// unsubscribe(); <-- Unsubscribers from events
```


## 06 - Action Constants & Action Creators

``` JavaScript
// store/actions.ts

// action constants
export const ADD_TODO = '[Todo] Add Todo';
```

``` JavaScript
// store/reducers.ts
import * as fromActions from './actions';

switch(action.type) {
	case fromActions.ADD_TODO: {
		// code here
	}
}
```

``` JavaScript
// store/index.ts
export  * from './actions'
```

``` Javascript
// app.ts
store.dispatch({ 
	type: fromStore.ADD_TODO, 
	payload 
});
```


#### Action Creator

``` JavaScript
// store/actions.ts

// action creator
export class AddToDo {
	readonly type = ADD_TODO;
	constructor( private payload: any){}
}
```

``` Javascript
// app.ts
store.dispatch(new fromStore.AddToDo(payload));
```