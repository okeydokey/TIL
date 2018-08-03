# [Isolation Level](https://en.wikipedia.org/wiki/Isolation_(database_systems))
Transaction isolation is one of the foundations of database processing. Isolation is the I in the acronym ACID; the isolation level is the setting that fine-tunes the balance between performance and reliability, consistency, and reproducibility of results when multiple transactions are making changes and performing queries at the same time.

## [MySQL 에서의 격리 수준](https://dev.mysql.com/doc/refman/5.7/en/innodb-transaction-isolation-levels.html)
InnoDB offers all four transaction isolation levels described by the SQL:1992 standard: READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, and SERIALIZABLE. The default isolation level for InnoDB is REPEATABLE READ.
