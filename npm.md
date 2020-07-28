npm config set strict-ssl false
npm config set registry "http://registry.npmjs.org/"
npm --proxy http://username:password@proxy:port install express

npx create-react-app my-app
cd my-app
npm start
npm audit fix
npm config get prefix

Run: $ npm install -g nodemon
Run: $ nodemon filename.js
Run: $ npm install --save-dev nodemon
Change package.json file as:
"scripts": {
    "serve": "nodemon index.js"
}
Run: $ npm run serve