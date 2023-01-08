React-Webkit-Template

Webkit Set Up

npm init -y
npm install react react-dom
npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader

Create .babelrc and add the following code:
{  
    "presets": [
        "@babel/preset-react",
        "@babel/preset-env"
    ]
}

npm install --save-dev webpack webpack-cli webpack-dev-server
npm install --save-dev html-webpack-plugin style-loader css-loader file-loader

Create weback.config.js and add the following code:
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.[hash].js',
        path: path.resolve(__dirname, 'dist'),
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html',
        }),
    ],
    resolve: {
        modules: [__dirname, 'src', 'node_modules'],
        extensions: ['*', '.js', '.jsx', '.tsx', '.ts'],
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                exclude: /node_modules/,
                loader: require.resolve('babel-loader'),
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
            {
                test: /\.png|svg|jpg|gif$/,
                use: ['file-loader'],
            },
        ],
    },
};

Create src folder
Create src/App.js file and add the following code:
import React from "react";

const App = () => (
    <div>
        <h1>Hello React</h1>
    </div>
);

export default App;

Create src/index.js and add the following code:
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App/>,document.querySelector("#root"));

Create src/index.html and add the following code:

<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>React</title>
    </head>
    <body>
        <div id="root"></div>
    </body>
</html>

In package.json add the following scripts:
"scripts": {
    "start": "webpack serve  --hot --open",
    "build": "webpack --config webpack.config.js --mode production"
}

Linter & Prettier Setup

npm install eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-node eslint-config-node @babel/eslint-parser --save-dev
npx install-peerdeps --dev eslint-config-airbnb
npm install precise-commits --save-dev

Create .prettierrc and add the following code:
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": true,
    "singleQuote": true
}

Create .prettierignore and add the following:
# README
README.md 

# Node modules and packages
node_modules
package-lock.json

# HTML files
*.html

Create .eslintrc.js and add the following code:
module.exports = {
    extends: ['airbnb', 'prettier', 'plugin:node/recommended'],
    plugins: ['prettier'],
    parser: '@babel/eslint-parser',
    parserOptions: {
        ecmaVersion: 2016,
        sourceType: 'module',
        ecmaFeatures: {
            jsx: true,
        },
    },
    env: {
        es6: true,
        browser: true,
        node: true,
    },
    rules: {
        'prettier/prettier': 'error',
        'no-unused-vars': 'warn',
        'no-console': 'off',
        'func-names': 'off',
        'no-process-exit': 'off',
        'object-shorthand': 'off',
        'class-methods-use-this': 'off',
        'react/jsx-filename-extension': [1, { extensions: ['.js', '.jsx'] }],
        'react/function-component-definition': [
            2,
            {
                namedComponents: 'arrow-function',
                unnamedComponents: 'arrow-function',
            },
        ],
        'react/propTypes': 0,
    },
};

In package.json add the following scripts:
"scripts": {
    "format": "prettier --write \"**/*.{js,jsx,ts,tsx,json,css,scss,md}\"",
    "precise-commits": "precise-commits",
    "lint-staged": "lint-staged",
    "lint-prepush": "lint-prepush",
}

Create .eslintignore and add the following:
# Node modules
node_modules

# Artifacts
public
build

# Env files
*.env

npm install husky lint-staged lint-prepush --save-dev

In package.json add the following:
"lint-staged": {
    "**/*.{js,jsx}": [
        "npm run lint",
        "prettier --write"
    ]
},
"lint-prepush": {
    "base": "develop",
    "tasks": {
        "**/*.{js,jsx,ts,tsx}": [
            "eslint"
        ]
    }
}

npx husky-init

Change .husky/pre-commit to the following:
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run precise-commits && npm run format && npm run lint-staged

Create .husky/pre-push and add the following:
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run lint-prepush