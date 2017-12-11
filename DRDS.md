## ALIYUN DRDS 
DRDS,2RDS,16SCHEMA


## 分库(db partition)
<p>一张表拆分到不同的db中</p>
```mysql
CREATE TABLE `shop` (
  `shop_id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`);
```
