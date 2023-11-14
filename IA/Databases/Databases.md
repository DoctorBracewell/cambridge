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
# 3
---
The model for representing data is based of mathematical sets.

Suppose that `S` and `T` are sets. The cartesian product is the set:
![[Pasted image 20231024111007.png]]
![[Pasted image 20231024111121.png]]

A binary relation over `S X T` is any set `R` with:
![[Pasted image 20231024111310.png]]

n-ary relation:
![[Pasted image 20231024111508.png]]
or represented in tables:
![[Pasted image 20231024111644.png]]

This can be turned into a more "database-like" form by:
- Associating a name with each domain (this is the *attribute*)
- Use records - sets of pairs each associating an attribute name with a value in another domain.
This leads to:
![[Pasted image 20231024111847.png]]
i.e
![[Pasted image 20231024112351.png]]
is the same as:
![[Pasted image 20231024112329.png]]

**Database Query Language** - Takes an input of a collection of a relation instances, outputs a single relation instance.

Relational Algebra:
![[Pasted image 20231024112447.png]]
where:
- `p` is a boolean predicates over attribute values
- `X` is a set of attributes
- `M` is a renaming map

Q is a recursive type, it is formed of repeated expressions all the way down until all expressions becomes `R`, a base relation.

**Examples**:
Selection (all attributes with predicate)
![[Pasted image 20231024113202.png]]
Projection (certain attributes)
![[Pasted image 20231024113411.png]]
Renaming (renames attributes)
![[Pasted image 20231024113451.png]]
Union (combines selections from two relations)
![[Pasted image 20231024113702.png]]
Intersection (finds same selections from two relations)
![[Pasted image 20231024113740.png]]
Difference (finds all non-shared records)
![[Pasted image 20231024114043.png]]
Product (forms records of each possible enumeration)
![[Pasted image 20231024114056.png]]
![[Pasted image 20231024114131.png]]

**Natural Join** - Joining over a foreign key
![[Pasted image 20231024114743.png]]
![[Pasted image 20231024114841.png]]

**Redundancy** - Storing data that can be computed from another source in the database - any manner of storing data in two ways.
# 4
---
**How can an ER model be implemented relationally**?
![[Pasted image 20231031110845.png]]

**One Large table** 
![[Pasted image 20231031110835.png]]
But this has a number of problems:
- May want to insert a new person not attached to a film (insertion)
- Lose information about a director if all of their films are deleted (deletion)
- Updating director may do it for one record, but for all ones (update)
- Transaction implementing a simple update has a lot of work to to (could end up locking entire table).

**Break into Multiple Tables**:
![[Pasted image 20231031111348.png]]
![[Pasted image 20231031111355.png]]
Connected by:
![[Pasted image 20231031111410.png]]
or represented in SQL:
```sql
SELECT movie_id, title, year, person_id, name, birthYear 
FROM movies 
JOIN directed ON directed.movie_id = movies_id 
JOIN people ON people.person_id = person_id
```

Both relations and entities are implemented in tables. This makes it easier to maintain consistency during updates to the table, but there is more complexity involved when joining the tables.

**Key** - A unique handle on a record. Or a more specific set theory definition:
![[Pasted image 20231031112220.png]]

**Foreign Key** - 
![[Pasted image 20231031113058.png]]
Projection of Z in S (`SELECT Z FROM S`) is a member of projection of Z in R. This ensures that foreign keys do not link to non-existent records.

**Referential Integrity** - A database has referential integrity when all foreign key constraints are satisfied.

**Implementing ERs as relational schemas**:
![[Pasted image 20231031113532.png]]
- many to many -> R(**X**, **Z**, U)
- one-to-many -> R(**X**, Z U)

However another table may not be required:
- Rather than implementing a new table R, could expand T to T(**X**, Y, Z, U) and let Z, U be null for rows not in the relationship.

![[Pasted image 20231031114027.png]]
Suppose there are two many-to-many relationships.
Instead of R(**X, Z**, U) and Q(**X, Z**, V) just have:

RQ(**X, Z, type**, U, V) and U or V will be `null` for relationships.
^ however this is **redundancy**, because the `type` column would indicate which U or V would be `null`.

