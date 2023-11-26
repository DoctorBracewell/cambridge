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

> iv) The SQL language contains a `GROUP BY` construct. Explain why the data returned by `GROUP BY` has to be passed through a reduction operator before it can be returned in a table?

The data must be passed through a reduction operator because `GROUP BY` essentially makes "subsets" of data grouped by some value passed to the construct, which cannot be represented in a table format. Instead these subsets are reduced into a single value or record that is attached to the distinct field used by the `GROUP BY` construct, such as counting the number of records or a particular value in each record.

> v) What mathematical properties should such a reduction operator have? Explain why.

Slightly unsure about this, but it should be able to turn any number of records with any number of fields into a single value.

> vi) A distributed database may use eventual consistency. Explain what this is. Give two advantages.

Eventual consistency is a technique that involves different nodes or locations of a distributed database potentially being out of sync or inconsistent with each other, but being guaranteed to eventually settle back into a consistent state.
An advantage of this is improved availability - less time will be taken to resolve transactions and more can be executed in the same amount of time, and that this is overall more scalable as the number of requests the database needs to handle increases.

> vii) State the principle disadvantage of eventual consistency? Provide a simple example.

That there is a period of time where the database is inconsistent, and requests in this time may see different states of the database on different nodes. For example, in a transfer between bank accounts one account may have the amount deducted but the it has not been added to the other account yet.

> viii) Explain why a graph database typically holds just one graph?

Unsure. Perhaps because of complexity relating queries between multiple graphs?
# 2023 Paper 3 Question 2
> a) Is the relational database join operator associative? Describe an example where different associations might result in significant execution time differences.

Yes, the join operator is associative. I am unsure when this might lead to a time difference in execution?

> Give an example relation where there is more than one potential key. Ensure your example has at least two reasons why some of these candidate keys are not suitable for use as the primary key. Explain the reasons.

