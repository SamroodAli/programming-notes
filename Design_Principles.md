# DESIGN PATTERNS

#### Solid Design Principles - by Robert C Martin.
### Single Responsibility Principle

This principle states that a single Class/Function/Component should handle a single responsibility.
Lets save we have a journal that handles entries and then persists the data using file operations.
Following the Single Responsibiltiy principle, we should seperate the file persistence concerns from the journal entry concerns
```js
// class Jounral only handles the responsibility related to the journal and it's entries
const fs = require('fs')

class Journal
{
  constructor(){
    this.entries = {}
    this.count =  0
  }
  addEntry(text){
    let c = ++this.count;
    let entry = `${c}: ${text}`;
    this.entries[c] = entry;
    return c
  }
  toString(){
    return Object.values(this.entries).join("\n")
  }

  saveToFile(filename){ //this is bad and should be seperated as per single responsibility principle
    fs.writeFileSync(filename,this.toString())
  }
}

class PersistenceManager
{
  saveToFile(journal,filename){
    fs.writeFileSync(filename,journal.toString()) //so you can use other objects which needs to be written to files too
  }

  loadFromFile(){} // also easy to add and debug all concerns related to file persistence
}
```
#### Anti-pattern - God Object
Anti patterns are opposite principles. Often considered bad.
There is an oppposite anti pattern to single responsibility principle known as the `God object` which is a single object handling all the responsibilities. This is a bad principle in most cases.

There is the related principle `Seperation of concerns` where you have different concerns. Seperate them into differnet components
