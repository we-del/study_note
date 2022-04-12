# mySQL

+ where语句和having语句作用相同，一个是在查询前触发(where)，一个是在查询后触发(having,having通常配合group by使用)。**即where用来在查询前进行查询限制,having用于查询后对查询结果进行过滤**

## 前言

在cmd中操作mysql数据库时，需要先选择使用的数据库

+ mysql安装

  ```java
  /*
  特别注意
    安装mysql中，如果出错或需重置数据库，可以使用 **sc delete mysql** 指令 清除已有数据库（慎重）
  
  安装注意
    安装路径不要有空格或中文
    
  安装地址
  https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-win64.zip 
  
  设置环境变量
  将mysql下的bin目录添加到系统环境变量
  
  在当前mysql安装目录下创建 my.ini 文件
  	my.ini 内容
  		[client]
          port=3306
          default-character-set=utf8
          [mysqld]
          # 设置自己的mySQL安装目录
          basedir=D:\aLearning\java\mySQL\mysql-5.7.19-winx64\
          #设置mySQL数据目录
          datadir=D:\aLearning\java\mySQL\mysql-5.7.19-winx64\data\
          port=3306
          character_set_server=utf8
          #跳过安全检测
          skip-grant-tables
   
  运行cmd以管理员方式
  	切换到mySQL文件下的bin目录，使用指令 mysqld -install 安装mysql数据库
  初始化数据库：mysqld --initialize-insecure --user=mysql
  启动mysql服务：输入 net start mysql
  输入：mysql -u root -p , 进入mysql，其中 -u 表示用户， -p 表示密码，可先按两次回车进入mysql稍后设置密码
  修改root用户密码
  	use mysql;
  	update user set authentication_string=password('lyx') where user='root' and Host='localhost';
   	flush privileges;(刷新权限)
   	quit(退出mySQL)
  最后打开my.ini注销跳过安全检测的代码
  	#skip-grant-tables // 此时重启mysql后，登入mysql需要输入密码
  	
  注意
  	net start mysql //开启mysql服务
  	net stop mysql // 停止mysql服务
  	在mysql服务器启动后，连接到Mysql数据库需要在cmd中输入 mysql -h 主机名 -P 端口号 -u 用户名 -p密码 。（-p密码不要加空格，-p后面如果没写密码，回车后也还是需要写，如果没有写-h 主机名，默认为本机地址，如果没写-P 端口，默认为3306,在企业中一般会修改此端口，免遭攻击）
  */
  ```



+ mySQL管理软件

  ```java
  /*
   NaviCat
   SQLyog
  */
  ```

  

+ 数据库三层结构

  ```java
  /*
   一个数据库系统可以有多个数据库，一个数据库可以有多张表，我们最终操作的就是表
   
   注意
   	数据库本质上是一个文件夹，表本质上是一个个文件，存放在data目录下
   	表中每一列数据被称为列或属性，每一行被称为行或元组，表中的一行被称为一条记录
  */
  ```

  ![image-20211114130516458](   D:\typora_import_images\typora-user-images\image-20211114130516458.png)



## 1.0 SQl语句

+ 分类

  DDL:数据定义语言[create]

  DML:数据操作语句[增加，修改，删除]

  DQL:数据查询语句[select]

  DCL:数据控制语句[管理数据库用户权限，grand(授权) revoke(撤销)]




+ SQL语句中，如果有多个属性或语句用逗号分隔，如果是最后一个属性或语句不能使用逗号(表示结束)

+ 数据库操作

  ```java
  /*
   DROP DATABASE 数据库名 // 删除数据库
   CREATE DATABASE 数据库名 // 创建数据库
   CREATE DATABASE 数据库 CHARACTER utf8 // 创建数据库并指定字符集
   CREATE DATABASE 数据库 CHARACTER utf8  COLLATE utf_bin// 创建数据库并指定校验字符集
   SHOW DATABASES // 显示全部数据库
   SHOW CREATE DATABASE 数据库 // 可以查看该数据库的创建语句
  
  数据库的备份与恢复
  	备份
           mysqldump -u 用户 -p -B 数据库1 数据库2 > d://back.sql // 该指令用于完成数据库备份，需要在命令行完成,其中  mysqldump -u 用户 -p -B 为前缀 数据库1 数据库2... 是需要备份的数据库，  >d://back.sql 是备份的位置
           mysqldump -u 用户 -p 数据库1 表名... // 该指令用于完成数据库中表的备份，只会备份表
  	恢复
  		source d://back.sql // 即可恢复数据库，需要在命令行且进入mysql命令行输入此句，其中 source  是前缀，d://back.sql 为备份数据库的路径
   	注意
   		在备份数据库备份中，我们使用 mysqldump指令，其中 -u 用户 -p [密码，可以稍后输入] 表示数据库密码校验，-B 数据库1.。 表示备份数据库 ， 不加 -B表示备份表 ，  > d://back.sql 表示备份路径
   	
   注意
   	字符集 utf8_bin 区分大小写 ， utf8_general_ci 不区分大小写
   	表也可以指定校验规则，如果没有指定，则以数据库的规则为准
   	在创建或删除一个数据库时，该数据库名如果是关键字或者保留字需要使用``包裹
   	
  */
  ```
  
  

