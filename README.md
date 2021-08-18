# `Core Concepts in Redux`

## State
```js
const state=['Take Five','Claire de Lune','Respect']
```

## Actions
```js
onst state = [ 'Take Five', 'Claire de Lune', 'Respect' ];
const addNewSong={
  type:'songs/addSong',
  payload:'Halo'
}
const removeSong={
  type:'songs/removeSong',
  payload:'Take Five'
}
const removeAll={
  type:'songs/removeAll'
}
```

## Reducers
```js
// Define reducer here
const reducer=(state=initialState,action)=>{
  switch(action.type){
    case 'songs/addSong':
      return [...state,action.payload]
    case 'songs/removeSong':
      const newState=state.filter(item=>{
        return item!=action.payload
      })
      return newState
    default:
      return state
  }
}


const initialState = [ 'Take Five', 'Claire de Lune', 'Respect' ];

const addNewSong = {
  type: 'songs/addSong',
  payload: 'Halo'
};

const removeSong = {
  type: 'songs/removeSong',
  payload: 'Take Five'
};

const removeAll = {
  type: 'songs/removeAll'
}
```
## Rules of Reducers
```js
/ Reducer violates rule 1: 
// They should only calculate the new state value based on the state and action arguments.
 
const globalSong = 'We are the World';

const playlistReducer = (state = [], action) => {
 switch (action.type) {
   case 'songs/addGlobalSong': {
     return [...state, action.payload];
   }
   default:
     return state;
 }
}
 
// Example call to reducer
const state = [ 'Take Five', 'Claire de Lune', 'Respect' ];
const addAction = { type: 'songs/addGlobalSong', payload: 'We are the World' };
const newState = playlistReducer(state, addAction);
```
```js
// Reducer violates rule 2: 
// They are not allowed to modify the existing state. 
// Instead, they must copy the existing state and make changes to the copied values.

const todoReducer = (state = [], action) => {
 switch (action.type) {
   case 'todos/addTodo': {
     return [...state,action.payload];
   }
   case 'todos/removeAll': {
     return [];
   }
   default: {
     return state;
   }
 }
}

// Example call to reducer
const state = [ 'Print trail map', 'Pack snacks', 'Summit the mountain' ];
const addTodoAction = { type: 'todos/addTodo', payload: 'Descend' };
const newState = todoReducer(state, addTodoAction);
```
```js
// Reducer violates rule 3:
 // They must not do any asynchronous logic or have other “side effects”.

const initialState = [0, 1, 2];

const reducer = (state = initialState, action) => {
 switch (action.type) {
   case 'numbers/addRandom': {
     return [...state, action.payload];
   }
   default: {
     return state;
   }
 }
}
 
// Example call to reducer
const state=[1,2,3]
const randomAction = { type: 'numbers/addRandom', payload: Math.random() };
const newState = reducer(state, randomAction);
```
## Immutable Updates and Pure Functions
```js
const removeItemAtIndex = (list, index) => {
 let left=list.slice(0, index)
 let right=list.slice(index+1)
 return [...left,...right]
};

console.log(removeItemAtIndex(['a', 'b', 'c', 'd'], 1));
```
```js
const fs = require('fs');
const file = './data.txt';

const message = fs.readFileSync(file, 'utf8');
const capitalizeMessage = (message) =>message.toUpperCase()
console.log(capitalizeMessage(message));
```
# `Core Redux API`

