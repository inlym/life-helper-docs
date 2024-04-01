# MySQL 数据库

## 规范

### 字段

#### 主键 ID

所有数据表必须有主键 ID，并且对应建表语句如下：

```sql
CREATE TABLE `xxx` (
	`id` bigint UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键ID',
	PRIMARY KEY (`id`)
)
```

#### 创建和更新时间

所有数据表必须包含 2 个由 **数据库自身维护** 的“创建时间”和“更新时间”，对应字段名分别为：

1. 创建时间 → `auto_create_time`
2. 更新时间 → `auto_update_time`

对应的建表语句为：

```sql
CREATE TABLE `xxx` (
  `auto_create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '由数据库维护的创建时间',
  `auto_update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '由数据库维护的更新时间',
)
```

这里由数据库维护的 2 个时间字段，不得在应用层使用，仅用于在进行数据库层面的调试。另外还有 2 个由应用层维护的“创建时间”和“更新时间”，对应字段名分别为：

1. 创建时间 → `create_time`
2. 更新时间 → `update_time`

对应的建表语句为：

```sql
CREATE TABLE `xxx` (
  `create_time` datetime NOT NULL COMMENT '创建时间',
  `update_time` datetime NOT NULL COMMENT '更新时间',
)
```