+ 表操作

  ```java
  /*
    创建表
    	CREATE TABLE table_name
    	(
    		field daastype.
    		....
    	) character set utf8 collate utf8_bin engine INNODB
    如
    	CREATE TABLE `user`
    	(
    		`name` varchar(30),
           gender varchar(30),
           hobby varchar(255)
    	)character set utf8 collate utf8_bin engine INNODB
    	
    添加表中的列(属性)
    	ALTER TABLE table_name
    	ADD
    	(
    		column datatype [default expr]
    	)
    
    修改表中的列(属性)
    	ALTER TABLE table_name
    	CHANGE
    	(
    		old_column new_column datatype,
          
    	)
    	
    删除表中的列(属性)
     ALTER TABLE table_name
     DROP
     (
  	column 
     )
     
    练习
    	添加属性
    		ALTER TABLE stu
    		ADD hobby VARCHAR(50) NOT NULL DEFAULT '' AFTER salary // 不允许为空，没有填写默认为'',并把该字段插入到salary后
     
   注意
   	想要查看表的结构，使用指令,desc 表名
   	修改表名： Rename table 表名 to 新表名
   	修改表的字符集： alter table 表名 character 字符集
  */
  ```

+ Mysql常用数据类型

  ```java
  /*
   数值型
   	整形
   		tinyint(1个字节)
   		smallint(2个字节)
   		mediumint(3个字节)
   		int(4个字节，多用)
   		bigint(8个字节)
   		bit(1-8) // 其中1-8表示二进制位数，最多表示255
   	小数类型
   		float(单精度4个字节)
   		double(双精度8个字节，多用)
   		decimal(M,D 大小不确定，多用) // M表示该数的整数部分的最大位数，D表示该数的小数部分最大位数，M最大为65，D最大为30，如果D省略默认为0(0表示没有小数部分)，如果M省略默认为10
   	注意
   		在声明变量时，默认为有符号变量，表示的范围在(-n~n),如果是无符号变量，表示的范围在(0~n),无符号变量声明方式：  age int unsigned ，整形和小数类型都能声明为无符号
   文本，二进制类型
   	字符串
   		char(0-255，多用) //存放0-255字符，不管是中文还是英文
           varchar(0-65535，多用)  // 存放0-65535字节，其中实际最大存放为65532,3个字节用于记录字段大小，如果当前是utf8编码每个字符占3个字节，如果是gbk编码每个字符占2个字节
           text(0-65535，多用)
           mediumtext(0~2^24-1)
           longtext(0~2^32-1)
        注意
        	char(4) ,表示最多存放4个字符，varchar(4) 表示最多存放4个字符，且当前全部字节长度不能超过65535，当前单个字节长度由所选用的编码决定
        	char在声明大小后所占用的大小就是声明的大小，varchar在声明大小后，所占用的大小为当前所存储的值的大小，值的长度不能超过设置的长度
        	在一个值定长时，优先使用char(查询速度快)，当一个值不确定长度时优先使用varchar
       文本
       	biob(0~2^16-1)
       	longbiob(0~2^32-1)
   时间日期类型
   	日期
   		date[日期 年 月 日]
   	时间
   		time[时间 时 分 秒]
   	时间日期内省
   		datetime[年 月 日 时 分 秒 YYYY-MM-DD HH:mm:ss，多用]
   	时间戳
   		timestamp
   	年
   		year
   		
   注意
   	在满足需求的情况下，精良选择占用空间小的类型
  */
  ```



+ MySQl CRUD

  ```java
  /*
   insert 语句
   	使用 insert 语句向表中插入数据
   	语法
   		INSERT INTO table_name (column...)
   			VALUES(property..)
   	使用
   		INSERT INTO stu(id,name,sex)  
   			VALUES(_001,'张三','男');
   	注意
   		添加数据时，VALUES语句中的属性索引和INTO表的列排列索引一致，存入时一一对应存储
   		插入的数据应和创建表时，设置的类型一致
   		当插入一条varchar类型的数据时，不能超过设置好的varchar长度
   		varcahr数据如果是全数字，再存入时如果接收列类型是int，则该数据会转换为数字类型 
   		字符和日期类型数据应被单引号包裹
   		在列创建时允许插入空值的情况下，values插入的数据对应该列时，可以插入空值
   		values允许插入多条字段，每个字段用,分隔,()内的属性索引仍和INTO表的列排列索引对应，写为 VALUES(),(),()...
   		如果给表中的所有列添加属性，可以不写 列列表，默认按表中列的创建索引存储
   		
   update 语句
   	UPDATE	table_name
   		SET col
   		[WHERE col=xxx]
   	使用
   		UPDATE stu   // 把表中所有人的sex和nation改为以下信息
  			SET sex='女',nation='彝族'
  			
  		UPDATE stu  // 把表中name='张星彩'的sex和nation改为以下信息
              SET sex='女',nation='彝族'
              WHERE name = '张星彩'
   	注意
   		update语句要指明更改的时哪一个数据(where)，如果没有指明，默认更改全部数据
   		要修改多个字段，他们之间用逗号间隔
   		
   delete 语句
   	DELETE FROM table_name
   		[WHERE col=xxx]
   	使用
          DELETE FROM stu  // 删除stu表中名为张三的哪一行
              WHERE name = '张三'
   	
   		DELETE FROM stu  // 删除stu表中所有的数据
   	注意
   		如果不写WHERE删除的是表中所有的数据
   		删除只能删除一行的数据，不能单独删除一列的数据，只能给一列的数据至空值(使用UPDATE)
   		删除只能针对表里的数据，而不是删除表，如果要删除表需要使用drop
   		
   select 语句
   	SELECT FROM table_name
   		[WEHRE col=xxx]
   		[ORDER BY]  xxx [desc|asc] // 排序运算符，选项有 asc(升序),desc(降序)默认为 asc(升序)，登场放在select语句的结尾
   	注意
   		select语句支持运算，支持给一个查询名取别名(不会改变源数据库列)，该别名仅在执行时生效(状态栏中生效)，如果不添加别名，则在显示时默认为指定查找方式显示,如 select MAX(sal) FROM stu,则查找出来的显示为 MAX(sal) ... , 如果给该查找起一个别名 MAX(sal) AS max_salary FROM stu , 则查找出来的显示为 max_salary ...
   			如
   				SELECT `name`,(english+Chinese+math) AS total from stu // 从stu表中取出name，english,Chinese,math,并把english,Chinese,math相加变为total
   	
  运算符
  	比较运算符
  		+ - * /（通用）
  		> <  >= = <> !==
  		BETWEEN ... AND ...  // 在...之间，如  BETWEEN 23 ADN 30
  		IN(set) // 显示在in列表中的值，如 IN(100,200)， 表示选择100或者200的
  		col LINK '张' ，col NOT LIKE ''  // 模糊查询,如 `name` LIKE '张%' ,查询姓张的人，还有模糊查询项 %(匹配随机0-n个),_(匹配1个)，
  		IS NULL // 判断是否为空
  	逻辑运算符
  		and(与) ,or(或) ,not(取反)
  	注意
  		通常运算符都写在where行，多个运算符之间用逻辑运算符连接
  		在select语句查询一个表时，如果不带数据库前缀则默认查询的是当前数据库的表，如 select * from user(查询当前数据库下的user表), select * from db2.user (查询db2数据库下的user表)
  */
  ```



