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
`babel/core & babel-loader:` the interface between babel and webpack. It allows them to work with each other to produce the final bundle.
`babel/preset-env:` a preset responsible for transpiling ES6 (or above) to ES5.
`babel/preset-react:` a present responsible for compiling JSX to regular JS. It is possible to forego installing this dependency, but then we will not be able to write our React components using JSX.
npm install html-webpack-plugin html-loader --save-dev
`html-webpack-plugin` will generate the HTML from the React components we're about to write.
`html-loader` exports the HTML as string and can minimize it
npm install clean-webpack-plugin --save-dev
`CleanWebpackPlugin` is good practice to clean up the /dist folder before each build in order to ensure the proper output files are being used. 
npm install css-loader style-loader --save-dev
npm install node-sass  --save
`css-loader` is a webpack plugin that interprets and resolves syntax like @import or url() that are used to include .scss files in components.
`style-loader` is a webpack plugin that injects the compiled css file in the DOM.
`node-sass` is a Node.js library that binds to a popular stylesheet pre-processor called LibSass . It lets us natively compile .scss files to css in a node environment.
`sass-loader` is a webpack plugin that will allow us to use Sass in our project.

npm install webpack-merge --save-dev
`merge` with each other at compile time to render the application

npm install -D mini-css-extract-plugin 
The plugin lets you minimise the CSS file in webpack build.

type null > index.html
type null > App.js
type null > main.js
type null > webpack.config.js
type null > .babelrc
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
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
    devServer: {
        contentBase: './dist',
        hot: true
    },
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
                query: {
                    presets: ['es2015', 'react']
                }
            },
            {
                test: /\.html$/,
                use: [
                    {
                        loader: "html-loader"
                    }
                ]
            },
            {
                test: /\.s[ac]ss$/i,
                use: [
                    // Creates `style` nodes from JS strings
                    'style-loader',
                    // Translates CSS into CommonJS
                    'css-loader',
                    // Compiles Sass to CSS
                    'sass-loader',
                ]
            }
        ]
    },
    plugins: [
      new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            template: './src/index.html',
            filename: "./index.html"
        })
    ]
}
```
Explanation:

`devServer` contains config rules for the server instance we will run to host our application using dev-server. hot: true enables hot module replacement.
`pipe` all files with an extension of .js or .jsx through babel-loader, with the exception of files inside node_modules directory.
`use` the html plugin and loader we installed in the previous step to generate HTML from React components and the front end packaged code bundle and inject the bundle in a <script/> tag in the HTML.

touch .babelrc
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}

package.json
npm install webpack-dev-server --save-dev

  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production",
    "start": "webpack-dev-server --open --mode development"
  },

// to install any package
npm install react react-dom
React should be installed as a regular dependency and not as devDependencies

webpack.production.js --- to contain production env settings
webpack.development.js --- to contain development env settings

new HtmlWebpackPlugin({
    // hashes the file to prevent bad caching
  hash: true,
  minify: {
    removeComments: true,
    collapseWhitespace: true
  },
  chunks: ["vendorCSS", "infoPageCSS", "common", "vendor"],
  filename: "about.html",
  template: "!!ejs-compiled-loader!./views/about.ejs",

  // exclude worthless JS files
    excludeAssets: [/vendorCSS.*.js/, /resultsCSS.*.js/, /infoPageCSS.*.js/],

})

function customHtmlWebpackPlugin(specificOptions) {
let defaults = {

};
// cool ES6 spread operator add the default options with custom object passed as parameter
return new HtmlWebpackPlugin({ ...defaults, ...specificOptions });
}

customHtmlWebpackPlugin({
    filename: "about.html",
    template: "!!ejs-compiled-loader!./views/about.ejs",
    chunks: ["vendorCSS", "infoPageCSS", "common", "vendor"]
}),

hash: adds a hash to the filename for cache-busting
minify: takes an object of html-minifier’s options. I wanted to remove comments and collapse whitespace. More on that here.
chunks: the chunks created by Webpack that should get injected to the final HTML file
filename: the HTML filename to be created
template: the syntax to use ejs-compiled-loader to compile the EJS file into an HTML file, to then use that as the template for HtmlWebpackPlugin. Yep, you read that right. A template for a template!