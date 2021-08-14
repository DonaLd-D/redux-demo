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
