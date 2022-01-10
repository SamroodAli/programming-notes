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