## Create a Redux Store
```js
// Import createStore here
import {createStore} from 'redux'
const initialState = 0;
const countReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'increment':
      return state + 1;
    default:
      return state;
  }
}

// Create the store here
const store=createStore(countReducer)
```
## Dispatch Actions to the Store
```js
import { createStore } from 'redux';

const initialState = 0;
const countReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'increment':
      return state + 1;
    case 'decrement':
      return state-1
    default:
      return state;
  }
}

const store = createStore(countReducer);

// Dispatch your actions here.
store.dispatch({type:'increment'})
store.dispatch({type:'increment'})
console.log(store.getState())
store.dispatch({type:'decrement'})
store.dispatch({type:'decrement'})
store.dispatch({type:'decrement'})
console.log(store.getState())
```
## Action Creators
```js
import { createStore } from 'redux';

// Create your action creators here.
const increment=()=>{
  return {
    type:'increment'
  }
}
const decrement=()=>{
  return {
    type:'decrement'
  }
}

const initialState = 0;
const countReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'increment':
      return state + 1;
    case 'decrement':
      return state - 1;
    default:
      return state;
  }
}

const store = createStore(countReducer);

// Modify the dispatches below.
store.dispatch(increment());
store.dispatch(increment());
console.log(store.getState());

store.dispatch(decrement());
store.dispatch(decrement());
store.dispatch(decrement());
console.log(store.getState());
```
## Respond to State Changes
```js
import { createStore } from 'redux';

const increment = () => {
  return { type: 'increment' }
}

const decrement = () => {
  return { type: 'decrement' }
}

const initialState = 0;
const countReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'increment':
      return state + 1;
    case 'decrement':
      return state - 1;
    default:
      return state;
  }
}

const store = createStore(countReducer);

// Define your change listener function here.
const printCountStatus=()=>{
  console.log(`The count is ${store.getState()}`);
}
store.subscribe(printCountStatus)
store.dispatch(decrement()); // store.getState() === -1
store.dispatch(increment()); // store.getState() === 0
store.dispatch(increment()); // store.getState() === 1
```
## Connect the Redux Store to a UI
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<link rel="stylesheet" href="./index.css">
	<title>Learn ReactJS</title>
</head>

<body>
  <main id="app">
    <p id='counter'>Waiting for current state.</p>
    <button id='incrementer'>+</button>
    <button id='decrementer'>-</button>
  </main>
	
</body>
<!-- Do Not Remove -->
<script src="https://content.codecademy.com/courses/React/react-16-redux-4-bundle.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.min.js" integrity="sha512-P36ourTueX/PrXrD4Auc1kVLoTE7bkWrIrkaM0IG2X3Fd90LFgTRogpZzNBssay0XOXhrIgudf4wFeftdsPDiQ==" crossorigin="anonymous"></script>
<script src="./store.js"></script>
</html>
```
```js
/* Note to learners: 
Normally, you would import redux like this:

  import { createStore } from 'redux';

Due to Codecademy's technical limitations 
for testing this exercise, we are using 
`require()`.
*/
const { createStore } = require('redux');

// Action Creators
function increment() { 
  return {type: 'increment'}
}

function decrement() { 
  return {type: 'decrement'}
}

// Reducer / Store
const initialState = 0;
const countReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'increment':
      return state + 1; 
    case 'decrement':
      return state - 1; 
    default:
      return state;
  }
};  
const store = createStore(countReducer);

// HTML Elements
const counterElement = document.getElementById('counter');
const incrementer = document.getElementById('incrementer');
const decrementer = document.getElementById('decrementer');
// Store State Change Listener
const render = () => {
  counterElement.innerHTML = store.getState();
}
store.subscribe(render);
render();
// DOM Event Handlers
const incrementerClicked = () => {
  store.dispatch(increment());
}
incrementer.addEventListener('click', incrementerClicked);
 
const decrementerClicked = () => {
  store.dispatch(decrement());
}
decrementer.addEventListener('click', decrementerClicked);

```
## React and Redux
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<link rel="stylesheet" href="./index.css">
	<title>Learn ReactJS</title>
</head>

<body>
  <div id="root">
  </div>
</body>
<!-- Do Not Remove -->
<script src="https://content.codecademy.com/courses/React/react-16-redux-4-bundle.min.js"></script>
<script src="./store.compiled.js"></script>
</html>
```
```js
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';

// REDUX CODE
///////////////////////////////////

const toggle = () => {
  return {type: 'toggle'} 
}
 
const initialState = 'off';
const lightSwitchReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'toggle':
      return state === 'on' ? 'off' : 'on';
    default:
      return state; 
  }
} 
 
const store = createStore(lightSwitchReducer);

// REACT CODE
///////////////////////////////////
 
// Pass the store's current state as a prop to the LightSwitch component.
const render = () => {
  ReactDOM.render(
    <LightSwitch 
      state={store.getState()}
    />,
    document.getElementById('root')
  )
}
 
render(); // Execute once to render with the initial state.
store.subscribe(render); // Re-render in response to state changes.

// Receive the store's state as a prop.
function LightSwitch(props) {
  const state = props.state; 

  // Adjust the UI based on the store's current state.
  const bgColor = state === 'on' ? 'white' : 'black';
  const textColor = state === 'on' ? 'black' : 'white';  
 
  // The click handler dispatches an action to the store.
  const handleLightSwitchClick = () => {
    store.dispatch(toggle());
  }
 
  return (  
    <div style={{background : bgColor, color: textColor}}>
      <button onClick={handleLightSwitchClick}>
        {state}
      </button>
    </div>
  )
}
```
## Implementing a React+Redux App
```js
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';

// REDUX CODE
///////////////////////////////////

const increment = () => {
  return {type: 'increment'} 
}

const decrement = () => { 
  return {type: 'decrement'}
}

const initialState = 0;
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'increment':
      return state + 1;
    case 'decrement':
      return state - 1;
    default:
      return state; 
  }
} 

const store = createStore(counterReducer);

// REACT CODE
///////////////////////////////////

const render = () => {
  ReactDOM.render(
    <CounterApp 
      state={store.getState()}
    />,
    document.getElementById('root')
  )
}
render()

// Render once with the initial state.
// Subscribe render to changes to the store's state.

function CounterApp(props) {
  const state=props.state
  const onIncrementButtonClicked = () => {
    // Dispatch an 'increment' action.
    store.dispatch(increment())
  }
 
  const onDecrementButtonClicked = () => {
    // Dispatch an 'decrement' action.
    store.dispatch(decrement())
  }
  
  return (   
    <div id='counter-app'>
      <h1> {state} </h1>
      <button onClick={onIncrementButtonClicked}>+</button> 
      <button onClick={onDecrementButtonClicked}>-</button>
    </div>
  )
}
store.subscribe(render)
```
## Slices
```js
const initialState={
  allRecipes:[],
  favoriteRecipes:[],
  searchTerm:''
}
```

