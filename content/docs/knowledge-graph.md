---
title: "Knowledge Graph"
weight: 3
aliases: ["/docs/knowledge-graph/"]
summary: "Learn to create and query knowledge graphs with RDF data using Kolibrie's powerful graph capabilities."
---

## Table of Contents

1. [Introduction to Knowledge Graphs](#introduction-to-knowledge-graphs)
2. [RDF and Knowledge Graph Components](#rdf-and-knowledge-graph-components)
3. [Creating a Knowledge Graph in Kolibrie](#creating-a-knowledge-graph-in-kolibrie)
4. [Querying Knowledge Graphs](#querying-knowledge-graphs)

   * [Basic Query](#basic-query)
   * [Advanced Query](#advanced-query)

---

## Introduction to Knowledge Graphs

A **knowledge graph** is a structured representation of interconnected data. It captures entities, relationships between entities, and attributes in a meaningful network, making it easier to explore and derive insights from complex datasets.

## RDF and Knowledge Graph Components

In RDF (Resource Description Framework), a knowledge graph consists of:

* **Entities:** Represented by subjects and objects (e.g., people, organizations).
* **Relationships:** Represented by predicates connecting entities.
* **Attributes:** Properties providing additional details about entities.

## Creating a Knowledge Graph in Kolibrie

Here's a simple example of creating a knowledge graph with Kolibrie in Rust:

```rust
use kolibrie::parser::*;
use kolibrie::sparql_database::*;

fn create_knowledge_graph() {
    let rdf_data = r#"
    <?xml version=\"1.0\" encoding=\"UTF-8\"?>
    <rdf:RDF xmlns:rdf=\"http://www.w3.org/1999/02/22-rdf-syntax-ns#\"
             xmlns:foaf=\"http://xmlns.com/foaf/0.1/\"
             xmlns:ex=\"http://example.org/\">
      <rdf:Description rdf:about=\"http://example.org/alice\">
        <foaf:name>Alice Smith</foaf:name>
        <ex:worksAt rdf:resource=\"http://example.org/company1\"/>
        <foaf:knows rdf:resource=\"http://example.org/bob\"/>
      </rdf:Description>
      <rdf:Description rdf:about=\"http://example.org/bob\">
        <foaf:name>Bob Johnson</foaf:name>
        <ex:worksAt rdf:resource=\"http://example.org/company2\"/>
      </rdf:Description>
    </rdf:RDF>
    "#;

    let mut database = SparqlDatabase::new();
    database.parse_rdf(rdf_data);
}
```

## Querying Knowledge Graphs

SPARQL queries are used to interact with knowledge graphs, retrieving entities and their relationships.

### Basic Query

Retrieve basic information about entities and their workplaces:

```sparql
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX ex: <http://example.org/>

SELECT ?person ?name ?company
WHERE {
    ?person foaf:name ?name .
    ?person ex:worksAt ?company
}
```

### Advanced Query

Query for relationships between people working at different companies:

```sparql
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX ex: <http://example.org/>

SELECT ?person1 ?person2
WHERE {
    ?person1 foaf:knows ?person2 .
    ?person1 ex:worksAt ?company1 .
    ?person2 ex:worksAt ?company2 .
    FILTER(?company1 != ?company2)
}
```

This advanced query demonstrates the power of knowledge graphs in uncovering indirect relationships and connections across diverse entities.


[→ Go to Optimization Guide](../optimization/)
