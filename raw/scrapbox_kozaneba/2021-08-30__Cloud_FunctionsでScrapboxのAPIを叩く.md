---
title: Cloud FunctionsでScrapboxのAPIを叩く
created: 2021-08-30
updated: 2021-09-01
source: Scrapbox /nishio
in_title: False
---

# Cloud FunctionsでScrapboxのAPIを叩く

Cloud FunctionsでScrapboxのAPIを叩く
from [Kozaneba開発日記2021-08-29]
[Cloud Functions]で[ScrapboxAPI]を叩く

いきさつ:
	KozanebaにScrapboxこざねを追加した。
	各種パラメータを人間が設定するのは面倒
	ページのURLを渡したらScrapboxのAPIを叩いて必要なデータを取ってくるようにしようと考えた。
	しかしScrapboxのAPIのCORS制限により、Kozanebaを開いたブラウザからScrapboxのAPIは直接叩けない。
	そこでCloud Functionsで叩く。

絵で解説
　[https://gyazo.com/2dba023d5006b6d506f526d52bd3b7d2]


やったことメモ
　料金プランのアップグレード
　サーバサイドはPythonに慣れてるけど、ずっとPythonだけなのもなんだし、今回のは難しくないはずだからNode.jsでやるか
　　近いうちに[scrabox-parser]を使いたいし
	[https://cloud.google.com/functions/docs/writing Writing Cloud Functions | Cloud Functions Documentation | Google Cloud]
 エミュレータにFunctionsを追加するのどうするんだ
code::
 $ firebase init emulators
 
 You're about to initialize a Firebase project in this directory:
 
   /Users/nishio/kozaneba
 
 Before we get started, keep in mind:
 
   * You are initializing within an existing Firebase project directory
[https://gyazo.com/78375db9636c8d0659bee712c2839151]
できた
　設定を変更する方法が「init」なのおかしいだろ…

できてない
$ firebase emulators:start --import firebase_emulator_data
[https://gyazo.com/e1be11881d578cfc80150730880d92ce]
>⚠  functions: The functions emulator is configured but there is no functions source directory. Have you run firebase init functions?

$ firebase init functions
code::
 ? What language would you like to use to write Cloud Functions? TypeScript
 ? Do you want to use ESLint to catch probable bugs and enforce style? Yes
 ? Do you want to install dependencies with npm now? Yes

これでできたかな？
$ firebase emulators:start --import firebase_emulator_data
code::
 ⚠  Error: Cannot find module '/Users/nishio/kozaneba/functions/lib/index.js'. Please verify that the package.json has a valid "main" entry ...
 ⚠  We were unable to load your functions code. (see above)
    - It appears your code is written in Typescript, which must be compiled before emulation.
    - You may be able to run "npm run build" in your functions directory to resolve this.
あー、TypeScriptを選択したから先にビルドしろとのことらしい

サンプルコードがあったからコメントアウトを外してビルドしてみる
code:ts
 import * as functions from "firebase-functions";
 
 // // Start writing Firebase Functions
 // // https://firebase.google.com/docs/functions/typescript
 //
 export const helloWorld = functions.https.onRequest((request, response) => {
   functions.logger.info("Hello logs!", { structuredData: true });
   response.send("Hello from Firebase!");
 });

今度こそできた
[https://gyazo.com/3c15fc1641e8e7a27ec20d7ec43212de]
`http://localhost:5001/regroup-d4932/us-central1/helloWorld`にブラウザでアクセスしてレスポンスを見る

`  functions.logger.info("Hello logs!", { structuredData: true });`に関してはエミュレータが走ってるターミナルにこう表示されてた
code::
 i  functions: Beginning execution of "us-central1-helloWorld"
 >  {"structuredData":true,"severity":"INFO","message":"Hello logs!"}
 i  functions: Finished "us-central1-helloWorld" in ~1s

さて、ScrapboxAPIを作る
requestオブジェクトの仕様はどこにあるかな
[https://gyazo.com/7f105c12581a995e808e2796a65301fd]
なるほどExpressと同じか
https://expressjs.com/en/4x/api.html#req

code:ts
 export const get_scrapbox_page = functions.https.onRequest(
   (request, response) => {
     console.log(request.body);
     response.json(request.body);
     // functions.logger.info("Hello logs!", { structuredData: true });
     // response.send("Hello from Firebase!");
   }
 );
code:js
 fetch("http://localhost:5001/regroup-d4932/us-central1/get_scrapbox_page", {
   method: "post",
   body: JSON.stringify({ foo: "bar" }),
 })
   .then((x) => x.json())
   .then((x) => console.log(x));
code:output
 {"foo":"bar"}
よし、やりとり部分はできた。
ではScrapboxのAPIを叩こう。

code:ts
 import fetch from "node-fetch";
 
 export const get_scrapbox_page = functions.https.onRequest(
   (request, response) => {
     const body = JSON.parse(request.body);
     const url = body.url;
     const api_url = url.replace("scrapbox.io/", "scrapbox.io/api/pages/");
     fetch(api_url).then((req) => {
       console.log(req);
       req.text().then((text) => {
         console.log(text);
         response.send(text);
       });
     });
   }
 );
code:js
 fetch("http://localhost:5001/regroup-d4932/us-central1/get_scrapbox_page", {
   method: "post",
   body: JSON.stringify({
     url: "https://scrapbox.io/nishio/2021-08-28Kozaneba%E9%96%8B%E7%99%BA%E6%97%A5%E8%A8%98",
   }),
 })
   .then((x) => x.json())
   .then((x) => console.log(x));
output
[https://gyazo.com/8703a9fcaa4f8fec4b0c26b8443f1a3d]
できた

じゃあアプリからこのAPIを叩こう→CORS
https://cloud.google.com/functions/docs/writing/http#handling_cors_requests
	[https://gyazo.com/c22f02704c9ea44de89383deddd5643c]
なるほど、こうか。
code:ts
 export const get_scrapbox_page = functions.https.onRequest((req, res) => {
   res.set("Access-Control-Allow-Origin", "*");
 
   if (req.method === "OPTIONS") {
     // Send response to OPTIONS requests
     res.set("Access-Control-Allow-Methods", "GET");
     res.set("Access-Control-Allow-Headers", "Content-Type");
     res.set("Access-Control-Max-Age", "3600");
     res.status(204).send("");
     return;
   }
 
   const body = JSON.parse(req.body);
   const url = body.url;
   const api_url = url.replace("scrapbox.io/", "scrapbox.io/api/pages/");
   fetch(api_url).then((req) => {
     req.text().then((text) => {
       res.send(text);
     });
   });
 });
問題なくアプリから叩けるようになった。

できたできた
[https://gyazo.com/9c5646f9b6aae0c45c0cbc4351f97867]
あとはfirebase deployして...
$ firebase deploy
[https://gyazo.com/702e52e97041dee67073fc8713680227]
ESLintが文句を言ってうるさいので無効にする

$ firebase init functions
code::
 ? Do you want to use ESLint to catch probable bugs and enforce style? No

再度デプロイ
$ firebase deploy
code::
 i  functions: creating Node.js 14 function get_scrapbox_page(us-central1)...
 ✔  functions[get_scrapbox_page(us-central1)]: Successful create operation. 
 i  functions: cleaning up build files...
 Function URL (get_scrapbox_page(us-central1)): https://us-central1-regroup-d4932.cloudfunctions.net/get_scrapbox_page
[https://gyazo.com/74cc324e13b6c473331b5535e3f4b57d]
できた

叩くのを`localhost:5001`から`https://us-central1-regroup-d4932.cloudfunctions.net/get_scrapbox_page`にかえる

code:ts
 fetch("https://us-central1-regroup-d4932.cloudfunctions.net/get_scrapbox_page", {
   method: "post",
   body: JSON.stringify({
     url: "https://scrapbox.io/nishio/2021-08-28Kozaneba%E9%96%8B%E7%99%BA%E6%97%A5%E8%A8%98",
   }),
 })
   .then((x) => x.json())
   .then((x) => console.log(x));

できた
[https://gyazo.com/56af245ad9d26f348640d586b1258b98]