## Actions and Payloads For Complex State
```js
const allRecipesData = [
  { id: 0, name: 'Biscuits', img: 'img/biscuits.jpg'},
  { id: 1, name: 'Bulgogi', img: 'img/bulgogi.jpg'},
  { id: 2, name: 'Calamari', img: 'img/calamari.jpg'},
  { id: 3, name: 'Ceviche', img: 'img/ceviche.jpg'},
  { id: 4, name: 'Cheeseburger', img: 'img/cheeseburger.jpg'},
  { id: 5, name: 'Churrasco', img: 'img/churrasco.jpg'},
  { id: 6, name: 'Dumplings', img: 'img/dumplings.jpg'},
  { id: 7, name: 'Fish & Chips', img: 'img/fishnchips.jpg'},
  { id: 8, name: 'Hummus', img: 'img/hummus.jpg'},
  { id: 9, name: 'Masala Dosa', img: 'img/masaladosa.jpg'},
  { id: 10, name: 'Pad Thai', img: 'img/padthai.jpg'},
];

export default allRecipesData;
```
```js
import allRecipesData from './data.js';

const initialState = {
  allRecipes: [],
  favoriteRecipes: [],
  searchTerm: ''
};

// Dispatched when the user types in the search input.
// Sends the search term to the store.
const setSearchTerm = (term) => {
  return { 
    type: 'searchTerm/setSearchTerm', 
    payload: term 
  };
}

// Dispatched when the user presses the clear search button.
const clearSearchTerm = () => {
  return { 
    type: 'searchTerm/clearSearchTerm' 
  };
}

// Dispatched when the user first opens the application.
// Sends the allRecipesData array to the store.
const loadData = () => {
  return {
    type:'allRecipes/loadData',
    payload:allRecipesData
  }
}

// Dispatched when the user clicks on the heart icon of 
// a recipe in the "All Recipes" section.
// Sends the recipe object to the store.
const addRecipe = (recipe) => {
  return {
    type:'favoriteRecipes/addRecipe',
    payload:recipe
  }
}

// Dispatched when the user clicks on the broken heart 
// icon of a recipe in the "Favorite Recipes" section.
// Sends the recipe object to the store.
const removeRecipe = (recipe) => {
  return {
    type:'favoriteRecipes/removeRecipe',
    payload:recipe
  }
}

```

