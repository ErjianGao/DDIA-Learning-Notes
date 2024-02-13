# Data Models and Query Languages

Different data models in different layers:

- Data structures
- JSON, or XML documents, table in a relational database, or graph model
- Bytes in memory, on disk, or on a network
- Electrical currents

## Relational model and Query Languages

- Dominance of relationship database has lasted around 25-30 years
- Turn out to generalize very well.

Birth of NoSQL - Not only SQL

- Greater scalability than relational database
- Free and open source software
- Specialized query operations

Object-Relational mismatch

- Translation between object and rows in RDB is required. 

How to represent one-to-many relationship? 

- Another table with foreign key
- Either perform multiple queries or perform a multi-way JOIN between the users table and its subordinate tables. 

- A way to resolve: Use JSON as a document to represent, like resume. 
  - Better locality than multi-table schema

Many-to-one and Many-to-many 

- Example: geographic regions
  - Avoid ambiguity and inconsistence
  - Easy to update
  - search
- In document database, JOINs are not needed for one-to-many structures. Support for joins is often weak. We have to emulate the process to queries databases. The work is shifted from database to the application level. 

When it comes to representing many-to-one and many-to-many relationships, relational and document databases are not fundamentally different. In both case, the related item is referenced by a unique number. 

Nowadays comparison between relational database and document database

- If there are many-to-many relationships in application, you’d better use relational databases. 
- Schema flexibility: most document databases don’t enforce any schema. Schema-on-read.
- There is a performance advantage to storage locality (document storage). But on updates to a document, the entire document usually needs to be rewritten. 
- The idea of grouping related data together is also used in relational databases, like google spanner, Hbase, Cassandra. 

## Query languages for data

Two types:

- declarative - just specify the pattern of the data, not how to achieve the goal. 
  - Easy to use a parallel implement of the query language. 
- imperative - like a programming language
  - Support better functionality. 

**Declarative queries on the web**

- CSS and XML are declarative languages. 
- Much better than manipulating styles imperatively in JavaScript. 
- In databases, declarative query languages, like SQL, turned out to be much better than imperative APIs. 

**MapReduce Querying**

*MapReduce*: a programming model for processing large amounts of data in bulk across many machines. (Batch processing)

Querying language: Neither declarative nor imperative, but something in between. 

- The query is expressed in a snippets of code, which are called by the processing framework. 

```typescript
db.observations.mapReduce(
    function map() {
    	var year = this.observationTimestamp.getFullYear();
    	var month = this.observationTimestamp.getMonth() + 1;
    	// emit the key and value
		emit(year + "-" + month, this.numAnimals);
    },
    function reduce(key, values) {
        return Array.sum(values)
    }, 
    {
        query: { family: "Sharks" },
        out: "monthlySharkReport"
    }
);
```

- Map is called once for every document that matches query
- For all the pairs with the same key, the reduce function is called once. 
- The final report is written to the collection “monthlySharkReport”
- We can only use pure functions. 
- MR is fairly low-level
- For now, we can use MongoDB’s support for a declarative query language called aggregation pipeline. 

## Graph-like data models

As the connections within the data become more complex, it becomes more natural to model the data as a graph. 

- vertices
- edges

Examples:

- network page
- road
- Social graph

But another use of graphs is to provide a consistent way of storing completely different types of objects in a single datastore. 

**Property Graphs**

Each vertex contains:

- Unique id
- Outgoing edges
- Incomming edges
- Properties (json)

Each edge contains:

- Unique id
- Head vertex
- Tail vertex
- Label (represent the relationship)
- Properties (json)

Feature:

- Easy to traverse the graph
- By using diff labels to store diff kinds relationships
- Great for evolvability

**The Cypher Query Language**

Create several vertices and connect them together. 

```cypher
CREATE  (NAmerica:Location {name:'North America', type:'continent'}),  (USA:Location      {name:'United States', type:'country'  }),  (Idaho:Location    {name:'Idaho',         type:'state'    }),  (Lucy:Person       {name:'Lucy' }), (Idaho) -[:WITHIN]->  (USA)  -[:WITHIN]-> (NAmerica),  (Lucy)  -[:BORN_IN]-> (Idaho)
```

Query the needed vertices:

```cypher
MATCH  (person) -[:BORN_IN]->  () -[:WITHIN*0..]-> (us:Location {name:'United States'}),  (person) -[:LIVES_IN]-> () -[:WITHIN*0..]-> (eu:Location {name:'Europe'})
RETURN person.name
```

You eventually reach a vertex whose location name is United States.

`*0..`means 0 or more times. 

**Triple-Stores and SPARQL**

All information is stored in the form of three-part statements: (subject, predicate, object). For example, Tom likes apples.

- Subject: tail vertex
- Predicate: edge
- Object: head vertex

**Datalog**

Datalog’s data model is similar to triple-store model. 

**Other databases**

- Full-text search is also a kind of data model that is frequently used alongside databases. 
