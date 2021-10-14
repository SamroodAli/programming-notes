# Redis: A in memory data store

Example from docker course
```js
1   const express = require('express')
  1 const redis = require('redis')
  2 
  3 const app = express()
  4 const client = redis.createClient()
  5 
  6 
  7 client.on("error", function(error) {
  8   console.error(error);
  9 });
 10 
 11 client.set('visits',0)
 12 
 13 
 14 app.get("/",(req,res)=>{
 15   client.get('visits',(err,visits)=>{
 16     if(!err){
 17       res.send(`Number of visits is ${visits}`)
 18       client.set('visits',parseInt(visits) + 1)
 19     }
 20   })
 21 })
 22 
 23 const PORT = 3000
 24 app.listen(PORT,()=>{
 25   console.log(`Node running on port ${PORT}`)
 26 })

```