## Immutable Updates & Complex State
```js
// The data has been reduced to make the output
// terminal easier to read.

const allRecipesData = [
  { id: 0, name: 'Biscuits', img: 'img/biscuits.jpg'},
  { id: 1, name: 'Bulgogi', img: 'img/bulgogi.jpg'},
  { id: 2, name: 'Calamari', img: 'img/calamari.jpg'},
  { id: 3, name: 'Ceviche', img: 'img/ceviche.jpg'},
];

export default allRecipesData;
```
```js
import { createStore } from 'redux';
import allRecipesData from './data.js';

const initialState = {
  allRecipes: [],
  favoriteRecipes: [],
  searchTerm: ''
};

const setSearchTerm = (term) => {
  return {
    type: 'searchTerm/setSearchTerm',
    payload: term
  };
}

const clearSearchTerm = () => {
  return {
    type: 'searchTerm/clearSearchTerm'
  }; 
};

const loadData = () => {
  return { 
    type: 'allRecipes/loadData', 
    payload: allRecipesData
  };
};

const addRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/addRecipe', 
    payload: recipe 
  };
};

const removeRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/removeRecipe', 
    payload: recipe 
  };
};

/* Complete this reducer */
const recipesReducer = (state = initialState, action) => {
  switch(action.type) {
    case 'allRecipes/loadData':
      return { 
        ...state,
        allRecipes: action.payload
      }
    case 'searchTerm/clearSearchTerm':
      return {
        ...state,
        searchTerm: ''
      }
    
    case 'searchTerm/setSearchTerm':
      return {
        ...state,
        searchTerm: action.payload
        } 

    case 'favoriteRecipes/addRecipe':
      return {
        ...state,
        favoriteRecipes:[...state.favoriteRecipes,action.payload]
      }

    case 'favoriteRecipes/removeRecipe':
      let newArr=state.favoriteRecipes.filter((item)=>(item.id!=action.payload.id))
      return {
        ...state,
        favoriteRecipes:newArr
      }

    default:
      return state;
  }
};

const store = createStore(recipesReducer);

/* DO NOT DELETE */
printTests();
function printTests() {
  store.dispatch(loadData());
  console.log('Initial State after loading data');
  console.log(store.getState());
  console.log();
  store.dispatch(addRecipe(allRecipesData[0]));
  store.dispatch(addRecipe(allRecipesData[1]));
  store.dispatch(setSearchTerm('cheese'));
  console.log("After favoriting Biscuits and Bulgogi and setting the search term to 'cheese'")
  console.log(store.getState());
  console.log();
  store.dispatch(removeRecipe(allRecipesData[1]));
  store.dispatch(clearSearchTerm());
  console.log("After un-favoriting Bulgogi and clearing the search term:")
  console.log(store.getState());
}

```

## Reducer Composition
```js
/* 
Notice that, for each recognized action type, the entire
state object must be reconstructed by first copying the 
contents of the state using `...state`.
*/
const recipesReducer = (state = initialState, action) => {
  switch(action.type) {
    
    case 'allRecipes/loadData':
      return { 
        ...state,
        allRecipes: action.payload
      }
    
    case 'searchTerm/clearSearchTerm':
      return {
        ...state,
        searchTerm: ''
      }
    
    case 'searchTerm/setSearchTerm':
      return {
        ...state, 
        searchTerm: action.payload
      };

    case 'favoriteRecipes/addRecipe':
      return {
        ...state,
        favoriteRecipes: [...state.favoriteRecipes, action.payload]
      };

    case 'favoriteRecipes/removeRecipe':
      return {
        ...state,
        favoriteRecipes: state.favoriteRecipes.filter(element => element.id !== action.payload.id)
      };

    default:
      return state;
  }
};
```
```js
import { createStore } from 'redux';
import allRecipesData from './data.js';

// Action Creators
////////////////////////////////////////

const addRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/addRecipe', 
    payload: recipe 
  };
}

const removeRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/removeRecipe', 
    payload: recipe 
  };
}

const setSearchTerm = (term) => {
  return {
    type: 'searchTerm/setSearchTerm',
    payload: term
  }
}

const clearSearchTerm = () => {
  return {
    type: 'searchTerm/clearSearchTerm'
  }; 
}

const loadData = () => {
  return { 
    type: 'allRecipes/loadData', 
    payload: allRecipesData
  }; 
}

// Reducers
////////////////////////////////////////

const initialAllRecipes = [];
const allRecipesReducer = (allRecipes = initialAllRecipes, action) => {
  switch(action.type) {
    case 'allRecipes/loadData':
      return action.payload
    default:
      return allRecipes;
  }
}

const initialSearchTerm = '';
const searchTermReducer = (searchTerm = initialSearchTerm, action) => {
  switch(action.type) {
    case 'searchTerm/setSearchTerm':
      return action.payload;
    case 'searchTerm/clearSearchTerm':
      return '';
    default: 
      return searchTerm;
  }
}

// Create the initial state for this reducer.
const initialFavoriteRecipes=[]
const favoriteRecipesReducer = (favoriteRecipes =initialFavoriteRecipes, action) => {
  switch(action.type) {
    case 'favoriteRecipes/addRecipe':
      return [...favoriteRecipes,action.payload]
    // Add action.type cases here.
    case 'favoriteRecipes/removeRecipe':
      return favoriteRecipes.filter(item=>item!=action.payload)
    // Don't forget to set the default case!
    default:
      return favoriteRecipes
  }
}


const rootReducer = (state = {}, action) => {
  const nextState = {
    allRecipes: allRecipesReducer(state.allRecipes, action),
    searchTerm: searchTermReducer(state.searchTerm, action),
    // Add in the favoriteRecipes slice using the 
    // favoriteRecipesReducer function. 
    favoriteRecipes:favoriteRecipesReducer(state.favoriteRecipes,action)
  } 
  return nextState;
}

const store = createStore(rootReducer);

```

