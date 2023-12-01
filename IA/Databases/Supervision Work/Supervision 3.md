**Name:** Brace Godfrey
**CRSId:** dg681
# 2023 Paper 3 Question 1
> A manufacturer makes three models of vehicle that vary in their engine type and number of seats. One model is available in two paint colours. Engines are either petrol or electric. Engines come from different suppliers, dependent on their fuel and horsepower.

> i) Draw a suitable E/R diagram.

![[Pasted image 20231126220651.png]]

> ii) By writing out a suitable number of short tables, give a small relational database example holding vehicles and engines. Make sure every field has a name. Underline table keys.

![[Pasted image 20231126220849.png]]
![[Pasted image 20231126221101.png]]
![[Pasted image 20231126221114.png]]

> iii) Which fields in your example are foreign keys? Explain whether your example satisfies referential integrity.

The `engine_id` field between Models and Engines, and the `supplier_id` between Engines and Suppliers act as foreign keys. My example does satisfy referential integrity because all foreign keys refer to a valid, existing primary key in a record.

> The SQL language contains a `GROUP BY` construct. Explain why the data returned by `GROUP BY` has to be passed through a reduction operator before it can be returned in a table?

The data must be passed through a reduction operator because `GROUP BY` essentially makes "subsets" of data grouped by some value passed to the construct, which cannot be represented in a table format. Instead these subsets are reduced into a single value or record that is attached to the distinct field used by the `GROUP BY` construct, such as counting the number of records or a particular value in each record.

> What mathematical properties should such a reduction operator have? Explain why.

Slightly unsure about this, but it should be able to turn any number of records with any number of fields into a single value.

> A distributed database may use eventual consistency. Explain what this is. Give two advantages.

Eventual consistency is a technique that involves different nodes or locations of a distributed database potentially being out of sync or inconsistent with each other, but being guaranteed to eventually settle back into a consistent state.
An advantage of this is improved availability - less time will be taken to resolve transactions and more can be executed in the same amount of time, and that this is overall more scalable as the number of requests the database needs to handle increases.

> State the principle disadvantage of eventual consistency? Provide a simple example.

That there is a period of time where the database is inconsistent, and requests in this time may see different states of the database on different nodes. For example, in a transfer between bank accounts one account may have the amount deducted but the it has not been added to the other account yet.

> Explain why a graph database typically holds just one graph?

Unsure. Perhaps because of complexity relating queries between multiple graphs?
# 2023 Paper 3 Question 2
> a) Is the relational database join operator associative? Describe an example where different associations might result in significant execution time differences.

Yes, the join operator is associative. I am unsure when this might lead to a time difference in execution?

> Give an example relation where there is more than one potential key. Ensure your example has at least two reasons why some of these candidate keys are not suitable for use as the primary key. Explain the reasons.

One example relation is a table storing information about people, such as employees. One candidate for a primary key might be the full name of the person as this is used to "identify" people, however this is not suitable because two people can have the same full name, so the key would not be unique. Another one might be the social security number of an employee. Whilst this may be guaranteed to be unique between each person, not every employee might have one (such as an international employee). A primary key must exist for every record for it to be suitable.

> In this question, an XML document is a tree with named nodes, called elements, whose leaves are character strings. In addition, an element has an unordered list of string pairs where the strings are an attribute name and its value.
> ![[Pasted image 20231127203240.png]]

> a) A document may be stored in XML in various ways, varying from rigidly structured to loosely structured. Describe when this can be useful. How can variations in structure be tolerated or reported?

The way a document is structured is called its schema. Schemas can exist externally to the DBMS as documentation or guidelines, or implemented in the DBMS such that data that does not follow the schema cannot be added to the database.

Some schemas can be very rigid, and any document must contain a certain amount of specific fields that all have a value or an order or other elements inside them. This is useful when using a document database to act similarly to a relational database, as a rigid schema leads to a table-like structure with fields that are guaranteed to exist for every "record". This may be useful when the benefits of a document database (such as advanced linking queries between documents) are wanted whilst the database is structured more relationally.
  
