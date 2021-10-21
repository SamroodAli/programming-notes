```js
var all_files = {
  "./src/message.js": (module) => {
    module.exports = "HI there";
  },
};
var cache = {};

function require(moduleId) {
  if (cache[moduleId]) {
    return cache[moduleId].exports;
  }

  var module = (cache[moduleId] = {
    exports: {},
  });
  all_files[moduleId](module, module.exports, require);
}

function index() {
  const message = require("./src/message.js");
  console.log(message);
}

index();
```