## combineReducers
```js
const allRecipesData = [
  { id: 0, name: 'Biscuits', img: 'img/biscuits.jpg'},
  { id: 1, name: 'Bulgogi', img: 'img/bulgogi.jpg'},
  { id: 2, name: 'Calamari', img: 'img/calamari.jpg'},
  { id: 3, name: 'Ceviche', img: 'img/ceviche.jpg'},
  { id: 4, name: 'Cheeseburger', img: 'img/cheeseburger.jpg'},
  { id: 5, name: 'Churrasco', img: 'img/churrasco.jpg'},
  { id: 6, name: 'Dumplings', img: 'img/dumplings.jpg'},
  { id: 7, name: 'Fish & Chips', img: 'img/fishnchips.jpg'},
  { id: 8, name: 'Hummus', img: 'img/hummus.jpg'},
  { id: 9, name: 'Masala Dosa', img: 'img/masaladosa.jpg'},
  { id: 10, name: 'Pad Thai', img: 'img/padthai.jpg'},
];

export default allRecipesData;
```
```js
// Import combineReducers from redux here.
import { createStore, combineReducers} from 'redux';
import allRecipesData from './data.js';

// Action Creators
////////////////////////////////////////

const addRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/addRecipe', 
    payload: recipe 
  };
}

const removeRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/removeRecipe', 
    payload: recipe 
  };
}

const setSearchTerm = (term) => {
  return {
    type: 'searchTerm/setSearchTerm',
    payload: term
  }
}

const clearSearchTerm = () => {
  return {
    type: 'searchTerm/clearSearchTerm'
  }; 
}

const loadData = () => {
  return { 
    type: 'allRecipes/loadData', 
    payload: allRecipeData
  };
}

// Reducers
////////////////////////////////////////

const initialAllRecipes = [];
const allRecipesReducer = (allRecipes = initialAllRecipes, action) => {
  switch(action.type) {
    case 'allRecipes/loadData': 
      return action.payload
    default:
      return allRecipes;
  }
}

const initialSearchTerm = '';
const searchTermReducer = (searchTerm = initialSearchTerm, action) => {
  switch(action.type) {
    case 'searchTerm/setSearchTerm':
      return action.payload
    case 'searchTerm/clearSearchTerm':
      return ''
    default: 
      return searchTerm;
  }
}

const initialFavoriteRecipes = [];
const favoriteRecipesReducer = (favoriteRecipes = initialFavoriteRecipes, action) => {
  switch(action.type) {
    case 'favoriteRecipes/addRecipe':
      return [...favoriteRecipes, action.payload]
    case 'favoriteRecipes/removeRecipe':
      return favoriteRecipes.filter(recipe => {
        return recipe.id !== action.payload.id
      });
    default:
      return favoriteRecipes;
  }
}

// Create your `rootReducer` here using combineReducers().
const reducers={
  allRecipes:allRecipesReducer,
  favoriteRecipes:favoriteRecipesReducer,
  searchTerm:searchTermReducer
}
const rootReducer=combineReducers(reducers)
const store=createStore(rootReducer)
```
## A File Structure for Redux
```js
export const addRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/addRecipe', 
    payload: recipe 
  };
}

export const removeRecipe = (recipe) => {
  return { 
    type: 'favoriteRecipes/removeRecipe', 
    payload: recipe 
  };
}
 
const initialFavoriteRecipes = [];
export const favoriteRecipesReducer = (favoriteRecipes = initialFavoriteRecipes, action) => {
  switch(action.type) {
    case 'favoriteRecipes/addRecipe':
      return [...favoriteRecipes, action.payload]
    case 'favoriteRecipes/removeRecipe':
      return favoriteRecipes.filter(recipe => {
        return recipe.id !== action.payload.id
      });
    default:
      return favoriteRecipes;
  }
}
```
```js
import { createStore, combineReducers } from 'redux';
import {favoriteRecipesReducer} from '../features/favoriteRecipes/favoriteRecipesSlice.js'
import {allRecipesReducer} from '../features/allRecipes/allRecipesSlice.js'
import {searchTermReducer} from '../features/searchTerm/searchTermSlice.js'
// Import the slice reducers here.

const reducers = {
  // Add the slice properties here
  favoriteRecipes:favoriteRecipesReducer,
  allRecipes:allRecipesReducer,
  searchTerm:searchTermReducer
}

// Declare the store here.
const store=createStore(combineReducers(reducers))

export default store
```

