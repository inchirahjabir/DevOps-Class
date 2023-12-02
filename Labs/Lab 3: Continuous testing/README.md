# Continuous Testing

This lab focuses on continuous testing, with objectives such as using a prepared User API application, running  tests and implementing the GET user functionality through test-driven development (TDD).

## Functionality

1. Use prepared User API application and run tests
2. Using test-driven development (TDD) create GET user functionality

## Installation

1. [Install Redis](https://redis.io/download)

2. Install an **IDE or a text editor**, for example, [Atom](https://atom.io/) or [VS Code](https://code.visualstudio.com/)
   
3. [Install NodeJS](https://nodejs.org/en/download/)
   
Install application:

```
npm install
```

Run tests:

```
npm test
```

Start application:

```
npm start
```


## Usage

Create a REST API GET `user` method that retrieves user information from the database.

1) Create `get` user controller:   
  - Create **2 unit tests** (in the file `lab/test/user.controller.js`):
    - get a user by username
    - cannot get a user when it does not exist
  - Create **the controller method** (in the file `lab/src/controllers/user.js`)

2) Create GET user REST API method:   
  - Create **2 API tests** (in the file `lab/test/user.router.js`):
    - successfully get user
    - cannot get a user when it does not exist
  - Create **GET user route** (in the file `lab/src/routes/user.js`)
