## ALIYUN DRDS 
DRDS,2RDS,16SCHEMA
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
##分库(db partition)
<p>一张表拆分到不同的db中</p>
```mysql
CREATE TABLE `shop` (
  `shop_id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`)
```
