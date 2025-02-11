# 使用分区表调优

## 什么是分区表？

分区表是数据库中的一种数据组织方法，即是一种表分割方法，它将表数据分散到多个分区中，每个分区相当于一个独立的小表。

MatrixOne 将表中的数据根据表的一个或多个列的键值划分到不同的存储对象中。这些列对应的键值称为数据分区或范围。

数据分区或范围是表的一部分，包含表行的子集，并与其他行集分开存储。根据 `CREATE TABLE` 语句的 `PARTITION BY` 子句中提供的规范，给表中的数据划分为多个数据分区或范围。

分区表简化了表数据的读与写，在分区表中，SQL 执行的时候，会优先判断数据对应哪个分区，再对对应分区进行读写，避免了对表内所有数据进行操作。使用分区表可以带来很多好处，例如加快查询速度、优化数据维护、提高数据可用性等。

## 分区表的应用场景

- 通过仅扫描表的一部分来提高查询性能。
- 通过仅对表内一部分数据插入更新删除来提高写入性能。
- 批量删除某一范围的数据，而不是全表操作。
- 对数据做更精细的优化与管理。

## 分区表能做哪些性能调优？

- **查询性能调优**：使用分区表可以使查询只针对特定分区进行，从而提高查询效率。例如，如果表是按照日期分区的，则可以通过查询特定日期范围内的数据，而无需扫描整个表。

- **数据维护性能调优**：使用分区表可以加快数据维护的速度，例如插入、更新和删除操作。分区表可以通过更有效地利用 I/O 和 CPU 资源来减少需要处理的数据量，从而提高维护性能。

- **存储空间使用性能调优**：使用分区表可以更好地管理表数据，减少不必要的存储空间使用。例如，如果表按照时间分区，则旧的分区可以轻松地删除或归档，而无需影响其他分区的数据。

- **可用性和可靠性性能调优**：使用分区表可以提高系统的可用性和可靠性。例如，可以通过备份或复制特定分区来增加系统的恢复能力，从而减少系统出现故障的风险。

## 分区表对性能的影响

通常情况下，随着表内数据的不断增加，表的性能会逐渐下降。但如果将同样数据存储在分区表中，则可以通过将不同范围的数据划分到不同的存储对象中来实现数据访问方式的优化。

与全表访问相比，优化后的访问方式将更优先访问特定分区，从而使得数据的增删改查请求能够更有效地分流到不同的分区中，同时避免 IO 热点的出现，最终获得更好的性能提升。

## 如何使用分区表进行调优

使用分区表时，需要考虑以下前提条件是否满足：

- 数据量较大的表，数据按照某种规律分布，例如按年份或月份分布；
- 希望通过仅扫描表的一部分来提高查询性能；
- 数据的频繁读写只限定在某个特定范围内，其他范围低频读写；
- 数据的频繁读写通常带有某个或某组固定的过滤条件。

### 选择分区键

分区键是物理划分表的关键因素，正确选择分区键可以大大提高读写性能。

在选择分区键时，需要考虑以下因素：

- 高基数：具有高基数的分区键将导致更好的数据分布和可扩展性。例如，对于一个表，如果有学生的性别和年龄两种属性，年龄的基数比性别要高，更适合作为分区键。
- 独特性：分区键应尽量保持独特性，从而避免某个分区成为单独的热点。使用复合键可以实现独特性。例如，在某个公司中，如果半数以上不同部门的员工都在同一个城市中，以城市作为分区键将导致该部门成为热点。而使用复合键，例如部门 ID+城市 ID 的组合分区方式，可以大幅降低热点分区的概率。

### 制定分区策略

即使选择了具有高基数和独特性的列作为分区键，如果分区的范围不合适，仍有可能出现热点分区。因此，在分区策略上需要更精细化的管理。

- 数据分布应尽量均匀，避免出现极端分布。例如，如果以学生年龄作为分区键，需要评估公司的年龄分布，从而避免某个分区的年龄段过于集中。例如，一个学校可能有很多 18 岁 -22 岁的学生，但 18 岁以下或 22 岁以上的学生相对较少，这种情况可以将每个年龄作为一个分区。
- 添加随机数分散分区，在高并发写入的场景中，将待分区键与一个随机数列组合为分区键。例如，在某个订单系统中，某种商品的订单数量非常高，可以在设计表时添加一列随机数列，取值范围为 1-10，并与商品类型一起作为分区键。每次写入一笔订单时，会随机生成一个 10 以内的数字，将这些订单随机写入到 10 个分个分区中，降低了出现热点分区的概率。

## MatrixOne 分区表类型

在 MatrixOne 中，可以支持多种对表的分区模式，每个模式对应的分区方法均有不同。

### Range 分区

范围分区是一种基于连续值的分区方式，分区之间必须是互不重叠的，分区的定义使用 `VALUES LESS THAN` 运算符。

