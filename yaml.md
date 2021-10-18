# YAML
YAML aint markup language => .yml or .yaml

# What
Widely used format for writing configuration settings.
Data Serialization language just like xml or json. (Standard format to transfer data).

# Why ?
Human readable and intuitive

# How ? 
- Line seperations and spaces with indentations
- Recommended is 2 spaces for indentation

# Example

```yml
# This is a comment, using the pound symmbol ( # )
# We can use double quotes,single quotes and  no quotes.
app: "user-authentication"
port: '9000'
version: 1.7
# Objects with key value pairs
objectName:
  key: value

# Use lists in yml file by using a dash ( - ) followed by space
```

There’s another small quirk to YAML. All YAML files (regardless of their association with Ansible or not) can optionally begin with --- and end with .... This is part of the YAML format and indicates the start and end of a document.


The below note about list is from [Ansible](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)
All members of a list are lines beginning at the same indentation level starting with a "- " (a dash and a space):

```yml
---
# A list of tasty fruits
- Apple
- Orange
- Strawberry
- Mango
...
```

## List of objects
```yml
microservices:
  - app: user-authentication
    port: 9000
    version: 1.7
  - app: shopping-cart
    port: 9002
    versions:
      - 1.9 
      - 2.0
      - 2.1
    # The above list does not need indentation
    dependencies:
    - "express"
    - "axios"
    #  It can also literally a list
    versionOtherway: [1.9,2.0,2.1]
```

# Objects
  Indentation of 2 spaces

```yml
--- 
user: 
  age: 21
  name: Samrood
  stack: 
    - 
      microservices: 
        front-end: react
        server: express
    - 
      jamstack: 
        Api: Javascript
        cms: Strapi
        site-generator: Gatsby
  subjects: 
    - "Web"
    - "Dev Ops"
```

# MultiLine strings
<!-- To be continued -->
