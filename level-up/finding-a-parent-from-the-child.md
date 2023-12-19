# ![Mongoose Relationships - Level Up - Finding a Parent From the Child](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to locate a parent document based on the properties of a child document using Mongoose.

## Finding a parent from a child

Imagine you need to find a parent document, but your starting point is a property held within one of its child documents. Thankfully, Mongoose query methods are sophisticated enough to filter results based on properties of subdocuments.

Let's see an example using the `findOne()` method:

```javascript
const todo = await Todo.findOne({
  'subtasks.text': 'Learn how props work'
})
```

Notice the syntax used. We specify a property (`text`) of an object within the `subtasks` array, and the value we're searching for. This operation returns the parent `todo` that contains a `subtask` that matches this criteria.

With access to the parent `todo`, we can now use a JavaScript array method like `find()` to access the appropriate `subtask`:

```javascript
const todo = await Todo.findOne({
  'subtasks.text': 'Learn how props work'
})

const subtask = todo.subtasks.find((subtask) => {
  return subtask.text === 'Learn how props work'
})
```

## Real-world scenario

Consider a an application where a user can see a list of all the subtasks in the database. The user wants to delete a specific subtask, but each document has been removed from the context of its parent. To achieve this functionality, we need to first find the correct parent document, and then find and remove the appropriate subtask within.

Let's build out the relevant pieces now. Add the following to `queries.js`:

```javascript
const findParentAndRemoveSubtask = async () => {
  const todo = await Todo.findOne({
    'subtasks.text': 'Learn how props work'
  })

  const subtask = todo.subtasks.find((subTask) => {
    return subtask.text === 'Learn how props work'
  })

  subtask.deleteOne()

  await todo.save();
  console.log('Updated todo:', todo);
}
```

> 💡 The `deleteOne()` method is accessible on each subdocument.

Next, call upon `findParentAndRemoveSubtask` in the `runQueries` function:

```javascript
const runQueries = async () => {
  console.log('Queries running.');
  await findParentAndRemoveSubTask()
};
```

Finally, run the `queries.js` file with the following command:

```bash
node queries.js
```

You should see that the relevant `subtask` has been removed from the `subtasks` array.