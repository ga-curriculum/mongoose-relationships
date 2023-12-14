# ![Mongoose Relationships - Embedding](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## What is Embedding?

Embedding connected data in a single document can reduce the number of read operations required to obtain data. In general, you should structure your schema so your application receives all of its required information in a single read operation.

## Why use Embedding



## Creating an embedded document

To create an embedded subdocument, first we'll need to create the schema for the document. As 

Let's say we want to let users create sub-tasks under each Todo. For instance, if a user created a Todo called "Pack for Vacation", they could have associated sub-tasks like "Toothbrush", "Swimsuit", and "Laptop". 

Since this is a one-to-many relationship (each Todo can have multiple subtasks, but each subtask will only belong to a single Todo), it makes sense to embed sub-tasks inside of Todo. This will help reduce the number of read operations required to obtain our data. 

First, let's create a new schema for our sub-tasks. They will need a `text` field and an `isComplete` field: 

```js
const mongoose = require('mongoose');

const subTaskSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean
})

const todoSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean,
});

const Todo = mongoose.model('Todo', todoSchema);

module.exports = Todo;
```

Great, we now have two schemas. The next step is to give them a relationship, as they are currently not associated at all. 

To do so, we nest our `subTaskSchema` in our `todoSchema`.

Mongoose represents an array of subdocuments as an embedded schema wrapped in square brackets (`[schema]`). This indicates that subTasks will be an array whose elements conform to the `subTaskSchema`. 

```js
const mongoose = require('mongoose');

const subTaskSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean
})

const todoSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean,
  subTasks: [subTaskSchema]
});

const Todo = mongoose.model('Todo', todoSchema);

module.exports = Todo;
```

> As with normal JavaScript conventions, the field name for this array should be a pluralized version of the elements it contains. In this case, it makes sense to refer to an array full of individual subTask elements as `subTasks`. 

## Adding a subtask

```js
/*-------------------------------- Query Functions --------------------------------*/

const Todo = require('./models/todo.js');

const createTodo = async () => {
  const todoData = {
    text: "learn React",
    isComplete: false,
  };
  const todo = await Todo.create(todoData);
  console.log("New todo:", todo);
};

const createSubTask = async () => {
  // Assume that the todo we want to create a
  // sub-task for has the following id: 
  const id = '6573745144784f6dc034e1df'
  const todo = await Todo.findById(id);

  const subTaskData = {
    text: "learn how props work",
    isComplete: false,
  };

  const subTask = todo.push(subTaskData);
  await todo.save();
}

const findTodos = async () => {
  const todos = await Todo.find({ });
  console.log("All todos:", todos);
};
```

-> function to add subtask
-> find to find it

