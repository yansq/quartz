# Redux

## Terminology

![redux dateflow](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

### Action

An action is an event that describes something that happened in the application.

```javascript
const addTodoAction = {
    // first part is the feature or category that this action belongs to
    // second part is the specific thing that happened
    type: 'todos/todoAdded',
    // additional information about what happened
    payload: 'Buy milk'
}
```

### Reducer

A **reducer** is a function that receives the current `state` and an `action` object, decides how to update the state if necessary, and returns the new state: `(state, action) => newState`. 

> [!idea] You can think of a reducer as an event listener which handles events based on the received action (event) type.

```javascript
const initialState = { value: 0 }

counterReducer = (state = initialState, action) {
    if (action.type === 'counter/increment') {
        return {
            ...state,
            value: state.value + 1
        }
    }
    return state
}
```

### Store

The current Redux application state lives in an object called the store. The store is created by passing in a reducer, and has a method called `getState` that returns the current state value.

```javascript
import { configureStore } form '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })
// { value: 0 }
console.log(store.getState())
```


### Dispatch

The only way to update the state is to call `store.dispatch()` and pass in an action object. The store will run its reducer function and save the new state value inside.

```javascript
store.dispatch({ type: 'counter/increment' })
// { value: 1 }
console.log(store.getState())
```

We typically call action creators to dispatch the right action:
```javascript
const increment = () => {
    return {
        type: 'counter/increment'
    }
}
store.dispatch(increment())
```

> [!idea] You can think of dispatching actions as "triggering an event"

---

### Selector

Selectors are functions that know how to extract specific pieces of information from a store state value. As an application grows bigger, this can help avoid repeating logic as different parts of the app need to read the same data.

```javascript
const selectCounterValue = state => state.value
const currentValue = selectCounterValue(store.getState())
```

### Redux Slices

A "slice" is a collection of Redux reducer logic and actions for a single feature in your app, typically defined together in a single file.

```javascript
import { configureStore } from '@reduxjs/toolkit'  
import usersReducer from '../features/users/usersSlice'  
import postsReducer from '../features/posts/postsSlice'  
import commentsReducer from '../features/comments/commentsSlice'  
  
export default configureStore({  
    reducer: {  
        users: usersReducer,  
        posts: postsReducer,  
        comments: commentsReducer  
    }  
})
```

Or can just use `cerateSlice` method in redux-toolkit to create reducers and actions in a single file:

```java
import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
    name: 'counter',
    initialState: {
        value: 0
    },
    reducers: {
        increment: state => state.value += 1,
        decrement: state => state.value -= 1,
        incrementByAmount: (state, action) 
                => state.value += action.payload
    }
})

export const { increment, decrement, incrementByAmount } 
        = counterSlice.actions   
export default counterSlice.reducer
```

In the code above, we create three actions:

- `{ type: "counter/increment" }`
- `{ type: "counter/decrement" }`
- `{ type: "counter/incrementByAmount" }`

`createSlice` automatically generates action creators with the same names as the reducer functions we wrote. We can check that by calling one of them and seeing what it returns:

```javascript
console.log(counterSlice.actions.increment())  
// {type: "counter/increment"}
```

> [!danger] You can _only_ write "mutating" logic in Redux Toolkit's `createSlice` and `createReducer` because they use Immer inside! If you write mutating logic in reducers without Immer, it _will_ mutate the state and cause bugs!