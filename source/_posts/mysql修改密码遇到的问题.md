---
title: mysql修改密码遇到的问题
date: 2018-10-18 09:53:45
categories:
- mysql
tags:
- mysql
- sequelize
- nodejs
---

# 最近在研究 nodejs 的 sequelize遇到了一些 mysql 的管理问题

## 首先安装mysql

```bash
# 安装
brew install mysql
# 需要 mysql 在开机自动运行就执行
brew services start mysql
# 反过来就应该是
brew services stop mysql
# 手动开启 mysql
mysql.server start
# 默认安装是没有密码的可以直接连接
mysql -uroot
```

> 执行上面这些命令 mysql 就安装好了

## 连接 mysql 创建数据库

```sql
create database $database_name
```

## 用 sequelize 创建 migration

```bash
# 生成 migration 文件
sequelize model:generate --name User --attributes firstName:string,lastName:string,email:string

# 执行 migration
sequelize db:migrate

# 报错
Client does not support authentication protocol requested by server; consider upgrading MySQL client
```

> 这个错误是由于没有设置密码导致的所以进入 mysql 控制台设置密码

```sql
use mysql;
alter user 'root'@'127.0.0.1' identified with mysql_native_password by '123456';

# 报错
Your password does not satisfy the current policy requirements.
```

> 现在解决上面的报错,是由于密码策略太强引起的,我们开发不需要那么强的密码策略, 继续在 mysql 控制台里修改

```sql
# 查找变量,因为随着版本更迭这个变量可能会改名😭
SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)

# 设置密码策略强度
set validate_password.policy=LOW;

# 设置密码长度
SET GLOBAL validate_password_length = 6;

# 重新设置密码
use mysql;
alter user 'root'@'localhost' identified with mysql_native_password by '123456';
# 刷新
flush privileges;
```

> 下面就可以愉快的使用 mysql 了 🤣🤣🤣