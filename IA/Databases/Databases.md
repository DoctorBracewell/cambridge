# 1
---
### Definitions
**DBMS** - Database Management System
**SQL** - Structured Query Language, most common language to interact with databases

Fixed-Field lengths were widely used on punched cards but are not good for data with variable lengths (names, etc.)

**Associative Store*** - A key/value store with two API operations:
`store: string, string -> unit`
`retrieve: string -> option<string>`
crucially they can be called in any order, no invocation ordering constraints exist (e.g. open -> read/write -> close).

**Value** - Any piece of data stored in a database, e.g. string, number, data, etc.
**Field** - A place to hold a value, also an attribute or column.
**Record** - A sequence of fields
**Schema** - A specification of how data is to be arranged, specifying field names and types and also consistency rules
**Key** - The field or concatenation of fields used to locate a record
**Index** - A derived structure able to find relevant records quickly.
**Query** - A retrieve or lookup function
**Update** - A modification of the data, preserving consistency (often implemented as a transaction)
**Transaction** - An atomic change of a set of fields, usually follows ACID rules/properties.

Interfaces are important as they liberate application writers from low-level details. In a perfect word implementations can change without the interface changing. However "mission-creep", "software misengineering" and "breaking changes" over the lifetime of a software change the public interface.
### Typical Database Logic Systems
![[Pasted image 20231010112608.png]]
or
![[Pasted image 20231010112935.png]]

DBMS can use both RAM and secondary storage to run a database - often data is saved on secondary-storage for long-term use so it is not deleted if the computer loses power, but RAM (in-core) is used for in-progress operations.

### Variations on Typical Systems
The database setup can vary depending on requirements

**OS Filesystem** - Most operating systems provide a special filesystem for ease of use (e.g. folders, file names and permissions, etc.) Some DBMSs will save data into this filesystems but many will also save *directly to secondary storage*, as the filesystem may be necessary if the data is only ever accessed through the DBMS.
**Big Data** - Too big to fit in the primary store
**In-Core** - All data fits in primary store

**Important factors to consider**
- Amount of access, some data is rarely written to/read from, some is accessed very very often etc.
- Database arrangement, graph/table/spatial etc.
- Consistency, ensuring that all data is valid (for high risks systems etc.)
### Consistency
**Consistency** - Ensuring that all data is valid in a database and makes sense. Examples include:
- Range check
- Foreign key referential integrity
- Value atomicity
- Entity Integrity

### DBMS
The course will cover how a DBMS provides an elegant interface to work with a database.
Low-level implementation details such as networking, memory/storage management, query processing etc. will not be covered.

**Create** - Insert new data items into the database
**Read** - Query the database
**Update**- Modify objects in the database
**Delete** - Remove data from the database

Various management processes are also provided but mostly beyond course scope, examples include changing schema, changing view, changing permissions, backups, statistic generation etc.

### Data Models
**Relational Model** - Data is stored in tables, optimised for high throughput of many concurrent updates
**Document-Oriented Model** - Optimised for for read-oriented databases with few updates and semi-structured data
**Graph-Oriented Model** - Made of nodes and edges, optimised for "path" tracing and algorithms to connect different nodes in various ways

### Relational Databases
Consists of a number of 2D tables. 
![[Pasted image 20231010115115.png]]

**Distributed Databases** - Database held over multiple machines or multiple datacentres.
**Scalability** - The data set or workload can be too large for a single machine
**Fault Tolerance** - The service can survive the failure of some machinery
**Lower Latency** - Data can be located closer to widely distributed users

However after an update there is a massive overhead in providing a consistent view. This can be mitigated with relaxed-consistency model, but can cause different users to have different versions of the database at different times. Additional challenges include:
**Consistency** -
**Availability** -
# 2
---
It is useful to have an implementation-independent technique to describe data stored in a database.
### Entity-Relationship Model
**Entity** - The "noun" of our model
**Attributes** - Properties of an entity
**Key** - Attribute who's value uniquely identifiers an entity instance
**Scope** - Is limited, as we must explicitly define all the properties a particular entity has.
![[Pasted image 20231017110830.png]]

**Keys** - Should always be unique, formed by some algorithm, or a *synthetic key* that is automatically generated in the database and only has meaning there.
**Relationships** - "verbs" between entities. For example, a `Person` "directed" a `Movie`.
### Many-To-Many Relationship
- Any `S` can be related to zero or more `T`s
- Any `T` can be related to zero or more `S`s
- The relationship can be symmetric or relate an entity domain to itself.
- Relationships can also have attributes just like entities.
- Ensure that value atomicity is kept, don't use **multi-value attributes**.

![[Pasted image 20231017112853.png]]
Designing a model can be complicated - there are many different methods that your database could use to represent data, certain forms may be too artificial or too simple based on your use case.
### Many-To-One Relationship
Can be denoted with an arrow, shows that some entity can be related to at most one entity of another type. 

Suppose every member of `T` is related to at most one member of `S`.
- The relationship is **many-to-one** between `T` and `S`
- The relationship is **one-to-many** betweeen `S` and `T`
### One-To-One Relationship
One-To-One relationships are rare because mostly these can be represented as a single entity or as an attribute on one entity.

One-To-One cardinality does not mean correspondence between all entities - some entities of a particular type may not have a connection to another one, even if overall those two entities have a one-to-one relationship.
### Weak Entities
**Weak Entity** - One that relies on the existence of another entity (or a relationship to that entity)
![[Pasted image 20231017114328.png]]
Weak entities have *discriminators* instead of keys, and can be uniquely identified using their discriminator and the key of their stronger entity.
### Entity Hierarchy
Entities can have OOP-like hierarchies, where some entity has a sub-entity that inherit all attributes and relationships of the parent entity.
![[Pasted image 20231017114502.png]]
### ER Diagrams
- Forces you to think about the model you want to implement
- Easy to learn and understand
- Valuable when developing a model in collaboration with clients who don't know database implementation details.
![[Pasted image 20231017114727.png]]