Other schemas can be loose, and describe a more flexible structure where documents within the database can vary in the number and types of fields they contain. Different documents may have different structure depending on how much and what kind of data is stored. A more dynamic schema approach is beneficial in scenarios where data is diverse or when the exact structure of documents is uncertain in advance. However, looser schemas may require additional validation or management to ensure consistency and integrity especially when querying multiple documents. For example a query may try to retrieve a field that does not exist for a particular document, so a way to handle this case must be implemented.

> b) How might all the data held in a relational DBMS sensibly be exported into a single XML document?

Each table in the relational database would become the name of an XML element. Each record in that table would be a unique instance of that element, with the values of each field being stored in the unordered string list associated with the element. Relations between tables could be mapped using the tree structure of XML - a one-to-many or many-to-one relationship would exist as a single parent element (with all field values from the single record in its string list) containing multiple child elements, each of which are one of the records the single record relates to.

> d) What is the purpose of the project operator in the relational algebra? Would there be an equivalent operator in a document or graph database?

The project operator selects certain attributes from a relation or set. In a document or graph database the structure is often looser, and not every attribute is guaranteed to exist for every document. However, if a query in a document database returns an array of matching documents, an equivalent might be some "map" operation that transforms all of those documents into just their value for a particular key, or `null` if a particular document does not have that key (the `null`s could later be filtered out if required).
# 2018 Paper 3 Question 1
> In the database context, what do we mean by redundant data?

Redundant data is that which can be reconstructed from existing data in the table.

> Why might it be a good idea to have redundant data in a database?

Redundant data has some uses, such as foreign keys between tables or for data that takes such significant complexity or processing time to be constructing from existing data that it is better to store it as well.

> Why might it be a bad idea to have redundant data in a database?

Redundant data requires more storage, and can add complexity to write operations as multiple fields may need to be updated when changing one value.

> Suppose a database has tables `R(A, B) and S(B, C)`. Explain how using an index could improve performance when joining `R` and `S`. Is there a downside to using an index?

When joining `R` to `S` on `B`, an index could be used on `S`. This would be that for each record in `R`, *not* every record in `S` has to be visited because the index would indicate which records in `S` need to be joined. I am not 100% sure how exactly it achieves this.

> In SQL, what could be returned when evaluating the following expression?
> ![[Pasted image 20231127212028.png]]

This could only return `false` (or `null` if `a` is null?)

> Suppose `R(start, end)` is a table in a relational database representing arcs in a directed graph. That is, each record `(x, y) ∈ R` represents an arc from node `x` to node `y`.

> Write an SQL query that returns the start and end of all 3-hop paths in the directed graph represented by R. Your query should return columns named `start, end`. Each row `(x, y)` in the result of your query should indicate that there exists a path in R `x → z → u → y` for some nodes `z` and `u`.

```sql
SELECT R.start AS start, R2.end AS end FROM R
JOIN R AS R1 ON R.end = R1.start
JOIN R AS R2 ON R1.end = R2.start
```

> What is the transitive closure of `R`? Why is this difficult to compute in SQL if we ignore recursive query constructs?

The transitive closure of `R` is all of possible paths between any two particular nodes in the graph. This would be difficult to compute in SQL because of how `R` would have to be repeatedly looped through to ensure there are possible paths from the current node to another one, and then "backtracked" to a previous state to test paths from the node before.
# 2016 Paper 4 Question 6
> This question deals with the variety of approaches to database design.

> What is meant by the term on-line transaction processing (OLTP)?

OLTP is a method of database operations that involves "transaction processing", which involves a mix of queries and updates to live data. It is used for data that changes day-to-day and needs to be up-to-date. It has a specific focus on updating data efficiently. and into a consistent state, and can achieve this using a variety of techniques such as the ACID properties of transactions.

