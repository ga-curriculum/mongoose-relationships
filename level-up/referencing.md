# ![Mongoose Relationships - Level Up - Referencing](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will understand the concept of referencing in MongoDB.

## What is referencing?

Referencing is the an alternate approach to establishing relationships between data entities in MongoDB. It is the practice of establishing relationships between data entities by storing a **reference** (in the form of an `ObjectId`) to a related document within another document, rather than embedding the related data directly

Referencing differs from embedding in a few key ways. With embedding, a parent document contains the related sub-documents. Whereas with **referencing**, a document only contains the related document’s `ObjectId`. Whenever a document requires information on the document that it references, it can utilize the referenced `ObjectId` to locate the appropriate document. 

# ![tktk Hunter - example of referencing(erd or documents)](./assets/referencing.png)

> 💡 Both embedding and referencing can be used simultaneously in the same document.
>

## Deciding which document should hold the reference

When referencing data in MongoDB using Mongoose, you have the ability to store the reference (`ObjectId`) in either of the two related documents, or even both. The decision depends upon the design and functionality of your application, and it’s not always clear-cut.

In typical **one-to-many relationships**, it's common for the **many** side (`Todo`) to hold the reference to the **one** side (`User`). This approach is practical because it's more efficient to store a single reference in many documents, rather than storing countless references in a single document.

However, certain scenarios might call for the opposite approach, where the `User` document holds an array of references to the associated `Todo` documents. For example, if your application frequently retrieves a user along with all their assigned tasks, this inverse referencing could be beneficial. Mongoose is versatile enough to support both approaches.


## Why choose referencing over embedding?

While embedding can be more efficient for read operations, referencing has its advantages:

- If the amount of data stored can exceed the 16MB size limit for a document. Exceeding this size is quite uncommon though - all Shakespeare’s works together are fewer than 5 megabytes!
- When multiple parent documents need access to the same child document and that child’s data changes frequently. For example, a document modeling a *bank account* should be referenced because it could be “owned” by more than one individual. Suppose the account data were embedded in two or more parent documents. Can you imagine how difficult it would be to keep the transactional & balance data in sync?
- If it makes sense for your application. For example, if you wanted to view all *posts* on your landing page, regardless of the user who posted them, it would take more effort to extract the *posts* from each user if they were embedded. However, it would be quick to get the *posts* from their own collection.

## Implementing references

Let's implement an example of referencing using two models: `Todo` and `User`. Our goal is to build a **one-to-many (1:M) relationship** between the two.

First, we'll update the `todoSchema`, so that it includes an `assignee` property. This property will store a reference to the `User` model:

```javascript
// models/todo.js
const mongoose = require('mongoose');

const todoSchema = mongoose.Schema({
  text: String,
  isComplete: Boolean,
  // Reference to a User:
  assignee: { type: mongoose.Schema.Types.ObjectId, ref: 'User' } 
});

const Todo = mongoose.model('Todo', todoSchema);
module.exports = Todo;
```

Next, let's create a new file to store our `User` model:
```bash
touch models/user.js
```

In `models/user.js`, we'll define the `userSchema`, compile it into a model, and export it:

```javascript
// models/user.js
const mongoose = require('mongoose');

// Define the User schema
const userSchema = mongoose.Schema({
  name: String,
  email: String,
});

const User = mongoose.model('User', userSchema);
module.exports = User;
```

In this scenario, each `todo` document can reference one `user` through the `assignee` field. This field stores the `ObjectId` of the `user` document being referenced. Conversely, a single `user` can be referenced by multiple `todo` documents, thus establishing a **one-to-many (1:M) relationship**. In an application, this would mean one user can have multiple tasks assigned to them. Whenever a `todo` document requires information on the associated `user`, its `ObjectId` reference can be used to locate the appropriate document.