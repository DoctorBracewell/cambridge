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
****