> What is meant by the term on-line analytic processing (OLAP)?

OLAP is a method of database operations that focuses less on updating data into a current, up-to-date state and more about storing large amounts of historical data and reading it when required. It supports analysis of this data, and often contains redundant data because of how it is optimised for read rather than write queries.

> Compare and contrast the approach to schema design for OLTP and OLAP databases.

Schemas for OLAP databases may be looser than OLTP databases, and be more denormalised because they are designed for analytical processing of the data. Queries can be more complex, but more redundant data tends to be stored for faster computation time.

OLTP schemas may more rigid and ensure that the database is in a normalised form, usually with minimised redundant data. This helps reduce the complexity of operations performed by write operations. In a relational database, OLTP schemas often involve small tables connected by foreign keys to avoid data duplication.

> Give an example of a set of requirements whose solution would need to combine OLAP, OLTP and NoSQL. Describe an architecture integrating these elements in the system design.

An example where OLAP, OLTP and NoSQL techniques might all be applicable could be a blogging website, with the following requirements:
- New users can make an account with some personal information on their profile
- Users can publish blog posts and assign particular topics, or have topics auto-suggested from the post contents.
- Users reading habits and interested topics are analysed to generate recommended blog posts.

The architecture of such an application may include the following components:
- A relational database used to store user information. It could have tables such as `User(user_id, username, pronouns, profile_picture, status)`. This database would hold up-to-date information about current users, and would be written to and read from often.
- A graph, relatively unstructured database that that includes all users and blog posts, alongside topics that users are interested in or posts are about. NoSQL techniques and OLAP data analysis algorithms would be used to link together users and topics and generate recommendations.
- A historical document database used to hold blog posts or writer biographies. 

OLTP techniques could mostly be applied to the relational `user` database. Since the data in there needs to be kept up-to-date and consistent, transaction processing techniques would be used to read from and write to the database.
Transaction properties such as ACID could be enforced to ensure that new user records are created during the signup processes and profile data is stored efficiently and consistently.

OLAP techniques could be applied to the document database. The document database would generally be used to hold historical data, as blog posts tend to be written once and then only read by other users. However, analysis of blog posts could form a large part of a recommendations algorithm, and OLAP processes would need to be used to analyse post contents for the topic auto-suggestion process.
Whilst the graph database (holding user interests and blog topics) might be updated often with new posts or new user reading data, OLAP techniques could potentially be applied there. A recommendations algorithm would need to analyse large amounts of user reading and interest data, which OLAP is well suited for.

NoSQL techniques could be used in the recommendations process in the graph database. If connections are formed between user nodes, post nodes and topic nodes, a relational query language such as SQL would struggle to retrieve relevant data for the recommendation algorithm. A less structured querying method such as NoSQL could be more useful when trying to form connections between nodes and retrieve data from them.
# Definitions
**Atomicity** - All changes in a transaction are performed or none are, so it acts as a single operation.
**Consistency** - A transaction applied to a consistent database must bring the database into a consistent state.
**Isolation** - Only one transaction applies to a particular piece of data (value, record, table etc.) to one time, and the intermediate state of the data is invisible to any other transaction.
**Durability** - Once a transaction has completed any changes to the data persist even in system failure (e.g. ensuring transactions save changes to secondary storage so the result is not lost in the case of power loss).

**Key** - A field used to locate particular records in a query.
**Foreign Key** - A value in a record that is used to link to another record.
**Superkey** - A combination of multiple fields used to act as a key.

**Normal Form** - A way of uniquely structuring a database to ensure it is in a standardised consistent state, often aimed to minimised the amount of redundant data stored.
**Referential Integrity** - A property of a database such that all foreign key constraints are satisfied, and that no foreign key points to a non-existent record.
**Eventual Consistency** - A technique that involves different nodes or locations of a distributed database potentially being out of sync or inconsistent with each other, but being guaranteed to eventually settle back into a consistent state. Could be used in a bank account transfer.



