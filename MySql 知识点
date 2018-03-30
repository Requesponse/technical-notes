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

> %d 十进制整数
> %s 字符串

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
  if (!empty($keyword)) $map['username|mobile'] = array('like', "%$keyword%");
  ```

- 原生

  ```Mysql
  SELECT * FROM `magazine` WHERE CONCAT(`title`,`tag`,`description`) LIKE ‘%关键字%’
  ```




## USING的用法

连标查询时，如果链接字段在两表中字段名相同，可以使用 USING，等同于`on a.uid = b.uid`。



##MySql 5.6之前的版本不允许使用2个 timestamp 字段


