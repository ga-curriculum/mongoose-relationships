# ![Mongoose Relationships - Setup](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to set up a basic Mongoose environment in a Node.js application.

## Setup

Open your Terminal application and navigate to your `~/code/ga/lectures` directory:

```bash
cd ~/code/ga/lectures
```

Make a new directory called `mongoose-relationships`, then enter this directory:

```bash
mkdir mongoose-relationships
cd mongoose-relationships
```

Create a node project along with its `package.json` file by using this command:

```bash
npm init
```

## Install `mongoose` and `dotenv` from NPM

To use mongoose in our application, we'll first need to install the `mongoose` and `dotenv` packages from NPM:

```bash
npm i mongoose dotenv
```

## Add the MongoDB Atlas connection string to your `.env` file

First, create a `.env` file in your project's root directory:

```bash
touch .env
```

This file will be used to store any sensitive, secret information that our application needs to run, but that we don't want to commit to GitHub. Database connection strings definitely qualify: we don't want just anybody to be able to access our database using this string!

`.env` files are a simple list of key-value pairs defined like so:

```plaintext
SECRET_NUMBER=13
PASSWORD=12345
```

This will allow an application to access the `SECRET_NUMBER`, and `PASSWORD` properties on a `process.env` object.

You can now paste your MongoDB Atlas connection string in the app's `.env` file, assigning it to a `MONGODB_URI` environment variable. For example:

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@sei-w0kys.azure.mongodb.net/?retryWrites=true
```
> Do not use the above connection string in your application, it will not work.

It is important that there are no spaces between `MONGODB_URI`, `=`, and your atlas connection string. It should be written as one continuous string with no spaces.

This will make the connection string available in our application on the `process.env.MONGODB_URI` property.

Your connection string will default to a generic unnamed database, as indicated by the /? towards the end of the connection string. However, you must update this to your preferred database name. In this application that will be todos. You can specify the preferred database name by adding it between the / and the ? in the connection string. Here's how that will look for this app: ``/todos?`. That will make the full connection string for this app look something like this:

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@sei-w0kys.azure.mongodb.net/todos?retryWrites=true
```

> Again, do not use the above connection string in your application, it will not work.

Anytime you need to make a new app you can use this same connection string and replace the database name portion of the string. Ensure the name you assign is unique to that project - for example, once we've used the name `todos` for this app, you shouldn't use it again. Also, ensure it doesn't contain any special characters.

## Add a `.gitignore`

Create a `.gitignore` file in your project's root directory:

```bash
touch .gitignore
```

A `.gitignore` file is useful when working with Mongoose and MongoDB, as it prevents sensitive files (like your `.env`) from being committed to GitHub. In this application, the `.env` contains our database connection string. We'll add the `.env` file to our `.gitignore` to prevent it from being committed to version control, avoiding the risk of exposing sensitive information online.

Add the following to your `.gitignore` file:

```plaintext
.env
```

## Defining a model

Let's create that directory and a file to handle the `todos` resource:

```bash
mkdir models
touch models/todo.js
```

Add the following to `models/todo.js`:

```javascript
// models/todo.js
const mongoose = require('mongoose');

const todoSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean,
});

const Todo = mongoose.model('Todo', todoSchema);

module.exports = Todo;
```

## Query file

Create a `queries.js` file. We'll use this file to execute various Mongoose operations:

```bash
touch queries.js
```

Open the contents of the directory in VS Code:

```bash
code .
```

Make sure you have the following in `queries.js`:

```javascript
/*-------------------------------- Starter Code --------------------------------*/

const dotenv = require('dotenv');
dotenv.config();

const mongoose = require('mongoose');
const Todo = require('./models/todo.js');

const connect = async () => {
    await mongoose.connect(process.env.MONGODB_URI);
    console.log('Connected to MongoDB');
    await runQueries()

    await mongoose.disconnect();
    console.log('Disconnected from MongoDB');
    process.exit();
  }
};

connect()

const runQueries = async () => {
  console.log('Queries running.');
  await createTodo();
};

/*-------------------------------- Query Functions --------------------------------*/

const createTodo = async () => {
  const todoData = {
    text: "learn React",
    isComplete: false,
  };
  const todo = await Todo.create(todoData);
  console.log("New todo:", todo);
};

const findTodos = async () => {
  const todos = await Todo.find({});
  console.log("All todos:", todos);
};
```