## 1.1 函数

+ 统计(合计)函数

  ```java
  /*
   count(*) //*可以为任意表中的列名， 用于统计表中满足条件的数据个数 ， 可以配合where使用，当()内为表中列名时会排除为null的记录(数据)
   
   sum(col) // 返回满足where条件的行的和 
   
   avg(col) // 返回满足where条件的行的平均数
   
   max(col) // 返回满足where条件中最大的的行 
   
   min(col) // 返回满足where条件中最小的的行 
   
   注意
   	sum.avg.max,min用在数值数据中
  */
  ```

  

+ select语句查找配置项

  ```java
  /*
   SELECT * 
   FROM table_name
   [WHERE 限制条件]  // 以什么方式查找
   [GROUP BY 以什么分组] // 以什么属性分组，被分组的数据会分组执行该查找，若需要分多个组则以，GROUP BY grade,class 方式书写即可，多个属性用逗号间隔最后一个属性不需要加逗号
   [ORDER BY asc||desc] // 查找出来的数据是升序还是降序， asc升序，desc降序
   
  */
  ```

  

+ 字符串相关函数

  ```java
  /*
   CHARSET(str) 返回字符串字符集，如utf8
   	charset(str)
   CONCAT(str...) 连接字符串
   	concat(str...)
   INSTR(string,substring) 返回substring在string中出现的位置，没有返回0
   	instr(string,substring)
   UCASE(string2) 转换为大写
   	ucase(string2)
   LCASE(string2) 转换为小写
   	lcase(String2)
   LEFT(string2,length) 从string2 中的左边起取length个字符
   	left(string2,length)
   LENGTH(string) 获得string长度(按照字节)
   	length(string)
   REPLACE(str,search_str,replace_str) 在str中用replace_str替换search_str
   	replace(str,search_str,replace_str)
   STRCMP(string1,string2) 比较两个字符串大小
   	strcmp(string1,string2)
   SUBSTRING(str,position[,length]) 从str的posiiton开始(从1开始计算),取length个字符
   	substring(str,position[,length]) 
   
  */
  ```

  

+ 数学相关函数

  ```java
  /*
   ABS(n) // 绝对值
   	abs(n)
   BIN(n) // 十进制转二进制
   	bin(n)
   CEILING(n) // 向上(大)取整
   	ceiling(n)
   CONV(n,10(n的进制),2(n转换为的进制)) // 把一个数n从一个进制转换为另一个进制
   	conv(n,10(n的进制),2(n转换为的进制)) 
   FLOOR(n) // 向下(小)取整
   	floor(n)
   FORMAT(n,x) // 对n这个浮点数保留x个小位数，下一位四舍五入
   	format(n,x)
   HEX(n) // 把一个数转换为16进制
   	hex(n)
   LEAST(n...) // 返回多个值中的最小数
   	least(n...)
   MOD(n,denomination) // 求余,如 MOD(10,3) ,即10%3为1
   	mod(n,denomination)
   RAND([seed]) // 返回一个0-1的随机数,当指定seed值后该随机数会固定，seed可以为任意值
   	rand([seed])
  */
  ```

  

+ 时间日期相关函数

  ```java
  /*
   CURRENT_DATE() // 得到当前日期
   	current_date()
   CURRENT_TIME() // 得到当前时间
   	current_time()
   CURRENT_TIMESTAMP() // 得到当前时间戳
   	current_timestamp()
   DATE(datetime) // 返回datetime的日期部分
   	date(datetime)
   DATE_ADD(date2,INTERVAL d_value d_type) // 在date2中加上时间或日期，d_type可以是year,minute, second ,day,hour,month
   	date_add(date2,INTERVAL d_value d_type) 
   DATE_SUB(date2,INTERVAL d_value d_type) //在date2中减去一个时间
   	date_sub(date2,INTERVAL d_value d_type)
   DATEDIFF(date1,date2) // 两个日期差，结果为天
   	datediff(date1,date2)
   TIMEDIFF(date1,date2)  //  两个时间差,结果为多少小时多少分钟
   	timediff(date1,date2)
   NOW() // 得到当前时间
   	now()
   YEAR(datetime)  // 返回该时间字符串的年部分
   	year(datetime)
   MONTH(datetime)  // 返回该时间字符串的月部分
   	month(datetime)
   DAY(datetime)  // 返回该时间字符串的日部分
   	day(datetime)
   UNIX_TIMESTAMP() // 返回 1970-1-1 到当前时间的毫秒数
   	unix_timestamp()
   FROM_UNIXTIME(unixTime,'%y-%m-%d %H:%i:%s') // 把一个unix时间戳转换为 年-月-日 时:分:秒，可以单独获取 年-月-日
   from_unitime(unixTime,'%y-%m-%d %H:%i:%s')
   使用
   	查询一个内容是否在10分钟以前发布
   		SELECT *
   			FROM mes
   			WHERE DATE_ADD(time, INTERVAL 10 MINUTE) >= NOW() // DATE_SUB()用法相同
   	计算 2021-11-11 到 1990-1-1 相差的天数
          SELECT DATEDIFF('2011-11-11','1990-1-1') as intervalDate
          	FROM DUAL // DUAL 为mySQL内置库
          	
    注意
    	在实际开发中，也经常使用int保存一个unix时间戳，然后使用from_unixtime()进行转换，相当有实用价值
  */
  ```
  
  



