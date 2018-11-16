
安装  
```
yum install https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-centos96-9.6-3.noarch.rpm
yum install postgresql96
yum install postgresql96-server
/usr/pgsql-9.6/bin/postgresql96-setup initdb
systemctl enable postgresql-9.6
systemctl start postgresql-9.6
```

使用  
```
切换用户  
su - postgres 
登录控制台
psql
创建用户
CREATE USER dbuser WITH PASSWORD 'password';
创建数据库
CREATE DATABASE exampledb OWNER dbuser;
修改用户密码
\password dbuser
```
接下来我们就可以使用刚才创建的用户登录控制台并连接到创建的数据库中来进行一系列的操作了  
```
$ psql -U dbuser -d exampledb
```

常用的控制台命令
```
\password           设置密码
\q                  退出
\h                  查看SQL命令的解释，比如\h select
\?                  查看psql命令列表
\l                  列出所有数据库
\c [database_name]  连接其他数据库
\d                  列出当前数据库的所有表格
\d [table_name]     列出某一张表格的结构
\x                  对数据做展开操作
\du                 列出所有用户
```

常用的SQL语句  
```
# 创建新表
CREATE TABLE table_name(name VARCHAR(20), birth DATE);
# 插入数据
INSERT INTO table_name(name, birth) VALUES('欧文', '1994-08-23');
# 查询记录
SELECT * FROM table_name;
# 更新数据
UPDATE table_name set name = '勒夫' WHERE name = '欧文';
# 删除记录
DELETE FROM table_name WHERE name = '欧文' ;
# 添加字段
ALTER TABLE table_name ADD email VARCHAR(40);
# 更改字段类型
ALTER TABLE table_name ALTER COLUMN birth SET NOT NULL;
# 设置字段默认值（注意字符串使用单引号）
ALTER TABLE table_name ALTER COLUMN email SET DEFAULT 'example@example.com';
# 去除字段默认值
ALTER TABLE table_name ALTER email DROP DEFAULT;
# 重命名字段
ALTER TABLE table_name RENAME COLUMN birth TO birthday;
# 删除字段
ALTER TABLE table_name DROP COLUMN email;
# 表重命名
ALTER TABLE table_name RENAME TO backup_table;
# 删除表
DROP TABLE IF EXISTS backup_table;
# 删除库
\c postgres;
DROP DATABASE IF EXISTS hello;
```
