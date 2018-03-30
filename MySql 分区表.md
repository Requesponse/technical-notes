1. 查询 MySql 服务器是否支持分区表
   ```mysql
   mysql > SHOW PLUGINS;
   Name         Status   Type             Library   License
   partition    ACTIVE   STORAGE ENGINE   (NULL)    GPL
   ```
2. 特点：数据逻辑表现为一个表，物理存储在多个文件中
   ```mysql
   # mysql建表后自动生成，存储表结构
   customer_login_log.frm
   # 当引擎为 Innodb 生成的文件，并且使用的表空间为“独立表空间”
   customer_login_log#P#p0.ibd		
   customer_login_log#P#p1.ibd
   customer_login_log#P#p2.ibd
   customer_login_log#P#p3.ibd
   ```