+ 加密和系统函数

  ```java
  /*
   USER() // 查询当前用户
   	user()
   DATEBASE() // 查询当前使用的数据库名称
   	datebase()
   MD5(str) // 用于加密一个字符串，加密后的长度为32的字符串
   	md5()
   PASSWORD(str) // 用于加密一个字符串，加密后的长度为41的字符串
   	password()
   注意
   	经过MD5加密后的字符串是32位的，所以可以使用char(32) 来存储，这样查询效率高
   	在mysql数据库中的用户密码使用的是 PASSWORD()加密方式
   	使用 mysql.user 可以查看当前数据库系统的所有用户
   	存放密码时一定要加密
  */
  ```



+ 流程控制函数

  ```java
  /*
   IF(exp1,exp2,exp3) // 如果表达式exp1为真则返回exp2的值，否则返回exp3的值 
   IFNULL(exp1,exp2) // 如果表达式exp1不为NULL则返回exp1,否则返回exp2
   CASE WHEN ... THEN .... ELSE ... END , 从when开始查找一个为true的表达式，如果查找到了，则执行其THEN，并退出，如果都没查找到则执行 ELSE 表达式
   如
   	SELECT CASE   
   		WHEN TRUE THEN '1'  // 返回 1 查找第一个为真的值
   		WHEN TRUE THEN '2'
   		ELSE '3' END
   
   使用
   	判断com是否为null，如果是null则为0.0
   		IF(com IS NULL, 0.0 , com)  // 在 IF 判断中要使用 is关键字判断
   	
  	如果job=1则输出hi，如果job=2则输出hello,否则输出goout
  		case
  			when  job = 1 then 'hi'
  			when  job = 2 then 'hello'
  			else 'goout' END
  			
   注意
   	判断一个数值是否为null都成用 is关键字 ， 如 a IS NULL
  */
  ```
  
  

## 1.2 高级查询

+ DISTINCT 去掉相同的字段

+ 分页查询

  ```java
  /*
   由于在以后数据会过于庞大，一次获取所有数据服务器压力会巨大，因此就需要分页查询
   基本语法
   	select ....
   	limit start , rows  // start为开始索引，rows为取出行号
   使用
   	SELECT * FROM DUAL  // 第一页
   	LIMIT 0,3
   	
   	SLECT * FROM DUAL  // 第二页
   	LIMIT 3,3  
   	
   	SELECT * FROM DUAL // 第三页
   	LIMIT 6,3
   注意
   	推导公式：每页显示记录 *（当前页-1），每页显示记录
  */
  ```

  

+ exercise

  ```java
  /*
   按照部门和工资排序并取出员工的所有信息
       SELECT * FROM clerk
       ORDER BY salary desc,department asc
  */
  ```

  

+ 增强分组查询

  ```java
  /*
   group by xxx, 按xxx分组，然后按组查找满足条件的字段
   
  */
  ```

  



+ 查询顺序

  ```java
  /*
   前言
   	having是对分组后的结果进行过滤
   	
   select column ... from  table
   	group by column
   	having condition
   	order by column
   	limit start,rows
  */
  ```



+ 多表查询

  ```java
  /*
   在进行多表查询时，sql会一次列举第一张表的每一行数据对第二章表的所有数据进行结合，如第一张表为2行数据，第二张表为4行数据，则结果为 2*4 = 8 , 该查询方式就为笛卡尔积 ， 在该查询的基础上我们可以做限制，找到需要的那一行或多行数据，而不是全部的数据
  
   注意
   	当两个表进行笛卡尔乘积过滤后，如果有相同的列，则需要使用表名.属性方式指明哪个是需要的列
   	在多表查询中最好在两个表中有一个相同的属性项，这样有助于过滤掉不需要的信息 
   	为了方便多表查询，相同的属性项不能少于表的个数-1
  	
  	
  */，
  ```

  



+ 自连接

  ```java
  /*
   当两个有关联的属性在一张表示，可以给表起一个别名，这样就可以使用该别名同时笛卡尔积两张相同的表
   给表起名字的格式为 select * from 原表名 表名别，原表名 表别名
   	如 select * from emp worker,emp boss  
   	   where  // 在相同的表自连接后，即可按条件对表本身进行查询
   
   注意
   	自连接特点就是一张表当两张表使用，此时需要给表取别名，取属性的使用也需要用该别名取
  */
  ```

  

+ 子查询

  ```java
  /*
   又称嵌套查询，即在限制条件中又进行了一次查询
   	
   使用
   	-- 查询和 刘备同一部门的全部信息
      SELECT *
      FROM clerk 
      WHERE department = (
          SELECT department
          FROM clerk
          WHERE `name` = '刘备'
      ) AND `name` <> '刘备'
      
   使用场景
   	把子查询当作一个临时表，可以解决很多复杂查询
   	// 以商品类别分类，查询每个销量最好的商品的全部信息
   	SELECT *
   		FROM(
   			SELECT _id, MAX(shop_price) as max_price
   			FROM goods
   			ORDER BY _id
   		) temp ,goods
   		WEHRE temp._id = goods._id AND temp.max_price = goods.shop_price
   		
   	-- 显示工资比执行部工资高的员工的姓名，工资和部门
      SELECT DISTINCT `name`,clerk.salary,department
      	FROM (
              SELECT salary FROM clerk
              WHERE department = '执行部'
              ) execute_salary ,clerk
      WHERE clerk.salary > execute_salary.salary  AND department <> '执行部'
    
  多列子查询
  	 -- 查询和李焕英相同部门的人的全部信息
          SELECT * FROM clerk
          WHERE (department ,age)= (
              SELECT department,age FROM clerk
              WHERE `name` = '王兰英' 
          ) AND `name` != '王兰英'
   
  注意
  	查询时还可以使用 all，表示所有的  。any 表示任意一个
   	表名.* 可以把临时表查询的所有属性列出来
  */
  ```

  

