```
npm config set strict-ssl false
npm config set registry "http://registry.npmjs.org/"
npm --proxy http://username:password@proxy:port install express

npx create-react-app my-app
cd my-app
npm start
npm audit fix
npm config get prefix

npm install -g nodemon
nodemon filename.js
npm install --save-dev nodemon
// change package.json file as:
"scripts": {
    "serve": "nodemon index.js"
}
npm run serve
```
### Package
```
npm install grunt nodemon --save-dev
grunt
```
npm config set strict-ssl false
npm config set registry "http://registry.npmjs.org/"
npm --proxy http://username:password@proxy:port install express
npm --proxy http://webproxy.website.com:8080 install express