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

```
Exercise 5.2: Retrieve particular nodes that have a relationship.
```

```
Exercise 5.3: Modify the query to retrieve nodes that are exactly three hops away.
```

```
Exercise 5.4: Modify the query to retrieve nodes that are one and two hops away.
```

```
Exercise 5.5: Modify the query to retrieve particular nodes that are connected no matter how many hops are required.
```

```
Exercise 5.6: Specify optional data to be retrieved during the query.
```

```
Exercise 5.7: Retrieve nodes by collecting a list.
```

```
Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists.
```

```
Exercise 5.10: Retrieve nodes and their relationships as lists.
```

```
Exercise 5.11: Retrieve the actors who have acted in exactly five movies.
```

```
Exercise 5.12: Retrieve the movies that have at least 2 directors with other optional data.
```

```
### Exercício 6 – Controlling results returned
Exercise 6.1: Execute a query that returns duplicate records.
```

```
Exercise 6.2: Modify the query to eliminate duplication.
```

```
Exercise 6.3: Modify the query to eliminate more duplication.
```

```
Exercise 6.4: Sort results returned.
```

```
Exercise 6.5: Retrieve the top 5 ratings and their associated movies.
```

```
Exercise 6.6: Retrieve all actors that have not appeared in more than 3 movies.
```

```
### Exercício 7 – Working with cypher data
Exercise 7.1: Collect and use lists.
```

```
Exercise 7.2: Collect a list.
```

```
Exercise 7.3: Unwind a list.
```

```
Exercise 7.4: Perform a calculation with the date type.
```

```
### Exercício 8 – Creating nodes
Exercise 8.1: Create a Movie node.
```

```
Exercise 8.2: Retrieve the newly-created node.
```

```
Exercise 8.3: Create a Person node.
```

```
Exercise 8.4: Retrieve the newly-created node.
```

```
Exercise 8.5: Add a label to a node.
```

```
Exercise 8.6: Retrieve the node using the new label.
```

```
Exercise 8.7: Add the Female label to selected nodes.
```

```
Exercise 8.8: Retrieve all Female nodes.
```

```
Exercise 8.9: Remove the Female label from the nodes that have this label.
```

```
Exercise 8.10: View the current schema of the graph.
```

```
Exercise 8.11: Add properties to a movie.
```

```
Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties.
```

```
Exercise 8.13: Add properties to the person, Robin Wright.
```

```
Exercise 8.14: Retrieve an updated Person node.
```

```
Exercise 8.15: Remove a property from a Movie node.
```

```
Exercise 8.16: Retrieve the node to confirm that the property has been removed.
```

```
Exercise 8.17: Remove a property from a Person node.
```

```
Exercise 8.18: Retrieve the node to confirm that the property has been removed.
```

```
### Exercício 9 – Creating relationships
Exercise 9.1: Create ACTED_IN relationships.
```

```
Exercise 9.2: Create DIRECTED relationships.
```

```
Exercise 9.3: Create a HELPED relationship.
```

```
Exercise 9.4: Query nodes and new relationships.
```

```
Exercise 9.5: Add properties to relationships.
```

```
Exercise 9.6: Add a property to the HELPED relationship.
```

```
Exercise 9.7: View the current list of property keys in the graph.
```

```
Exercise 9.8: View the current schema of the graph.
```

```
Exercise 9.9: Retrieve the names and roles for actors.
```

```
Exercise 9.10: Retrieve information about any specific relationships.
```

```
Exercise 9.11: Modify a property of a relationship.
```

```
Exercise 9.12: Remove a property from a relationship.
```

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
