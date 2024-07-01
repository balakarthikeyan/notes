npm init -y
npm install react --save
npm install react-dom --save
npm install webpack --save
npm install webpack-dev-server --save
npm install webpack-cli --save
npm install babel-core --save-dev
npm install babel-loader --save-dev
npm install babel-preset-dev --save-dev
npm install babel-preset-react --save-dev
npm install html-webpack-plugin --save-dev

type null > index.html
type null > App.js
type null > main.js
type null > webpack.config.js
type null > .babelrc

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
 
module.exports = {
   entry: './main.js',
   output: {
      path: path.join(__dirname, '/bundle'),
      filename: 'index_bundle.js'
   },
   devServer: {
      inline: true,
      port: 8080
   },
   module: {
      rules: [
         {
            test: /.jsx?$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            query: {
               presets: ['es2015', 'react']
            }
         }
      ]
   },
   plugins:[
      new HtmlWebpackPlugin({
         template: './index.html'
      })
   ]
}