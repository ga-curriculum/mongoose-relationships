# ![Mongoose Relationships - Level Up - Adding References](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to update documents with references in Mongoose.

## Forming relationships through referencing

Our `todoSchema` includes an `assignee` field, which is a reference to a `User`:

```javascript
// models/todo.js
const todoSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean,
  assignee: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
});
```

This field allows us to link documents in separate collections. The schema is configured correctly, but forming a relationship between `Todo` and  `User` documents will require an additional operation. This can be accomplished by updating a `Todo` document to include the `ObjectId` of a `User` in the `assignee` field. It's this step that establishes the direct connection between two documents.

## Creating a new user

To demonstrate this, we'll need to add a new user to our database.

Import the `User` model in `queries.js`:

```javascript
const User  = require('./models/user.js')
```

Add the following function to `queries.js`:

```javascript
const createUser = async () => {
  const userData = {
    name: "Alex",
    email: "alex@mail.com",
  };
  const user = await User.create(userData);
  console.log("New user:", user);
};
```

Next, call upon `createUser` within the `runQueries` function:

```javascript
const runQueries = async () => {
  console.log('Queries running.');
  await createUser();
};
```

Run the `queries.js` file with the following command:

```bash
node queries.js
```

You should see the following output in your terminal:

```plaintext
New user: {
  name: 'Alex',
  email: 'alex@mail.com',
  _id: new ObjectId('6581d503b8b46a5314295d66'),
  __v: 0
}
```

## Add a reference

With our user added, let's build a function to form the relationship between a `todo` and a `user`.

```javascript
const assignTodo = async () => {
  const todoId = '657b25adc8146427465857d7';
  const userId = '6581d503b8b46a5314295d66';

  const updatedTodo = await Todo.findByIdAndUpdate(
    todoId,
    { assignee: userId },
    { new: true }
  );

  console.log('Updated document:', updatedTodo);
};
```

> 🚨 Make sure that you replace `todoId` and `userId` with actual `ObjectIds` from your database.

Next, call upon `assignTodo` within the `runQueries` function:

```javascript
const runQueries = async () => {
  console.log('Queries running.');
  await assignTodo();
};
```

Run the `queries.js` file with the following command:

```bash
node queries.js
```

You should see the following output in your terminal:

```plaintext
Updated document: {
  _id: new ObjectId('657b25adc8146427465857d7'),
  text: 'Learn React',
  isComplete: false,
  __v: 6,
  subTasks: [],
  assignee: new ObjectId('6581d503b8b46a5314295d66')
}
```

In the example above, the `ObjectId` of a `user` document has been added to the `assignee` property of a `todo` document. When the `todo` document needs information about the assigned user, the appropriate document can be retrieved with this `ObjectId`.