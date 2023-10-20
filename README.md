### react-boiler-plate
react webpack boiler plate

### Setup React

#### Setup folder with npm and git
```bash
mkdir my-app
cd my-app
npm init
```
At this point, npm will ask you to answer some basic questions about your project and then will create a package.json file
in the root directory.

Optionally you can also set up git for your project by using his command
```bash
git init .
```
#### Create HTML and Javascript (React) file

Start by creating a new source folder
```bash
mkdir src
cd src
```
Now let’s add an HTML and javascript file (if not using typescript)
```bash
touch index.html
touch index.js
touch App.js
```
If Using TypeScript (optional)
```bash
touch index.html
touch index.tsx
touch App.tsx
```

First, we need to install react and react-dom
```bash
npm install react react-dom react-router-dom
```
If Using TypeScript (optional)

TypeScript is a popular way to add type definitions to JavaScript codebases. Out of the box, TypeScript supports JSX and you can get full React Web support by adding @types/react and @types/react-dom to your project.
```bash
npm install @types/react @types/react-dom @types/node
```

#### Setup webpack

First, we need to install webpack to our project.
```bash
npm install webpack webpack-cli webpack-dev-server --save-dev
```

This install 3 packages main webpack package, webpack-cli to run webpack commands and webpack-dev-server to run react locally.


webpack to inject bundled javascript file as a script tag to the HTML file, need to install a webpack plugin.
```bash
npm install html-webpack-plugin --save-dev
```

Install loaders packages:
```bash
npm i sass sass-loader css-loader style-loader url-loader file-loader --save-dev
```

If Using TypeScript (optional)
```bash
npm i ts-loader --save-dev
```

#### Setup Babel


Babel is a transpiler so we need to tell it what to transpile, we do this using presets. These are predefined
configuration that is used to transpile different type to javascript to browsers understandable one.

```bash
npm install @babel/core babel-loader --save-dev
```
Let’s install these presets first.
```bash
npm install @babel/preset-env @babel/preset-react --save-dev
```
Here we are installing @babel/core which is the core transpiler. Then we have babel-loader which is a webpack loader that will
help webpack to use babel transpiler. Learn more about @babel/core and babel-loader.

Configure babelrc
Create the file on the project's root directory

```bash
touch .bablerc
```
Add this line of code

    {
      "presets": ["@babel/preset-env", "@babel/preset-react"]
    }

###  Setup webpack configuration file

Once we install the webpack and babel, it's time to configure it. We can do that by adding webpack configuration file to the root folder.
```bash
touch webpack.config.js
```

webpack.config.js

      const path = require('path');
      const HtmlWebpackPlugin = require('html-webpack-plugin');
      
      module.exports = {
          entry: path.join(__dirname, "src", "index.js"),
          // entry: path.join(__dirname, "src", "index.tsx"),
          output: {
              path: path.resolve(__dirname, "dist"),
              filename: "bundle.js",
              publicPath: '/'
          },
          devServer: {
              port: process.env.PORT || 3000,
              historyApiFallback: true,
          },
          module: {
              rules: [
                  {
                      test: /\.(js|jsx)$/,
                      exclude: /node_modules/,
                      use: {
                          loader: "babel-loader",
                          options: {
                              presets: ['@babel/preset-env', '@babel/preset-react']
                          }
                      }
                  },
                  {
                      test: /\.(sa|sc|c)ss$/, // styles files
                      use: ["style-loader", "css-loader", "sass-loader"],
                  },
                  {
                      test: /\.(svg)$/, // to import images and fonts
                      loader: "url-loader",
                      options: { limit: false },
                  },
                  {
                      test: /\.(jpg|jpeg|png|gif|svg)$/i,
                      loader: "file-loader",
                  },
              ]
          },
          plugins: [
              new HtmlWebpackPlugin({
                  template: path.join(__dirname, "src", "index.html"),
              }),
        ],
      }


#### Scripts to build run test

Now that we have created the config file it is time to actually build and run the app.
To do that we need to add some scripts to package.json.

    "scripts": {
      "start": "webpack serve --mode development",
      "build": "webpack",
      "test": "jest"
    }

dev will use the webpack dev server and run the application locally.
build will create a bundle of assets that can be deployed to servers.

#### Run application

Copy sample env and update required configs in .env
```bash
cp .env.sample .env
```
Run locally
```bash
npm run start:dev
```
Build bundle
```bash
npm run build
```
