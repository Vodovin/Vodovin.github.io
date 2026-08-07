---
title: 'End-to-End Data Pipeline: Relational, XML & NoSQL Ecosystems'
description: A comprehensive data engineering project encompassing relational database design, hierarchical data validation (XML/XSD), and migration to NoSQL (MongoDB) and Graph (Neo4j) databases for advanced telemetry analytics.
publishDate: 'May 28 2026'
seo:
  image:
    src: '../../assets/images/project-4.jpg'
---

![Project preview](../../assets/images/project-4.jpg)

**Project Overview:**
This extensive data engineering project simulates a real-world telemetry and progression data ecosystem for a multiplayer video game. Developed in three iterative phases, the project demonstrates the complete lifecycle of data management: starting from a foundational Relational Database (SQL), transitioning into hierarchical data serialization (XML/XSD), and culminating in a full migration to NoSQL Document (MongoDB) and Graph (Neo4j) architectures.

## Design Objectives

1. **Relational Foundation:** Establish a robust, normalized Entity-Relationship (ER) model capable of storing players, characters, matches, and progression data.
2. **Data Serialization & Validation:** Extract relational data into JSON and transform it into strictly formatted XML documents, enforcing data integrity through custom XSD schemas.
3. **NoSQL Migration & Optimization:** Migrate the dataset to MongoDB to leverage document-based horizontal scalability, optimizing complex queries through strategic indexing and execution profiling.
4. **Graph Analytics:** Utilize Neo4j to model deeply interconnected N:M relationships (e.g., players, matches, and characters) to perform advanced behavioral analytics using Cypher.

## Phase 1: Relational Database Design (SQLite)
   * **Data Modeling:** Designed the conceptual, logical, and physical ER diagrams using Redgate Data Modeler to structure 12 interconnected tables (including Players, Brawlers, Clubs, and Matches).
   * **Automated Data Population:** Developed a Python script utilizing the `sqlite3` and `Faker` libraries to generate and insert massive amounts of randomized, semantically correct data into the SQLite database to simulate a production environment.
   * **SQL Analytics:** Authored complex SQL queries utilizing `JOIN`, `GROUP BY`, and aggregation functions to extract game balancing metrics, such as the highest win-rate characters and top-performing clubs.

## Phase 2: Hierarchical Data & Validation (XML)
   * **ETL Pipeline:** Extracted relational data via SQL-to-JSON queries, then processed and reformatted the JSON outputs into well-formed XML documents using Python scripting and Regex.
   * **Strict Schema Validation:** Authored custom XSD (XML Schema Definition) files to validate the generated XMLs. This included creating custom data types, such as regular expressions for strict DateTime formatting (`YYYY-MM-DD HH:MM:SS`) and character rarity enumerations.
   * **XQuery Extraction:** Implemented XQuery scripts with FLWOR expressions to query the hierarchical data, enabling grouped and filtered analytical outputs natively from the XML files.

## Phase 3: NoSQL & Graph Analytics (MongoDB & Neo4j)
   * **MongoDB Aggregation & Indexing:** 
     * Redesigned the schema to fit a document-oriented approach, embedding array structures (e.g., `Progreso_Brawler` inside `Jugadores`) to minimize costly join operations.
     * Utilized the MongoDB Aggregation Framework (`$unwind`, `$group`, `$project`, `$match`) to recreate complex analytics natively.
     * Implemented single and compound indexes, verifying their performance impact by analyzing `explain("executionStats")` to drastically reduce `COLLSCAN` operations and `totalDocsExamined`.
   * **Neo4j & Cypher Traversals:**
     * Remodeled the domain as a Graph, creating intermediate nodes (`Participacion en partida`) to accurately resolve ternary relationships between Matches, Players, and Characters.
     * Authored advanced Cypher queries requiring up to 6 node hops to uncover deep behavioral patterns, such as identifying scenarios where a player was defeated by an opponent using their "main" character.

## Technology Stack

- **Databases:** SQLite (Relational), MongoDB (Document NoSQL), Neo4j (Graph).
- **Data Serialization & Querying:** XML, XSD, XQuery, JSON.
- **Database Languages:** SQL, MongoDB Aggregation Pipeline, Cypher.
- **Tooling & Scripting:** Python (ETL scripting), Redgate Data Modeler, Regex.
