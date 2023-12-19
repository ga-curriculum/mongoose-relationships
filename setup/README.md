# ![Mongoose Relationships - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to set up a basic Mongoose environment in a Node.js application.

## Setup

Open your Terminal application and navigate to your `~/code/ga/lectures/intro-to-mongoose` directory:
```bash
cd ~/code/ga/lectures/intro-to-mongoose
```

From there, navigate to the `intro-to-mongoose` directory:

```bash
cd intro-to-mongoose
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
  try {
    await mongoose.connect(process.env.MONGODB_URI);
    console.log('Connected to MongoDB');
    await runQueries()
  } catch (err) {
    console.error(err);
  } finally {
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
  const todos = await Todo.find({ });
  console.log("All todos:", todos);
};

```