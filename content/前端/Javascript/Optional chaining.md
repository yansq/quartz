# Optional chaining(?.)

If the **object accessed** or **function called** using this operator is `undefined` or `null`, the expression short circuits and evaluates to `undefined` instead of throwing an error.

```javascript
const adventurer = {
    name: 'Alice',
    cat: {
        name: 'Dinah'
    }
}

const dogName = adventurer.dog?.name
console.log(dogName)
// Expected output: undefined

console.log(adventurer.someNonExistentMethod?.())
// Expected output: undefined
```