---
title: "Getting Started"
weight: 1
aliases: ["/docs/getting-started/"]
---

Welcome to Kolibrie! This guide will help you get started quickly.

## Installation

```bash
git clone https://github.com/your-repo/kolibrie.git
cd kolibrie
cargo build --release
```

## Basic Usage

```rust
use kolibrie::parser::*;
use kolibrie::sparql_database::*;

fn main() {
    let mut database = SparqlDatabase::new();
    println!("Kolibrie database initialized!");
}
```

## Next Steps

- Check out the [Kolibrie Database](../kolibriedb/) documentation
- Explore [examples](../examples/)