**Implementing Weak Entities** -
![[Pasted image 20231031114852.png]]
![[Pasted image 20231031114836.png]]

**Implementing Entity Hierarchy** -
![[Pasted image 20231031114911.png]]
![[Pasted image 20231031114926.png]]
# 5
---
**Transaction** - A series of queries and changes that externally appear to be atomic.
**Internal Transaction** - Some number of values are read, then various values changed based on read values. All values read or written are inside the same database.
**External Transaction** - Some of the values changed or other side effects are external to DBMS. DBMS cannot help make these atomic, however many do allow aborting transactions.

**Transaction client flow**:
![[Pasted image 20231107131337.png]]
- Start and commit calls bracket body
- Body consists of any number of queries and updates
- Client can abort at any time, all updates are undone.
- DBMSs support concurrent transactions.

**Atomicity** - All changes are performed as if they are a single operation (all are performed or none are).
**Consistency** - Every transaction applied to a consistent database leaves it in a consistent states.
**Isolation** - Intermediate state of a transaction is invisible to other transaction. Esp. important for concurrent transactions.
**Durability** - After a transaction successfully completes, changes to data persist and are not undone even int he even of system failure.

**BASE** - NoSQL systems often difficult to implement ACID, so
- Basically Available
- Soft state
- Eventual Consistency

**Lock** - A special software or hardware primitive that provides mutual exclusion. A resource can be locked for exclusive access by at most one application which must unlock it after use. Other applications have to wait.
In transactions, locks begin and end when the transaction does and can be different *granularities* - could lock entire database or just a single value.

**Redundant Data** - If it can be deleted and then reconstructed from the data remaining in the database. This is an issue because copies can disagree. 
**Closure** - An iteration is repeated until there are no further changes. 

**Normal Form** - A unique way of representing data in a specific, standard way.
**Database Normal Forms** - Usually a representation of a database aimed to limit redundant information stored. Automated procedures have been written to put databases into common normal forms such as 3rd (data should be dependent on the primary key).

Low redundancy gives good update throughput, high redundancy gives good query times.
![[Pasted image 20231107134343.png]]
However data redundancy can lead to stored data inconsistency if updates are not thorough.

**Fundamental Tradeoff** - Introducing data redundancy can speed up read-oriented transactions at the expense of slowing down write-oriented transactions.
You should consider when you might want a read-oriented database, such as:
- Data is read often but written seldom 
- Read-oriented data afford to be mildly out of sync with the write-oriented. Consider periodically making snapshots.

**FIDO** - Fetch Intensive Data Organisation
**OLAP** - Online Analytical Processing, write once or journal/ledger updates. Commonly associated with decision support, data warehouse, etc. HISTORICAL data, such that once a record is added is unlikely to be added to again.
**OLTP** - Online Transaction Processing, a mix of queries and updates to live data.
![[Pasted image 20231107135213.png]]

Version control system (e.g. Git) stores update history, adding an additional **dimension** to the stored documents. Even for OLTP a limited update history is usually stored for ACID durability.
# 6
---
**Semi-Structured Data** - Not all data fits neatly into a table form. For example, a textbook has chapters with names that have numbers and sections, diagrams with their own numbering, cross references between sections, etc. etc.

Two approaches to represent this:
- Store the document in its native form, store the indexable features in relational tables
- Store "shredded" and largely in native form (JSON, XML), develop database tools to navigate such semi-structured data. Generally a different querying technique than SQL.

Real world data is often analogue and noisy. 

**BA** - Basically available, availability is promoted over consistency. Any change in data made at one point is publicised to all other nodes.
**Soft State** - Stored values may change without any application intervention owing to eventual consistency updates.
**Eventual Consistency** - All readers throughout the system will eventually see the same state as each other.

**Blob** - A sequence of bytes
- Can be used as a value in a key/value store
- Any structure inside the blob is opaque to the key/value store
- Many implementations are distributed, spreading data over all participating machines known as shards
- Opaqueness implies DBMS knows nothing about what is stored
- Distribution provides redundancy and load balancing
- Implementations can range between ACID and BASE semantics.

