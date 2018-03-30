1. 查询 MySql 服务器是否支持分区表
   ```mysql
   mysql > SHOW PLUGINS;
   Name         Status   Type             Library   License
   partition    ACTIVE   STORAGE ENGINE   (NULL)    GPL
   ```
2. 特点：数据逻辑表现为一个表，物理存储在多个文件中
   建表语句
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
