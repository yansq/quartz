A promise is an object representing the eventual completion or failure of an asynchronous operation.

A promise can have three distinct state:

1. **pending**: means that the final value is not available yet
2. **fulfilled**: means that the operation has been completed and the final value is available, which generally is a successful operation. The state is sometimes also called **resolved**.
3. **rejected**: means that an error prevented the final value from being determined, which generally represents a failed operation.

If, and when, we want to access the result of the operation represented by the promise, we must register an event handler to the promise. This is achieved using the method `then`:

```js
const promise = axios.get("http://localhost:3001/apple")

promise.then(response => {
    console.log(reponse)
})
```