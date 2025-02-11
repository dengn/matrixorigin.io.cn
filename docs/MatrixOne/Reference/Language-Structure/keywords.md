# 关键字

本章介绍 MatrixOne 的关键字，在 MatrixOne 中对保留关键字和非保留关键字进行了分类，你在使用 SQL 语句时，可以查阅保留关键字和非保留关键字。

**关键字**是 SQL 语句中具有特殊含义的单词，例如 `SELECT`，`UPDATE`，`DELETE` 等等。

- **保留关键字**：关键字中需要经过特殊处理才能作为标识符的字，被称为保留关键字。

   将保留关键字作为标识符使用时，必须使用反引号包裹，否则将产生报错：

```
\\未将保留关键字 select 使用反引号包裹，产生报错
mysql> CREATE TABLE select (a INT);
ERROR 1064 (HY000): SQL parser error: You have an error in your SQL syntax; check the manual that corresponds to your MatrixOne server version for the right syntax to use. syntax error at line 1 column 19 near " select (a INT)";

\\正确将保留关键字 select 使用反引号包裹
mysql> CREATE TABLE `select` (a INT);
Query OK, 0 rows affected (0.02 sec)
```

- **非保留关键字**：关键字中可以直接作为标识符，被称为非保留关键字。

   将非保留关键字作为标识符使用时，可以直接使用，无需使用反引号包裹。

```
\\BEGIN 未非保留关键字，可以无需使用反引号包裹
mysql> CREATE TABLE `select` (BEGIN int);
Query OK, 0 rows affected (0.01 sec)
```

!!! note
    与 MySQL 不同，在 MatrixOne 中，如果使用了限定符*.*，保留关键字如果不使用反引号包裹也会产生报错，建议在创建表和数据库时，避免使用保留关键字：

```
mysql> CREATE TABLE test.select (BEGIN int);
ERROR 1064 (HY000): SQL parser error: You have an error in your SQL syntax; check the manual that corresponds to your MatrixOne server version for the right syntax to use. syntax error at line 1 column 24 near "select (BEGIN int)";
```

## 保留关键字

### A

- ADD
- ADDDATE
- ADMIN_NAME
- AFTER
- ALL
- ALTER
- ANALYZE
- AND
- APPROX_COUNT
- APPROX_COUNT_DISTINCT
- APPROX_PERCENTILE
- AS
- ASC
- ASCII
- AUTOEXTEND_SIZE
- AUTO_INCREMENT
- AVG

### B

- BACKEND
- BETWEEN
- BINARY
- BINDINGS
- BIT_AND
- BIT_CAST
- BIT_OR
- BOOLEAN
- BOTH
- BSI
- BTREE
- BY

### C

- CALL
- CASCADED
- CASE
- CAST
- CHANGE
- CHAR
- CHARACTER
- CHECK
- CIPHER
- CLIENT
- CLUSTER_CENTERS
- COALESCE
- COLLATE
- COLLATION
- COLUMN
- COLUMN_NUMBER
- CONFIG
- CONNECT
- CONSTRAINT
- CONVERT
- COPY
- COUNT
- CREATE
- CREDENTIALS
- CROSS
- CURDATE
- CURRENT
- CURRENT_DATE
- CURRENT_ROLE
- CURRENT_TIME
- CURRENT_TIMESTAMP
- CURRENT_USER
- CURRVAL
- CURTIME

### D

- DATABASE
- DATABASES
- DATE
- DATE_ADD
- DATE_SUB
- DAY_HOUR
- DAY_MICROSECOND
- DAY_MINUTE
- DAY_SECOND
- DEALLOCATE
- DECLARE
- DEFAULT
- DELAYED
- DELETE
- DENSE_RANK
- DESC
- DESCRIBE
- DISABLE
- DISCARD
- DISTINCT
- DIV
- DRAINER
- DROP

### E

- ELSE
- ELSEIF
- ENABLE
- ENCLOSED
- END
- ESCAPE
- ESCAPED
- EVENT
- EVENTS
- EXCEPT
- EXCLUSIVE
- EXECUTE
- EXISTS
- EXPLAIN
- EXTERNAL
- EXTRACT

### F

- FAILED_LOGIN_ATTEMPTS
- FALSE
- FILE
- FILL
- FIRST
- FOLLOWING
- FOR
- FORCE
- FOREIGN
- FROM
- FULLTEXT
- FUNCTION

### G

- GRANTS
- GROUP
- GROUPS
- GROUP_CONCAT

### H

- HANDLER
- HAVING
- HEADERS
- HIGH_PRIORITY
- HOUR
- HOUR_MICROSECOND
- HOUR_MINUTE
- HOUR_SECOND

