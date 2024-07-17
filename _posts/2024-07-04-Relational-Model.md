---
title: What is Relational Model?
categories: [Data Engineering, Database]
tag: [Database]
---

# Relational Model
- The relational model is essential for organizing data in relational databases. It uses tables to store data and represent relationships between them.

# Concepts of Relational Model

### Attribute
- Defines a property within a relation (table).

### Column
- Contains the set of values for a specific attribute.

### Tuple
- Represents each row in a relation.

### Relation Schema
- Defines the name of the relation and its attributes.

### Relation Instance
- A finite set of tuples within a relational database.

### Relation Key
- Used to uniquely identify rows or establish relationships between tables.

### Degree
- Total number of attributes in a relation.

### Cardinality
- Total number of rows in a table.

### NULL Values 
- Indicate missing or undefined data.

# Features of Relational Model

### No Duplicate Tuples
- Each row (tuple) in a table is unique.

### Order of Tuples
- The sequence of rows does not affect the data.

### Attribute Domain
- The domain of an attribute does not determine the order of values.

### Distinct Attribute Names
- Each column (attribute) has a unique name.

### Atomic Values
- Each cell in a table holds a single, indivisible value.

### Distinct Relation Names
- Each table (relation) has a unique name within the database.

# Constraints in Relational Model

### Domain Constraint
- Ensures each attribute domain contains atomic values.

### Entity Integrity Constraint
-  No primary key can be NULL.

### Referential Integrity Constraint
- Ensures that values in one table reference valid values in another table.

### Key Constraint (Uniqueness Constraint)
- Ensures each tuple in a relation is unique.
