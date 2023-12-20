# Postgre SQL

## Data Types

Primitives: Integer, Numeric, String, Boolean

Structured: Date/Time, Array, Range / Multirange, UUID

Document: JSON/JSONB, XML, Key-value (Hstore)

Geometry: Point, Line, Circle, Polygon

Customizations: Composite, Custom Types

## Data Integrity

UNIQUE, NOT NULL

Primary Keys

Foreign Keys

Exclusion Constraints

ensure that if any two rows are compared on the specified columns or expressions using the specified operators, at least one of these operator comparisons will return false or null.

Explicit Locks, Advisory Locks

PostgreSQL provides various lock modes to control concurrent access to data in tables. These modes can be used for application-controlled locking in situations where MVCC does not give the desired behavior

PostgreSQL provides a means for creating locks that have application-defined meanings. the system does not enforce their use — it is up to the application to use them correctly



## Concurrency, Performance

Indexing: B-tree, Multicolumn, Expressions, Partial

Advanced Indexing: GiST, SP-Gist, KNN Gist, GIN, BRIN, Covering indexes, Bloom filters

Sophisticated query planner / optimizer, index-only scans, multicolumn statistics

Transactions, Nested Transactions (via savepoints)

Multi-Version concurrency Control (MVCC)

Parallelization of read queries and building B-tree indexes

Table partitioning

All transaction isolation levels defined in the SQL standard, including Serializable

Just-in-time (JIT) compilation of expressions

## Reliability, Disaster Recovery

Write-ahead Logging (WAL)

Replication: Asynchronous, Synchronous, Logical

Point-in-time-recovery (PITR), active standbys

Tablespaces

## Security

Authentication: GSSAPI, SSPI, LDAP, SCRAM-SHA-256, Certificate, and more

Robust access-control system

Column and row-level security

Multi-factor authentication with certificates and an additional method

## Extensibility

Stored functions and procedures

Procedural Languages: PL/PGSQL, Perl, Python (and many more)

SQL/JSON path expressions

LIMIT returns only the number of rows, OFFSET indicates the number of rows to skip.

Foreign data wrappers: connect to other databases or streams with a standard SQL interface

Customizable storage interface for tables

Many extensions that provide additional functionality, including PostGIS

## Internationalisation, Text Search

Support for international character sets, e.g. through ICU collations

Case-insensitive and accent-insensitive collations

Full-text search