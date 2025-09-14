---
applyTo: "*.gradle"
---

When performing a code review make sure this file type have at least one of the following dependencies:
```gradle
// ✅ R2DBC (Relational Databases)
implementation 'org.springframework.boot:spring-boot-starter-data-r2dbc'
runtimeOnly 'io.r2dbc:r2dbc-h2'            // H2 (in-memory DB, great for dev/test)
runtimeOnly 'io.r2dbc:r2dbc-postgresql'    // PostgreSQL
runtimeOnly 'io.r2dbc:r2dbc-mssql'         // Microsoft SQL Server
runtimeOnly 'dev.miku:r2dbc-mysql'         // MySQL / MariaDB

// ✅ Reactive NoSQL
implementation 'org.springframework.boot:spring-boot-starter-data-mongodb-reactive'
implementation 'org.springframework.boot:spring-boot-starter-data-redis-reactive'
implementation 'org.springframework.boot:spring-boot-starter-data-cassandra-reactive'
implementation 'org.springframework.boot:spring-boot-starter-data-couchbase-reactive'
```
in case u can't find any of these leave a comment in the current pull request being reviewed.