+ 表复制

  ```java
  /*
   当我们需要大量的数据时，可使用自我复制(蠕虫复制) 方式获取 ， 这样可以对数据库执行速度进行测试
   复制方法
   	INSERT INTO table_name
   		(id,sal,job)
   		SELECT id,sal,job FROM other_table // 取出的属性和填入属性类型匹配即可
   	
   	// 自我复制
   		INSERT INTO table_name  // 前后连个表名相同，自我复制，通常用来进行压力测试，指数增长
   			SELECT * FROM table_name
   
   当创建一张表时，如果项要改表和另一张表字段相同则可使用以下语句
   	CREATE TABLE table_name LIKE other_table  // 改语句即复制了另一张表的字段结构
   	
   对一个表进行去重方法
   	CREATE TABLE temp_table LIKE origin_table//(1)创建一个临时表复制改表的结构
   	INSERT INTO temp_table SELECT DISTINCT * FROM origin_table(2)给改临时表插入数据
   	DELETE FROM origin_table // 清除原表数据
   	INSERT INTO origin_table SELECT * FROM temp_table // 完成去重
   	DROP TABLE temp_table // 删除临时表
   	使用
   		-- 表去重
          CREATE TABLE temp_table LIKE clerk
  
          INSERT INTO temp_table
              SELECT DISTINCT * FROM clerk
  
          SELECT * FROM temp_table
          SELECT * FROM clerk
          DELETE FROM clerk
          INSERT INTO clerk
              SELECT * FROM temp_table
          DROP TABLE temp_table
  	
  */
  ```

  

+ 合并查询

  ```java
  /*
   将两个查询结果合并到一张表显示，使用关键字  union all(不去重) && union(自动取宠)
   
   如
   	SELECT department FROM clerk
      WHERE gender = '女'
      union 
      SELECT department FROM department
   注意
   	使用 union 和 union all 时，被合并的属性列名必须相同，否则无法合并
  */
  ```

  



+ 外连接

  ```java
  /*
   外连接常用的有 左外连接（左侧的表完全显示），和右外连接(右侧的表完全显示)
   定义左外连接方法
   	SELECT * FROM table_name LEFT JOIN other_name
   	ON table_name.id = other_name.id  
   	运行过程：table_name先和other_name进行笛卡尔乘积，找出 table_name.id = other_name.id  的行列，最后加上未匹配的左外连接的行列(没有的列会自动为null值)
   	
   定义右外连接方法
   	-- 右外连接
          SELECT *
          FROM clerk RIGHT JOIN department
          ON clerk.department = department.department
      运行过程：clerk先和department进行笛卡尔乘积，找出 clerk.department =department.department 的行列，最后加上未匹配的右外连接的行列(没有的列会自动为null值)
   	
  */
  ```

  





## 1.3 约束

+ 约束

  ```java
  /*
   约束用于确保数据库的数据满足特定的规则，约束包括：not null,unique,primary key,foreign key,check 5类
   
   primary key(主键)
     定义方法
     	方式一：字段名 字段类型 primary key // 被主键约束的列表示改列下的字段不能重复，只能是唯一的值
      方式二：
      	CREATE TABLE table_name (id int,`name` varchar(10),primary key(id)) // 在所有属性后 设置主键
     注意
     	被primary key修饰的列不能重复且不能为空
     	desc 表名 ，可以查看约束情况
     	每个表只能有一个主键或一个复合主键,在复合主键中，添加的复合主键的字段不能和之前添加的相同，主键在存储时会先内部查找是否有相同的主键，多个复合组件按组存入，对比时也是比较的组存储值，有则不让添加，无则可以添加
     	 	如
     	 		CREATE TABLE keyt_table
     	 		(
     	 			_id varchar(20) primary key,  // 单个主键，该主键列的字段不能相同
     	 			`name` varchar(10) 
     	 		)
     	 		
     	 		CREATE TABLE keyt_table
     	 		(
     	 			_id varchar(20),  // 单个主键，该主键列的字段不能相同
     	 			`name` varchar(10),
     	 			primary key (_id,`name`)  // 复合主键
     	 		)
     	 		
   not null(非空)
   	被 not null 修饰的属性不能为空
   
   unique(唯一)
   	当定义了 unique约束后，该列值不能重复(null 除外)
   	注意
   		如果unique没有指定 Not null 则可以有多个null值
   		一张表可以有多个unique
   		
   foreign key(外键)
   	外键一般在从表使用，当属性被  foreign key约束后，说明该列的属性和主表的key primary或unique修饰的属性有关联关系。
   	用法
   		CREATE TABLE primary_table  // 创建主表
   		(
   			id INT PRIMARY KEY, // 主键
   			COURSE VARCHAR(10)
   		)
   		
   		CREATE TABLE foreign_table
   		(
   			id INT PRIMARY KEY, // 主键
   			pid INT,
   			FOREIGN KEY(pid) REFERENCES primary_table(id) // pid定义为 primary_table.id的外键
   		)
   		
   	注意
   		外键修饰的变量只能和被key primary或unique修饰的列有关联关系，因为被其他键修饰会出现重复数据，这样外键就不知道该和哪个数据关联
   		被外键修饰的列，在关联了主表的列后，存储的数据只能为该列里已经存在的数据(或null数据(在不被not null修饰的情况下))，存储主表里不存在的数据则会报错
   		只有表的类型为innodb时，该表才支持外键
   		从表中被外键约束的数据的数据类型要和主表中对应的被主键或enique修饰的数据的数据类型一致
   		如果主外键的关系确定了，数据就不能随意删除，要先删除所有指向该主键的外键后，该主键才能删除
   		
  check约束
  	用于强制行数据必须满足的某种条件，如果不满足则会报错，(mysql5.7中只会做语法校验,sql server会生效)
  	使用
  		CREATE TABLE check_table
          (
              id INT PRIMARY KEY,
              sex CHAR(1) CHECK( sex BETWEEN '男' AND '女')
          )
  */
  ```

  



