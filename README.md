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
