# StarRocks version 2.5

## 2.5.2

发布日期： 2023 年 2 月 21 日

### 新增特性

- 访问 AWS S3 以及 AWS Glue 时支持基于 Instance Profile 和 Assumed Role 来进行认证和鉴权。 [#15958](https://github.com/StarRocks/starrocks/pull/15958)
- 新增 3 个 bit 函数：[bit_shift_left](../sql-reference/sql-functions/bit-functions/bit_shift_left.md)，[bit_shift_right](../sql-reference/sql-functions/bit-functions/bit_shift_right.md)，[bit_shift_right_logical](../sql-reference/sql-functions/bit-functions/bit_shift_right_logical.md)。 [#14151](https://github.com/StarRocks/starrocks/pull/14151)

### 功能优化

- 优化了部分内存释放的逻辑，在查询中包含大量的聚合查询时可以显著的降低内存峰值使用。 [#16913](https://github.com/StarRocks/starrocks/pull/16913)
- 优化了排序的内存使用，对于部分带有窗口函数的查询或者是排序查询，可以减少一倍以上的内存消耗。 [#16937](https://github.com/StarRocks/starrocks/pull/16937) [#17362](https://github.com/StarRocks/starrocks/pull/17362) [#17408](https://github.com/StarRocks/starrocks/pull/17408)

### 问题修复

修复了如下问题：

- 无法刷新含有 MAP 或 ARRAY 数据类型的 Apache Hive 外表。[#17548](https://github.com/StarRocks/starrocks/pull/17548)
- Superset 无法识别出物化视图列类型。[#17686](https://github.com/StarRocks/starrocks/pull/17686)
- 在对接 BI 时因无法解析 SET GLOBAL/SESSION TRANSACTION 而导致的连接性问题。[#17295](https://github.com/StarRocks/starrocks/pull/17295)
- Colocate 组内的动态分区表无法修改分桶数，并返回报错信息。[#17418](https://github.com/StarRocks/starrocks/pull/17418/)
- 修复了在 prepare 阶段失败可能导致的潜在问题。 [#17323](https://github.com/StarRocks/starrocks/pull/17323)

### 行为变更

- 新增保留关键字 CHARACTER。[#17488](https://github.com/StarRocks/starrocks/pull/17488)

## 2.5.1

发布日期： 2023 年 2 月 5 日

### 功能优化

- 外表物化视图支持查询改写。 [#11116](https://github.com/StarRocks/starrocks/issues/11116)[#15791](https://github.com/StarRocks/starrocks/issues/15791)
- CBO 自动全量采集支持用户设置采集时间段，防止因集中采集而导致的集群性能抖动。 [#14996](https://github.com/StarRocks/starrocks/pull/14996)
- 增加 Thrift server 队列，避免 INSERT INTO SELECT 时因为 Thrift server 中的请求过于繁忙而失败。 [#14571](https://github.com/StarRocks/starrocks/pull/14571)
- 建表时如果没有显式指定 `storage_medium` 属性，则系统根据 BE 节点磁盘类型自动推导并设定表的存储类型。参见[CREATE TABLE](../sql-reference/sql-statements/data-definition/CREATE%20TABLE.md) 中的参数描述。[#14394](https://github.com/StarRocks/starrocks/pull/14394)

### 问题修复

修复了如下问题：

- SET PASSWORD 导致空指针。 [#15247](https://github.com/StarRocks/starrocks/pull/15247)
- 当 KEY 为空时无法解析 JSON 数据。 [#16852](https://github.com/StarRocks/starrocks/pull/16852)
- 非法数据类型可以成功转换至 ARRAY 类型。 [#16866](https://github.com/StarRocks/starrocks/pull/16866)
- 异常场景下 Nested Loop Join 无法中断。  [#16875](https://github.com/StarRocks/starrocks/pull/16875)

### 行为变更

- 取消 FE 参数 `default_storage_medium`，表的存储介质改为系统自动推导。 [#14394](https://github.com/StarRocks/starrocks/pull/14394)

## 2.5.0

发布日期： 2023 年 1 月 22 日

### 新增特性

- [Hudi catalog](../data_source/catalog/hudi_catalog.md) 和 [Hudi 外部表](../data_source/External_table.md#apache-hudi-外表)支持查询 Merge On Read 表。[#6780](https://github.com/StarRocks/starrocks/pull/6780)
- [Hive catalog](../data_source/catalog/hive_catalog.md)、Hudi catalog 和 [Iceberg catalog](../data_source/catalog/iceberg_catalog.md) 支持查询 STRUCT 和 MAP 类型数据。[#10677](https://github.com/StarRocks/starrocks/issues/10677)
- 提供 [Local Cache](../data_source/Block_cache.md) 特性，在查询 HDFS 或对象存储上的热数据时，大幅优化数据访问效率。[#11597](https://github.com/StarRocks/starrocks/pull/11579)
- 支持 [Delta Lake catalog](../data_source/catalog/deltalake_catalog.md)，无需导入数据或创建外部表即可查询 Delta Lake 数据。[#11972](https://github.com/StarRocks/starrocks/issues/11972)
- Hive catalog、Hudi catalog 和 Iceberg catalog 兼容 AWS Glue。[#12249](https://github.com/StarRocks/starrocks/issues/12249)
- 支持通过[文件外部表](../data_source/file_external_table.md)查询 HDFS 或对象存储上的 Parquet 和 ORC 文件。[#13064](https://github.com/StarRocks/starrocks/pull/13064)
- 支持基于 Hive、Hudi 或 Iceberg catalog 创建物化视图，以及基于物化视图创建物化视图。相关文档，请参见[物化视图](../using_starrocks/Materialized_view.md)。[#11116](https://github.com/StarRocks/starrocks/issues/11116) [#11873](https://github.com/StarRocks/starrocks/pull/11873)
- 主键模型表支持条件更新。相关文档，请参见[通过导入实现数据变更](../loading/Load_to_Primary_Key_tables.md#条件更新)。[#12159](https://github.com/StarRocks/starrocks/pull/12159)
- 支持 [Query Cache](../using_starrocks/query_cache.md)，通过保存查询的中间计算结果提升简单高并发查询的 QPS 并降低平均时延。[#9194](https://github.com/StarRocks/starrocks/pull/9194)
- 支持为 Broker Load 作业指定优先级。相关文档，请参见 [BROKER LOAD](../sql-reference/sql-statements/data-manipulation/BROKER%20LOAD.md)。[#11029](https://github.com/StarRocks/starrocks/pull/11029)
- 支持为 StarRocks 原生表手动设置数据导入的副本数。相关文档，请参见 [CREATE TABLE](../sql-reference/sql-statements/data-definition/CREATE%20TABLE.md)。[#11253](https://github.com/StarRocks/starrocks/pull/11253)
- 支持查询队列功能。相关文档，请参见[查询队列](../administration/query_queues.md)。[#12594](https://github.com/StarRocks/starrocks/pull/12594)
- 支持通过资源组对导入计算进行资源隔离，从而间接控制导入任务对集群资源的消耗。相关文档，请参见[资源隔离](../administration/resource_group.md)。[#12606](https://github.com/StarRocks/starrocks/pull/12606)
- 支持为 StarRocks 原生表手动设置数据压缩算法：LZ4、Zstd、Snappy 和 Zlib。相关文档，请参见[数据压缩](../table_design/data_compression.md)。[#10097](https://github.com/StarRocks/starrocks/pull/10097) [#12020](https://github.com/StarRocks/starrocks/pull/12020)
- 支持[用户自定义变量](../reference/user_defined_variables.md) (user-defined variables)。[#10011](https://github.com/StarRocks/starrocks/pull/10011)
- 支持 [Lambda 表达式](../sql-reference/sql-functions/Lambda_expression.md)及高阶函数，包括：[array_map](../sql-reference/sql-functions/array-functions/array_map.md)、[array_filter](../sql-reference/sql-functions/array-functions/array_filter.md)、[array_sum](../sql-reference/sql-functions/array-functions/array_sum.md) 和 [array_sortby](../sql-reference/sql-functions/array-functions/array_sortby.md)。[#9461](https://github.com/StarRocks/starrocks/pull/9461) [#9806](https://github.com/StarRocks/starrocks/pull/9806) [#10323](https://github.com/StarRocks/starrocks/pull/10323) [#14034](https://github.com/StarRocks/starrocks/pull/14034)
- [窗口函数](../sql-reference/sql-functions/Window_function.md)中支持使用 QUALIFY 来筛选查询结果。[#13239](https://github.com/StarRocks/starrocks/pull/13239)
- 建表时，支持指定 uuid 或 uuid_numeric 函数返回的结果作为列默认值。相关文档，请参见 [CREATE TABLE](../sql-reference/sql-statements/data-definition/CREATE%20TABLE.md)。[#11155](https://github.com/StarRocks/starrocks/pull/11155)
- 新增以下函数：[map_size](../sql-reference/sql-functions/map-functions/map_size.md)、[map_keys](../sql-reference/sql-functions/map-functions/map_keys.md)、[map_values](../sql-reference/sql-functions/map-functions/map_values.md)、[max_by](../sql-reference/sql-functions/aggregate-functions/max_by.md)、[sub_bitmap](../sql-reference/sql-functions/bitmap-functions/sub_bitmap.md)、[bitmap_to_base64](../sql-reference/sql-functions/bitmap-functions/bitmap_to_base64.md)、[host_name](../sql-reference/sql-functions/utility-functions/host_name.md) 和 [date_slice](../sql-reference/sql-functions/date-time-functions/date_slice.md)。[#11299](https://github.com/StarRocks/starrocks/pull/11299) [#11323](https://github.com/StarRocks/starrocks/pull/11323) [#12243](https://github.com/StarRocks/starrocks/pull/12243) [#11776](https://github.com/StarRocks/starrocks/pull/11776) [#12634](https://github.com/StarRocks/starrocks/pull/12634) [#14225](https://github.com/StarRocks/starrocks/pull/14225)

### 功能优化

- 优化了 [Hive catalog](../data_source/catalog/hive_catalog.md)、[Hudi catalog](../data_source/catalog/hudi_catalog.md) 和 [Iceberg catalog](../data_source/catalog/iceberg_catalog.md) 的元数据访问速度。[#11349](https://github.com/StarRocks/starrocks/issues/11349)
- [Elasticsearch 外部表](../data_source/External_table.md#elasticsearch-外部表)支持查询 ARRAY 类型数据。[#9693](https://github.com/StarRocks/starrocks/pull/9693)
- 物化视图优化
  - 多表异步刷新物化视图支持 SPJG 类型的物化视图查询的自动透明改写。相关文档，请参见[物化视图](../using_starrocks/Materialized_view.md#使用多表物化视图查询改写)。[#13193](https://github.com/StarRocks/starrocks/issues/13193)
  - 多表异步刷新物化视图支持多种异步刷新机制。相关文档，请参见[物化视图](../using_starrocks/Materialized_view.md#关于多表异步物化视图刷新策略)。[#12712](https://github.com/StarRocks/starrocks/pull/12712) [#13171](https://github.com/StarRocks/starrocks/pull/13171) [#13229](https://github.com/StarRocks/starrocks/pull/13229) [#12926](https://github.com/StarRocks/starrocks/pull/12926)
  - 优化了物化视图的刷新效率。[#13167](https://github.com/StarRocks/starrocks/issues/13167)
- 支持在建表时自动设置适当的分桶数。相关文档，请参见 [CREATE TABLE](../sql-reference/sql-statements/data-definition/CREATE%20TABLE.md)。[#10614](https://github.com/StarRocks/starrocks/pull/10614)
- 导入优化
  - 优化多副本导入性能，支持 single leader replication 模式，导入性能提升 1 倍。关于该模式的详细信息，参见 [CREATE TABLE](../sql-reference/sql-statements/data-definition/CREATE%20TABLE.md) 的 `replicated_storage` 参数。[#10138](https://github.com/StarRocks/starrocks/pull/10138)
  - 在单 HDFS 集群或单 Kerberos 用户下无需部署 broker 即可通过 Broker Load 或 Spark Load 进行数据导入。如果您配置了多个 HDFS 集群或者多个 Kerberos 用户，需要继续通过 Broker 进程执行导入。相关文档，请参见[从 HDFS 或外部云存储系统导入数据](../loading/BrokerLoad.md)和[使用 Apache Spark™ 批量导入](../loading/SparkLoad.md)。[#9049](https://github.com/starrocks/starrocks/pull/9049) [#9228](https://github.com/StarRocks/starrocks/pull/9228)
  - 优化了 Broker Load 在大量 ORC 小文件场景下的导入性能。[#11380](https://github.com/StarRocks/starrocks/pull/11380)
  - 优化了向主键模型表导入数据时的内存占用。[#12068](https://github.com/StarRocks/starrocks/pull/12068)
- 优化了 StarRocks 内置的 `information_schema` 数据库以及其中的 `tables` 表和 `columns` 表；新增 `table_config` 表。相关文档，请参见 [Information Schema](../administration/information_schema.md)。[#10033](https://github.com/StarRocks/starrocks/pull/10033)
- 优化备份恢复：
  - 支持数据库级别的备份恢复。相关文档，请参见[备份与恢复](../administration/Backup_and_restore.md)。[#11619](https://github.com/StarRocks/starrocks/issues/11619)
  - 支持主键模型表的备份恢复。相关文档，请参见备份与恢复。[#11885](https://github.com/StarRocks/starrocks/pull/11885)
- 函数优化：
  - [time_slice](../sql-reference/sql-functions/date-time-functions/time_slice.md) 增加参数，可以计算时间区间的起始点和终点。[#11216](https://github.com/StarRocks/starrocks/pull/11216)
  - [window_funnel](../sql-reference/sql-functions/aggregate-functions/window_funnel.md) 支持严格递增模式，防止计算重复的时间戳。[#10134](https://github.com/StarRocks/starrocks/pull/10134)
  - [unnest](../sql-reference/sql-functions/array-functions/unnest.md) 支持变参。[#12484](https://github.com/StarRocks/starrocks/pull/12484)
  - lead 和 lag 窗口函数支持查询 HLL 和 BITMAP 类型数据。相关文档，请参见[窗口函数](../sql-reference/sql-functions/Window_function.md)。[#12108](https://github.com/StarRocks/starrocks/pull/12108)
  - [array_agg](../sql-reference/sql-functions/array-functions/array_agg.md)、[array_sort](../sql-reference/sql-functions/array-functions/array_sort.md)、[array_concat](../sql-reference/sql-functions/array-functions/array_concat.md)、[array_slice](../sql-reference/sql-functions/array-functions/array_slice.md) 和 [reverse](../sql-reference/sql-functions/string-functions/reverse.md) 函数支持查询 JSON 数据。[#13155](https://github.com/StarRocks/starrocks/pull/13155)
  - `current_date`、`current_timestamp`、`current_time`、`localtimestamp`、`localtime` 函数后不加`()`即可执行。例如 `select current_date;`。[#14319](https://github.com/StarRocks/starrocks/pull/14319)
- 优化了 FE 日志，去掉了部分冗余信息。[#15374](https://github.com/StarRocks/starrocks/pull/15374)

### 问题修复

修复了如下问题：

- append_trailing_char_if_absent 函数对空值操作有误。[#13762](https://github.com/StarRocks/starrocks/pull/13762)
- 使用 RECOVER 语句恢复删除的表后，表不存在。[#13921](https://github.com/StarRocks/starrocks/pull/13921)
- SHOW CREATE MATERIALIZED VIEW 返回的结果缺少 catalog 及 database 信息。 [#12833](https://github.com/StarRocks/starrocks/pull/12833)
- waiting_stable 状态下的 schema change 任务无法取消。 [#12530](https://github.com/StarRocks/starrocks/pull/12530)
- `SHOW PROC '/statistic';` 命令在 leader FE 和非 leader FE 上返回的结果不同。 [#12491](https://github.com/StarRocks/starrocks/issues/12491)
- FE 生成的执行计划缺少 partition ID，导致 BE 获取 Hive partition 数据失败。[#15486](https://github.com/StarRocks/starrocks/pull/15486)
- SHOW CREATE TABLE 返回结果中 ORDER BY 子句位置错误。[#13809](https://github.com/StarRocks/starrocks/pull/13809)

### 行为变更

- `AWS_EC2_METADATA_DISABLED` 参数默认设置为 `False`，即默认获取 Amazon EC2 的元数据，用于访问 AWS resource。
- 会话变量 `is_report_success` 更名为 `enable_profile`，可通过 SHOW VARIABLES 语句查看。
- 新增四个关键字：`CURRENT_DATE`, `CURRENT_TIME`, `LOCALTIME`, `LOCALTIMESTAMP`。[#14319](https://github.com/StarRocks/starrocks/pull/14319)
- 表名和库名的长度限制放宽至不超过 1023 个字符。 [#14929](https://github.com/StarRocks/starrocks/pull/14929) [#15020](https://github.com/StarRocks/starrocks/pull/15020)
- BE配置项 `enable_event_based_compaction_framework` 和 `enable_size_tiered_compaction_strategy` 默认开启，能够在 tablet 数比较多或者单个 tablet 数据量比较大的场景下大幅降低 compaction 的开销。

### 升级注意事项

- 可以从 2.0.x，2.1.x，2.2.x，2.3.x 或 2.4.x 升级。一般不建议回滚版本，如需回滚，建议只回滚到 2.4.x。
- 如果您的系统中存在已经创建的 List 分区表，需要先删除后再进行升级。