### I

- IDENTIFIED
- IF
- IGNORE
- ILIKE
- IMPORT
- IN
- INDEX
- INFILE
- INLINE
- INNER
- INOUT
- INPLACE
- INSERT
- INSTANT
- INT1
- INT2
- INT3
- INT4
- INT8
- INTERSECT
- INTERVAL
- INTO
- INVISIBLE
- INVOKER
- IS
- ISSUER
- ITERATE
- IVFFLAT

### J

- JOIN
- JSONTYPE

### K

- KEY
- KILL

### L

- LAST
- LASTVAL
- LEADING
- LEAVE
- LEFT
- LIKE
- LIMIT
- LINES
- LOAD
- LOCALTIME
- LOCALTIMESTAMP
- LOCK
- LOCKS
- LOOP
- LOW_PRIORITY

### M

- MANAGE
- MATCH
- MAX
- MAXVALUE
- MEDIAN
- MERGE
- MICROSECOND
- MID
- MIN
- MINUS
- MINUTE
- MINUTE_MICROSECOND
- MINUTE_SECOND
- MOD
- MODIFY
- MODUMP
- MYSQL_COMPATIBILITY_MODE

### N

- NATURAL
- NEXT
- NEXTVAL
- NONE
- NOT
- NOW
- NULL
- NULLS

### O

- ON
- OPTIONAL
- OPTIONALLY
- OR
- ORDER
- OUT
- OUTER
- OUTFILE
- OVER
- OWNERSHIP

### P

- PARSER
- PARTITION
- PASSWORD
- PASSWORD_LOCK_TIME
- PERCENT
- PLUGINS
- POSITION
- PRECEDING
- PREPARE
- PREV
- PRIMARY
- PRIVILEGES
- PUMP

### Q

- QUERY_RESULT
- QUICK

### R

- RANDOM
- RANK
- RECURSIVE
- REFERENCE
- REFERENCES
- REGEXP
- RELOAD
- RENAME
- REPEAT
- REPLACE
- REQUIRE
- RESET
- RESTRICTED
- RETURNS
- REUSE
- REVERSE
- RIGHT
- ROUTINE
- ROW
- ROWS
- ROW_COUNT
- ROW_NUMBER
- RTREE

### S

- SAMPLE
- SAN
- SCHEMA
- SCHEMAS
- SECOND
- SECONDARY
- SECOND_MICROSECOND
- SECURITY
- SELECT
- SEPARATOR
- SEQUENCE
- SERVERS
- SESSION_USER
- SET
- SETVAL
- SHARED
- SHOW
- SHUTDOWN
- SLAVE
- SLIDING
- SPHERICAL_KMEANS
- SQL_BIG_RESULT
- SQL_BUFFER_RESULT
- SQL_CACHE
- SQL_NO_CACHE
- SQL_SMALL_RESULT
- SQL_TSI_DAY
- SQL_TSI_HOUR
- SQL_TSI_MINUTE
- SQL_TSI_MONTH
- SQL_TSI_QUARTER
- SQL_TSI_SECOND
- SQL_TSI_WEEK
- SQL_TSI_YEAR
- SSL
- STARTING
- STD
- STDDEV
- STDDEV_POP
- STDDEV_SAMP
- STRAIGHT_JOIN
- STREAM
- SUBDATE
- SUBJECT
- SUBSTR
- SUBSTRING
- SUM
- SUPER
- SUSPEND
- SYSDATE
- SYSTEM_USER

### T

- TABLE
- TABLESPACE
- TABLE_NUMBER
- TABLE_SIZE
- TABLE_VALUES
- TEMPORARY
- TEMPTABLE
- TERMINATED
- THEN
- TIME
- TIMESTAMP
- TIMESTAMPDIFF
- TO
- TRAILING
- TRANSLATE
- TRIM
- TRUE
- TRUNCATE

### U

- UNBOUNDED
- UNDEFINED
- UNDERSCORE_BINARY
- UNION
- UNIQUE
- UNTIL
- UPDATE
- USAGE
- USE
- USING
- UTC_DATE
- UTC_TIME
- UTC_TIMESTAMP

### V

- VALIDATION
- VALUES
- VARIANCE
- VAR_POP
- VAR_SAMP
- VERBOSE
- VISIBLE

### W

- WHEN
- WHERE
- WHILE
- WITH
- WITHOUT

### X

- XOR

### Y

- YEAR_MONTH

### Z

- ZONEMAP

## 非保留关键字

### A

