# Note
# # 复习下php的基础知识，面试专用
## 1、说下php中的多态？ ##
   php中的多态是指相同的操作或者函数，可作用多种不同类型的对象上并获得不同的结果，即一个对外接口，多个内部方法
## 2、redis如何持久化？ ##
   snapshotting（保存数据），append-only-file（保存操作）
## 3、如何跨域？ ##
   jsonp方式，type：get, dataType：jsonp , 后端指定header，接收callback的名并调用
## 4、攻击方式及防范？ ##
   XSS,CSRF，DDOS（购买防护，封IP）
## 5、数据库索引？ ##
  1.  innodb ：并发好，行锁,插入速度慢，支持外键；存储为2个文件：表结构+数据,索引
1.    mysiam ：并发差，表锁，不支持事务，支持压缩（需要重建索引），适合频繁读取写入的系统；存储为3个文件：表结构，表数据，索引
1.    Memory ：容易丢失，速度最快
## 6、数据库优化策略？ ##
1.    优化索引和存储引擎，sql语句，开启慢查询日志分析sql
1.    大量查询的可做缓存和页面静态化
1.    配置主从集群，读写分离
1.    整理数据碎片
1.    换个好点的服务器，最后分区分表(很麻烦)，实在不行就别用mysql，换用ES（分布式存储）
## 7、如何使用索引？ ##
   1. 列独立，查询的不参与运算
1.    like左侧不要有通配符
1.    使用or，要求参与or运算的两边的字段都存在索引
1.    把用到最频繁的字段放最左边（复合索引）
1.    查询数据不要超过20%，否则自动放弃使用索引
## 8、php的运行模式？ ##
   web模块运行   cgi运行
   cli模式运行   fast-cgi运行
## 9、session  & cookies的缺点？ ##
   session过度使用不好维护，依赖cookies，服务器重启数据丢失
   cookies容量小，不安全，易盗用
## 10、session如何共享？ ##
   redis or memchche 共享
   集群：1，组播方式  2，存储介质（设备）  3，完全用cookies（推荐）
## 11、mysql主从分布式数据库数据同步的原理？ ##
   二进制日志文件的读取
## 12、计划任务？ ##
   crontab -e
## 13、开发中用到的php设计模式？ ##
   单例模式，工厂模式，适配器模式
## 14、加密接口的实现？ ##
   访问之前加密，调用解密，比如解密结果
## 15、如何防盗链？ ##
   确定来源Referer
## 16、魔术方法？ ##
   __construct() __destruct()  __autoload()
   __call()   __tostring()

## 33、说出linux中的常用的15个指令 ##
1.    ls cp rm mv cd init su pwd touch mkdir useradd usermod userdel groupadd groupmod groupdel
1.    目录切换指令ls cd pwd
1.     init 
1.    用户切换su
1.    文件操作：touch  cp  mv  rm
1.    文件夹操作：mkdir  cp -r  mv  rm -rf
1.    用户操作：useradd usermod userdel 
1.    组操作：groupadd groupmod groupdel
1.    权限：Chmod
## 34、说说使用nginx遇到过的问题 ##
nginx是不支持tp框架的PATH_INFO模式的，需要配置重写规则
请求的访问路径中含有 .php 时, 通过正则表达式构造出 PATHINFO, 并设置到 fastcgi 的参数 PATH_INFO 中
## 35、什么是负载均衡？如何处理高并发的请求？有什么解决方案？ ##
当一台web服务器无法满足并发请求时，可以通过增加web服务器来提高性能，这时就需要使用到负载均衡技术。负载均衡技术就是负责把用户的请求分发到不同的web服务器。

