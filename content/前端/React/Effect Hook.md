Effect Hooks introduced by React since version 16.8.0. The effect hook lets you perform **side effects** on function components. Data fetching, setting up a subscription, and manually changing the DOM in React components are all examples of **side effects**.

```js
import { useState, useEffect } from 'react'
import axios from 'axios'

const App = () => {
    const [notes, setNotes] = useState([])
    // ...
    useEffect(() => {
        axios
            .get('http://localhost:3001/notes')
            .then(response => {
                console.log('promise fulfilled')
                setNotes(response.data)
            })
    }, [])
}
```

By default, `useEffect` runs after every completed render, but you can choose to fire it only when certain values have changed.

The `useEffect` takes two parameters: 
- The first is a function, the effect itself.
- The second parameter of `useEffect` is used to specify how often the effect is run. If the second parameter is an empty array `[]`, then the effect is only run along with the first render of the component.