- ACCOUNT
- ACCOUNTS
- ACTION
- AGAINST
- ALGORITHM
- ANY
- ATTRIBUTE
- AUTO_RANDOM
- AVG_ROW_LENGTH

### B

- BACKUP
- BEGIN
- BIGINT
- BIT
- BLOB
- BOOL

### C

- CANCEL
- CASCADE
- CHAIN
- CHARSET
- CHECKSUM
- CLUSTER
- COLUMNS
- COLUMN_FORMAT
- COMMENT_KEYWORD
- COMMIT
- COMMITTED
- COMPACT
- COMPRESSED
- COMPRESSION
- CONNECTION
- CONNECTOR
- CONNECTORS
- CONSISTENT
- CYCLE

### D

- DAEMON
- DATA
- DATE
- DATETIME
- DAY
- DECIMAL
- DEFINER
- DELAY_KEY_WRITE
- DIRECTORY
- DISK
- DO
- DOUBLE
- DUPLICATE
- DYNAMIC

### E

- ENCRYPTION
- ENFORCED
- ENGINE
- ENGINES
- ENGINE_ATTRIBUTE
- ENUM
- ERRORS
- EXPANSION
- EXPIRE
- EXTENDED
- EXTENSION

### F

- FIELDS
- FILESYSTEM
- FIXED
- FLOAT_TYPE
- FORCE_QUOTE
- FORMAT
- FULL

### G

- GEOMETRY
- GEOMETRYCOLLECTION
- GLOBAL
- GRANT

### H

- HASH
- HEADER
- HISTORY

### I

- INCREMENT
- INDEXES
- INSERT_METHOD
- INT
- INTEGER
- ISOLATION

### J

- JSON

### K

- KEYS
- KEY_BLOCK_SIZE

### L

- LANGUAGE
- LESS
- LEVEL
- LINEAR
- LINESTRING
- LIST
- LISTS
- LOCAL
- LONGBLOB
- LONGTEXT
- LOW_CARDINALITY

### M

- MAX_CONNECTIONS_PER_HOUR
- MAX_FILE_SIZE
- MAX_QUERIES_PER_HOUR
- MAX_ROWS
- MAX_UPDATES_PER_HOUR
- MAX_USER_CONNECTIONS
- MEDIUMBLOB
- MEDIUMINT
- MEDIUMTEXT
- MEMORY
- MINVALUE
- MIN_ROWS
- MODE
- MONTH
- MULTILINESTRING
- MULTIPOINT
- MULTIPOLYGON

### N

- NAMES
- NCHAR
- NEVER
- NO
- NODE
- NUMERIC

### O

- OFFSET
- ONLY
- OPEN
- OPTIMIZE
- OPTION

### P

- PACK_KEYS
- PARALLEL
- PARTIAL
- PARTITIONS
- PASSWORD
- PAUSE
- PERSIST
- POINT
- POLYGON
- PROCEDURE
- PROCESSLIST
- PROFILES
- PROPERTIES
- PROXY
- PUBLICATION
- PUBLICATIONS

### Q

- QUARTER
- QUERY

### R

- RANGE
- READ
- REAL
- REDUNDANT
- RELEASE
- REORGANIZE
- REPAIR
- REPEATABLE
- REPLICATION
- RESTRICT
- RESUME
- REVOKE
- ROLE
- ROLES
- ROLLBACK
- ROW_FORMAT

### S

- S3OPTION
- SECONDARY_ENGINE_ATTRIBUTE
- SEQUENCES
- SERIALIZABLE
- SESSION
- SHARE
- SIGNED
- SIMILARITY_FUNCTION
- SIMPLE
- SMALLINT
- SNAPSHOT
- SOME
- SOURCE
- SPATIAL
- SQL
- STAGE
- STAGEOPTION
- STAGES
- START
- STATS_AUTO_RECALC
- STATS_PERSISTENT
- STATS_SAMPLE_PAGES
- STATUS
- STORAGE
- SUBPARTITION
- SUBPARTITIONS
- SUBSCRIPTIONS

### T

- TABLES
- TASK
- TEXT
- THAN
- TIME
- TIMESTAMP
- TINYBLOB
- TINYINT
- TINYTEXT
- TRANSACTION
- TRIGGER
- TRIGGERS
- TYPE

### U

- UNCOMMITTED
- UNKNOWN
- UNLOCK
- UNSIGNED
- UNUSED
- URL
- USER
- UUID

### V

- VALUE
- VARBINARY
- VARCHAR
- VARIABLES
- VECF32
- VECF64
- VIEW

### W

- WARNINGS
- WEEK
- WORK
- WRITE

### X

- X509

### Y

- YEAR

### Z

- ZEROFILL
