---
title: "Kolibrie Database Tutorial"
weight: 2
aliases: ["/docs/kolibrie-tutorial/"]
summary: "Learn Kolibrie's system architecture, SPARQL queries, and advanced processing modes with practical examples."
---

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Getting Started](#getting-started)
3. [Introduction to SPARQL](#introduction-to-sparql)
4. [Writing Queries](#writing-queries)

   * [Basic Queries](#basic-queries)
   * [Aggregations](#aggregations)
   * [Filtering and Bindings](#filtering-and-bindings)
   * [Nested Queries](#nested-queries)
   * [Data Insertion](#data-insertion)
   * [Advanced Examples](#advanced-examples)
5. [Advanced Processing Modes](#advanced-processing-modes)

---

## System Architecture

Kolibrie Database handles large-scale RDF data using a modular architecture:

* **Triple Store:** Stores RDF triples.
* **Dictionary:** Encodes and decodes RDF terms efficiently.
* **Index Manager:** Accelerates query processing.
* **Query Parser:** Parses SPARQL queries into executable plans.
* **Query Executor:** Executes queries with parallelism and SIMD optimizations.
* **User-Defined Functions (UDFs):** Enables custom functions.
* **Stream Manager:** Manages real-time data streams and windowing.

---

## Getting Started

Ensure KolibrieDB is installed and configured. Import required modules in Rust:

```rust
use kolibrie::parser::*;
use kolibrie::sparql_database::*;
```

---

## Introduction to SPARQL

SPARQL (SPARQL Protocol and RDF Query Language) is a powerful query language for retrieving and manipulating data stored in RDF (Resource Description Framework) format. RDF data is represented as triples, consisting of subject, predicate, and object. SPARQL queries match patterns in RDF triples to extract relevant data.

### SPARQL Syntax Basics

* **PREFIX**: Defines namespaces to simplify URIs.
* **SELECT**: Retrieves data matching query patterns.
* **WHERE**: Specifies triple patterns for data retrieval.
* **FILTER**: Restricts query results based on conditions.
* **BIND**: Assigns a value to a variable within a query.
* **GROUP BY**: Aggregates results based on specified variables.
* **INSERT**: Adds new data triples into the RDF dataset.

Example query structure:

```sparql
PREFIX ex: <http://example.org/>
SELECT ?subject ?predicate ?object
WHERE {
  ?subject ?predicate ?object .
  FILTER(?predicate = ex:someProperty)
}
```

---

## Writing Queries

### Basic Queries

Retrieve individuals by occupation:

```sparql
PREFIX ex: <http://example.org/>
SELECT ?person WHERE {
  ?person ex:hasOccupation "Engineer"
}
```

### Aggregations

Calculate average salary:

```sparql
PREFIX ds: <https://data.cityofchicago.org/resource/xzkq-xp2w/>
SELECT AVG(?salary) AS ?average_salary WHERE {
  ?employee ds:annual_salary ?salary
}
GROUP BY ?average_salary
```

### Filtering and Bindings

Filter by author:

```sparql
PREFIX dc: <http://purl.org/dc/elements/1.1/>
SELECT ?title ?author WHERE {
  ?book dc:title ?title .
  ?book dc:creator ?author .
  FILTER (?author = "Jane Austen")
}
```

Concatenate names:

```sparql
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
SELECT ?name WHERE {
  ?P foaf:givenName ?G .
  ?P foaf:surname ?S .
  BIND(CONCAT(?G, " ", ?S) AS ?name)
}
```

### Nested Queries

Find names of friends connected to Alice:

```sparql
PREFIX ex: <http://example.org/>
SELECT ?friendName WHERE {
  ?person ex:name "Alice" .
  ?person ex:knows ?friend {
    SELECT ?friend ?friendName WHERE {
      ?friend ex:name ?friendName
    }
  }
}
```

### Data Insertion

Insert a new triple:

```sparql
PREFIX ex: <http://example.org/>
INSERT {
  <http://example.org/JohnDoe> ex:occupation "Software Developer"
} WHERE {
  <http://example.org/JohnDoe> ex:age "30"
}
```

### Advanced Examples

Join multiple RDF descriptions:

```sparql
PREFIX ex: <http://example.org/>
SELECT ?person ?location ?city ?zipcode WHERE {
  ?person ex:worksAt ?location .
  ?location ex:located ?city .
  ?location ex:zipcode ?zipcode
}
```

Integrate ML predictions:

```sparql
RULE :DetectCongestion() :-
CONSTRUCT {?road ex:congestionLevel ?level.}
WHERE {
  ?d ex:road ?road;
     ex:avgVehicleSpeed ?speed;
     ex:vehicleCount ?count.
}
ML.PREDICT(MODEL "congestion_model",
  INPUT {
    SELECT ?road (AVG(?speed) AS ?avgSpeed) (MAX(?count) AS ?maxCount)
    WHERE {
      ?d ex:road ?road;
         ex:avgVehicleSpeed ?speed;
         ex:vehicleCount ?count.
    }
  }, OUTPUT ?level)
```

---

## Advanced Processing Modes

### Single-Threaded Processing

Process RDF data in single-threaded mode:

```rust
database.parse_rdf(&file);
```

### Multi-Threaded Processing

Leverage multi-threading:

```rust
database.parse_rdf_from_file(file_path);
```

### CUDA-Enabled Processing

GPU-accelerated query processing:

Setup:

For Unix:

```shell
export LD_LIBRARY_PATH=<path>:$LD_LIBRARY_PATH
cmake .
cmake --build .
```

For Windows:

```shell
cmake -G "NMake Makefiles" -DCMAKE_BUILD_TYPE=Release .
cmake --build .
```

Rust example with CUDA:

```rust
#[gpu::main]
fn main() {
    database.parse_rdf_from_file(file_path);
    // Execute queries with GPU acceleration
}
```

[→ Go to Knowledge Graph Documentation](/knowledge-graph/)