# Nullish coalescing operator (??)

> [!info] Since ES11.

The **nullish coalescing (`??`)** operator is a logical operator that returns its right-hand side operand when its left-hand side operand is `null`or `undefined`, and otherwise returns its left-hand side operand.

```javascript
const foo = null ?? 'default string';
console.log(foo);
// Expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// Expected output: 0
```

The logical OR(||) operator checks for falsy values, which includes null, undefined, false, 0, and an empty string.

If you want to check only for null or undefined values, it's better to use nullish values using `??` instead of the **a Logical OR (||)**.  In the code below, "0" maybe is a valid value, we don't want to do Short-circuit evaluation in this situation.

```javascript
const value1 = 0;  
const value2 = 2;
const result = value1 || value2;
console.log(result); // 2

const result2 = value1 ?? value2;
console.log(result2); // 0
```
