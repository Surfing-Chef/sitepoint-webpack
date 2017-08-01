# Sitepoint: A Beginner’s Guide to Webpack 2 and Module Bundling #
>[LINK TO THE TUTORIAL HERE](https://www.sitepoint.com/beginners-guide-to-webpack-2-and-module-bundling/) or download the [FULL DEMO APP FROM GITHUB](https://github.com/sitepoint-editors/webpack-demo)  
[WEBPACK 2](https://webpack.js.org)
[WEBPACK](https://webpack.github.io/)

## Branches: ##
**1.0 - A Basic Project Template**  

***Folder Structure:***
```console
├───project root
│   ├───.git
│   ├───.gitignore
│   ├───index.html
│   ├───package.json
│   ├───README.md
│   ├───webpack.config.js
│   ├───node_modules/
│   ├───dist/
│   └───src/
│       ├───app.js
│       └───CSS/  
│       └───style.scss  


```

- `.git, .gitignore` - indicative of project tracking with Git and GitHub
- `README.md` - a *markdown* file outlining the project - a Git necessity
- `package.json` - *node.js* configuration file - sets up package management for *node.js* and *NPM*
- `/node_modules` - project specific packages defined by `package.json` for use within the project - i.e. *Webpack*, *Gulp*, etc
- `/dist` - processed assets generated with tools such as *Webpack* or *Gulp* - deployment ready
- `/src` - working assets that will be processed for the build. Assets usually consist of files like *SCSS* that require processing to *CSS* or script files that may need transpiling for browser compatibility purposes

**webpack.config.js**
```javascript
1   const webpack = require('webpack')
2   const path = require('path')
3
4   const config = {
5     context: path.resolve(__dirname, 'src'),
6     entry: './app.js',
7     output: {
8       path: path.resolve(__dirname, 'dist'),
9       filename: '[name].bundle.js'
10    },
11    module: {
12      rules: [{
13        test: /\.js$/,
14        include: path.resolve(__dirname, 'src'),
15        use: [{
16          loader: 'babel-loader',
17          options: {
18            presets: [
19              ['es2015', { modules: false }]
20            ]
21          }
22        }]
23      }]
24    }
25  }
26
27  module.exports = config
```
*Note: `__dirname` refers to the directory where this `webpack.config.js` lives*

***Lines 1 and 2*** load *webpack* and *path* packages for use within the config file  

***Lines 4 to 25*** create the configuration object
  >`context`: *The base directory (absolute path!) for resolving the entry option*  
  `entry`: *the entry point for the bundle*  
  `output`: *options affecting the output of the compilation. output options tell Webpack how to write the compiled files to disk. Note, that while there can be multiple entry points, only one output configuration is specified*  
  `module`: options affecting the normal modules  
  `plugins`:  add functionality typically related to bundles in Webpack

  Starting from the `context` folder, it looks for `entry` filenames and reads the content. Every `import` (ES6) or `require()` (Node) dependency it finds as it parses the code, it bundles for the final build. It then searches those dependencies, and those dependencies’ dependencies, until it reaches the very end of the "tree" - only bundling what it needed to, and nothing else. From there, Webpack bundles everything to the `output.path` folder, naming it using the `output.filename` naming template (`[name]` gets replaced with the object key from entry).

  The `module:` determines how the different types of modules within a project will be treated.  In this example:
  - `rule:` defines a `test:` for `js` files using a regular expression to define criteria -  `/\.js$/`
  - `include` defines where the files to test reside
  - `use:` defines a `loader:` and its `options:` including transpiling `presets:`

***Line 19***`{ modules: false }` enables Tree Shaking to remove unused exports from your bundle to bring down the file size        

***Line 27*** exports the configuration object, feeds Webpack the configuration

**SUMMARY:** The `webpack.config.js` file initially instructs Webpack to compile our entry point `src/app.js` into our output `/dist/bundle.js` and all *.js* files will be transpiled from ES2015 to ES5 with Babel.

***package.json***

Four packages are defined for installation as `"devDependencies"` - `webpack` to enable the use of Webpack, plus three for transpiling ES2015 to ES5 with *Babel*: `babel-core`, the *loader* `babel-loader` and the *preset* `babel-preset-es2015` for the flavor of JavaScript we want to write.

If installing this package as is, `npm install` will install the defined packages for project use.  If the `package.json` does not contain any `devDependencies:`, ensure the command line is in the same folder and run the following command `npm install babel-core babel-loader babel-preset-es2015 --save-dev`

The scripts section of `package.json`:
```console
"scripts": {
  "start": "webpack --watch",
  "build": "webpack -p"
},
```

Running `npm start` from the command line will start webpack in watch mode which will recompile the bundle whenever a `.js` file is changed in the `src` directory. The output in the console displays feedback regarding the bundles being created, it’s important to keep an eye on the number of bundles and the size.

Running `npm run build` from the command line will create a production ready (minified, etc) version, to the defined `output:`.

### LINKS ###
[WEBPACK 2: configuration](https://webpack.js.org/configuration/entry-context/)
[Getting Started with webpack 2](https://blog.madewithenvy.com/getting-started-with-webpack-2-ed2b86c68783)
