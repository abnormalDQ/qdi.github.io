# aliyun drds
1. DRDS,2RDS,16SCHEMA
<table>
<tr>
<td>DRDS SCHEMA</td>
<td>RDS</td>
<td>RDS SCHEMA</td>
</tr>
<tr>
    <td rowspan="16"> <br/>
        columbudrdspublic.mysql.rds.aliyuncs.com<br/>
        SCHEMA:columbu </td>
    <td rowspan="8">columbupieces1.mysql.rds.aliyuncs.com</td>
    <td>columbu_jmay_0000</td>
</tr>
<tr><td>columbu_jmay_0001</td></tr><tr><td>columbu_jmay_0002</td></tr><tr><td>columbu_jmay_0003</td></tr><tr><td>columbu_jmay_0004</td></tr><tr><td>columbu_jmay_0005</td></tr><tr><td>columbu_jmay_0006</td></tr><tr><td>columbu_jmay_0007</td></tr>
 <tr>
    <td rowspan="8">columbupieces2.mysql.rds.aliyuncs.com</td>
    <td>columbu_jmay_0008</td>
</tr>
<tr><td>columbu_jmay_0009</td></tr><tr><td>columbu_jmay_0010</td></tr><tr><td>columbu_jmay_0011</td></tr><tr><td>columbu_jmay_0012</td></tr><tr><td>columbu_jmay_0013</td></tr><tr><td>columbu_jmay_0014</td></tr><tr><td>columbu_jmay_0015</td></tr>
</table>
2.分库（db partition）
```
CREATE TABLE `shop` (
  `shop_id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`)
```
3.分表（table partition）
dbpartition by hash(`shop_id`) 
tbpartition by hash(`shop_id`) tbpartitions 3
``` 
CREATE TABLE `shop` (
  `shop_id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`) tbpartition by hash(`shop_id`) tbpartitions 3
``` 
4.广播表(broadcast):每个子SCHEMA 都存一份数据  
``` 
CREATE TABLE `shop` (
  `shop_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT BY GROUP,
  `shop_name` varchar(64) DEFAULT NULL COMMENT '店铺名称',
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 COMMENT='店铺表' broadcast
``` 
广播表采用的是多写，不是先写db1再同步的方式，所以在drds层面使用了sequence
我们一次性会缓存10万，然后规格比较大的话，每个节点缓存端会不太一样。


5.DRDS Sequence
Group Sequence（GROUP）
隐式 Sequence，在为主键定义 AUTO_INCREMENT 后，用于自动填充主键，由 DRDS 自动维护。
broadcast 与 group 联合使用
``` 
CREATE TABLE `shop` (
  `shop_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT BY GROUP,
  `shop_name` varchar(64) DEFAULT NULL COMMENT '店铺名称',
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 COMMENT='店铺表' broadcast
``` 
6.UNIQUE key 失效
``` 
CREATE TABLE `shop` (
  `id`  bigint(20) unsigned NOT NULL AUTO_INCREMENT by group,
  `shop_id`  bigint(20) unsigned NOT NULL,
  `sn` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `shop_id` (`shop_id`) USING BTREE,
  UNIQUE KEY `sn` (`sn`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`) 
``` 
INSERT INTO `print_tool`.`shard_table` (`shop_id`, `sn`) VALUES ( 1, '1');
INSERT INTO `print_tool`.`shard_table` (`shop_id`, `sn`) VALUES ( 2, '1');
7.主键不唯一 PRIMARY KEY
``` 
CREATE TABLE `shop` (
  `id`  bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `shop_id`  bigint(20) unsigned NOT NULL,
  `sn` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `shop_id` (`shop_id`) USING BTREE,
  UNIQUE KEY `sn` (`sn`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`) 
``` 
INSERT INTO `print_tool`.`shard_table` (`id`,`shop_id`, `sn`) VALUES ( 1,1, '1');
INSERT INTO `print_tool`.`shard_table` (`id`,`shop_id`, `sn`) VALUES ( 1,2, '1');
8.sql 使用注意事项
``` 
CREATE TABLE `order_info` (
  `order_id`  bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `shop_id`  bigint(20) unsigned NOT NULL,
  `shipping_id`  bigint(20) unsigned NOT NULL,
  `user_id`  bigint(20) unsigned NOT NULL,
  `sn` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `shop_id` (`shop_id`) USING BTREE,
  UNIQUE KEY `sn` (`sn`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`);
CREATE TABLE `order_goods` (
  `id`  bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `shop_id`  bigint(20) unsigned NOT NULL,
  `order_id`  bigint(20) unsigned NOT NULL,
  `sn` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `shop_id` (`shop_id`) USING BTREE,
  UNIQUE KEY `sn` (`sn`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`)
``` 
``` 
CREATE TABLE `shipping` (
  `shipping_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT BY GROUP,
  `shipping_name` varchar(64) DEFAULT NULL COMMENT '店铺名称',
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 COMMENT='店铺表' broadcast;
CREATE TABLE `user` (
  `user_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
  `user_name` varchar(64) DEFAULT NULL COMMENT '店铺名称',
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 COMMENT='店铺表';
``` 

(1)分表单独查询，select * from order_info where shop_id = 11;
(2)分表join
``` 
select * 
from order_info oi 
inner join order_goods og on oi.order_id = og.order_id and oi.shop_id = og.shop_id 
where oi.shop_id = 101;
``` 
(3)分表和广播表join
``` 
select *
from shipping ss 
inner join order_info oi on ss.shipping_id = oi.shipping_id 
where  oi.shop_id = 101 ;
``` 
(4)分表和单表join 效率非常低下。 
``` 
select *
from user ss 
inner join order_info oi on ss.user_id = oi.user_id 
where  oi.shop_id = 101 ;
``` 


