Run: $  npm init -y
Run: $  npm install react --save
Run: $  npm install react-dom --save
Run: $  npm install webpack --save
Run: $  npm install webpack-dev-server --save
Run: $  npm install webpack-cli --save
Run: $  npm install babel-core --save-dev
Run: $  npm install babel-loader --save-dev
Run: $  npm install babel-preset-dev --save-dev
Run: $  npm install babel-preset-react --save-dev
Run: $  npm install html-webpack-plugin --save-dev

Run: $  type null > index.html
Run: $  type null > App.js
Run: $  type null > main.js
Run: $  type null > webpack.config.js
Run: $  type null > .babelrc

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