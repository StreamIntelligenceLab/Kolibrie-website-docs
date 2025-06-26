---
title: "Introduction"
weight: 2
aliases: ["/docs/kolibriedb/"]
summary: "Comprehensive documentation of the Kolibrie Database Engine, a high-performance, scalable system for managing RDF data with SPARQL queries."
---

Welcome to the comprehensive documentation of the **Kolibrie Database Engine**, a high-performance, scalable, and extensible system designed to manage and query RDF (Resource Description Framework) data using the SPARQL query language. This documentation aims to provide an in-depth understanding of the engine's architecture, data structures, indexing mechanisms, query parsing, execution strategies, and performance optimizations. Whether you are a database architect, a software developer, or an academic researcher, this guide will equip you with the knowledge required to effectively utilize and extend the database engine.

## What is Stream Reasoning?

**Stream reasoning** is continuous, real-time logical reasoning on dynamic, heterogeneous data streams. Unlike static reasoning, it handles fast-changing, noisy, and incomplete information. Its primary goal is to deliver timely and actionable insights by continuously analyzing streaming data in domains like IoT, smart cities, social media, and finance.

Key challenges include handling data volume and velocity using sliding windows to limit processing to recent data, and performing complex inference under low-latency conditions.

## Domains Contributing to Stream Reasoning

Stream reasoning combines strengths from multiple research areas:

* **Stream Processing:** Offers techniques for continuous queries and real-time data handling through sliding windows.
* **Knowledge Graphs:** Provide semantic structure using RDF and OWL, enabling the integration of heterogeneous data streams.
* **Logic Reasoning:** Supports inference based on explicit rules, enabling semantically sound and explainable decisions.
* **Machine Learning:** Manages noisy data, discovers patterns, and enables predictive analysis, complementing symbolic logic with inductive reasoning.

## Real-World Example: Smart Traffic Scenario

In a smart-city scenario, multiple sensors (e.g., cameras, speed detectors) continuously provide data about pedestrian movements and vehicle speeds. Stream reasoning integrates these multimodal streams, applying symbolic rules (e.g., safety rules) triggered by machine-learning detections (e.g., identifying a child entering a crosswalk), leading to immediate safety actions such as traffic alerts or adjusting signals. This approach demonstrates stream reasoning’s ability to combine real-time analytics, domain knowledge, and predictive ML for timely decision-making.

## Key Challenges in Stream Reasoning

* **Resource Efficiency:** Handling massive data streams efficiently with limited computational resources.
* **Edge Adaptability:** Distributing reasoning tasks intelligently between edge devices and cloud infrastructure.
* **Multimodal Integration:** Seamlessly integrating various data types (audio, video, text) into unified reasoning.
* **Neuro-Symbolic Integration:** Combining symbolic logic with machine learning in real-time, managing uncertainty and concept drift.

## Why Hybrid Stream Reasoning?

No single method—symbolic or statistical—is sufficient alone. Hybrid stream reasoning combines symbolic logic and ML, addressing:

* ML’s limitations in explainability and adaptability.
* Symbolic reasoning’s difficulties with uncertainty and scalability.
* Neuro-symbolic systems' gaps in real-time adaptability.
* RDF-only methods’ inability to handle uncertainty or predictive tasks effectively.

Hybrid systems integrate both data-driven predictions and logic-based inference, enabling richer, explainable, and adaptive real-time responses.

## What is Hybrid Stream Reasoning?

**Hybrid stream reasoning** merges symbolic AI (logical reasoning) with ML (data-driven insights) in real-time streaming environments. It enables:

* **Semantic Enrichment:** ML provides symbolic facts from raw data, enabling semantic interpretation.
* **Real-Time Adaptation:** Continuously integrates new data to update reasoning dynamically.
* **Complex Event Recognition:** Combines ML detection with logical rules for situational awareness.
* **Context-Aware Decisions:** Makes timely decisions informed by both immediate data and accumulated knowledge.

Hybrid systems offer the adaptability of ML with the clarity and rigor of symbolic reasoning, essential for real-time intelligent systems.

## KolibrieDB: A Hybrid Stream Reasoning Engine

**KolibrieDB** exemplifies a modern hybrid stream reasoning engine. Built in Rust for high-performance, KolibrieDB integrates SPARQL-based querying, RDF stream processing, logic-based inference, and machine learning predictions.

### Architecture Overview:

KolibrieDB efficiently processes RDF streams using:

* Extensible dictionary encoding for efficient data storage.
* SPARQL 1.1 queries with native streaming support.
* Sliding window and timestamp management for continuous query evaluation.
* Rule-based inference supporting schema and instance reasoning.
* Optimized parallel processing leveraging CPU and GPU capabilities.

### Key Differentiators:

* High performance due to Rust’s low-level optimizations.
* Integrated querying, streaming, and inference within a single system.
* Deep semantic integration for multimodal data streams.
* Edge computing readiness and extensible architecture for real-world deployments.

KolibrieDB is positioned as a practical solution combining real-time analytics, semantic clarity, and scalable reasoning, suitable for next-generation intelligent streaming applications.

[→ Go to Kolibrie Tutorial](/kolibrie-tutorial/)