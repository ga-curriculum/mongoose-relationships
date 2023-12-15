# ![Mongoose Relationships - Level Up - Referencing](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## What is referencing?

Referencing is the an alternate approach to establishing relationships between data entities in MongoDB. With embedding, parent documents hold sub-documents. With **referencing**, a document only contains the related document’s `ObjectId`. Both approaches can be used simultaneously in the same document.

> 📚 Referencing is the practice of establishing relationships between data entities by storing a reference (in the form of an ObjectId) to a related document within another document, rather than embedding the related data.
>



## Which Document Should Hold the Reference?

When referencing data in MongoDB, you can hold the `ObjectId` in either document or both!

The decision depends upon the design and functionality of your application, and it’s not always clear-cut.


## If Embedding is More Efficient, Why Reference at All?

- If the amount of data stored can exceed the 16MB size limit for a document. Exceeding this size is quite uncommon though - all Shakespeare’s works together are fewer than 5 megabytes!
- When multiple parent documents need access to the same child document and that child’s data changes frequently. For example, a document modeling a *bank account* should be referenced because it could be “owned” by more than one individual. Suppose the account data were embedded in two or more parent documents. Can you imagine how difficult it would be to keep the transactional & balance data in sync?
- If it makes sense for your application. For example, if you wanted to view all *posts* on your landing page, regardless of the user who posted them, it would take more effort to extract the *posts* from each user if they were embedded. However, it would be quick to get the *posts* from their own collection.











    What is referencing?
    • relationships between different collections.
    • linking documents with objectids
    • how is it different than embeding

    ERD EXAMPLE

    Why use it?
    • compare contrast embedding
    • examples where it is prefered
    • touch on performance

    Demonstrate how to define referenced relationships
    • demo query
    • .populate() method in Mongoose to "join" documents from different collections.
    • Discuss best practices for when to use .populate() and the performance considerations.




