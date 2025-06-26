---
title: "API Reference"
weight: 5
aliases: ["/docs/api-reference/", "/api/"]
---

## Table of Contents

1. [QueryBuilder Overview](#querybuilder-overview)
2. [Basic Filtering](#basic-filtering)
3. [Advanced Filtering](#advanced-filtering)
4. [Joining Databases](#joining-databases)
5. [Sorting and Ordering](#sorting-and-ordering)
6. [Result Retrieval](#result-retrieval)
7. [Aggregation Functions](#aggregation-functions)
8. [Python API](#python-api)

---

## QueryBuilder Overview

The `QueryBuilder` provides a fluent interface for constructing and executing queries against RDF triple stores. It supports filtering, joining, sorting, and various result formats.

### Creating a QueryBuilder

**Rust:**
```rust
use kolibrie::sparql_database::SparqlDatabase;
use kolibrie::query_builder::QueryBuilder;

let db = SparqlDatabase::new();
let query = QueryBuilder::new(&db);
```

**Python:**
```python
from py_kolibrie import PySparqlDatabase

db = PySparqlDatabase()
query = db.query()
```

---

## Basic Filtering

### Subject Filtering

#### `with_subject(subject: &str)`
Filter triples by exact subject value.

**What it does:** Returns only triples where the subject exactly matches the provided string.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .with_subject("http://example.org/Alice")
    .get_decoded_triples();
```

**Python Example:**
```python
results = (db.query()
    .with_subject("http://example.org/Alice")
    .get_decoded_triples())
```

#### `with_subject_like(pattern: &str)`
Filter triples by subject containing a substring.

**What it does:** Returns triples where the subject contains the specified pattern as a substring.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .with_subject_like("example.org")
    .get_decoded_triples();
```

#### `with_subject_starting(prefix: &str)`
Filter triples by subject starting with a prefix.

**What it does:** Returns triples where the subject begins with the specified prefix.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .with_subject_starting("http://example.org/")
    .get_decoded_triples();
```

#### `with_subject_ending(suffix: &str)`
Filter triples by subject ending with a suffix.

**What it does:** Returns triples where the subject ends with the specified suffix.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .with_subject_ending("Alice")
    .get_decoded_triples();
```

### Predicate Filtering

#### `with_predicate(predicate: &str)`
Filter triples by exact predicate value.

**What it does:** Returns only triples where the predicate exactly matches the provided string.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .with_predicate("http://example.org/knows")
    .get_decoded_triples();
```

**Python Example:**
```python
results = (db.query()
    .with_predicate("http://example.org/knows")
    .get_decoded_triples())
```

#### `with_predicate_like(pattern: &str)`
Filter triples by predicate containing a substring.

**What it does:** Returns triples where the predicate contains the specified pattern.

#### `with_predicate_starting(prefix: &str)`
Filter triples by predicate starting with a prefix.

#### `with_predicate_ending(suffix: &str)`
Filter triples by predicate ending with a suffix.

### Object Filtering

#### `with_object(object: &str)`
Filter triples by exact object value.

**What it does:** Returns only triples where the object exactly matches the provided string.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .with_object("http://example.org/Bob")
    .get_decoded_triples();
```

#### `with_object_like(pattern: &str)`
Filter triples by object containing a substring.

#### `with_object_starting(prefix: &str)`
Filter triples by object starting with a prefix.

#### `with_object_ending(suffix: &str)`
Filter triples by object ending with a suffix.

---

## Advanced Filtering

#### `filter<F>(predicate: F)`
Apply a custom filter function to all triples.

**What it does:** Applies a user-defined function to filter triples based on custom logic.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .filter(|triple| {
        // Custom logic: only include triples where subject contains "Alice"
        db.dictionary.decode(triple.subject)
            .map(|s| s.contains("Alice"))
            .unwrap_or(false)
    })
    .get_decoded_triples();
```

---

## Joining Databases

#### `join(other: &SparqlDatabase)`
Join with another SPARQL database.

**What it does:** Prepares to join the current query results with triples from another database.

#### `join_on_subject()`
Specify join condition on subject.

**What it does:** Joins triples where the subject values match between databases.

**Rust Example:**
```rust
let other_db = SparqlDatabase::new();
// ... populate other_db ...

let results = QueryBuilder::new(&db)
    .join(&other_db)
    .join_on_subject()
    .get_decoded_triples();
```

#### `join_on_predicate()`
Specify join condition on predicate.

**What it does:** Joins triples where the predicate values match between databases.

#### `join_on_object()`
Specify join condition on object.

**What it does:** Joins triples where the object values match between databases.

#### `join_with<F>(condition: F)`
Specify a custom join condition.

**What it does:** Joins triples based on a user-defined condition function.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .join(&other_db)
    .join_with(|left, right| {
        // Custom join logic
        left.subject == right.object
    })
    .get_decoded_triples();
```

---

## Sorting and Ordering

#### `order_by<F>(key: F)`
Order results by a specified key function.

**What it does:** Sorts the results based on a key extracted from each triple.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .order_by(|triple| {
        db.dictionary.decode(triple.subject).unwrap_or("").to_string()
    })
    .get_decoded_triples();
```

#### `asc()`
Set sort direction to ascending (default).

**What it does:** Sorts results in ascending order.

#### `desc()`
Set sort direction to descending.

**What it does:** Sorts results in descending order.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .order_by(|triple| {
        db.dictionary.decode(triple.subject).unwrap_or("").to_string()
    })
    .desc()
    .get_decoded_triples();
```

---

## Result Retrieval

#### `distinct()`
Return only distinct results.

**What it does:** Removes duplicate triples from the result set.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .with_predicate_like("knows")
    .distinct()
    .get_decoded_triples();
```

**Python Example:**
```python
results = (db.query()
    .with_predicate_like("knows")
    .distinct()
    .get_decoded_triples())
```

#### `limit(n: usize)`
Limit the number of results.

**What it does:** Returns at most `n` results from the query.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .limit(10)
    .get_decoded_triples();
```

**Python Example:**
```python
results = (db.query()
    .limit(10)
    .get_decoded_triples())
```

#### `offset(n: usize)`
Skip the first n results.

**What it does:** Skips the first `n` results, useful for pagination.

**Rust Example:**
```rust
let results = QueryBuilder::new(&db)
    .offset(20)
    .limit(10)  // Get results 21-30
    .get_decoded_triples();
```

#### `get_decoded_triples()`
Get results as decoded (subject, predicate, object) tuples.

**What it does:** Returns human-readable string tuples instead of encoded internal IDs.

**Return Type:** `Vec<(String, String, String)>`

#### `get_subjects()`
Get only the subjects from the results.

**What it does:** Extracts and returns only the subject components from matching triples.

**Return Type:** `Vec<String>`

**Rust Example:**
```rust
let subjects = QueryBuilder::new(&db)
    .with_predicate("http://example.org/knows")
    .distinct()
    .get_subjects();
```

#### `get_predicates()`
Get only the predicates from the results.

**What it does:** Extracts and returns only the predicate components from matching triples.

**Return Type:** `Vec<String>`

#### `get_objects()`
Get only the objects from the results.

**What it does:** Extracts and returns only the object components from matching triples.

**Return Type:** `Vec<String>`

#### `get_triples()`
Get the raw triple results.

**What it does:** Returns the internal triple representation (encoded IDs).

**Return Type:** `BTreeSet<Triple>`

---

## Aggregation Functions

#### `count()`
Count the number of results without retrieving them.

**What it does:** Returns the total number of triples that match the query conditions without materializing the results.

**Return Type:** `usize`

**Rust Example:**
```rust
let count = QueryBuilder::new(&db)
    .with_predicate("http://example.org/knows")
    .count();
println!("Found {} relationships", count);
```

**Python Example:**
```python
count = (db.query()
    .with_predicate("http://example.org/knows")
    .count())
print(f"Found {count} relationships")
```

#### `group_by<F, K>(key_fn: F)`
Group results by a key function.

**What it does:** Groups the matching triples by a key extracted from each triple.

**Return Type:** `BTreeMap<K, Vec<Triple>>`

**Rust Example:**
```rust
let groups = QueryBuilder::new(&db)
    .group_by(|triple| triple.predicate);

for (predicate_id, triples) in groups {
    println!("Predicate {}: {} triples", predicate_id, triples.len());
}
```

---

## Python API

The Python API provides the same functionality through a more Pythonic interface:

### Complete Python Example

```python
from py_kolibrie import PySparqlDatabase

def main():
    # Create database and add data
    db = PySparqlDatabase()
    db.add_triple("http://example.org/Alice", "http://example.org/knows", "http://example.org/Bob")
    db.add_triple("http://example.org/Bob", "http://example.org/knows", "http://example.org/Carol")
    db.add_triple("http://example.org/Alice", "http://example.org/likes", "http://example.org/IceCream")
    
    # Build and execute query
    query = (db.query()
        .with_subject("http://example.org/Alice")
        .distinct()
        .limit(20))
    
    # Get different result formats
    triples = query.get_decoded_triples()
    print("Decoded triples:")
    for s, p, o in triples:
        print(f"  {s} -- {p} --> {o}")
    
    subjects = query.get_subjects()
    print("\nDistinct subjects:")
    for s in subjects:
        print(" ", s)
    
    count = query.count()
    print(f"\nTotal matching triples: {count}")

if __name__ == "__main__":
    main()
```

### Method Chaining

Both Rust and Python APIs support fluent method chaining:

**Python:**
```python
results = (db.query()
    .with_subject_like("example.org")
    .with_predicate("http://example.org/knows")
    .distinct()
    .limit(50)
    .get_decoded_triples())
```

**Rust:**
```rust
let results = QueryBuilder::new(&db)
    .with_subject_like("example.org")
    .with_predicate("http://example.org/knows")
    .distinct()
    .limit(50)
    .get_decoded_triples();
```

[→ Go to Examples](../examples/)
