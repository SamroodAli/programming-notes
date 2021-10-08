# Welcome to TypeScript

[Notes](https://www.typescript-training.com/course/fundamentals-v3) from mike north's typscript fundamentals

## Variables

### Let variables

TS can infer from the value what the type is.

```ts
let age = 6;
```

Here, type of age is number.
We can assign it something else. but TS will yell at us. It will run though.

```ts
let age = 6;
age = "Not a number";
```

### Const variables

```js
const age = 6;
```

Here age has the literal type `6`. It can only a number that is 6. Since typescript knows const cannot point to anything else and the value it is pointing to cannot be changed.

## Functions

```ts
function add(a: number, b: number): number {
  return a + b;
}
```

## Collections: [Arrays, Objects, Types]

### Object: type is the shape of the data

```ts
//declaration
let car: {
  make: string;
  model: string;
  year: number;
};

// assignment
car = {
  make: "Toyota",
  model: "Corolla",
  year: 2002,
};
```

#### Optional property with ?:

```ts
let car: {
  chargeVoltage?: number;
};
```

## Type guard

```ts
let user: {
  name: string;
  age: number;
  student: boolean;
  friends?: [{ name: string; age: number; student: true }];
} = {
  name: "Samrood",
  age: 24,
  student: true,
};

function printUser(user: {
  name: string;
  age: number;
  student: boolean;
  friends?: [{ name: string; age: number; student: true }];
}) {
  let str: string = `${user.name}, ${user.age}, student:${user.student}`;

  if (user.friends) {
    str += `${user.friends[0].age.toString()}`;
  }
  console.log(str);
}

printUser(user);
```

#### Index signatures - Consistent values (dictionaries) in objects

Sometimes we have consistent keys and consistent values

```ts
const phones = {
  home: { country: "+1", area: "211", number: "652-4515" },
  work: { country: "+1", area: "670", number: "752-5856" },
  fax: { country: "+1", area: "322", number: "525-4357" },
};
```

This can be represented as follows.

```ts
let object: {
  [k: string]: {
    [k: string]: string;
    number: number;
  };
};
```

#### Preventing against keys that does not exit when indexing (allowing any keys)

```ts
let user: {
  [k: string]: { age: number } | undefined; // this undefined will make sure we are type guarding against possible undefined when retrieving a key that does not exist
};
```

```ts
if (user.samrood) {
  // since user.samrood can be undefined, we do type guarding here.
  user.samrood.age;
}
```

## Arrays.

Declaration

```ts
let array: string[] = ["samrood"];
let array: string[][] = [["samrood"], ["leon"]];
let array: { name: string }[] = [{ name: "Samrood" }];
```

## Tuples

Fixed length arrays

```ts
let age: [string, number] = ["samrood", "leon"];
```

There is currently no enforcing on the tuple's length. So push and pop bypass length for tuples.
But pushing types not specified does show an error.

```ts
let array: [string, number] = ["Samrood", 10];
array.push(10);
```

## Types of types

[Read on Mike north's typescript website](https://www.typescript-training.com/course/fundamentals-v3/05-structural-vs-nominal-types/)

## Union types ( string | number )

We create Union types with |
"success" | "error"
string | number

```ts
function flipCoin(): "heads" | "tails" {
  if (Math.random() > 0.5) return "heads";
  return "tails";
}
```

```ts
function maybeGetUserInfo():
  | ["error", Error]
  | ["success", { name: string; email: string }] {
  if (flipCoin() === "heads") {
    return [
      "success",
      { name: "Mike North", email: "mike@example.com" },
    ]
  } else {
    return [
      "error",
      new Error("The coin landed on TAILS :("),
    ]
  }

```

[Check out the chapter here on Union and Intersection](https://www.typescript-training.com/course/fundamentals-v3/06-union-and-intersection-types/)

## Intersection types ( Date & {end: Date} )

```ts
const ONE_WEEK = 1000 * 60 * 60 * 24 * 7; // 1w in ms
/// ---cut---
function makeWeek(): Date & { end: Date } {
  //⬅ return type

  const start = new Date();
  const end = new Date(start.valueOf() + ONE_WEEK);

  return { ...start, end }; // kind of Object.assign
}

const thisWeek = makeWeek();
thisWeek.toISOString();
thisWeek.end.toISOString();
```

## Type Aliases

We can reuse types using the type keyword

```ts
type ReactTextNode = string | number;

function getText(score): ReactTextNode {
  if (score > 100) return "Congratulations, you have won";
  return score;
}
```

```ts
type car = { make: string; number: string; year: number };
```

With our previous tagging example

**```ts**
type UserInfoOutcomeError= = ["error" , Error]
type UserInforOutcomeSuccess = ["success",{name:string,profession:string}]
type UserInfoOutcome = UserInfoOutcomeError |UserInforOutcomeSuccess

function getUserInfo() : UserInfoOutcome{
if(some error reason)return ["error",new Error("reason for error")]
return ["success",{name:"Samrood",profession:"Programmer"}]
}

````

## Type Inheritance

```ts
type SpecialDate = Date & { getReason(): string };
````

# Stephen grider typescript course

# Types

Types are information about the properties and functions of values
A string 'samrood' has functions on it.

# Type annotation vs inference

## Annotation

Annotation is when we tell typescript what types are

### Annotations with class

```ts
class Car {}

let car: Car;
```

### Annotations with Object literal

```ts
let point: { x: number; y: number } = {
  x: 10,
  y: 20,
};
```

or with Inferface

```ts
interface Point {
  x: number;
  y: number;
}

let point: Point = { x: 10, y: 20 };
```

### Annotation with functions

```ts
const logNumber: Function = (i: number): string => {
  return "a string";
};
```

## Inference

Inference is when typescript figures out what types are

```ts
variable declaration = variable initialization
const color = 'red'
```

if declaration and initialization happen in the same line, typescript figures out the type from the value assigned

If we declared a variable, but did not assign a variable,the type is going to be `any`.

```ts
let number; // type is `any`
number = 5;
```

`Avoid 'any' types at any cost.`

# When to use type annotation

1 ) Functions that return any type, example: JSON.parse()
2 ) Delayed initialization (declaration and initialization in different lines)
3 ) Where type was inferred but was incorrect

example

```ts
let numbers = [-10, -1, 12];
let numberAboveZero = false;

numbers.forEach((num) => {
  if (num > 0) {
    numberAboveZero = num; // there will be a type error as typescript thought numberAboveZero is always going to be a boolean value, so we need to give type annotation
  }
});
```

fix with type annotation:

```ts
let numbers = [-10, -1, 12];
let numberAboveZero: boolean | number = false;

numbers.forEach((num) => {
  if (num > 0) {
    numberAboveZero = num; // there will be a type error as typescript thought numberAboveZero is always going to be a boolean value, so we need to give type annotation
  }
});
```
