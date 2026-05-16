---
title: Cloud FunctionsでkintoneのAPIを叩く
created: 2021-09-13
updated: 2021-09-14
source: Scrapbox /nishio
in_title: False
---

# Cloud FunctionsでkintoneのAPIを叩く

Cloud FunctionsでkintoneのAPIを叩く
[https://www.npmjs.com/package/@kintone/rest-api-client @kintone/rest-api-client - npm]
をCloud Functionの中で使おうと思ったのだが、ローカルのEmulatorで試すために
$ npm run build
した段階で下記のエラーになってしまった。

tsconfig.jsonがこうだから
code:tsconfig.json
 {
   "compilerOptions": {
     "module": "commonjs",
     ...

`import { KintoneRestAPIClient } from "@kintone/rest-api-client";`
ではなく
`const { KintoneRestAPIClient } = require("@kintone/rest-api-client");`
が正解。

code:error
 node_modules/@kintone/rest-api-client/lib/http/AxiosClient.d.ts:2:8 - error TS1259: Module '".../kozaneba/functions/node_modules/@kintone/rest-api-client/node_modules/form-data/index"' can only be default-imported using the 'esModuleInterop' flag
 
 2 import FormData from "form-data";
          ~~~~~~~~
 
   node_modules/@kintone/rest-api-client/node_modules/form-data/index.d.ts:10:1
     10 export = FormData;
        ~~~~~~~~~~~~~~~~~~
     This module is declared with using 'export =', and can only be used with a default import when using the 'esModuleInterop' flag.