## Passing Store Data Through the Top-Level React Component
```js
import React from 'react';

import { AllRecipes } from '../features/allRecipes/AllRecipes.js';
import { SearchTerm } from '../features/searchTerm/SearchTerm.js';

export function App(props) {
  const {state, dispatch} = props;

  const visibleAllRecipes = getFilteredRecipes(state.allRecipes, state.searchTerm);
  const visibleFavoriteRecipes = getFilteredRecipes(state.favoriteRecipes, state.searchTerm);

// You'll add the <FavoriteRecipes /> component in the next exercise!
  return (
    <main>
      <section>
        <SearchTerm
          searchTerm={state.searchTerm}
          dispatch={dispatch}
        />
      </section>
      <section>
        <h2>Favorite Recipes</h2>
        
      </section>
      <hr />
      <section>
        <h2>All Recipes</h2>
        <AllRecipes
          allRecipes={visibleAllRecipes} 
          dispatch={dispatch}
        />
      </section>
    </main>
  )
}

/* Utility Helpers */

function getFilteredRecipes(recipes, searchTerm) {
  return recipes.filter(recipe => recipe.name.toLowerCase().includes(searchTerm.toLowerCase()));
}
```

```js
import React from 'react';
import ReactDOM from 'react-dom';
import {store} from './app/store.js'
import { App } from './app/App.js';
// Import 'store' here.


const render = () => {
  // Pass `state` and `dispatch` props to <App />
  ReactDOM.render(
    <App 
    state={store.getState()}
    dispatch={store.dispatch}
    />,
    document.getElementById('root')
  )
}
render();
// Subscribe render to changes to the `store`
store.subscribe(render)
```

## Using Store Data Within Feature Components
```js
import React from 'react';
import FavoriteButton from "../../components/FavoriteButton";
import Recipe from "../../components/Recipe";
const unfavoriteIconUrl = 'https://static-assets.codecademy.com/Courses/Learn-Redux/Recipes-App/icons/unfavorite.svg'
import {removeRecipe} from './favoriteRecipesSlice.js'
// Import removeRecipe from favoriteRecipesSlice.js

export const FavoriteRecipes = (props) =>{
  
  // Extract dispatch and favoriteRecipes from props.
  const {dispatch,favoriteRecipes}=props
  const onRemoveRecipeHandler = (recipe) => {
    // Dispatch a removeRecipe() action.
    dispatch(removeRecipe(recipe))
  };

  // Map the recipe objects in favoriteRecipes to render <Recipe /> components.
  return (
    <div id='favorite-recipes' className="recipes-container">
      {favoriteRecipes.map(createRecipeComponent)}
    </div>
  );

  // Helper Function
  function createRecipeComponent(recipe) {
    return (
      <Recipe recipe={recipe} key={recipe.id}>
        <FavoriteButton
          onClickHandler={() => onRemoveRecipeHandler(recipe)}
          icon={unfavoriteIconUrl}
        >
          Remove Favorite
        </FavoriteButton>
      </Recipe>
    )
  }
  
};
```
```js
import React from 'react';
import {FavoriteRecipes} from '../features/favoriteRecipes/FavoriteRecipes.js'
import { AllRecipes } from '../features/allRecipes/AllRecipes.js';
import { SearchTerm } from '../features/searchTerm/SearchTerm.js';

// Import the FavoriteRecipes component here.

export function App(props) {
  const {state, dispatch} = props;

  const visibleAllRecipes = getFilteredRecipes(state.allRecipes, state.searchTerm);
  const visibleFavoriteRecipes = getFilteredRecipes(state.favoriteRecipes, state.searchTerm);

  // Render the <FavoriteRecipes /> component.
  // Pass `dispatch` and `favoriteRecipes` props.
  return (
    <main>
      <section>
        <SearchTerm
          searchTerm={state.searchTerm}
          dispatch={dispatch}
        />
      </section>
      <section>
        <h2>Favorite Recipes</h2>
        <FavoriteRecipes
        favoriteRecipes={visibleFavoriteRecipes}
        dispatch={dispatch}
        />
      </section>
      <hr />
      <section>
        <h2>All Recipes</h2>
        <AllRecipes
          allRecipes={visibleAllRecipes} 
          dispatch={dispatch}
        />
      </section>
    </main>
  )
}

/* Utility Helpers */

function getFilteredRecipes(recipes, searchTerm) {
  return recipes.filter(recipe => recipe.name.toLowerCase().includes(searchTerm.toLowerCase()));
}
```

