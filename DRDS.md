##MaHua是什么?
一个在线编辑markdown文档的编辑器

向Mac下优秀的markdown编辑器mou致敬

##MaHua有哪些功能？

* 方便的`导入导出`功能
    *  直接把一个markdown的文本文件拖放到当前这个页面就可以了
    *  导出为一个html格式的文件，样式一点也不会丢失
* 编辑和预览`同步滚动`，所见即所得（右上角设置）
* `VIM快捷键`支持，方便vim党们快速的操作 （右上角设置）
* 强大的`自定义CSS`功能，方便定制自己的展示
* 有数量也有质量的`主题`,编辑器和预览区域
* 完美兼容`Github`的markdown语法
* 预览区域`代码高亮`
* 所有选项自动记忆

##有问题反馈
在使用中有任何问题，欢迎反馈给我，可以用以下联系方式跟我交流

* 邮件(dev.hubo#gmail.com, 把#换成@)
* QQ: 287759234
* weibo: [@草依山](http://weibo.com/ihubo)
* twitter: [@ihubo](http://twitter.com/ihubo)

##捐助开发者
在兴趣的驱动下,写一个`免费`的东西，有欣喜，也还有汗水，希望你喜欢我的作品，同时也能支持一下。
当然，有钱捧个钱场（右上角的爱心标志，支持支付宝和PayPal捐助），没钱捧个人场，谢谢各位。

##感激
感谢以下的项目,排名不分先后

* [mou](http://mouapp.com/) 
* [ace](http://ace.ajax.org/)
* [jquery](http://jquery.com)

##关于作者


##分库(db partition)
一张表拆分到不同的db中
```mysql
CREATE TABLE `shop` (
  `shop_id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) ,
  PRIMARY KEY (`shop_id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8 dbpartition by hash(`shop_id`)
```

## ALIYUN DRDS 
DRDS,2RDS,16SCHEMA
<html>
 <head></head>
 <body>
  <table> 
   <tbody>
    <tr> 
     <td>DRDS SCHEMA</td> 
     <td>RDS</td> 
     <td>RDS SCHEMA</td> 
    </tr> 
    <tr> 
     <td rowspan="16"> <br /> columbudrdspublic.mysql.rds.aliyuncs.com<br /> SCHEMA:columbu </td> 
     <td rowspan="8">columbupieces1.mysql.rds.aliyuncs.com</td> 
     <td>columbu_jmay_0000</td> 
    </tr> 
    <tr>
     <td>columbu_jmay_0001</td>
    </tr>
    <tr>
     <td>columbu_jmay_0002</td>
    </tr>
    <tr>
     <td>columbu_jmay_0003</td>
    </tr>
    <tr>
     <td>columbu_jmay_0004</td>
    </tr>
    <tr>
     <td>columbu_jmay_0005</td>
    </tr>
    <tr>
     <td>columbu_jmay_0006</td>
    </tr>
    <tr>
     <td>columbu_jmay_0007</td>
    </tr> 
    <tr> 
     <td rowspan="8">columbupieces2.mysql.rds.aliyuncs.com</td> 
     <td>columbu_jmay_0008</td> 
    </tr> 
    <tr>
     <td>columbu_jmay_0009</td>
    </tr>
    <tr>
     <td>columbu_jmay_0010</td>
    </tr>
    <tr>
     <td>columbu_jmay_0011</td>
    </tr>
    <tr>
     <td>columbu_jmay_0012</td>
    </tr>
    <tr>
     <td>columbu_jmay_0013</td>
    </tr>
    <tr>
     <td>columbu_jmay_0014</td>
    </tr>
    <tr>
     <td>columbu_jmay_0015</td>
    </tr> 
   </tbody>
  </table>
 </body>
</html>

