# ![Mongoose Relationships - Embedding](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## What is Embedding?

Embedding is the practice of storing related data within a single document. This is achieved by nesting *sub-documents* or arrays of sub-documents inside a parent document.

A subdocument is a document nested within another document. Subdocuments are very similar to regular documents. The difference is that they are not saved directly - but instead when the document they are embedded within is saved. 

## Why use Embedding

Embedding connected data in a single document can reduce the number of read operations required to obtain data. In general, you should structure your schema so your application receives all of its required information in a single read operation.

## Embedding sub-documents



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

> Subdocuments have their own **schema**, but since subdocuments are not saved to a collection, we **do not compile a subdocument’s schema into a Model**.

Great, we now have two schemas. The next step is to give them a relationship, as they are currently not associated at all. 

To do so, we nest our `subTaskSchema` in our `todoSchema`.

Mongoose represents an array of subdocuments as an embedded schema wrapped in square brackets (`[schema]`). This indicates that subTasks will be an array whose elements conform to the `subTaskSchema`. 

```js
const mongoose = require('mongoose');

const subTaskSchema = mongoose.Schema({
  text: String,
  priorityLevel: String,
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

Add the following to bottom of `server.js`, under our existing query files: 

```js
const createSubTask = async () => {
  // Assume that the todo we want to create a
  // sub-task for has the following id: 
  const id = '6573745144784f6dc034e1df'
  // Look up the todo by id, assign the returned object to `todo`
  const todo = await Todo.findById(id);

  const subTaskData = {
    text: "Learn how props work",
    priorityLevel: "High",
    isComplete: false,
  };

  // Push the new sub-task data into the subTasks array on the todo:
  const subTask = todo.subTasks.push(subTaskData);
  // Save the parent document: 
  await todo.save();
};
```

[tktk how does hunter want to do this]
To find a specific sub-task, we can use the built in `.id` method that mongoose arrays have: 

```js
const findTodos = async () => {
  const todos = await Todo.find({ });
  console.log("All todos:", todos);
};
```


