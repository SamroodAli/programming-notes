# Javascript.

TC39, The standard body that decides what to put in javascript.

javascript is a single threaded evaluated language.
There is two parts to running a function -  calling it(parenthesis) and passing arguments(insert data into parenthesis)

Sync code = Called in the event loop
Microtask queue => called before the start of the next event loop
Macrotask queue/callback queue =<>
The var declarations are hoisted and initialized with undefined.
The formal function declarations are hoisted and initialized with their function reference.
let and const variables are hoisted too but they cannot be accessed before their declarations. This is called Temporal Dead Zone.


{...otherObj} doest create new object, but otherObj's nested object's will still be the otherObj.

Map and set are also objects

The var declarations are hoisted and initialized with undefined.
The formal function declarations are hoisted and initialized with their function reference.
let and const variables are hoisted too but they cannot be accessed before their declarations. This is called Temporal Dead Zone.

V8
The v8 only has a heap and a callstack. The things like setTimeout and ajax are all web apis

Javascript is both an interpreted and compiled language.
The parser first parses the language. It takes in the stream of characters and turns it into the AST(Abstract syntax tree) using tokens it finds(from keyworks). Much like the parsing of the HTML document into the DOM. After the AST is created, it starts running the code.(citation and more research neededclear)
The interpretter part is where javascript runs line by line which is the unoptimised code
The profiler keeps watching this running code and seeks out places to optimise code
Where it sees places it can optimise, it takes that code and compiles it and replaces it's unoptimised counterpart in the running code thus making javascript gradually run faster and faster.


Javascript two phases
First it compiles the code, seeks out function and variable declarations (where hoisting happen I assume!)
Second, it starts running the code.
