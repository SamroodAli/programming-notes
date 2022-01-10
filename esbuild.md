# Esbuild with webassembly

1. Install and import the library
```bash
npm install esbuild-wasm
```
import * as esbuild from "esbuild-wasm"

2. Copy the wasm file from node_modules to react public folder or somewhere accessible on the web

3. Start the service

esbuild.startService({
    worker: true,
    wasmURL:"/esbuild.wasm"
 })


4. Get code to esbuild transform api

esbuild.transform(codeAsString,{
    loader:"jsx",
    target:"es2015"
})
