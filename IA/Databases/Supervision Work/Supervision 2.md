**Name:** Brace Godfrey
**CRSId:** dg681
# 2017 Paper 3 Question 1
> Consider the following Entity-Relationship (ER) diagram. ![[Pasted image 20231104175658.png]]
> Suppose we wish to implement this diagram in a relational database using three tables, `S(sid, A)`, `T (tid, C)`, and `R(· · · )`. Describe the schema you would use for `R` depending on the cardinality of the relationship.

> a) When `R` is a many-to-many relationship between `S` and `T`. 

I would use a linking table `R(tid, sid, B)`. `sid` would be a foreign key between `S` and `R`, and `tid` would be a foreign key between `T` and `R`. Primary keys in `R` would be composite of both the `tid` and `sid` fields.

> b) When `R` is a one-to-many relationship between `S` and `T`. 

A linking table `R(tid, sid, B)`, where `sid` is a foreign key from `S` to `R`, and `tid` is both the **primary key** in `R` and a foreign key to `T`.

> c) When `R` is a many-to-one relationship between `S` and `T`. 

A linking table `R(tid, sid, B)` where `tid` is a foreign key from `T` to `R`, and `sid` is both the **primary key** in `R` and a foreign key to `S`.

> d) When `R` is a one-to-one relationship between `S` and `T`. 

Either of the above schemas for one-to-many or many-to-one would work for this? The composite key from a) would not be needed. 

> e) Suppose R is a many-to-one relationship. Rather than implementing a new table for R, can we modify one of the tables representing S or T to implement this relationship? Discuss the advantages and disadvantages of such a representation.

The `S` table can be modified to contain the `B` field from the `R` relationship, as well as new `tid` field that acts as a foreign key to the `T` table. This would allow many records from `S` to point to the same record in `T`.
This has the advantage of only requiring two tables instead of three in the original implementation, saving space and leading to less complicated queries on the database.
However, this may make the schema less changeable, as extra work would be needed if the relationship was ever updated to many-to-many.

> f) Suppose that we add the following diagram to our ER model. ![[Pasted image 20231104214206.png]]Note that this implicitly defines a relationship between `S` and `U` resulting from the composition of relationships`R` and `Q`. Discuss the difficulties that you might encounter in attempting to implement this derived relationship directly in a table `W`.

A table `W` could be used to implement the derived relationship between `S` and `U` in the above ER. Since `S` has a relationship `R` with `T`, and `T` a relationship `Q` with `U`, the table may look like `W(sid, uid, B, D)` to hold the relationships and the data between `S` and `U`.
However, this table has potential issues, because of either a lack of a path between records in `S` to `U` or multiple paths between them (depending on the cardinalities of relationships between `S`, `T` and `U`). Consider that one record in `S` may point to multiple records in `T` (with different values of `B` for each of them), and that these records in `T` both point to the same value in `U` (again with different values of `D`). Whilst one record in `S` is only pointing to one record in `U`, there are multiples values for the relation fields, and it is unclear which of these should be stored in table `W`. 
# 2018 Paper 3 Question 2
> Suppose that we have a relational database with the following tables. ![[Pasted image 20231104215353.png]]
> In table `ActsIn`, `pid` is a foreign key into `People` and `mid` is a foreign key into `Movies`. In table `HasGenre`, `mid` is a foreign key into `Movies` and `gid` is a foreign key into `Genres`.

> a) Suppose that the attribute `role` was not considered part of the key for table `ActsIn`. How would this change your interpretation of the database?

This would limit the form of data that could be stored of the database, as it would mean that an actor could not play multiple roles within the same movie without violating the rule of unique primary keys.

> b) Suppose we replaced the tables `Genres(gid, genre)` and `HasGenre(gid, mid)` with a single table `MovieGenres(mid, genre)`. Would this change what data can be captured in the database? Explain your answer.

This would not change what data could be captured in the database, as there could still be a many-to-many relationship between movies and genres. The `MovieGenres` table would have to have a composite primary key of `mid` and `genre`, and the lack of use of a linking table and a `gid` field may mean that there is duplication in the database, the `genre` field would have to be repeated multiple times in the `MovieGenres` table and this would not be ideal.

> c) Write an SQL query that returns `title`, `mid`, for those movies that are not associated with any genre.

```sql
SELECT Movies.title, Moviesmid 
FROM Movies 

LEFT JOIN HasGenre ON Movies.mid = HasGenre.mid 
WHERE HasGenre.gid IS NULL
```

