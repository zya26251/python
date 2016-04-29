# 一 My SQL数据库基础 #

## 1. 数据库分类   
-    关系型数据库：MySQL ,ORACLE,SQL Server,PostgreSQL  
-    非关系型数据库：hadoop(处理大数据),mongoDB（文档数据库）,redis,cassandra
## 2. 通向老司机之路  
   a、正确使用数据库  
   b、运维调优数据库  
   c、数据库内在原理

## 3. mysql安装   

[http://mooc.study.163.com/learn/NEU-1000077004?tid=2001224000#/learn/content?type=detail&id=2001452002](http://mooc.study.163.com/learn/NEU-1000077004?tid=2001224000#/learn/content?type=detail&id=2001452002)
## 4. 连接数据库  
*用管理员身份登陆  
   

- 本地连接  mysql -hlocalhost -P3306 -uroot -p  


- TCP/IP链连接  mysql -h127.0.0.1 -P3306 -uroot -p

- 通过socket连接  mysql -S/tem/mysql.sock -uroot -p  
  这条是linux系统下，-S后面是socket的路径(后面有方法说明怎么查询socket的路径)；windows下mysql -SMySQL -uroot -p
## 5. 配置环境变量 ##
## 6. 常用命令 
   ![](http://ww4.sinaimg.cn/mw690/ea60ffb8gw1f3cj72zio5j20kj0ciq4f.jpg)

- mysql>status :查看mysql基本信息
  

-  mysql>show processlist；: 当前连到数据库的链接状态
   

- 习惯用help命令
## 7.socket 
- 查看socket存储路径：mysql>show global variables like 'socket';
- 文件权限要求为777 ，不要更改，否则无法通过socket连接数据库,如果不小心删除socket文件，需要重启mysql
- 不要把密码直接输在命令行，会存在风险

## 8、关系型数据库 ##
- 每一行叫记录

## 其他 ##
- Oracle VM VirtualBox虚拟机uuid更改 [http://jingyan.baidu.com/article/454316ab781713f7a6c03a5a.html](http://jingyan.baidu.com/article/454316ab781713f7a6c03a5a.html)

# 二 MySQL数据库对象与应用 #
## 1、数据类型 ##
- **整形**  
int(11)和int(21) 区别：本身没有任何区别，存储空间和存储范围都一样  
int(11)如果是输入1，用0填满，会在前面补10个0；int(21)如果输入1，用0填满，前面会补20补0,是显示上的区别  
![](http://ww4.sinaimg.cn/mw690/ea60ffb8gw1f3cj790ilij20qv0ddq56.jpg)

- **浮点型**  
float和double都会造成精度丢失的问题，存储空间不会变  
定点数：DECIMAL
![](http://ww2.sinaimg.cn/mw690/ea60ffb8gw1f3cj7785txj20pi0dd765.jpg)
![](http://ww2.sinaimg.cn/mw690/ea60ffb8gw1f3cj75y07dj20pq0d9mzv.jpg)
**经验之谈：**  
存储性别和省份，推荐使用tinyint型，1字节；char(1)，1字节;或者枚举类型enum(如：enum（woman'.'man'）2字节)
BIGINT存储空间更大，INT和BIGINT之间通常选择BIGINT  
存储交易等高精度数据的时候，选择用DECIMAL  


- **字符与字节的区别**
![](http://ww1.sinaimg.cn/mw690/ea60ffb8gw1f3cj79llzej20ox0cgwgo.jpg)
推荐用utf8mb4  
**CHAR和VARCHAR：**定义的长度为字符长度不是字节长度，CHAR存储定长，容易造成空间浪费；VARCHAR存储变长，节省存储空间  
![](http://ww2.sinaimg.cn/mw690/ea60ffb8gw1f3cj729t98j20o404jt8y.jpg)
- **TEXT CHAR VARCHAR的区别**
![](http://ww4.sinaimg.cn/mw690/ea60ffb8gw1f3cj743azjj20ob0c7766.jpg)
- **时间类型**  
timestamp 随时区的改变而改变改变  datetime 不会变化  
![](http://ww1.sinaimg.cn/mw690/ea60ffb8gw1f3cj783wktj20oa0bzdh7.jpg)
![](http://ww2.sinaimg.cn/mw690/ea60ffb8gw1f3cj74i0rtj20q10a10tx.jpg)
**经验之谈：**  
DATE和TIME进度比较低  
根据业务需要选择  
BIGINT也可以存储时间

## 2、MYSQL数据对象 ##

- **常见的数据对象**  
DataBase/Schema  

- **库、表、行层级关系** 
![](http://ww2.sinaimg.cn/mw690/ea60ffb8gw1f3cj77u1bmj20os0apgmz.jpg)
- **多DATABASE用途：**  
  业务隔离；   
  资源隔离；  

- **表上常用的数据对象**  
  索引：数据库中数据的目录；提高检索数据的效率；数据发生变更索引也会发生变更  
  约束：  
  a、唯一约束，如用户ID，手机号，身份证号，大部分是两个以上字段组合；是一种特殊的索引；可以在建表的时候建好，也可以后续补上；主键也是唯一约束  
  b、外键约束 外键是指两张表的数据通过某种条件关联起来 ；创建外键必须两个表都是INNODB表，别的引擎不支持；两个约束的字段类型必须一样；主表的字段必须要有索引；约束的名称要唯一（库级别），即使在不同的表上  
  view视图：是一种虚拟结构，简化数据库访问，隐藏后端表结构，提高安全性  
  Trigger:触发器 增删改可激活触发器，create不行
  Function:提供各种函数，自己也可以创建函数
  Procedure
## 3、MYSQL权限管理 ##

- 连接mysql的必要条件：  
  网络通畅  
用户名和密码正确  
数据库要加白名单  
更细粒度的验证（库，表，列权限）

- 赋值操作：grant

- 回收用户权限：revoke selecr on *.* from 'netease'@'localhost'  去除用户所有表的查询权限 

- 更改用户的密码：   
用新密码，grant语句重新授权；   
更改数据库记录，Update User表的Password字段，更改完需要用flush privileges刷新权限信息，不推荐
  
- 删除用户  drop user 'netease'@'localhost' 高危操作

- 小结：  
  权限相关的操作不要直接操作表，统一使用mysql命令；  
  使用二进制安装mysql安装后，需要重新管理用户（root）的密码；  
  线上数据库不要留test库（不受权限控制，存在安全隐患)

## 4、实践 ##

如果varchar长度超过范围，会转为text
![](http://ww4.sinaimg.cn/mw690/ea60ffb8gw1f3cne7xcatj20nk0bt0v0.jpg)
![](http://ww3.sinaimg.cn/mw690/ea60ffb8gw1f3cne6ru5jj20ln0bojsq.jpg)
![](http://ww1.sinaimg.cn/mw690/ea60ffb8gw1f3cne5fdjnj20pf0dk413.jpg)
**数据类型命名规范**
![](http://ww3.sinaimg.cn/mw690/ea60ffb8gw1f3cne67j7fj20op0deq4u.jpg)
**字段设计规范**
![](http://ww3.sinaimg.cn/mw690/ea60ffb8gw1f3cneam6epj20pj0b6q4g.jpg)
![](http://ww4.sinaimg.cn/mw690/ea60ffb8gw1f3cnea3pe2j20oo0b2jsv.jpg)
**用户赋权**
![](http://ww4.sinaimg.cn/mw690/ea60ffb8gw1f3cne9aogbj20nj0d9myn.jpg)

## 5、测验 ##
1、下面几个语句中属于授权的语句是：  
grant select on *.* to jeffrey@'localhost'  
create user jeffrey@'localhost'  
2、下面权限属于管理权限(Server Admin)的是 :ABC
A.create user  
B.replication client  
C.shutdown
D.file