+ 自增长

  ```java
  /*
   被 AUTO_INCREMENT 修饰的属性在添加数据时，如果指定为NULL或没有指定，则该数据会自增长(每次+1,且每增加一条数据就会执行一次)
  使用
  	CREATE TABLE test
      (
          id INT PRIMARY KEY AUTO_INCREMENT,  // id在每有指定或指定为null值时开启自增长
          sex CHAR(1) NOT NULL
      )
  
      INSERT INTO test(sex)
          VALUES
  			 ('男'),('女')
  
  注意
  	AUTO_INCREMENT通常修饰的数据的数据类型为INT ,且常配合primary key使用
  	自增长也可以单独使用，但通常配合 unique使用
  	自增长默认从 1 开始，但可以指定开始的值，指定方式： table 表名 auto_increment=10(任意数值)
  	在添加数据时，如果给被自增长修饰的数据指定了值，则以该指定值为主（且以后以该给定值开始自增长），如没有给值，则自增长会给值。
  */
  ```

  



## 1.4 索引

+ 索引

  ```java
  /*
    当在一个存放的千万级数据的数据中查找数据时会非常非常的慢，这时可以使用索引，索引可以提高数据库性能和查询速度(比如原来插叙那时间4.5s,使用索引后查询时间为0.003s)
   注意
   	创建索引也会有磁盘开销,且只有查询了有索引的列，性能和速度才会提升
   	频繁查询的字段应创建索引
   	唯一性太差的字段不适合单独创建索引，及时是频繁作为查询条件
   	更新非常频繁的字段不适合创建索引
   
  */
  ```

  

+ 索引的原理

  ```java
  /*
   在没有索引时会全表扫描数据所以非常慢，在建立索引后，会把所有数据排列为一个二叉树结构，查找速度会非常快 ， 但该树结构在进行增删改查时会改变树结构影响修改效率，不过在我们使用中查找数据占(90%),增删改查只占(10% )
  */
  ```

  

+ 索引的操作

  ```java
  /*
  
   	
   索引的类型
   	主键索引，主键自动为主索引
   	唯一索引，被unique修饰的数据会会添加唯一索引
   	普通索引，使用index修饰的数据会添加普通索引
   	全文索引,fulltext，适用于mylSAM，开发中使用 Solr或ElasticSearch库
   	注意
   		普通索引最常用,主键索引和唯一索引被修饰时就添加了
   		
   查询索引
   	方式一：SHOW INDEXES FROM 表名  // Non_unique行，如果为1表示不是唯一索引(普通索引),0表示是唯一索引
   	方式二：SHOW INDEX FROM 表名
   	方式三：SHOW KEYS FROM 表名
   	方式四： DESC 表名
   添加唯一索引
   	CREATE UNIQUE INDEX id_index ON 表名(属性) // 添加唯一索引
   	
   添加创建索引
   	方式一：CREATE INDEX 索引名 ON 表名 (属性)  // 给表里的某个属性创建了索引
   	方式二：ALTER TABLE 表名 ADD INDEX 索引名 (属性) 
   	
   添加主键索引
   	在创建表示被primary key修饰的属性即是主键索引，如果没有添加也可以在后面添加，语句如下
   	ALTER TABLE 表名 ADD PRIMARY KEY (id)
   	
   删除索引
   	DROP INDEX 索引名 ON 表名
   
   删除主键索引
   	ALTER TABLE 表名 DROP PRIMARY KEY 
   
   修改索引
   	先删除索引，在添加索引
   
   注意
   	如果给某一列添加索引时，如果该列的值不会重复建议添加唯一索引，否者使用普通索引。唯一索引速度 >普通索引
   	添加普通索引时，索引名可以为任意值
   	唯一性太差或更新非常频繁的字段不建议设置索引，如：性别，登录次数
  */
  ```
  
  



## 1.5 事务

+ 事务

  ```java
  /*
   事物用于保证数据的一致性。它由一组相关的dml语句组成，该组的dml语句要么全部成功，要么全部失败
   
   应用
   	如同时修改两个id设置相关联数据，一个设置成功了，另一个设置失败了，这个时候就出现了错误，因此要接入事务，回退到某一点，重新添加数据，来保证数据的一致性
   注意
   	dml语句包括(update,insert,delete)
   	当在执行事物操作时，mysql会在表上加锁，防止其他用户修改此事务数据
  */
  ```

