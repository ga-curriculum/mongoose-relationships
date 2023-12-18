# ![Mongoose Relationships - Level Up - Removing Embedded Sub-documents](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

embedding is good for read
but disadvantage comes from added complexity when making changes to embedded subdocuments
why? subdocs are never compiled to a model, no model methods
this means, when we want to find, delete, or update a sub doc, we must first go through the parent document

thankfully, there are various methods available for dealing with sub docs

## finding a subdoc

Each subdocument has an _id by default. 
Mongoose document arrays have a special id method for searching a document array to find a document with a given _id.

const doc = parent.children.id(_id);

```javascript
const findSubTask = async () => {
  const todoId = '657b25adc8146427465857d7'
  const subTaskId = '6580677cf30bfef697ae046f'

  const todo = await Todo.findById(todoId);
  const subTask = todo.subTasks.id(subTaskId);

  console.log('Subdocument:', subTask);
}
```

```plaintext
Subdocument: {
  text: 'Learn how props work',
  priorityLevel: 'High',
  isComplete: false,
  _id: new ObjectId('6580677cf30bfef697ae046f')
}
```

## updating a subdoc

find parent
modify property
save parent

what if we need to update multiple properties on a subdoc?
multiple ways to handle this, but programatically you could...

```javascript
const updateSubTask = async () => {
  const todoId = '657b25adc8146427465857d7'
  const subTaskId = '6580677cf30bfef697ae046f'

  const todo = await Todo.findById(todoId);
  const subTask = todo.subTasks.id(subTaskId);

  subTask.isComplete = true
  await todo.save()

  console.log('Updated document:', todo);
}
```

```plaintext
Updated document: {
  _id: new ObjectId('657b25adc8146427465857d7'),
  text: 'Learn React',
  isComplete: false,
  __v: 1,
  subTasks: [
    {
      text: 'Learn how props work',
      priorityLevel: 'High',
      isComplete: true,
      _id: new ObjectId('6580677cf30bfef697ae046f')
    }
  ]
}
```


## removing a subdoc
parent.children.pull(childId)


like push, need to save the parent


```javascript
const removeSubTask = async () => {
  const todoId = '657b25adc8146427465857d7';
  const subTaskId = '6580677cf30bfef697ae046f';

  const todo = await Todo.findById(todoId);

  todo.subTasks.pull(subTaskId);
  await todo.save();

  console.log('Updated document:', todo);
};
```

```plaintext
Updated document: {
  _id: new ObjectId('657b25adc8146427465857d7'),
  text: 'Learn React',
  isComplete: false,
  __v: 2,
  subTasks: []
}
```

