# `createStore(reducer, [initialState])`

Creates a Redux [store](Store.md) that holds the complete state tree of your app.  
There should only be a single store in your app.

#### Arguments

1. `reducer` *(Function)*: A reducer function that returns the next state tree, given the current state tree and the action to handle.

2. [`initialState`] *(any)*: The initial state. You may optionally specify it to hydrate the state from the server in universal apps, or to restore a previously serialized user session. If you produced `reducer` with [`combineReducers`](combineReducers.md), this must be a plain object with the same shape as the keys passed to it. Otherwise, you are free to pass anything that your `reducer` can understand.

#### Returns

([*`Store`*](Store.md)): An object that holds the complete state of your app, lets you change it by dispatching actions, and subscribe to the changes.

#### Example

```js
import { createStore } from 'redux';

function todos(state = [], action) {
  switch (action.type) {
  case 'ADD_TODO':
    return state.concat([action.text]);
  default:
    return state;
  }
}

let store = createStore(todos, ['Use Redux']);

store.dispatch({
  type: 'ADD_TODO',
  text: 'Read the docs'
});

console.log(store.getState());
// ['Use Redux', 'Read the docs']
```

#### Tips

* Don’t create more than one store in an application! Instead, use [`combineReducers`](combineReducers.md) to create a single root reducer out of many.

* It is up to you to choose the state format. You can use plain objects or something like [Immutable](http://facebook.github.io/immutable-js/). If you’re not sure, start with plain objects.

* If your state is a plain object, make sure you never mutate it! For example, instead of returning something like `Object.assign(state, newData)` from your reducers, return `Object.assign({}, state, newData)`. This way you don’t override the previous `state`. You can also write `return { ...state, ...newData }` if you enable [ES7 object spread proposal](https://github.com/sebmarkbage/ecmascript-rest-spread) with [Babel stage 1](http://babeljs.io/docs/usage/experimental/).

* For universal apps that run on the server, create a store instance with every request so that they are isolated. Dispatch a few data fetching actions to every store instance before you render the app with it.

* When a store is created, Redux dispatches a dummy action to your reducer to populate the store with the initial state. You are not meant to handle the dummy action directly. Just remember that your reducer should return some kind of initial state if the state given to it as the first argument is `undefined`, and you’re all set.