**示例**

如下两张表，就是分别以整数列与日期列作为分区列使用：

```sql
CREATE TABLE members (
    firstname VARCHAR(25) NOT NULL,
    lastname VARCHAR(25) NOT NULL,
    username VARCHAR(16) NOT NULL,
    email VARCHAR(35),
    joined DATE NOT NULL
)
PARTITION BY RANGE COLUMNS(joined) (
    PARTITION p0 VALUES LESS THAN ('1960-01-01'),
    PARTITION p1 VALUES LESS THAN ('1970-01-01'),
    PARTITION p2 VALUES LESS THAN ('1980-01-01'),
    PARTITION p3 VALUES LESS THAN ('1990-01-01')
)
```

需要注意的是，如果插入或更新的数据中，分区列的值没有对应的分区，那么相应的插入或更新操作将会失败。举个例子，如果插入一行 `joined` 日期为 2000-01-01 的数据，但是因为找不到相应的分区，该操作将会因为无法定位分区而失败。因此，在执行插入或更新操作时，一定要确保分区列的值属于对应的分区。

### Range Columns 分区

范围列分区可以被视为范围分区的一个扩展或变体，它允许使用多个列的组合范围进行更复杂的数据分割。

**示例**

```sql
CREATE TABLE e_orders (
    order_id INT NOT NULL,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    customer_level INT,
    total_amount DECIMAL(10, 2),
    product_id INT
)
PARTITION BY RANGE COLUMNS(order_date, customer_level) (
    PARTITION p0 VALUES LESS THAN ('2023-01-01', 2),
    PARTITION p1 VALUES LESS THAN ('2023-01-01', 5),
    PARTITION p2 VALUES LESS THAN ('2023-06-01', 2),
    PARTITION p3 VALUES LESS THAN ('2023-06-01', 5),
    PARTITION p4 VALUES LESS THAN (MAXVALUE, MAXVALUE)
);
```

### List 分区

列表分区要求每个分区必须由明确定义的值列表组成，且每个分区的列表成员不能有重复值。分区键的值应该是离散的，即分区定义能够清晰地列出所有可能的值或值的集合。

**示例**

```sql
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY LIST(store_id) (
    PARTITION pNorth VALUES IN (3,5,6,9,17),
    PARTITION pEast VALUES IN (1,2,10,11,19,20),
    PARTITION pWest VALUES IN (4,12,13,14,18),
    PARTITION pCentral VALUES IN (7,8,15,16)
);
```

需要注意的是，如果插入或更新的数据中，分区列的值没有对应分区，则会插入失败。

### List Columns 分区

列表列分区可以被视为列表分区的扩展，它允许使用多列的组合值进行更细致的数据分割。

**示例**

```sql
CREATE TABLE orders (
    order_id INT NOT NULL,
    order_date DATE NOT NULL,
    customer_id INT NOT NULL,
    region VARCHAR(10),  -- 北区或南区
    product_type VARCHAR(15),  -- 电子产品、家居用品
    amount DECIMAL(10, 2)
)
PARTITION BY LIST COLUMNS(region, product_type) (
    PARTITION pNorthElectronics VALUES IN (('北区', '电子产品')),
    PARTITION pNorthHomeGoods VALUES IN (('北区', '家居用品')),
    PARTITION pSouthElectronics VALUES IN (('南区', '电子产品')),
    PARTITION pSouthHomeGoods VALUES IN (('南区', '家居用品'))
);
```

### Hash 分区

哈希分区通常用于将数据均匀分布在不同的分区中。常见的哈希分区有 HASH 函数分区和 KEY 函数分区。

在 HASH 函数分区中，必须明确指定用于分区的列或表达式，该列或表达式的类型必须是整数，并且建议明确指定分区数量，该数量必须是正整数。

**示例**

```sql
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY HASH( YEAR(hired) )
PARTITIONS 4;
```

### Key 分区

在使用 KEY 函数分区时，数据库服务内部提供了自有的哈希方式进行分区，因此不需要显式定义分区数量。与 HASH 函数分区不同的是，KEY 函数分区支持除大对象类型（TEXT/BLOB）之外的所有类型作为分区键，而且不一定需要指定分区数量。例如，可以通过以下语句创建一个分区键为 s1 的表：

**示例**

```sql
CREATE TABLE tm1 (
s1 CHAR(32) PRIMARY KEY
)
PARTITION BY KEY(s1);
```

需要注意的是，如果分区键是该表的主键列，那么在不显式指定分区键时，系统会默认以主键列作为分区键。例如：

```sql
CREATE TABLE tm1 (
    s1 CHAR(32) PRIMARY KEY
)
PARTITION BY KEY();
```

<!--##分区调优最佳实践-->

## 限制

List / List column 暂不支持分区裁剪。

Range/ Range Columns 暂不支持分区裁剪。