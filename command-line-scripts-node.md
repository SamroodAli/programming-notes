# Command line scripts with Nodejs

Step 1: ## She-bang or Hash-bang : #!

We can tell the shell environment which program to hand the execution of the file over to

But since nodejs can be in different directories dependending on the system, we have to use a program that finds node wherever it is. The program is called env and it is shipped with every machine. It is located in usr/bin/env.

Hence, we use

#! /usr/bin/env node

Step 2: Make the file executable
We can read(r), write(w) or execute(x) a file.
To make a file executable, run the following command
```bash
chmod u+x fileName
```

Step 3: "use strict" mode at the top of the file.

Step 4: Always write prompts, manuals and other helper messages

# Parsing arguments

We can use the built-in process.argv ( posix style). It is an array with the first two arguments being two absolute paths, where node is installed and the path to the file being executed. The rest of the array will be everything passed when executing seperateed using a space delimiter(" ").

```bash
./index.js --hello=world -c9
```
Running the above code will expose in process.argv an array with:
[
'/path/where/node/is/installed',
'/path/to/the/file/being/executed',
'--hello=world',
'-c9'
]