> d) Write an SQL query that returns `name`, `pid`, for those people that act in at least one movie associated with the genre `'Drama'`.

```sql
SELECT People.name, People.pid
FROM People

JOIN ActsIn ON People.pid = ActsIn.pid
JOIN Movies ON ActsIn.mid = Movies.mid

JOIN (
	SELECT Movies.title
	FROM Movies
	
	JOIN HasGenre ON Movies.mid = HasGenre.mid
	JOIN Genres ON HasGenre.gid = Genres.gid
	
	WHERE Genres.genre = "Drama"
) AS subquery
ON subquery.title = Movies.title

GROUP BY People.name
```

> e) Write an SQL query that returns `title`, `mid`, `genre`, for those movies that have `genre` as their only genre. That is, if the query returns the row ![[Pasted image 20231104224119.png]]
> it means that this movie is associated only with the genre `'Comedy'` and no other genre.

```sql
SELECT Movies.title, Movies.mid, Genres.genre
FROM Movies

JOIN HasGenre ON Movies.mid = HasGenre.mid
JOIN Genres ON HasGenre.gid = Genres.gid

JOIN (
	SELECT Movies.mid
	FROM Movies

	JOIN HasGenre ON Movies.mid = HasGenre.mid
	JOIN Genres ON HasGenre.gid = Genres.gid

	GROUP BY Movies.mid
	HAVING COUNT(DISTINCT Genres.genre) = 1
) AS subquery

ON Movies.mid = subquery.mid
```
# 2020 Paper 3 Question 1
> Suppose we have a relational database with five tables. ![[Pasted image 20231104231908.png]]
> Here `R` implements a many-to-many relationship between the entities implemented with tables `S` and `T`, and `Q` implements a many-to-many relationship between the entities implemented with tables `T` and `U`.

> a) Write an SQL query that returns all records of the form `sid, uid` where `sid` is the key of an `S`-record and `uid` is the key of a `U`-record and these two records are related through the relations `R` and `Q`. 

```sql
SELECT S.sid, U.uid
FROM S

JOIN R ON R.sid = S.sid
JOIN T ON R.tid = T.tid
JOIN Q ON T.tid = Q.tid
JOIN U ON Q.uid = U.uid
```

> b) Write an SQL query that returns records of the form `A, C` where the `A`-value is from an `S`-record and the `C`-value is from a `U`-record and these two records are related through the relations `R` and `Q`.

```sql
SELECT S.A, U.C
FROM S

JOIN R ON R.sid = S.sid
JOIN T ON R.tid = T.tid
JOIN Q ON T.tid = Q.tid
JOIN U ON Q.uid = U.uid
```

> c) Could one of your queries from parts *(a)* and *(b)* return more records than the other? If so, which one? Justify your answer. 

The queries must return the same number of records because the joining is the exact same between them. This means that the same S-records and U-records will be returned for both queries, and the only difference is the fields selected from these records  (`sid` and `uid` for the first one, and `A` and `C` for the second).

> d) Consider again your query from part (a). If pair `sid, uid` is returned by this query then there must exist at least one “path” that goes from from table `S` to table `T` (via relation `R`) and then from table `T` to table `U` (via relation `Q`). Note that there can be many such paths for a given pair `sid, uid`. 
> Write an SQL query that returns records of the form `tid, total` where `tid` is a key of a record from table `T` and `total` indicates the total number of such paths that “go through” that record. 

Genuinely no earthly idea how to do this.
# 2021 Paper 3 Question 1
> The following tables are part of a library’s database system.![[Pasted image 20231104233349.png]]
> In the table `Book` the column `number_owned` is the number of copies of the book owned by the library, while the column `number_borrowed` is the number of copies currently out on loan. In table `Borrowed` the column `person_id` is a foreign key into the `Person` table, the column `book_id` is a foreign key into the `Book` table. The column `number` is the number of copies of the book borrowed by the associated person.
>
> If the database is internally consistent, then the column `number_borrowed` is redundant information that can be computed from the actual `number_borrowed`, and this can be derived from the `Borrowed` table.

```sql
SELECT Book.book_id, Book.number_borrowed, subquery.actual_number_borrowed
FROM Book

JOIN (
	SELECT Borrowed.book_id, SUM(Borrowed.number) as actual_number_borrowed
	FROM Person

	JOIN Borrowed ON Person.person_id = Borrowed.person_id

	GROUP BY Borrowed.book_id
) AS subquery
ON subquery.book_id = Book.book_id

WHERE Book.number_borrowed != subquery.actual_number_borrowed
```

