# ![Mongoose Relationships - Embedding](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will understand how to embed subdocuments in Mongoose.

## What is embedding?

Embedding is the practice of storing related data within a single document. This is achieved by nesting *sub-documents* or arrays of sub-documents inside a parent document.

Here’s an example of what a document with embedded data looks like (assume a document from a people collection):

```shell
{
  _id: ObjectId("5099803df3f4948bd2e983a4"),
  name: "Joe Smith",
  contacts: [
    {
      _id: ObjectId("5099803df3f4948bd2e98fff"),
      type: "mobile",
      contact: "(555) 555-5555"
    },
    {
      _id: ObjectId("5099803df3f4948bd2e99005"),
      type: "email",
      contact: "joe@smith.com"
    }
  ]
}
```

> 💡 A sub-document is a document nested within another document. Sub-documents are very similar to regular documents - the difference is that they are not saved directly, but instead when the document they are embedded within is saved. 

## Why use embedding

Embedding connected data in a single document can reduce the number of read operations required to obtain data. In general, you should structure your schema so your application receives all of its required information in a single read operation.

## Embedding sub-documents

Let's say we want to let users create sub-tasks under each `Todo`. For instance, if a user created a `Todo` called "Pack for Vacation", they could have associated sub-tasks like "Toothbrush", "Swimsuit", and "Laptop". 

Since this is a one-to-many relationship (each `Todo` can have multiple subtasks, but each subtask will only belong to a single `Todo`), it makes sense to embed sub-tasks inside of `Todo`. This will help reduce the number of read operations required to obtain our data. 

First, let's create a new schema for our sub-tasks. They will need a `text` field and an `isComplete` field: 

```js
const mongoose = require('mongoose');

const subtaskSchema = mongoose.Schema({
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

> 💡 Sub-documents have their own **schema**, but since sub-documents are not saved to a collection, we **do not compile a sub-document’s schema into a Model**.

Great, we now have two schemas. The next step is to give them a relationship, as they are currently not associated at all. 

To do so, we nest our `subtaskSchema` in our `todoSchema`.

Mongoose represents an array of sub-documents as an embedded schema wrapped in square brackets (`[schema]`). This indicates that subTasks will be an array whose elements conform to the `subtaskSchema`. 

```js
const mongoose = require('mongoose');

const subtaskSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean
})

const todoSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean,
  subtasks: [subtaskSchema]
});

const Todo = mongoose.model('Todo', todoSchema);

module.exports = Todo;
```

> 🏆 As with normal JavaScript conventions, the field name for this array should be a pluralized version of the elements it contains. In this case, it makes sense to refer to an array full of individual subtask elements as `subtasks`. 

## Adding a `subtask`

Add the following to bottom of `server.js`, under our existing query files: 

```js
const createSubtask = async () => {
  // Assume that the todo we want to create a
  // sub-task for has the following id: 
  const todoId = '657b25adc8146427465857d7'
  // Look up the todo by id, assign the returned object to `todo`
  const todo = await Todo.findById(todoId);

  const subtaskData = {
    text: "Learn how props work",
    isComplete: false,
  };

  // Push the new sub-task data into the subTasks array on the todo:
  const subtask = todo.subtasks.push(subtaskData);
  // Save the parent document: 
  await todo.save();
  console.log('Modified todo:', todo);
};
```

Call the `createSubtask` function and review the `todo` in the console.log. You should see an embedded sub-document in the `todo.subtask` array.

```plaintext
Modified todo: {
  _id: new ObjectId('657b25adc8146427465857d7'),
  text: 'Learn React',
  isComplete: false,
  __v: 1,
  subtasks: [
    {
      text: 'Learn how props work',
      isComplete: false,
      _id: new ObjectId('6580677cf30bfef697ae046f')
    }
  ]
}
```