## 36、用过memcache吗？你的项目中的哪些数据被存入了memcache？为什么存入？ ##
使用过，之前做过的一个电商项目中引入了memcache，把前台首页的商品分类数据存入了memcache。理由如下：1，因为这些数据请求的比较频繁，memcache是把数据存在了内存中，读取的速度比较快。2，这些数据从安全的角度来看不重要，是任何人都能看到的数据，所以不需要考虑安全性，memcache本身恰好没有提供任何的认证机制。3，这些数据修改不频繁，商品的分类已经固定了，不需要频繁的修改。4，商品分类数据量不是很大，小于1MB。
## 37、多台服务器，怎么解决session数据丢失的问题？ ##
1.    放数据库中；
1.    放memcache；
打开php.ini文件，配置session文件的存储方式
session数据入memcache的实现：
		步骤1：更改session的存储机制（存储方式和存储路径）
			方式1：修改php.ini 【不推荐】
			方式2：使用ini_set()【推荐】		
		步骤2：正常操作session数据即可。	
通过分布式算法来实现数据的存入和查询，这个算法是内置的，不需要我们来干预，可以通过Memcache::addServer — 向连接池中添加一个memcache服务器
## 38、为什么把session数据存入memcache？ ##
简单的来说是为了让各台服务器的session数据共享

## 39、如何把memcache的缓存时间设为30天？ memcahce缓存的最大有效期是多少？ ##
缓存周期有两种设置方式：
1.    时间间隔（秒数）【从当前的时间点向后顺延指定的秒数作为有效期】
时间间隔10*24*3600秒
1.    到期的时间戳，必须大于当前的时间戳才有效。【从1970-1-1 0:0:0 起到现在经历的秒数作为有效期】
到期的时间戳：time()+10*24*3600
## 40、Memcache和redis区别 ##
1.    内存缓存服务器memcache
1.    内存高速缓存数据库redis
1.    数据类型:memcache支持的数据类型就是字符串，redis支持的数据类型有字符串，哈希，链表，集合，有序集合。
1.    持久化：memcache数据是存储到内存里面，一旦断电，或重启，则数据丢失。redis数据也是存储到内存里面的，但是可以持久化，周期性的把数据给保存到硬盘里面，导致重启，或断电不会丢失数据。
1.    数据量：memcahce一个键存储的数据最大是1M,而redis的一个键值，存储的最大数据量是1G的数据量。
## 41、redis持久化的方法 ##
默认：snapshotting快照方式持久化：一次性将redis中的数据保存在硬盘中，不适合大数据的频繁进行持久化操作
append-only-file  追加方式持久化AOF：是把指令保存在文件中，影响性能
## 42、myisam和innodb区别 ##
1.    数据的文件存储格式：
1.    Innodb的数据文件由两个文件构成：结构，索引和数据，默认情况下，所有表的数据和索引文件存一个文件中，需要手动配置使每个表拥有独立的数据索引文件
1.    Myisam的数据文件：数据-索引-结构 存在不同的文件
1.    数据的记录存储顺序：
1.    Innodb(不适合大量数据的插入操作)，因为会根据主键进行排序，影响插入性能
1.    Myisam(适合大量数据的插入)，数据的存储顺序就是插入顺序，没有排序
1.        并发性：
1.    Innodb:支持行锁和行锁，并发性好，支持外键，事务
1.    Myisam：只支持表锁，并发性差,支持压缩（压缩后不能插入新数据，要先解压，一般来说这个表只需要查询操作就压缩这个表）
1.    总结：
1.    myisam： 插入数据非常快，适合使用场合dedecms/phpcms/discuz/微博系统等插入、读取操作多的系统。
1.    innodb: 适合业务逻辑比较强的系统，修改 操作较多的，例如ecshop、crm、办公系统、商城系统
## 43、实现数据库的优化方法 ##
1.    选择合适的存储引擎（根据业务需求）
1.    逆范式/反三范式：故意违反三范式，增加冗余字段，减少连表查询，提高查询效率。
1.    通过profile查看每条语句执行时间并优化sql语句（加索引）
1.    查询缓存：提高查询效率。
1.    分区技术：单表数据条数比较大，例如1亿条记录。
1.    分表技术：解决表中大数据的问题
1.    数据碎片修复：数据表的update和delete操作比较频繁，周期性的进行数据碎片修复

## 44、哪些字段适合建索引 ##
【创建索引时，注意事项】：
1.    频繁作为查询条件的字段，应当建立索引
1.    唯一性差的字段，不适合建索引，例如：性别字段。
1.    给经常查询的字段建立一个复合索引，用到索引覆盖。
1.    数据频繁更新的字段，不适合建索引。

