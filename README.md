# neo4j

## Atividade Prática – Neo4j

### Exercício 1- Retrieving Nodes

Exercise 1.1: Retrieve all nodes from the database.
```
MATCH (n) RETURN n
```
Exercise 1.2: Examine the data model for the graph.
```
call db.schema.visualization()
```
Exercise 1.3: Retrieve all Person nodes.
```
MATCH (p:Person) RETURN p
```
Exercise 1.4: Retrieve all Movie nodes
```
MATCH (m:Movie) RETURN m
```

### Exercício 2 – Filtering queries using property values

Exercise 2.1: Retrieve all movies that were released in a specific year.
```
MATCH (m:Movie {released:2004}) RETURN m
```
Exercise 2.2: View the retrieved results as a table.
```
{
  "title": "The Polar Express",
  "tagline": "This Holiday Season… Believe",
  "released": 2004
}
```
Exercise 2.3: Query the database for all property keys.
```
CALL db.propertyKeys
╒═════════════╕
│"propertyKey"│
╞═════════════╡
│"tagline"    │
├─────────────┤
│"title"      │
├─────────────┤
│"released"   │
├─────────────┤
│"born"       │
├─────────────┤
│"name"       │
├─────────────┤
│"roles"      │
├─────────────┤
│"summary"    │
├─────────────┤
│"rating"     │
└─────────────┘
```
Exercise 2.4: Retrieve all Movies released in a specific year, returning their titles.
```
MATCH (m:Movie {released: 2003}) RETURN m.title
╒════════════════════════╕
│"m.title"               │
╞════════════════════════╡
│"The Matrix Reloaded"   │
├────────────────────────┤
│"The Matrix Revolutions"│
├────────────────────────┤
│"Something's Gotta Give"│
└────────────────────────┘
```
Exercise 2.5: Display title, released, and tagline values for every Movie node in the graph.
```
MATCH (m:Movie) RETURN m.title, m.released, m.tagline
```
Exercise 2.6: Display more user-friendly headers in the table
```
MATCH (m:Movie) RETURN m.title AS `movie title`, m.released AS released, m.tagline AS tagLine
```
### Exercício 3 - Filtering queries using relationships
Exercise 3.1: Display the schema of the database.
```
CALL db.schema.visualization
```
Exercise 3.2: Retrieve all people who wrote the movie Speed Racer.
```
MATCH (p:Person)-[:WROTE]->(:Movie {title: 'Speed Racer'}) RETURN p
```
Exercise 3.3: Retrieve all movies that are connected to the person, Tom Hanks.
```
MATCH (m:Movie)<--(:Person {name: 'Tom Hanks'}) RETURN m
```
Exercise 3.4: Retrieve information about the relationships Tom Hanks had with the set of movies retrieved earlier.
```
MATCH (m:Movie)-[rel]-(:Person {name: 'Tom Hanks'}) RETURN m.title, type(rel)
```
Exercise 3.5: Retrieve information about the roles that Tom Hanks acted in.
```
MATCH (m:Movie)-[rel:ACTED_IN]-(:Person {name: 'Tom Hanks'}) RETURN m.title, rel.roles
```
### Exercício 4 – Filtering queries using WHERE clause
Exercise 4.1: Retrieve all movies that Tom Cruise acted in.
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE a.name = 'Tom Cruise'
RETURN m.title as Movie
```
Exercise 4.2: Retrieve all people that were born in the 70’s.
```
MATCH (a:Person)
WHERE a.born >= 1970 AND a.born < 1980
RETURN a.name as Nome, a.born as `Ano Nascimento`
```
Exercise 4.3: Retrieve the actors who acted in the movie The Matrix who were born after 1960.
```
MATCH (a:Person)-[rel:ACTED_IN]-(m:Movie)
WHERE a.born > 1960
AND m.title = 'The Matrix'
RETURN a.name as Nome, a.born as `Ano Nascimento`, m.title as `Filme`
```
Exercise 4.4: Retrieve all movies by testing the node label and a property.
```
MATCH (p) WHERE p:Person AND p.born >= 1960 RETURN p.name
```
Exercise 4.5: Retrieve all people that wrote movies by testing the relationship between two nodes.
```
MATCH (a)-[rel]->(m) WHERE a:Person AND type(rel) = 'WROTE' AND m:Movie RETURN a.name as Name, m.title as Movie
```
Exercise 4.6: Retrieve all people in the graph that do not have a property.
```
MATCH (a:Person) WHERE NOT exists(a.born) RETURN a
```
Exercise 4.7: Retrieve all people related to movies where the relationship has a property.
```
call db.schema.relTypeProperties
MATCH (a:Person)-[rel]->(m:Movie) WHERE exists(rel.roles) RETURN a.name as Name, m.title as Movie, rel.roles as Roles
```
Exercise 4.8: Retrieve all actors whose name begins with James.
```
MATCH (a:Person)-[:ACTED_IN]->(:Movie) WHERE a.name STARTS WITH 'James' RETURN a.name
```
Exercise 4.9: Retrieve all all REVIEW relationships from the graph with filtered results.
```
MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title in r.roles
RETURN  m.title as Movie, a.name as Actor
```
Exercise 4.10: Retrieve all people who have produced a movie, but have not directed a movie.
```
MATCH (a:Person)-[:PRODUCED]->(m:Movie) WHERE NOT ((a)-[:DIRECTED]->(:Movie)) RETURN a.name, m.title
```
Exercise 4.11: Retrieve the movies and their actors where one of the actors also directed the movie.
```
MATCH (a1:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(a2:Person)
WHERE exists( (a2)-[:DIRECTED]->(m) )
RETURN  a1.name as Actor, a2.name as `Actor/Director`, m.title as Movie
```
Exercise 4.12: Retrieve all movies that were released in a set of years.
```
MATCH (m:Movie)
WHERE m.released in [2000, 2003, 2005]
RETURN m.title, m.released
```
Exercise 4.13: Retrieve the movies that have an actor’s role that is the name of the movie.
```
MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title in r.roles
RETURN  m.title as Movie, a.name as Actor
```
### Exercício 5 – Controlling query processing
Exercise 5.1: Retrieve data using multiple MATCH patterns.
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person), (a2:Person)-[:ACTED_IN]->(m) 
WHERE a.name = 'Gene Hackman' 
RETURN m.title as movie, d.name AS director , a2.name AS co-actors
```
Exercise 5.2: Retrieve particular nodes that have a relationship.
```
MATCH (follower:Person)-[:FOLLOWS]-(p:Person) 
WHERE follower.name = 'James Thompson' RETURN follower, p
```
Exercise 5.3: Modify the query to retrieve nodes that are exactly three hops away.
```
MATCH (follower:Person)-[:FOLLOWS*3]-(p:Person) 
RETURN follower, p
```
Exercise 5.4: Modify the query to retrieve nodes that are one and two hops away.
```
MATCH (follower:Person)-[:FOLLOWS*1..2]-(p:Person) 
RETURN follower, p
```
Exercise 5.5: Modify the query to retrieve particular nodes that are connected no matter how many hops are required.
```
MATCH (follower:Person)-[:FOLLOWS*]-(p:Person) 
WHERE follower.name = 'James Thompson' 
RETURN follower, p
```
Exercise 5.6: Specify optional data to be retrieved during the query.
```
MATCH (p:Person) 
WHERE p.name STARTS WITH 'James' 
OPTIONAL MATCH (p)-[r:DIRECTED]->(m:Movie) 
RETURN p.name, type(r), m.title
```
Exercise 5.7: Retrieve nodes by collecting a list.
```
MATCH (a:Person)-[:ACTED_IN]->(m:Movie) 
WITH m, collect(m.title) as movies 
RETURN a.name, movies
```
Exercise 5.8: Retrieve all movies that Tom Cruise has acted in and the co-actors that acted in the same movie by collecting a list
```
MATCH (:Person {name: 'Tom Cruise'})-[:ACTED_IN]->(movies:Movie)<-[:ACTED_IN]-(people:Person)
WITH movies, collect(people.name) as co_actors
RETURN movies.title AS `Movies`, co_actors AS `Co-actors`
```
Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists.
```
MATCH (people:Person)-[:REVIEWED]->(movies:Movie)
WITH movies, count(people) as numReviewers, collect(people.name) as reviewers
RETURN movies.title AS `Movie`, numReviewers AS `Number of Reviewers`, reviewers AS `Reviewers`
```
Exercise 5.10: Retrieve nodes and their relationships as lists.
```
MATCH (directors:Person)-[:DIRECTED]->(movies:Movie)<-[:ACTED_IN]-(actors:Person)
WITH directors, count(actors) as actors_count, collect(actors.name) as actors_list
RETURN directors.name AS `Director`, actors_count AS `Number of Actors`, actors_list AS `Actors`
```
Exercise 5.11: Retrieve the actors who have acted in exactly five movies.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)
WITH actors, count(movies) as movies_count, collect(movies.title) as movies_list
WHERE movies_count = 5
RETURN actors.name AS `Actor`, movies_list AS `Movies`
```
Exercise 5.12: Retrieve the movies that have at least 2 directors with other optional data.
```
MATCH (directors:Person)-[:DIRECTED]->(movies:Movie)
OPTIONAL MATCH (reviewers:Person)-[:REVIEWED]->(movies)
WITH movies, count(DISTINCT directors) as numDirectors, collect(DISTINCT directors.name) as directors_list, collect(DISTINCT reviewers.name) as reviewers_list
WHERE numDirectors >= 2
RETURN movies.title AS `Movie`, directors_list AS `Directors`, reviewers_list AS `Reviewers`
```
### Exercício 6 – Controlling results returned
Exercise 6.1: Execute a query that returns duplicate records.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)
WHERE movies.released >= 1990 AND movies.released < 2000
RETURN movies.released AS `Release Year`, movies.title AS `Movie`, collect(actors.name) AS `Actors` 
```
Exercise 6.2: Modify the query to eliminate duplication.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)
WHERE movies.released >= 1990 AND movies.released < 2000
RETURN movies.released AS `Release Year`, collect(movies.title) AS `Movie`, collect(actors.name) AS `Actors` 
```
Exercise 6.3: Modify the query to eliminate more duplication.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)
WHERE movies.released >= 1990 AND movies.released < 2000
RETURN movies.released AS `Release Year`, collect(DISTINCT movies.title) AS `Movie`, collect(actors.name) AS `Actors` 
```
Exercise 6.4: Sort results returned.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)
WHERE movies.released >= 1990 AND movies.released < 2000
RETURN movies.released AS `Release Year`, collect(DISTINCT movies.title) AS `Movie`, collect(actors.name) AS `Actors`
ORDER BY movies.released DESC
```
Exercise 6.5: Retrieve the top 5 ratings and their associated movies.
```
MATCH (:Person)-[rel:REVIEWED]-(movies:Movie)
RETURN rel.rating AS `Rating`, movies.title AS `Movie`
ORDER BY rel.rating DESC
LIMIT 5
```
Exercise 6.6: Retrieve all actors that have not appeared in more than 3 movies.
```
MATCH (actors:Person)-[:ACTED_IN]-(movies:Movie)
WITH actors, count(movies) as numMovies, collect(movies.title) as movies_list
WHERE numMovies <= 3
RETURN actors.name AS `Actor`, movies_list AS `Movies`
```
### Exercício 7 – Working with cypher data
Exercise 7.1: Collect and use lists.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)<-[:PRODUCED]-(producers:Person)
WITH movies, collect(DISTINCT actors.name) as actors_list, collect(DISTINCT producers.name) as producers_list
RETURN DISTINCT movies.title AS `Movie`, actors_list AS `Actors`, producers_list AS `Producers`
ORDER BY size(actors_list)
```
Exercise 7.2: Collect a list.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)
WITH actors, collect(movies.title) as movies_list
WHERE size(movies_list) > 5
RETURN actors.name AS `Actor/Actress`, movies_list AS `Movie`
```
Exercise 7.3: Unwind a list.
```
MATCH (actors:Person)-[:ACTED_IN]->(movies:Movie)
WITH actors, collect(movies.title) as movies_list
WHERE size(movies_list) > 5
WITH actors, movies_list UNWIND movies_list as movies_unwinded 
RETURN actors.name AS `Actor/Actress`, movies_unwinded AS `Movie`
```
Exercise 7.4: Perform a calculation with the date type.
```
MATCH (tomHanks:Person {name: 'Tom Hanks'})-[:ACTED_IN]->(movies:Movie)
RETURN movies.title AS `Movie`,
movies.released AS `Year Released`,
date().year - movies.released AS `Age of the Movie`,
movies.released - tomHanks.born AS `Tom Hanks' Age when the Movie was Realesed`
ORDER BY movies.released DESC
```
### Exercício 8 – Creating nodes
Exercise 8.1: Create a Movie node.
```
CREATE (:Movie {title: 'Forrest Gump'})
```
Exercise 8.2: Retrieve the newly-created node.
```
MATCH (forrest:Movie {title: 'Forrest Gump'})
RETURN forrest
```
Exercise 8.3: Create a Person node.
```
CREATE (:Person {name: 'Robin Wright'})
```
Exercise 8.4: Retrieve the newly-created node.
```
MATCH (robin:Person {name: 'Robin Wright'})
RETURN robin
```
Exercise 8.5: Add a label to a node.
```
MATCH (movies:Movie)
WHERE movies.released < 2010
SET movies:OlderMovie
```
Exercise 8.6: Retrieve the node using the new label.
```
MATCH (movies:Movie:OlderMovie)
RETURN movies.title AS `Movie`, movies.released AS `Year Released`
ORDER BY movies.released DESC
```
Exercise 8.7: Add the Female label to selected nodes.
```
MATCH (people:Person)
WHERE people.name =~ 'Robin.*'
SET people:Female
```
Exercise 8.8: Retrieve all Female nodes.
```
MATCH (women:Female)
RETURN women.name AS `Name`
```
Exercise 8.9: Remove the Female label from the nodes that have this label.
```
MATCH (women:Female)
REMOVE women:Female
```
Exercise 8.10: View the current schema of the graph.
```
CALL db.schema.visualization()
```
Exercise 8.11: Add properties to a movie.
```
MATCH (m:Movie) 
WHERE m.title = 'Forrest Gump' 
SET m:OlderMovie, m.released = 1994, m.tagline = "Life is like a box of chocolates…you never know what you’re gonna get.", m.lengthInMinutes = 142 
RETURN m
```
Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties.
```
MATCH (m:OlderMovie) 
WHERE m.title = 'Forrest Gump' 
RETURN m
```
Exercise 8.13: Add properties to the person, Robin Wright.
```
MATCH (p:Person) 
WHERE p.name = 'Robin Wright' 
SET p.born = 1966, p.birthPlace = "Dallas" 
RETURN p
```
Exercise 8.14: Retrieve an updated Person node.
```
MATCH (p:Person) 
WHERE p.name = 'Robin Wright' 
RETURN p
```
Exercise 8.15: Remove a property from a Movie node.
```
MATCH (m:Movie) 
WHERE m.title = 'Forrest Gump' 
REMOVE m.lengthInMinutes 
RETURN m
```
Exercise 8.16: Retrieve the node to confirm that the property has been removed.
```
MATCH (m:Movie) 
WHERE m.title = 'Forrest Gump' 
RETURN m
```
Exercise 8.17: Remove a property from a Person node.
```
MATCH (p:Person) 
WHERE p.name = 'Robin Wright' 
REMOVE p.birthPlace 
RETURN p
```
Exercise 8.18: Retrieve the node to confirm that the property has been removed.
```
MATCH (p:Person) 
WHERE p.name = 'Robin Wright' 
RETURN p
```
### Exercício 9 – Creating relationships
Exercise 9.1: Create ACTED_IN relationships.
```
MATCH (a:Person), (m:Movie) 
WHERE a.name = 'Robin Wright' 
AND m.title = 'Forrest Gump' 
CREATE (a)-[:ACTED_IN]->(m) RETURN a, m
```
```
MATCH (a:Person), (m:Movie) 
WHERE a.name = 'Tom Hanks' 
AND m.title = 'Forrest Gump' 
CREATE (a)-[:ACTED_IN]->(m) RETURN a, m
```
```
MATCH (a:Person), (m:Movie) 
WHERE a.name = 'Gary Sinise' 
AND m.title = 'Forrest Gump' 
CREATE (a)-[:ACTED_IN]->(m) 
RETURN a, m
```
Exercise 9.2: Create DIRECTED relationships.
```
MATCH (a:Person), (m:Movie) 
WHERE a.name = 'Robert Zemeckis' 
AND m.title = 'Forrest Gump' 
CREATE (a)-[:DIRECTED]->(m) RETURN a, m
```
Exercise 9.3: Create a HELPED relationship.
```
MATCH (p1:Person) 
WHERE p1.name = 'Tom Hanks' 
MATCH (p2:Person) 
WHERE p2.name = 'Gary Sinise' 
CREATE (p1)-[:HELPED]->(p2)
```
Exercise 9.4: Query nodes and new relationships.
```
MATCH (p:Person)-[r]-(m:Movie) 
WHERE m.title = 'Forrest Gump' 
RETURN p, r, m
```
Exercise 9.5: Add properties to relationships.
```
MATCH (a:Person), (m:Movie) 
WHERE a.name = 'Tom Hanks' 
AND m.title = 'Forrest Gump' 
CREATE (a)-[rel:ACTED_IN]->(m) 
SET rel.roles = 'Forrest Gum' 
RETURN a, m
```
```
MATCH (a:Person), (m:Movie) 
WHERE a.name = 'Robin Wright' 
AND m.title = 'Forrest Gump' 
CREATE (a)-[rel:ACTED_IN]->(m) 
SET rel.roles = 'Jenny Curran' 
RETURN a, m
```
```
MATCH (a:Person), (m:Movie) 
WHERE a.name = 'Gary Sinise' 
AND m.title = 'Forrest Gump' 
CREATE (a)-[rel:ACTED_IN]->(m) 
SET rel.roles = 'Lieutenant Dan Taylor' 
RETURN a, m
```
Exercise 9.6: Add a property to the HELPED relationship.
```
MATCH (p1:Person)-[rel:HELPED]->(p2:Person) 
WHERE p1.name = 'Tom Hanks' 
AND p2.name = 'Gary Sinise' 
SET rel.research = 'war history'
```
Exercise 9.7: View the current list of property keys in the graph.
```
CALL db.propertyKeys
```
Exercise 9.8: View the current schema of the graph.
```
CALL db.schema.visualization()
```
Exercise 9.9: Retrieve the names and roles for actors.
```
MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie) 
WHERE m.title = 'Forrest Gump' 
RETURN p.name, rel.roles
```
Exercise 9.10: Retrieve information about any specific relationships.
```
MATCH (p:Person)-[rel:HELPED]-(p2:Person) 
RETURN p, p2
```
Exercise 9.11: Modify a property of a relationship.
```
MATCH (p:Person)-[rel:ACTED_IN]->(m:Movie) 
WHERE m.title = 'Forrest Gump' 
AND p.name = 'Gary Sinise' 
SET rel.roles =['Lt. Dan Taylor']
```
Exercise 9.12: Remove a property from a relationship.
```
MATCH (a:Person)-[rel:HELPED]->(p2:Person) 
WHERE a.name = 'Tom Hanks' 
AND p2.name = 'Gary Sinise' 
REMOVE rel.research 
RETURN a, rel, p2
```
Exercise 9.13: Confirm that your modifications were made to the graph.
```

```
### Exercício 10 – Deleting nodes and relationships
Exercise 10.1: Delete a relationship.
```

```
Exercise 10.2: Confirm that the relationship has been deleted.
```

```
Exercise 10.3: Retrieve a movie and all of its relationships.
```

```
Exercise 10.4: Try deleting a node without detaching its relationships.
```

```
Exercise 10.5: Delete a Movie node, along with its relationships.
```

```
Exercise 10.6: Confirm that the Movie node has been deleted.
```

```
