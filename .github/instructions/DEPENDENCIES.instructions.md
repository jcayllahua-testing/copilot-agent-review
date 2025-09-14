---
applyTo: "*.gradle"
---
When performing a code review make sure this file type has at least one of the following strings:
- "implementation 'org.springframework.boot:spring-boot-starter-data-r2dbc'"
- "runtimeOnly 'io.r2dbc:r2dbc-h2'"
- "runtimeOnly 'io.r2dbc:r2dbc-postgresql'"
- "runtimeOnly 'io.r2dbc:r2dbc-mssql'"
- "dev.miku:r2dbc-mysql"
- "spring-boot-starter-data-mongodb-reactive"
- "spring-boot-starter-data-redis-reactive"
- "spring-boot-starter-data-cassandra-reactive"
- "spring-boot-starter-data-couchbase-reactive"