## 45、一千万条记录，进行分页展示，20条记录为一页，会有什么问题？ ##
页码越大，查询效率越低。
				解决：① 业务，禁止分页大于100页。
					  ② limit offset,num 替换掉  where + order by + limit num;

## 46、是否了解网站建构？谈谈你的看法。##
1.    对于用户并发量比较多的网站，采用增加服务器的方法，为了转发用户的请求，需要专门做一个负载均衡。减轻apache服务器的压力。
1.    除此之外，如果mysql压力较大的话，就单独做多个mysql服务器，多个mysql服务器中，一个作为主服务器，其余作为从服务器，主服务器负责写操作，从服务器复制读操作，做到读写分离。
## 47、列举出web项目中存在的安全问题以及解决方法 ##
1.    Sql注入：addslashes函数转义
1.    Xss攻击：防xss工具htmlpurifier
1.    总的来说，对于用户的输入要转义后在存入数据库
1.    登陆密码暴力破解
1.         对每个用户输错密码的次数做限制，比如输错3次以上当日不能登陆，或者加入验证码来防止暴力破解
1.         对于密码，如果有必要应使用多次md5加密并混入其他字符
1.    文件上传漏洞
1.    上传文件要判断文件的类型和大小
1.    撞库
自动识别异常ip（1分钟尝试登陆100次），对于这些ip，整理成一个库并禁止访问
## 48、写出能够加快web端访问速度的方法 ##
先判断web段速度慢的原因，
1.    对于apache压力过大，做负载均衡，提升服务器配置与带宽，做防盗链；
1.    对于mysql压力大，先优化mysql数据库：建立索引，根据慢查询日志分析并优化sql语句，开启查询缓存，最后配置主从服务器（主从复制，首先需要开启mysql服务器的二进制日志文件，主从复制会根据日志记录的位置来进行同步）；
1.    从总体上，对html页面尽量使用静态化，对于数据不变的页面使用静态缓存
1.    使用memcache缓存存放常用且不重要的数据
## 49、提升数据库查询效率的方法除了使用缓存之外，还有哪些方法？ ##
1.    优化查询语句：尽量不用 * ,查询什么写什么，对查询的数据条数做限制
1.    适当建立索引（索引过多会影响插入和更新的效率）
1.    提高mysql服务器配置并配置主从mysql服务器
1.    尽量使用数字型字段，若只含数值信息的字段不要设计成字符型，在这个字段作为查询或连接的条件的时候，数字型仅仅比较一次，字符型要逐个比较
1.    对于表中的数据量过大，采用分库分表
## 50、高并发俩系统的部署方案？ ##
1.    实现负载均衡nginx（可防止单点故障，实现区域访问加速）
1.    使用高速缓存：memcache,减轻数据库的读写压力
1.    对于数据库可以配置主从服务器，读写分离
1.    数据表分区分库
## 51、压力测试工具 ##
阿里云PTS、LoadRunner、Apache JMeter
## 52、写结果 ##
1.    echo -10%3;        -1
1.    echo count(‘abc’)     1
1.    echo date('Y-m-d H:i:s',strtotime('-2 day'));   前天这个时刻的时间


## 53、truncate和delete命令有何区别 ##
TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同：二者均删除表中的全部行。但 TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少。  
1.    DELETE 语句每次删除一行，并在事务日志中为所删除的每行记录一项。
1.    TRUNCATE TABLE 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。 
1.    Rollbcak不能撤销truncate,能撤销delete

1.    TRUNCATE,DELETE,DROP放在一起比较：
1.    TRUNCATE TABLE：删除内容、释放空间但不删除定义。
1.    DELETE TABLE:删除内容不删除定义，不释放空间。
1.    DROP TABLE：删除内容和定义，释放空间。

## 54、什么是HTTP协议
协议是指计算机通信网络中两台计算机之间进行通信所必须共同遵守的规定或规则，超文本传输协议(HTTP)是一种通信协议，它允许将超文本标记语言(HTML)文档从Web服务器传送到客户端的浏览器

 