+ mysql控制事务

  ```java
  /*
   操作集
   	start transaction  // 开始一个事务
   	savepoint 保存点名  // 在此状态设置保存点
   	rollback to 保存点名  // 回退事务到保存点状态
   	rollback  // 回退全部事务，回退到事务开始的时间点
   	commit 提交事务 // 所有操作生效，不能再回退(数据全部提交，保存点清除)
   	
   事务工作流程
   	当你在进行数据修改操作时(update,delete,insert)，你可以添加一个事务(它可以在数据出现操作失误时，回退到你设置的某个点(该点之后的点会全部被清除)，这样就可以重新添加),这时mysql会在开始事务点添加一个原始保存点(使用rollback回退到的就是该点),同时会给添加一个隔离锁(强度由用户设置),当在做数据修改时，可以添加一个保存点，当出现操作失误时可以回退到该点，如果全部操作全部正确，即可提交事务(保存点全部移除，其他用户可以查看新增的修改)
   	
   	
   	
   注意
   	设置保存点后，在出现操作错误时，可以使用 rollback to 保存点，回退到该保存点
   	再回退到一个保存点时，它会删除之前添加的所有数据和保存点
   	当一个事务被提交后，表示该修改数据就正式生效了，(再提交前，其他人搜索不到你添加的任务)
   	如果不开始事务，默认情况下,dml操作后事务会自动添加自动提交，不能回滚
   	在开启一个事务后，会默认在开始点位放一个起始保存点，如果想要回退到该点，可以使用 rollback
   	保存点可以创建多个，在事务被提交后会全部移除
   	mysql事务只能运行在innodb存储引擎上，其他引擎不管用
   	开始事务可以写为 start transaction 或 set autocommit=off
   	回退点指定位可以使用无数次，回退到起始点位只能使用一次，回退到起始点位后需要重新设置
   	当事务提交时，代表着这个事务的结束
  */
  ```

+ 事务隔离级别 

  ```java
  /*
   当多个用户操作同一张表时，如果有一个用户设置事务，则其他用户看到表的内容，由该用户设置的隔离级别决定，如果不考虑隔离性可能会发生 脏读，不可重复读， 幻读现象
   	脏读
   		当一个事务读取到另一个尚未提交的事务的数据时，就产生了脏读
   	不可重复读
   		当一个事务读取到其他事务对表修改的数据时，此时就出现了不可重复读数据的现象，如事务a读取到123,事务b修改了3变为了4，此时事务a原本需要读取数据123却被事务B修改，读取到了124，这就是不可重复度
   	幻读
   		在同一个表中有多个事务连接时，若隔离级别较低，则某个用户提交数据后，其他事务的表中会出现该事务提交的数据，从而会影响其他用户对表的读取，这种现象叫做幻读，如事务a读取到12，事务b上传了3，导致本应该12的a数据读取到了3，从而产生幻读
   
   事务隔离级别定了事务与事务之间的隔离程度 
   
   注意
   	使用 SELECT @@tx_isolation 可以查询事务的隔离级别
   	使用 SET SESSION TRANSACTION ISOLATION LEVEL 可以设置隔离级别
   	一个正常的表读取，应为接入时读取的数据，不受其他表数据的添加而影响自己的数据读取
   	当隔离级别在Serializable时，当有事务连接到一个表，在该事务没有提交事务时，其他用户连接到此表不能读取数据(会一直等待该用户提交)，在该事务提交后才能读取
  */
  ```

  ![image-20211125151335818](   D:\typora_import_images\typora-user-images\image-20211125151335818.png)



+ mysql事务隔离级别

  ```java
  /*
   查看当前数据库隔离级别
   	select @@tx_isolation
   查看数据库系统当前隔离级别
   	select @@global.tx_isolation
   设置当前会话的隔离级别
   	set session transaction isolation level repeatable read
   设置数据库系统当前隔离级别
   	set global transaction isolation level repeatable read
   	
   全局修改事务级别方法
   	在my.ini文件进行修改
   	如
   		[mysqld]  #可选参数有： READ-UNCOMMITTED,READ-COMMITTED,REPREATABLE-READ,SERIALIZABLE
   		transaction-isolation = REPEATABLE-READ
   注意
   	mysql默认事务隔离级别为 repeatable read,一般情况下，没有特许要求没有必要修改
   	
  */
  ```

+ 事务的特性

  ```java
  /*
   原子性
   	事务是不可分割的工作单位，要么都发生要么都停止
   一致性
   	事务必须使数据库从一个一致性状态变换到另外一个一致性状态
   隔离性
   	事务的隔离性使多个用户并发访问数据库时，数据库为每个用户开启的事务，不能被其他的事务所干扰，每个事务之间相互隔离
   持久性
   	一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来及时数据库发生故障也不应该对其有任何影响
   	
  */
  ```

  

## 1.6 mySql管理

+ 存储引擎

  ```java
  /*
   MySQL的表类型由存储引起决定，主要包括MyISAM,innoDB,Memory等
   MySQL数据表主要支持六种类型，CSV,Memory,ARCHIVE,MRG_MYISAM<MYISAM,InnoDB,其中innoDB为一类"事务安全型"，其余被称为 "非事务安全型"
   
   常用的引擎为 MyiSAM,InnoDB,MEMORY
   	MyiSAM 不支持事务和外键，但其访问速度块，对事务和外键没有要求可以使用
   	innoDB存储引擎提供了事务和外键，但对比MyiSAM引擎，其效率较低且占用磁盘空间较大
   	MEMORY存储引擎使用内存进行创建表，每个MEMORY表对应一个磁盘文件，其访问数据非常快(支持索引，哈希)，但一旦服务关闭，其数据就会被清空(结构还在)
   	
   在创建一个表示就可以指定其表的存储引擎
   	CREATE TABLE t2(
   		id INT PRIMARY KEY,
   		age INT NOT NULL
   	) ENGINE MYISAM
   
   修改一个表的存储引擎
   	ALTER TABLE "表名" ENGINE = 存储引擎
   	ALTER TABLE t3 ENGINE = M
   
   引擎选择
   	如果数据需要用到事务和外键，且需要CRUD等则使用 innoDB引擎
   	如果数据不需要事务和外键，只做CRUD则使用 MyiSAM引擎
   	如果数据只需要一次性存储，则使用 MEMORY引擎
   
   注意
   	MyiSAM和InnoDB操作都会读取磁盘信息，而MEMORY不会，其直接使用内存
   	MEMORY表多用于存储临时数据，如qq用户状态(在线，忙碌等)
  */
  ```
  
  ![image-20220122202004352]( D:\typora_import_images\typora-user-images\image-20220122202004352.png)
  
  
  
  ![image-20220122202100480](   D:\typora_import_images\typora-user-images\image-20220122202100480.png)





