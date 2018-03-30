1. 查询 MySql 服务器是否支持分区表
   ```mysql
   mysql > SHOW PLUGINS;
   Name         Status   Type             Library   License
   partition    ACTIVE   STORAGE ENGINE   (NULL)    GPL
   ```
2. 特点：数据逻辑表现为一个表，物理存储在多个文件中
	- 建表语句
		```mysql
		CREATE TABLE `customer_login_log` (
		`login_id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '登录日志ID',
		`customer_id` int(10) unsigned NOT NULL COMMENT '登录用户ID',
		`login_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '用户登录时间',
		`login_ip` int(10) unsigned NOT NULL COMMENT '登录IP',
		`login_type` tinyint(4) NOT NULL COMMENT '登录类型:0未成功 1成功',
		PRIMARY KEY (`login_id`)
		) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='用户登录日志表'
		PARTITION BY HASH(customer_id)
		PARTITION 4;
		```
	- 生成的文件
		```shell
		//mysql建表后自动生成，存储表结构
		customer_login_log.frm
		//当引擎为 Innodb 生成的文件，并且使用的表空间为“独立表空间”
		customer_login_log#P#p0.ibd
		customer_login_log#P#p1.ibd
		customer_login_log#P#p2.ibd
		customer_login_log#P#p3.ibd
		```
3. 三种分区方法
	1. 按 HASH 分区
		- 特点：根据 MOD(分区键、分区数)的值把数据行存储到表的不同分区中，数据可以平均的分布在各个分区文件中
		- HASH分区的键值必须是一个INT类型的值，或是通过函数可以转为INT类型
		- 建立：PARTITION BY HASH(customer_id)   PARITITIONS 4（表示建立以customer_id为 hash值，4个分区文件）
		- 使用：正常使用sql语句  
	2. 按 RANGE 分区
		- 根据分区键值的范围把数据行存储到表的不同分区中
		- 多个分区的范围要连续，但是不能重叠
		- 默认情况下使用 VALUES LESS THAN 属性，即每个分区不包括指定的那个值
		- 建立：最好加入`剩余`分区，不然报错
		- 场景
			1. 分区键为日期或时间类型
			2. 所有查询中最好都包括分区键作为筛查条件，避免跨分区扫描数据
			3. 定期按分区范围清理历史数据
	3. 按 LIST 分区
		- 特点
			1. 按分区键取值的列表进行分区
			2. 各分区的列表值不能重复
			3. 每一行数据必须能找到对应的分区列表，否则数据插入失败
			```mysql
			INSERT INTO customer_login_list (customer_id, login_time, login_ip, login_type) VALUES (100, NOW(), 1, 10);
			// 错误代码：1526
			// Table has no paritition for value 10
			```
		- 建立
			```mysql
			PARTITION BY LIST(login_type)(
				PARITITION p0 VALUES in (1,3,5,7,9),
				PARITITION p1 VALUES in (2,4,6,8)
			);    
			```
4. 使用分区表的注意事项
		- 结合业务场景选择分区键，避免跨分区查询
		- 对分区表进行查询最好在 where 从句中包含分区键
		- 具有主键或唯一索引的表，主键或唯一索引必须是分区键的一部分
