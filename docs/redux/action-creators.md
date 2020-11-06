---
title: Action Creators
---

## What is an Action?

- Simple object, like {type : "ADD_TODO", text : "Buy milk"}.
- `type` is the value for the type field in an action. Per the Redux FAQ, this field should be a string, although Redux only enforces that a type field exists in the action.
An action creator is a function that returns an action, like:

```js
const addToCart = (payload) => ({
  type : "ADD_TO_CART",
  payload  
})
```

- thunk action creator is a function that returns a function
- with `redux-thunk`, inner function is called and given references to `dispatch` and `getState`:

```js
const actionWithSideEffects = (val) => (dispatch, getState) => {
  dispatch({type : "REQUEST_STARTED"});
  
  networkLibrary.post("/addToCart", {data : someValue})
    .then(response => dispatch({type : "REQUEST_SUCCEEDED", payload : response})
    .catch(error => dispatch({type : "REQUEST_FAILED", error : error});    
}
```

## Why Use Action Creators?

- Completely possible to do the work of making AJAX calls and calling dispatch entirely inline in a component, but it is generally a good practice to encapsulate behavior, separate concerns, and keep code duplication to a minimum
- also makes code more testable

To me, there are five primary reasons to use action creators rather than putting all your logic directly into a component:

- Basic abstraction
  - Rather than writing action type strings in every component that needs to create the same type of action, put the logic for creating that action in one place
- Documentation
  - The parameters of the function act as a guide for what data is needed to go into the action.
- Brevity and DRY
  - There could be some larger logic that goes into preparing the action object, rather than just immediately returning it
- Encapsulation and consistency 
  - Consistently using action creators means that a component doesn't have to know any of the details of creating and dispatching the action, and whether it's a simple "return the action object" function or a complex thunk function with numerous async calls
- Testability and flexibility: 
  - If a component only ever calls a function passed in as a prop rather than explicitly referencing dispatch, it becomes easy to write tests for the component that pass in a mock version of the function instead. It also enables reusing the component in another situation, or even with something other than Redux

## Dispatching Actions from Components
There's a number of semantically equivalent ways to dispatch Redux actions to the store from a component. For example, all these do the same thing:

```js
// approach 1: define action object in the component
props.dispatch({
    type : "EDIT_CANVAS", 
    payload : {
        newCanvasInfo: newCanvas
    }
})

/* 
  approach 2: 
  use an action creator function 
*/
const actionObject = editItemAttributes({
  newCanvas
})
props.dispatch(actionObject)

/* 
  approach 3: 
  directly pass result of action creator to dispatch 
*/
props.dispatch(
  editItemAttributes({
    newCanvas
  })
)

/* 
  approach 4: 
  pre-bind action creator to automatically call dispatch 
*/
const boundEditCanvas = bindActionCreators(newCanvasAttributes, dispatch);
boundEditCanvas(itemID, itemType, newAttributes);
```