+ 视图

  ```java
  /*
   视图是一个虚拟表，其内容由查询定义。同真实表一样，视图包含列，其数据来自对应的真实表，当视图数据修改时影响着真实表，真实表数据修改时也影响着视图表(视图和真实表之间是映射关系，可以看作视图中的列指向真实表中的列)
   
   视图基本使用
   	创建视图
   		create view 视图名 as select 语句 
   			使用
   				create view emp_view 
   				as
   				select emp_id,emp_name,emp_job from emp; // 从emp中映射emp_id,emp_name,emp_job列														组成一个新的视图
   	修改视图(原视图会被修改后的视图覆盖)
   		alter view 视图名 as select 语句
   	显示创建视图的指令
   		show create view 视图名
   			使用
   				show create viwe emp_view
   	删除视图
   		drop view 视图名1[,视图名2....]
   			使用
   				drop view emp_view
   
   视图特点
   	安全
   		一些数据表有着重要信息,通过添加视图,把有限的几个列映射到这个视图中,这样就可以避免用户查看敏感信息
   	性能
   		关系数据库常常会分表查询(用到join连接)，此时效率较低。我们可以额建立一个视图，通过视图来进行查询		 提高效率(视图可以把相关的表的字段组合在一起)
   	灵活
   		可以将一些旧表的常用列集成为一个新视图,通过查询这个视图来查询该旧表的重要信息，提高了效率和灵活性
   		
  使用
  	视图多表映射数据
  		create view emp_view
  		as
  		select empno,ename,grade
  		from emp,dept,salgrade
  		where emp.deptno = dept.deptno and
  		(sal between losal and hisal)
   		
   		
   注意
   	视图是真实表(基表)的一个映射，其指定了一些可供其他用户查询的信息，和真实表的数据是一个映射关系(即共享   一个数据)
   	视图和基表的映射关系相当于指向关系,操作视图里的数据(改)就是直接操作基表里的数据,然后把修改后的数据映射给指向该列的视图,获取视图里的数据(查)就是直接获取基表里对应列的数据
   	只有真实表有数据文件，视图没有数据文件(因为视图映射(指向)真实表的鼠标)
   	视图可以以视图为目标创建一个新视图，同时这个新视图映射的是该视图指向视图所指向的基表，如基表a(1,2,3),   视图b隐射了基表a的1,2变为 视图b(1,2) , 视图c映射了视图b上的1变为 视图c(1) , 此时所有视图实际指向的是     基表a里的数据，他们可以此映射关系来改变基表里的数据,同时通过映射把改变后的数据反馈到视图中
   	视图只能进行数据查询和修改，不能增加和删除
   	改变基本里的数据方式分为直接操作基表数据和操作视图里的数据(通过映射最终操作的还是基表数据)，基表里的数   据一旦改变，映射该数据的视图数据也随之改变
  */
  ```

  

+ Mysql管理

  ```java
  /*
   mysql中的用户，存储在系统数据库mysql中的user里
  	user表中的重要字段说明
  		host 运行登录的 位置(主机ip),localhost表示用于运行本机登录,也可以指定ip登录,如 192.168.1.100
  		user 表示用户名
  		authenticatioin_string 表示密码，该密码是被mysql的password()函数加密之后的密码
   
   创建用户(需要权限)
   	create user '用户名'@'允许登录的ip' identified by '密码' （创建用户，同时指定密码）
   		使用
   			create user 'educ'@'localhost' identified by '123456'
   			
   删除用户(需要权限)
   	drop user '用户名'@'允许登录的位置'
   		使用
   			drop user 'educ'@'localhost'
   
   修改自己的密码
   	set password = password("33333");
   	
   修改其他人的密码 (需要权限)
   	set password for 'user'@'localhost' = password("123456");
   	
   权限设置
   	基本语法
   		grant 权限列表 on 库.对象名 to '用户名' @ '登录位置' [identified by '密码']
   			如 grant select,create on sql.a to 'uu' @ 'localhost' identified by '12345'
   		注意
   			权限列表中，设置多个权限用逗号分隔
   			如果库.对象名使用此方式 *.*，表示所有库的所有对象 , 库名.* 表示该库的所有对象。(*表示任意			  的，所有的) 
   			[identified by '密码']如果写了，分两种情况，如果用户存在则是赋予用户以上权限同时修改密码。			如果用户不存在则是赋予用户以上权限同时创建用户 。如果没写表示给该用户设置以上权限
   
   撤销用户权限
   	基本语法
   		revoke 权限列表 on 库.对象名 to '用户名' @ '登录位置'
   			如 revoke
   权限生效指令(如果权限设置了没生效使用)
   	flush privileges
   	
   注意
   	在实际工作中，我们往往会对各个人员的权限进行限制
   	只有拥有给其他用户授予权限的权限才能给别人设置权限，(root拥有所有权限)
   	新用户在创建时，默认没有任何权限，需要管理员进行添加
   	@符之间不能有空格
   	如果不指定登录位置默认为为"%"即所有位置均可登录，同时，在指定登录位置的使用可以以192.169.%.%等方式表示,意为允许192.169开头的网段登录(192.%.%.%等同理)。在删除用户时,如果host不是%,需要明确指定'用户'@'host'
  	%为host位置比较少，在使用时要先确认
  */
  ```

  ![image-20220123122723685](   D:\typora_import_images\typora-user-images\image-20220123122723685.png)

