**Name:** Brace Godfrey
**CRSId:** dg681
### 1
> A relational implementation of an Entity-Relationship (ER) model typically attempts to avoid **redundancy**. But what does this mean exactly? Is **redundancy** the same thing as **duplication** of data values?

Data redundancy refers to when the same data is stored in multiple places in a database, specifically when it is unnecessary duplication of data. This can often happen when the same piece of data is used in multiple records, and can be avoided using entity-relationship models or other techniques to split data into multiple tables with links between them.
However, some data needs to be duplicated in a database, such as using foreign keys to connect records together across tables. Redundancy specifically refers to data that is stored multiple times unnecessarily.
### 2
> 1997 Paper 3, Question 11

a) 
A relation is a connection between two entities. In a relational schema it can be expressed as a line between entities, often with arrows to show if it is one-to-one, many-to-one, one-to-many or many-to-many. It can also have properties of its own.

b)
![[Pasted image 20231024101337.png]]

(not totally sure about these statements as they were written after 2 lectures, before we had learned SQL in-lecture).
c)
1. `SELECT * FROM Climbs INNER JOIN Climbers ON Climbers.name = Climbs.led_by WHERE Climbers.name = "J. Brown"`
2. `SELECT Climbs.name, Climbs.length FROM Climbs INNER JOIN ClimbingAreas ON Climbs.area_name = ClimbingAreas.name WHERE ClimbingAreas.name = "Snowdonia" AND Climbs.type = "slate" AND Climbs.grade = "hard severe"`
3. ?? Unsure
### 3
> Construct an Entity-Relationship model for the following scenario. Suppose we are conducting several experiments. Each experiment has a name and a description (text). Each experiment can be associated with many _runs_. Each run is associated with some input parameters and some output values.

![[Pasted image 20231024102533.png]]
### 4
> Discuss possible relational implementations of your model from above.

Unsure what exactly "relational implementation" means, but if it means a relational schema then a database could be structured like so:
- An `experiments` table, with `name` as a primary key field and `description` as a field.
- Since each experiment run has a different set of input parameters, different tables may need to be defined for each run type? For the example in question 3, a `simulation_run` table could be formed with `input_seed`, `size_parameter`, `total_messages`, `termination_time` fields.
### 5
> Suppose there is an experiment called "grid" that has input parameters "random_seed", "grid_width", and "grid_height" with output parameters "message_count" and "run_time". For fixed values of "grid_width" and "grid_height" we have run thousands of experiments just varying the "random_seed" value. Using your implementation from (4), we now want to write an SQL query that groups all runs of "grid" by "grid_width" and "grid_height" and returns for each group the average for each of the outputs. Recall that the average is computed by the aggregate function **AVG**.

Again, completing this work before lectures on SQL syntax or relational schema creation.

`SELECT AVG(message_count), AVG(run_time) FROM grid GROUP BY grid_width, grid_height`

