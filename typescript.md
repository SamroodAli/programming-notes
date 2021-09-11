# Welcome to TypeScript

## Variables

### Let variables
TS can infer from the value what the type is.

```ts
let age = 6
```

Here, type of age is number.
We can assign it something else. but TS will yell at us. It will run though.

```ts
let age = 6
age = "Not a number"
```
### Const variables

```js
const age = 6
```
Here age has the literal type `6`. It can only a number that is 6. Since typescript knows const cannot point to anything else and the value it is pointing to cannot be changed.

## Functions

```ts
function add(a: number, b: number): number {
  return a + b
}
```

## Collections: [Arrays, Objects, Types]

Object: type is the shape of the data

