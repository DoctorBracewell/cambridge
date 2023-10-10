# 1
---
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
![[Pasted image 20231010112608.png]]