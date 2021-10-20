# This is architecture notes on jbook-serve react typescript app

## Three big challenges
1. Code will provided to preview as a string. We have to execute it safely
2. Code migght have advanced Js syntax like jsx that the browser cannot execute
3. The code might have import statements for other files or css. We have to deal with those import statements before executing our code.
