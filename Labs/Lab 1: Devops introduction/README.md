# Introduction

This lab aims to implement DevOps practices by using Node.js, initializing a Node.js package, building a web application with Express, and producing meaningful documentation. The result of this lab is the creation of a documented Node.js project featuring a basic web server that displays a "Hello world!" message on its home page.

## Functionalities

1. Start a project
2. Initialize a Node.js package
3. Create a Node.js script
4. Create a web application using the Express package
5. Create a `CHANGELOG.md` file
6. Describe the project in a `README.md` file

## Installation Instructions

1. Install an **IDE or a text editor**, for example, [Atom](https://atom.io/) or [VS Code](https://code.visualstudio.com/).
2. Install **Git** 
3. Install **Node.js**: https://nodejs.org/
4. Open a command-line interface


## Initialize a Node.js package

1. We initialize a Node.js package running this command:
   
```
npm init -y
```

2. We run :
   
```
npm test
```

## Create a Node.js script

1. We create a file index.js with the following content:
   
```
str = "Hello Node.js!"
console.log(str)
```

2. We run the Node.js script in the terminal:
   
```
node index.js
```

3. We run the npm script with the command: 

```
npm start
```
## Create a web application using Express package

1. We install the Express package :

```
npm install express
```

2. We add this command  to  package.json:
   
```
"dependencies": {
    "express": "^4.17.1"
  }
```

3. We modify the index.js file so it can be accessed online.
   
4. Then we run the npm start script: 

```
npm start
```

