# 用户模块

## 用户账户子模块

### 建表语句

```sql
CREATE TABLE `user` (
  /* 下方是通用字段 */
  `id` bigint UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键 ID',
  `create_time` datetime NOT NULL COMMENT '创建时间',
  `update_time` datetime NOT NULL COMMENT '更新时间',
  `auto_create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '由数据库维护的创建时间',
  `auto_update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '由数据库维护的更新时间',
  `version` int NOT NULL COMMENT '乐观锁（修改次数）',

  /* 下方为业务字段 */
  `nick_name` varchar(20) NOT NULL DEFAULT '' COMMENT '昵称',
  `avatar_path` varchar(100) NOT NULL DEFAULT '' COMMENT '头像路径',
  `register_time` datetime NOT NULL COMMENT '注册时间',
  `last_login_time` datetime NOT NULL COMMENT '最近一次登录时间',
  `login_counter` bigint UNSIGNED NOT NULL COMMENT '登录次数',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB
DEFAULT CHARACTER SET=utf8mb4
COMMENT='用户账户表';
```

## 用户微信账户模块

### 建表语句

```sql
CREATE TABLE `user_account_wechat` (
  /* 下方为通用字段 */
  `id` bigint UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键 ID',
  `create_time` datetime NOT NULL COMMENT '创建时间',
  `update_time` datetime NOT NULL COMMENT '更新时间',
  `auto_create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '由数据库维护的创建时间',
  `auto_update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '由数据库维护的更新时间',
  `delete_time` datetime NULL COMMENT '删除时间（逻辑删除标志）',
  `version` int NOT NULL COMMENT '乐观锁（修改次数）',

  /* 下方为业务字段 */
  `app_id` varchar(50) NOT NULL DEFAULT '' COMMENT '小程序开发者 ID',
  `open_id` varchar(50) NOT NULL DEFAULT '' COMMENT '小程序的用户唯一标识',
  `union_id` varchar(50) NOT NULL DEFAULT '' COMMENT '用户在微信开放平台的唯一标识符',
  `user_id` bigint UNSIGNED NOT NULL COMMENT '对应的用户 ID',
  `counter` bigint UNSIGNED NOT NULL COMMENT '当前小程序登录次数',
  `last_time` datetime NOT NULL COMMENT '最近一次使用（指通过当前行记录用于登录）时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB
DEFAULT CHARACTER SET=utf8mb4
COMMENT='此处修改为表备注';
```
