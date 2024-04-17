# MySQL 数据库

笔者在项目开发中，使用了 MySQL 作为主存储数据库，当前文档总结了笔者在开发过程中遵循的规范和一些新的体会。考虑到笔者的开发水平以及项目架构，部分经验并不一定适合读者，请在阅读时仔细甄别。

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

## 范例

### 建表语句

以下是通用建表语句：

```sql
CREATE TABLE `xxx_table_name` (
  /* 下方是通用字段 */
  `id` bigint UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键 ID',
  `create_time` datetime NOT NULL COMMENT '创建时间',
  `update_time` datetime NOT NULL COMMENT '更新时间',
  `auto_create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '由数据库维护的创建时间',
  `auto_update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '由数据库维护的更新时间',
  `delete_time` datetime NULL COMMENT '删除时间（逻辑删除标志）',
  `version` int NOT NULL COMMENT '乐观锁（修改次数）',

  /* 下方是修改为业务字段 */
  `xxx_string_column` varchar(100) NOT NULL DEFAULT '' COMMENT '不定长字符串字段',
  `xxx_string2_column` char(32) NOT NULL DEFAULT '' COMMENT '定长字符串字段',
  `xxx_datetime_column` datetime NOT NULL COMMENT '时间字段',
  `xxx_date_column` date NOT NULL COMMENT '日期字段',
  `xxx_enum_column` tinyint NOT NULL COMMENT '枚举字段',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB
DEFAULT CHARACTER SET=utf8mb4
COMMENT='此处修改为表备注';
```
