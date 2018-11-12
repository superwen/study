```


sudo apt-key adv --keyserver keyserver.ubuntu.com --recv E0C56BD4    # optional

sudo apt-add-repository "deb http://repo.yandex.ru/clickhouse/deb/stable/ main/"
sudo apt-get update

sudo apt-get install -y clickhouse-server clickhouse-client

sudo service clickhouse-server start
clickhouse-client

参考运维指南->参数设置->设置数据目录
sudo service clickhouse-server start 

service clickhouse-server stop
service clickhouse-server start

clickhouse-client
clickhouse-client –host 192.168.3.54 –port 9000 –database default–user default –password “” 

放开远程访问
vi /etc/clickhouse-server/config.xml 
修改 listen-host 节点

内存限制设置
vi /etc/clickhouse-server/users.xml 
修改 max_memory_usage 节点

设置数据目录
vi /etc/clickhouse-server/config.xml 
修改 path 和 temp_path

错误日志
/var/log/clickhouse-server/



CREATE DATABASE IF NOT EXISTS test

CREATE TABLE code_province( \
    state_province        String, \
    province_name         String, \
    create_date           date \
) ENGINE = MergeTree(create_date, (state_province), 8192);

ENGINE：是表的引擎类型，最常用的MergeTree。还有一个Log引擎也是比较常用。MergeTree要求有一个日期字段，还有主键。Log没有这个限制。 
create_date：是表的日期字段，一个表必须要有一个日期字段。 
State_province：是表的主键，主键可以有多个字段，每个字段用逗号分隔 
8192：是索引粒度，用默认值8192即可。

–建表（sql语句） 
CREATE TABLE default.test3 ( id1 UInt32, id2 Float32, name1 String, name2 String, date1 Date, date2 DateTime) ENGINE = Log;

–测试数据（Linux命令） 
root@smartbi-virtual-machine:/data/test# cat test3.csv 
1,123.456,”abc123”,”abc”“’ 
123”,2017-05-06,2017-05-06 07:08:09

–执行导入（Linux命令） 
clickhouse-client –query “INSERT INTO default.test3 FORMAT CSV” < test3.csv




CREATE TABLE IF NOT EXISTS adlog (
			fn 	String,
			ac  String,
			ao  Int64,
			p   Int64,
			l   Int64,
			k 	Int64,
			app String,
			c1 	String,
			c2 	String,
			jc	String,
			d	Int64,
			r 	String,
			n  	Int64,
			cf 	Int64,
			sdf Int64,
			b 	String,
			mi 	String,
			mac String,
			hi  Int64,
			di  Int64,
			dv  Float32,
			dt  String,
			db  String,
			ct  DateTime,
			dr  String,
			al  String,
			s1  String,
			s2  String,
			it  DateTime
		) engine=Memory

```