# `Connect to React with React Redux`
## The `<Provider>` Component
```js
import React from 'react';

import { loadData } from '../features/allRecipes/allRecipesSlice.js';
import { AllRecipes } from '../features/allRecipes/AllRecipes.js';
import { SearchTerm } from '../features/searchTerm/SearchTerm.js';
import { FavoriteRecipes } from '../features/favoriteRecipes/FavoriteRecipes.js';

export function App() {
  return (
    <main>
      <section>
        <SearchTerm />
      </section>
      <section>
        <h2>Favorite Recipes</h2>
        <FavoriteRecipes />
      </section>
      <hr />
      <section>
        <h2>All Recipes</h2>
        <AllRecipes />
      </section>
    </main>
  )
}
```

```js
import React from 'react';
import ReactDOM from 'react-dom';
// Import Provider component below.
import {Provider} from 'react-redux'

import { App } from './app/App.js';
import { store } from './app/store.js';

ReactDOM.render(
  // Wrap root application with Provider component below.
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

## Selectors
```js
const initialState = ''

export const searchTermReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'searchTerm/setSearchTerm':
      return action.payload;
    case 'searchTerm/clearSearchTerm':
      return '';
    default:
      return state;
  }
}

export function setSearchTerm(term) {
  return {
    type: 'searchTerm/setSearchTerm',
    payload: term
  }
}

export function clearSearchTerm() {
  return {
    type: 'searchTerm/clearSearchTerm'
  }
}

export const selectSearchTerm = (state) => state.searchTerm;
```

```js
import allRecipesData from '../../../data.js'
import { selectSearchTerm } from '../searchTerm/searchTermSlice.js';

export const loadData = () => {
  return {
    type: 'allRecipes/loadData',
    payload: allRecipesData
  }
}

const initialState = [];
export const allRecipesReducer = (allRecipes = initialState, action) => {
  switch (action.type) {
    case 'allRecipes/loadData':
      return action.payload;
    case 'favoriteRecipes/addRecipe':
      return allRecipes.filter(recipe => recipe.id !== action.payload.id);
    case 'favoriteRecipes/removeRecipe':
      return [...allRecipes, action.payload]
    default:
      return allRecipes;
  }
}

// Implement the selectors below.
export const selectAllRecipes=(state)=>state.allRecipes
export const selectFilteredAllRecipes=(state)=>{
  const allRecipes=selectAllRecipes(state)
  const searchTerm=selectSearchTerm(state)
  return allRecipes.filter(recipe => recipe.name.toLowerCase().includes(searchTerm.toLowerCase()))
}

// This code is for testing the seletors only.
const testState = {
  allRecipes: allRecipesData,
  searchTerm: 'ch'
}

const testSelectAllRecipes = () => {
  console.log('All Recipes')
  console.log(selectAllRecipes(testState));
}

const testSelectFilteredAllRecipes = () => {
  console.log('\nRecipes filtered by searchTerm')
  console.log(selectFilteredAllRecipes(testState));
}

// Uncomment these to test each selector.
testSelectAllRecipes();
testSelectFilteredAllRecipes(); 
```

## The `useSelector()` Hook
```js

import React, { useEffect } from 'react';
// Implement the import statements below.
import {useSelector} from 'react-redux'

// Add the selector to the below import statement 
import { loadData,selectFilteredAllRecipes } from './allRecipesSlice.js';
import { addRecipe } from '../favoriteRecipes/favoriteRecipesSlice.js';
import FavoriteButton from "../../components/FavoriteButton";
import Recipe from "../../components/Recipe";
const favoriteIconURL = 'https://static-assets.codecademy.com/Courses/Learn-Redux/Recipes-App/icons/favorite.svg';