> Your job is to redesign this schema so that there is no need for such consistency checks. The first step is to design an Entity-Relationship model. You will do this by introducing a new entity called `Copy Of`. Each copy of a book owned by the library will be associated with a unique member of the `Copy Of` entity.
> Design an Entity-Relationship diagram based on this idea and argue that cardinality constraints will ensure that the database is internally consistent.

![[Pasted image 20231107002429.png]]

The cardinality constraints of the above ER diagram are such:
- `Copy_Of` is a weak entity relying on the existence of a `Book`, as a copy of a book cannot exist if the book does not exist.
- `exists` is a **one-to-many** weak relationship between `Book`s and `Copy_Of`s, as one book can have any number of copies in the library, including 0.
- `borrowed` is a **many-to-one** relationship between `Copy_Of`s and a `Person`, as one person can have as many copies of different books as they want, including 0.
These relationships ensure consistency in the database, because a person cannot have a copy of a book that does not exist, and only one person can have a particular copy.
The inconsistent problems of the previous representation are eliminated because the number of books borrowed is not stored as redundant data, but can be calculated **only** using the existing entities and relationships between them.

> Discuss at least two options for implementing your ER model in an SQL database. 

One implementation would be to use 5 tables as below:
Book(**book_id**, title)
Exists(**book_id**, **copy_id**)
Copy_Of(**copy_id**)
Borrowed(**copy_id**, **person_id**)
Peron(**person_id**, name, address)

Another way would be to eliminate the `Exists` table as no extra information is stored and the relationship can be represented just with 4 tables as below:
Book(**book_id**, title)
Copy_Of(**book_id**, **copy_id**)
Borrowed(**copy_id**, **person_id**)
Person(**person_id**, name, address)

> Using one of your relational implementations from the previous part, write an SQL query that reproduces the contents of the `Book` table from the original design. That is, write an SQL query that returns records of the form `(book_id, title, number_owned, number_borrowed)`.

```sql
SELECT Book.book_id, Book.title, subquery1.number_owned, subquery2.number_borrowed
FROM Book

JOIN (
	SELECT Book.book_id, COUNT(*) as number_owned
	FROM Book
	
	JOIN Copy_Of
	ON Book.book_id = Copy_Of.book_id
) as subquery1
ON Book.book_id = subquery1.book_id

JOIN (
	SELECT Person.person_id, COUNT(*) as number_borrowed
	FROM Person
	
	JOIN Copy_Of
	ON Person.person_id = Copy_Of.person_id
) as subquery2
ON Person.person_id = subquery2.person_id
```
# 2021 Paper 3 Question 2
> This question involves the relational movie database used in our SQL practical. In each part you are given the result of an SQL query together with a possibly incorrect conclusion drawn from this result.
> In each case your task is to argue for or against the conclusion. You must clearly justify your reasoning. If the SQL query can be corrected, then do so.

> a) The following query returns 1422.
> Conclusion: Our database contains information on 1422 directors.
> 
> ![[Pasted image 20231105174742.png]]

This is an incorrect conclusion, as the `has_position` table acts as a one-to-many relationship between directors and films. This means that one director may have multiple records in the table as they have directed multiple films. Counting every record in the table would not give the number of directors. The SQL should use `SELECT COUNT(DISTINCT name)` in the selection to give all distinct directors.

> b) The following query returns these records: ![[Pasted image 20231105175223.png]]
> Conclusion: Stan Lee did not produce any of the movies in our database.
> ![[Pasted image 20231105175355.png]]

This is a correct conclusion, as if Stan Lee had held any other roles other than `"writer"` then these would appear as a new record in the results, because the `GROUP BY` statement uses the `position` field.

> c) The following query attempts to return records `(role, year, total)` where Jennifer Lawrence plays the same `role` during the `year` a `total` number of times in different movies. The query returns these records:![[Pasted image 20231105175818.png]]
> Conclusion: Jennifer Lawrence played Katniss Everdeen in two movies in 2012.
> ![[Pasted image 20231105175826.png]]

This is an incorrect conclusion. When the `plays_role` table is joined to itself with the `person_id` key and filtered with the `WHERE r1.role = r2.role` clause, this could result in extra records. If the actor had the same role in two movies, the join would result in 3 records from the "1st movie - 1st movie", "1st movie - 2nd movie", "2nd movie - 2nd movie", which is more than the actual number of movies where the role was played in.

Unsure how to write correct SQL query.

