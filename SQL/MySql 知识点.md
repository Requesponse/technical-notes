## 导入 sql 文件，报 `MySql server has gone away`

```mysql
// insert into 数据超过设置
set global max_allowed_packet=104857600  （100M）
```



## 开启远程访问

1. 开放3306端口

   ```Shell
   iptables -I INPUT 4 -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT

   service iptables save #保存iptables规则
   ```

2. 咔叽 MySql 远程访问权限

   ```mysql
   GRANT ALL PRIVILEGES ON database_name.table_name TO 'user_name'@'%'IDENTIFIED BY 'password' WITH GRANT OPTION;

   FLUSH PRIVILEGES;
   ```



## PDO 查询并解析结果

```Php
if ( $res = $pdo->query('SELECT * FROM table_name') ) {
    $data = $res->fetchAll(PDO::FETCH_ASSOC);
}
```



## 修改表字符集和排序规则

```mysql
ALTER TABLE userinfo CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;
```


## case when then 用法

> %d: 十进制整数
> %s: 字符串

```Php
$sql = "UPDATE goods_combination SET flash_sale_price = CASE id "; 
foreach ($list as $id => $flash_sale_price) { 
	$sql .= sprintf("WHEN %d THEN %d ", $id, $flash_sale_price); 
} 
$sql .= "END WHERE id IN ($ids)"; 
```



## 字段结果中文排序

```Php
// order默认使用gb2312,如果数据使用的UTF-8,需要转换
$ds = $college->order(convert("name using gb2312"))->select();
```



## 多字段 `or ` 查询

- TP

  ```Php
  if (!empty($keyword)) $map['username|mobile'] = array('like', "%$keyword");
  ```

- 原生

  ```Mysql
  SELECT * FROM `magazine` WHERE CONCAT(`title`,`tag`,`description`) LIKE ‘%关键字’
  ```




## USING的用法

连标查询时，如果链接字段在两表中字段名相同，可以使用 USING，等同于`on a.uid = b.uid`。



## MySql 5.6之前的版本不允许使用2个 timestamp 字段

## FIND_IN_SET 函数
```mysql
在mysql中，有时我们在做数据库查询时，需要得到某字段中包含某个值的记录，但是它也不是用like能解决的，使用like可能查到我们不想要的记录，它比like更精准，这时候mysql的FIND_IN_SET函数就派上用场了，下面来具体了解一下。

FIND_IN_SET(str,strlist)函数

str 要查询的字符串

strlist 字段名 参数以”,”分隔 如 (1,2,6,8)

查询字段(strlist)中包含(str)的结果，返回结果为null或记录

下面举例说明

test表中有如下字段及值
下面我想查询area中包含”1″这个参数的记录
SELECT * from test where FIND_IN_SET('1',area)

FIND_IN_SET和like的区别

like是广泛的模糊匹配，字符串中没有分隔符，Find_IN_SET 是精确匹配，字段值以英文”,”分隔，Find_IN_SET查询的结果要小于like查询的结果。
```