export const AllRecipes = () => {
  // Implement allRecipes variable below.
  const allRecipes=useSelector(selectFilteredAllRecipes)
  
  const onFirstRender = () => {
    // dispatch(loadData());
  }
  useEffect(onFirstRender, []);

  const onAddRecipeHandler = (recipe) => {
    // dispatch(addRecipe(recipe));
  };

  return (
    <div className="recipes-container">
      {allRecipes.map((recipe) => (
        <Recipe recipe={recipe} key={recipe.id}>
          <FavoriteButton
            onClickHandler={() => onAddRecipeHandler(recipe)}
            icon={favoriteIconURL}
          >
            Add to Favorites
          </FavoriteButton>
        </Recipe>
      ))}
    </div>
  );
};
```

## The `useDispatch()` Hook
```js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

import { setSearchTerm, clearSearchTerm, selectSearchTerm } from './searchTermSlice.js';
const searchIconUrl = 'https://static-assets.codecademy.com/Courses/Learn-Redux/Recipes-App/icons/search.svg';
const clearIconUrl = 'https://static-assets.codecademy.com/Courses/Learn-Redux/Recipes-App/icons/clear.svg';


export const SearchTerm = () => {
  const searchTerm = useSelector(selectSearchTerm);
  const dispatch = useDispatch();

  const onSearchTermChangeHandler = (e) => {
    const userInput = e.target.value;
    dispatch(setSearchTerm(userInput));
  };
  
  const onClearSearchTermHandler = () => {
    dispatch(clearSearchTerm());
  };

  return (
    <div id="search-container">
      <img id="search-icon" alt="" src={searchIconUrl} />
      <input
        id="search"
        type="text"
        value={searchTerm}
        onChange={onSearchTermChangeHandler}
        placeholder="Search recipes"
      />
      {searchTerm.length > 0 && (
        <button
          onClick={onClearSearchTermHandler}
          type="button"
          id="search-clear-button"
        >
          <img src={clearIconUrl} alt="" />
        </button>
      )}
    </div>
  );
};
```

```js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

import { removeRecipe, selectFilteredFavoriteRecipes } from './favoriteRecipesSlice.js';
import FavoriteButton from "../../components/FavoriteButton";
import Recipe from "../../components/Recipe";
const unfavoriteIconUrl = 'https://static-assets.codecademy.com/Courses/Learn-Redux/Recipes-App/icons/unfavorite.svg';

export const FavoriteRecipes = () =>{
  const favoriteRecipes = useSelector(selectFilteredFavoriteRecipes);
  const dispatch = useDispatch();

  const onRemoveRecipeHandler = (recipe) => {
    // Dispatch the action below.
    dispatch(removeRecipe(recipe))
  };

  return (
    <div className="recipes-container">
      {favoriteRecipes.map(createRecipeComponent)}
    </div>
  );

  // Helper Function
  function createRecipeComponent(recipe) {
    return (
      <Recipe recipe={recipe} key={recipe.id}>
        <FavoriteButton
          onClickHandler={() => onRemoveRecipeHandler(recipe)}
          icon={unfavoriteIconUrl}
        >
          Remove Favorite
        </FavoriteButton>
      </Recipe>
    )
  } 
};
```

```JS
import React, { useEffect } from 'react';
// Add useDispatch to the import statement below.
import { useSelector,useDispatch } from 'react-redux';

import { addRecipe } from '../favoriteRecipes/favoriteRecipesSlice.js';
import { loadData, selectFilteredAllRecipes } from './allRecipesSlice.js';
import FavoriteButton from "../../components/FavoriteButton";
import Recipe from "../../components/Recipe";
const favoriteIconURL = 'https://static-assets.codecademy.com/Courses/Learn-Redux/Recipes-App/icons/favorite.svg';

export const AllRecipes = () => {
  const allRecipes = useSelector(selectFilteredAllRecipes);
  // Implement dispatch variable below.
  const dispatch=useDispatch()

  const onFirstRender = () => {
    dispatch(loadData());
  }
  useEffect(onFirstRender, []);
  
  const onAddRecipeHandler = (recipe) => {
    dispatch(addRecipe(recipe));
  };

  return (
    <div className="recipes-container">
      {allRecipes.map((recipe) => (
        <Recipe recipe={recipe} key={recipe.id}>
          <FavoriteButton
            onClickHandler={() => onAddRecipeHandler(recipe)}
            icon={favoriteIconURL}
          >
            Add to Favorites
          </FavoriteButton>
        </Recipe>
      ))}
    </div>
  );
};
```
