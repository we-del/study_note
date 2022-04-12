# Java

java没有引用指向的数据会被垃圾回收机制回收掉

## 编程技巧

+ **映射指，一个数据被两个或两个以上的对象关联，当这个数据的值该改变时，其引用这个数据的对象在使用这个数据时，就是改变后的值(每个对象都有权改变数据的值)。可以看作有n个对象同时指向一个数据源，当一个对象操作这个数据源时，其他对象在访问这个数据就是修改后的数据**

+ 哪里需要变量就在哪定义

+ **静态属性和非静态属性的区别，静态属性当类被加载出来后，会自动收集所有静态属性到一个空间，它的生命周期是此进程结束，而 非静态属性 只有当被实例化对象后才会被装载入该地址中，再失去指向时会被垃圾回收机制回收，因此经常用到的属性或方法可以做成静态的，全局保存并使用，很少用到的或容易产生更改的类就做成非静态的，这样节省空间**

+ 当一个地址没有人指向它时，该地址就会被jvm垃圾回收机制处理

+ 为了程序的健壮性和可维护性，最好把每个模块每个对象单独封装一个类或者函数，这样方便管理和扩展

+ IDEA可以使用快捷键 ctrl+alt+t 来快速try-catch

+ scanner.next()当碰到 空格 ' 换行会认为输入结束，所以如果包含这些需要使用 scanner.nextLine() 它只把回车当作结束

+ java中一个接口或类可能出现在不同包下，一定要注意引用的包是否正确

+ 编码技巧

  ```java
  /*
  1. 判断相关技巧
  	使用一个直观的常量去保存一个数据，在做判断时，使用该常量即可达到相同效果
  	优点：可以更直观的看到登录的状态，而不需要用晦涩的数字标识
   	interface messageType {
      String MESSAGE_TYPE_SUCCESS = "0";
      String MESSAGE_TYPE_FILED = "1";
      }
  
      public class Array implements messageType {
          public static void main(String[] args) {
              String type = null;
              Scanner scanner = new Scanner(System.in);
              System.out.println("输入状态");
              type = scanner.next();
              if(type.equals(MESSAGE_TYPE_SUCCESS)){
                  System.out.println("登录成功");
              } else if(type.equals(MESSAGE_TYPE_FILED)){
                  System.out.println("登录失败");
              }else{
                  System.out.println("输入不正确");
              }
          }
      }
      
  2. 在做简单判断时,可以使用switch来完成，简单明确
  3. 方法写在和该方法相关的类中 如，Number_Computed(){} 所有计算方法都写在该类中，外部只需调用其提供的方法即可。又如 有 A,B两类，A想要获得B的一个属性，则B就可以书写一个方法来返回该属性，A类只用生成B的实例对象调用其方法即可使用(类在哪里和该类相关的方法就定义在哪里，外部只用调用该方法即可)
  4. 类是存储一个个属性和方法的集合，可以使用类来传递数据(设置类中的需要用到的属性的值即可)
  5. 当程序需要终止但还有很多线程仍在运行时，可以使用 System.exit(0) 正常退出该进程(线程依附于进程，进程一旦退出线程也就退出了)
  6. 在工作中，如若有异常需要抛出，则可以以(throw new RuntimeEception(e)) 方式抛出(工具类通常使用此方法)，程序员可以捕获该异常获得报错信息，也可以不捕获，这时jvm会默认处理该异常(运行异常如果程序员不处理则jvm会帮忙处理)
  7. java通过分层(创建不同的包)管理每个包下实现其各特有自的功能，以达到每个包各司其职目的
  8. 每次引用工具类时，可以先测试其是否可用
  9. 当在写逻辑程序时，可以先用一下数据填充，等到这一片逻辑写完再来完善替换掉填充程序，如再写用户登录时需要判断用户是否合法，此时就可以先使用数据填充，待完成这一块逻辑后再去写数据库逻辑最后跟填充替换(写其他模块也可参考此思路，遇到验证或校验问题，先写一各数据填充，待完成此块逻辑后，在去写验证逻辑最后进行数据替换即可)
  10. 数据库中的密码在存储时一定要以加密方式存储(md5(),password()等)，在进行密码验证时，只需要在sql语句中的密码位 套用对应的加密函数即可 ，如  mysql密码存储以 md5(123) ,则在sql语句书写也要以该方式书写 如 select * from username=123345 and password = md5(123) ,(因为一个相同的值以一个相同的方式加密后的值仍然相等，所以可以用此方式找出对应的数据，因此在设置密码时应该使用强密码) （MD5加密为32位数据）
   
  11. 使用 过关法验证账号和密码是否存在
    // 判断用户是否存在
      public boolean checkUser(String userId,String password){
          User user = validHashMap.get(userId);
          if (user == null) {
               // 表示输出的userId在集合中不存在
              return false;
          }
          if(!(user.getPassword().equals(password))){
              // 表示输入的用户在集合中存在但输入的密码错误
              return false;
          }
          // 输入正确
          return true;
      } // 如果一个函数&方法，返回的就是boolean值数据，则直接在if等判断语句中调用该函数&方法即可
      
  12. 在使用java操作数据库时，写sql语句最好去navicat或sqlyoung测试一下，因为java端不好看到报错信息
  13. java大致分层(分包)为 util(存放工具类),domain(存放数据库对象类,一个类对应一个表里的一行数据)，DAO(存放数据库查询逻辑增删改查等),service(业务层，在此包类封装对应sql查询等业务逻辑，如得到一个用户的密码)，view(再次包进行数据显示，获取数据库数据可以调用service包下的内容)
  14. 一个经常复用的属性或方法最好做成静态的，反之做成非静态的，且只希望有一份的也可以做成静态的
  15. (int)((99* 1.9999)*100+0.5)/100 可以对一个数进行四舍五入
  16. 在写方法时，先过滤掉异常情况再来写逻辑代码
  17. 再写一个方法时，需要将其功能尽可能的最大化划分，这样可以提高方法的复用性，如需要 写一个获得随机姓名，随机性别，随机时间和随机电话的方法时，可以把随机性别划分为一个函数，随机性别划分为一个函数，随机时间划分为一个函数，随机电话划分为一个函数，验证关系划分为一个函数，然后再将他们放在该方法中统合调用。
  18. 需要得到一个数字的长度时，可以让该数字+一个字符串再调用.length()方法即可,如(12+"").length() 为 2
  19. 对某个目标操作的方法要定义在该目标的类中，外部只用调用方法即可，如 想要遍历一个链表，则该遍历就可以封装成一个方法定义在该链表类中，外部只用调用 该方法即可(好处，便于代码维护性和扩展性)
  20. 多个类相同的操作可以由父类来封装，不同的操作可以做成抽象方法由子类实现(此时父类做成抽象类)，然后子类继承该抽象类即可
  21. 再获得一个属性或值时，最好不改变原数值(如果要求一定要更改也可以更改)，如传入一个数组，按要求对其排序后返回一个新数值，但原数组不会改变(此时方便对比数组前后变化等)
  22. 在写一个方法时，可以把该方法先看成一个单独的个体，此时对其功能进行完善，完善后在对其他方法产生连接
  23. && 返回为假的那一个，如果都为真返回最后一个，|| 返回为真的那一个都为真返回最后一个 ，&&在判断中为假则短路(为假就没执行的必要了，直接为false)，||在判断中为真则短路(为真就没执行的必要了直接为true)
  24. 在对一个可能会越界的操作取值时，一定要做越界判断
  25. 如果子类和父类有相同的方法，同时又想去调用父类的那个方法，需要使用super.xx()方式进行调用，否则会由于d
  */
  ```

+ 关于当前目录有该类，但使用dos执行该类确无法执行

  ```java
  /*
  总的来说就是包名路径和当前Dos执行路径不匹配 详情见 https://bbs.csdn.net/topics/391862656?page=1
   你的HelloWorld类中把它放到了helloworld包下了，你的classpath设定了当前目录，你当前的目录是e:\dahong\java\helloworld，执行器会在当前的目录下找helloworld这个包，你实际上没有嘛，要么把package语句去掉，要么在当前目录下再创建个helloworld文件夹，把源文件放进去，编译的时候javac helloworld/HelloWorld.java，java helloworld.HelloWorld
  */
  ```

+ idea技巧

  ```java
  /*
   注释
   	单行注释 //
   	多行注释 /**/
  	文档注释 /**
  				*/
   .souf // 标准打印加自定义添加string
   .sout // 标准打印目标
   .soutv // 标准打印目标+前面跟目标字符串
  */
  ```
  
  

# java学完后，自己做前后端

# 学会看源码！

+ Java编码知识

  **main方法是java入口方法，一般main方法都在public class 中（只有public修饰的类外部才可以被编译器执行，并调用main方法），因此public class是执行文件类**

  **在同一个类中，允许一个类方法调用另一个static类方法(只有static修饰的方法，同类中的其他方法才可以直接调用)**

  **java每一个类就是一个单独的文件，在同一个包内，每个类的类名只能是独一无二的，且所有类都可以互相访问**

  **同一个包下可以有任意个工程类（可执行类，拥有自己的入口方法）**
  
  变量的数据类型要和你指向的数据类型一致，如指向一个数字可以用 int,指向一个实例对象，那数据类型就是那个对象

+ 对参数校验的技巧

  列出所有正确的情况取反即可表示所有不正确的情况(正确的情况很容易想到，不正确的情况很难想到)，如 if(!(arr.length!=null&&arr.length>0))，只有不满足这些条件的情况才会走if语句 。这就像一个饼状图，先编写出一个符合范围的区别，然后取反就是不符合范围的区间。通常符合范围的区间很好想到，而不符合范围的区间多且杂，很难想全

  ```java
  /*
   列出所有正确的情况取反即可，！可翻译为不满足...的情况就走这，正常情况可翻译为 满足...的情况就走这
  */
  ```

  

+ 关于数据类型指向

  一个实现接口类型或父类类型可以指向一个实现该接口或继承该类的实例对象，在打印时是调用的toString方法，如果当前类和父类都没有重写Object的toString方法，则由于继承关于会自动找到Object上并指向它的toString方法

  

  

+ 多态流程

  父类数据类型或接口类型可以指向子类的实例对象或实现接口的实例对象，在使用属性时会在父类继承链或接口上开始查找，在使用方法时，(分编译状态(父类&继承链上或接口有此方法)和运行状态(总是从当前实例对象上开始查找))，**从当前子类实例对象开始查找且父类或接口上要存在此方法**，此时分四种情况，1.子类有此方法父类有此方法，则运行子类上的方法，2.子类没有此方法父类有此方法，则运行父类上的方法，3. 子类上有此方法，父类上没此方法则需要向下转型，4. 子类和父类上都没有此方法则编译不通过  (如果使用的是静态方法则会从当前父类身上查找)

  

+ JUnit调试工具

  ```java
  /*
   可以随时调试某一行的代码(方法等)，非常的方便快捷
   使用方法 在需要测试的代码前 添加@Test，光标定在改行片刻后会出现一个红色灯泡，点击它，选择Add JUnit5.x后，点击该行的运行按钮即可
   注意
   	选择Add JUnit5.x版本即可，等待片刻即可点击该行的运行按钮运行代码
  */
  ```

  



+ 查看java源码

  在Idea中直接ctrl + 鼠标左键 点击想要查看源码的方法即可

  

​	常用编码表有: ASCII，unicode,UTF-8,kbg

+ **instanceOf用来一个实例对象是否是一个原型类的实例(包括继承)，在多态中以运行类型做判断**
+ **在Java中原型对象被称为类，类的实例被称为对象**

+ subline快捷键

  Ctrl+shift+D 向下复制一行

  Tab 向右缩进

  Shift + Tab 向左缩进

  Ctrl +shift+K 删除当前行

在1995年发行java的第一个版本，java先有sun公司开发，后再2009年被甲骨文公司收购

+ java语言特点

  java是面向对象语言，且具有健壮性，跨平台性(可在多个系统中运行，需要用到JVM,即java虚拟机)

  解释型语言：python,javascript等，需要一个解释器翻译代码

  编译型语言：C,C++,java等，会生成可执行文件，该文件直接由二进制码组成

+ java运行机制

  java核心机制-java虚拟机(JVM) ,它具有指令集并使用不同的存储区域负责执行指令，管理数据，内存等，这些操作都包含在JDK中，不同的系统下有不同的java虚拟机

  java虚拟机机制屏蔽了底层运行平台的差别，实现了**一处编译，到处运行**

+ JDK

  全称为(Java Develop Kit) 

  JDK = JRE + java开发工具 ，JDK是提供给Java开发人员使用的，其中包含了JRE

  JRE,指java运行环境，包括 **java虚拟机，java核心库**



+ 字符串对象方法equals

  ```js
  /*
  可以用来判断两个字符串的内容是否完全相等，如果相等则为true，不相等为false
   如 String name = "xiaoming";
   	String name2 = "xiaoming";
   	System.out.println(name.equals(name2)) // true
  */
  ```

  

+ java在cmd中的编译

  ```js
  /*
   java编写的encode码需和cmd中的encode码保持一致，否则会报错
   cmd中编译： javac xxx.java , 即将.java文件编译生成.class文件
   cmd中运行： java xxx , 即查询对应的class文件进行运行(把对应的class文件加载到java虚拟机运行) ， 不需要加.class后缀，加了会报错
   
   注意
   	未编译的java文件被称为 源文件
   	.class后缀的文件被称为 字节码文件
   	如果程序没有出错误和提示(没有提示就是最好的提示)，且Encode码符合，则会在当前文件夹下生成一个对应名称的class文件，通过java xxx执行
  */
  ```

+ Java开发注意事项

  ```js
  /*
   1. java源文件以.java扩展名，源文件的基本组成是类(class)
   2. Java应用程序的执行入口时main()方法，有固定的书写格式 如 public static void main(String[] args)
   3. Java严格区分大小写
   4. Java方法由一条语句构成，每个语句要以 ; 结尾
   5. 大括号都是成对出现，缺一不可
   6. 一个人源文件中最多有一个public类，其他类的个数不限
   7. 在一个正在编译的.java文件中，只有要一个对应的类，就会生成一个对应的class文件
   8. 如果源文件包含一个public类，则该文件名需按该类命名
   9. 一个源文件最多只能有一个public类，其他类的个数不限，每个类都有一个 public static void main()方法，当该类被运行时，作为入口函数执行
  */
  ```



+ 知识学习方法

  为什么要学，学他能干什么，和之前学的有什么不同或有什么交集

  ![image-20211013094303145](D:\typora_import_images\typora-user-images\image-20211013094303145.png)







+ 易犯错误

  ```js
  /*
   编译相关
   	可能出现路径错误，或文件输入不匹配
   	
   代码相关
   	缺少分号，或代码书写错误
   	尤其注意输出控制台语句为 System.out.println （ln而不是in）
  */
  ```

  

## 1.0 基本规范

+ 输出语法

  ```js
  /*
   public class Hello {  // public class表示为一个公共类
  	public static void main(String[] args){ // 入口函数
  		System.out.println("hello world"); //控制台输出
  	}
  }
  
  注意
  	公共类的类名需和文件夹名保持一致
  	System.out.println("hello world"); 其中println表示换行打印
  	System.out.print(""); 表示普通打印，即只打印内容，不坐附加操作(如 换行等)
  */
  ```


+ 输入语法

  ```js
  /*
   键盘输入语句在input.java包内，因此需要引入该包，
   	使用 
   		import java.util.Scanner 引入包
   		创建 Scanner 对象, Scanner myScanner =  new Scanner(System.in);
   		使用 myScanner.next();  即可接收用户输入，可以使用myScanner.nextInt(),等限制接收类型，直接写next()表示接收任意类型，next后可以根变量类型表示只接收改类型数据，如nextDouble，nextFloat等
   		
  */
  ```

  

+ 常用转义字符

  ```js
  /*
   \t 一个制表位，实现对齐功能
   \n 换行符
   \\ 一个 \ , \ 起转义作用
   \' 一个 ' 
   \" 一个 "
   \r  一个回车 ，置顶显示字符,且开头字符会被覆盖 如 hello world\r beijing 则实际输出 beijingrld
  */
  ```

+ 注释

  ```js
  
   单行注释 //
   多行注释 /**/
   注意
   	注释不会被当作代码执行
      多行注释不能嵌套多行注释
  /*
   文档注释
   	注释内容可以被JDK提供的工具javadoc解析，生成一套以网页文件形式体现的程序说明文档
   	注释以@开头
   		如 /*@author xxx @version xxx   */ 。然后在cmd中输入 javadoc -d (输出路径) -(文档注释) (文件)  如 javadoc -d D:\\temp -version -author comment.java , 即可在该指定文件生成对应的html文档手册，点击index.html进入即可
  	/*
  	常用文档注释有
  		@author(指定作者) @version(指定版本) @param(指定参数) @see(指定链接) ，更多内容自行百度
  	*/
   		
  */
  ```

  

+ Java代码规范

  ```js
  /*
   1. 类，方法的注释，要以javadoc方式来书写
   2. 非Java Doc注释，往往给代码维护者看，告诉对方代码功能，如何修改，注意什么
   3. 使用tab操作，实现缩进，shift tab向左移动
   4. 运算符两边通常加空格
   5. 源文件使用 utf-8编码
   6. 行宽度不要超过80个字符
   7. 代码编写次行风格和行尾风格 如
   	if () { // 行尾风格                            if() // 次行风格
   												{
   													
   												}
   	}
  */
  ```

  

+ dos相关命令

  ```java
  /*
   md (路径) 在指定路径创建一个dos文件夹
   rd (路径) 删除在指定路径的文件夹
   dir 查看当前文件夹的全部内容 ， dir (路径) 可以查看指定路径的内容
   cd .. 退到上级目录
   cd xx 进入到该文件夹或文件
   当其他盘符切到C盘时 需使用 cd /D c:  /D作为转换开关修饰符
   cls 清屏指令
   tree 显示当前windows系统下的所有路径
   echo xx >1.txt  在当前路径下创建1.txt 并写入xx内容
   type nul >2.txt 在当前路径下常见一个2.txt的空文件
   copy ok.txt d:\ok2.txt 把当前路径下的ok.txt拷贝到d盘下的ok2.txt
   move ok.txt d:\move\ok.txt 把当前了路径下的ok.txt 移动到d盘下的move文件夹
  */
  ```



+ 路径

  相对路径:相当于当前文件夹的路径

  绝对路径：以windows为参照点，在windows下的文件位置的路径 如 C:\\xx\xx



## 1.1 变量

一程序一世界，变量是程序的基本组成单位

变量是内存里的一个存储空间，每次赋值实际上就是在改变这个内存地址的指向

+ 变量类型

  ```js
  /*
   int 表示整型，如 1 2 -1 -2
   double 表示小数(浮点数) 如 1.1 2.12
   char 表示 字符   如 '男' '女'
   String 表示字符串 如 'marry'
   
   注意
   	变量表示内存中的一个存储空间，类型不同，占用空间大小不同，如 int 占用4个字节，double占用8个字节
   	变量必须先声明后使用(程序从上往下读取运行)
   	该区域存储的数据/值必须满足它所声明的变量类型
  	变量三要素，变量名，值，数据类型
  	变量在同一个作用域内不能重名
  */
  ```

  

+ +号的使用

  ```js
  /*
   当左右两边都是数据类型时，则做法加运算
   当左右两边有乙方为字符串则做拼接运行
   System.out.println(100+1) ; // 101
   System.out.println(199+"1"); // 1991
  
  */
  ```

  

+ Java数据类型

  ```js
  /*
   基本数据类型 
   	数值型(整数型(byte[1],short[2],int[4],long[8])，浮点型(float,double)) ， 字符型(char[2])，布尔型(boolean[1],true,false)
   复杂(引用)数据类型
   	类(class)，接口(interface)，数组
   
   基本数据类型转换
   	当java程序在进行赋值或运算时，精度小的类型自动转化为精度大的类型(自动转换)
   	char -> int->long ->float-> double 
   	byte -> short -> int -> long -> double , 此类型是以当前运算中类型最大的转，如一个char类型数据和一个int类型数据运算，则结果会自动转换为int类型
   
   强制类型转换
   	使用 (变量类型)常量 方式进行转换，如 int i = (int)1.3; 即把1.3转换为1然后赋值给i,其他类型转换同理
   	
   基本数据类型和String类型的转换
   	基本类型==>String类型,加""即可,如 int n=10; String s =n+""；
   	String==>基本类型
   		转为 int类型: int num = Integer.parseInt('12');
   		转为 double类型: double dounum = Double.parseDouble("12.123');
   		转为 float类型: float fnum = Float.parseFloat("12.23");
   		转为 long类型：long lnum = Long.parseLong("1234");
   		转为 byte类型: byte bnum = Byte.parseByte("12");
   		转为 boolean类型: boolean b = Boolean.parseBoolean("true");
   		转为 short类型: Short.parseShort("232");
   注意
   	无论这些数据类型在那个系统下都是占这么多空间
   	在一个数字后加L或l表示long类型数据 如 long n = 12L
   	在设置变量类型时，在没有越界的情况下尽量使用小的变量类型(避免浪费)，不过如果变量类型可能过大则可以考虑long类型(如一个国家的人数，或沙漠中的沙子)
   	浮点数 = 符号位 + 指数位 + 尾数位 ，浮点数尾部可能丢失数据
   	java浮点数常量默认为 double类型 ，声明float类型需在后加 f或F ,如 float a = 1.1F
   	浮点数进行除法运算，在有小数的时候可能会产生精度损失 如 8.1 / 3 结果为 2.66666666669
   	char存储单个字符，单个汉字，单个转义字符，和数字，如 'a','\t','三',98 ，char本质上是一个整数(可以参与运算)，在输出时会转换为对应的unicode码值
   	char类型在计算机底层的转换顺序为(假定存入a): 'a' ==> 97 ==>97的二进制数 ==>存储
  	单个字符使用 ''包裹 ，一个字符串使用""包裹
  	java中不能使用 0 或非0的整数替代false和true
  	有多种数据类型的数据混合运算时，系统会首先自动将所有数据转换为精度最大的数据
  	当把精度大的数据给精度小的数据时会报错，繁殖则进行自动类型转换
  	byte,short和char之间不能自动转换
  	一个范围在-128 - 127的数字直接赋值给byte数据可以成功，但如果这个数先辅助给int数据，然后在把这个数据传递给byte则会报错
  	byte,short,char 类型数据在参与算术运算时，会转换为int进行运算，因此byte，short,char数据传参只能通过赋值方式，不能通过计算方式
  	boolean类型不参与类型转换
  	java数据类型严格区分数据类型，只能精度小的往精度大的转，不能精度大的往精度小的转
  	一个整形数据默认为int类型，如果后面加L或l则代表long类型。一个浮点数据默认为double类型，如果后面加f后F则代表float类型。
  	当一个超出其存储单元的数被强制转换后，会发生溢出 如 int a=2222;byte b =(byte)a ，这时就会有溢出情况
  	强制转换只针对其紧挨着的一个数，为了提高优先级，通常使用()括起来
  	String 类型本质上是一个对象类
  	字符串对象转变为基本数据类型时，必须匹配对应的类型接收条件，如果不匹配，则会报错
  */
  ```
  
  

## 1.2 运算符

![image-20211014151549662](D:\typora_import_images\typora-user-images\image-20211014151549662.png)



+ 关系运算符(比较运算符)

  ![image-20211014152935899](D:\typora_import_images\typora-user-images\image-20211014152935899.png)

  

![image-20211014222501422](D:\typora_import_images\typora-user-images\image-20211014222501422.png)

![image-20211014222930657](D:\typora_import_images\typora-user-images\image-20211014222930657.png)



![image-20211014223103496](D:\typora_import_images\typora-user-images\image-20211014223103496.png)

![image-20211014223803247](D:\typora_import_images\typora-user-images\image-20211014223803247.png)

![image-20211014224214926](D:\typora_import_images\typora-user-images\image-20211014224214926.png)

![image-20211014224848358](D:\typora_import_images\typora-user-images\image-20211014224848358.png)



+ 标识符命名规范

  ```java
  /*
  规则
   	由字母(大小写字母)，数字，下划线 $组成，数字不能开头，不能使用关键字，保留字
  规范
  	包名:多单词组成时所有字母都小写使用.去间隔单词,如 aaa.bbb.ccc
  	类名和接口：多个单词组成时，采取大驼峰，XxxYyyZzz
  	变量名，方法名：多单词组成时，采取小驼峰，xxxYyyZzz
  	常量名，所有字母都大写，多个单词使用_间隔 如 XXX_ZZZ
  */
  ```

  

  

![image-20211014230643476](D:\typora_import_images\typora-user-images\image-20211014230643476.png)

![image-20211014230719632](D:\typora_import_images\typora-user-images\image-20211014230719632.png)



+ 进制

  二进制,以0b或0B开头

  八进制，以0开头

  十六进制，使用0x或0X开头



+ 原码，反码，补码(和c语言一致)

  ![image-20211015092942683](D:\typora_import_images\typora-user-images\image-20211015092942683.png)

![image-20211015094003439](D:\typora_import_images\typora-user-images\image-20211015094003439.png)

+ 位运算符

  算数右移 >> , 低位溢出，符号位不变，并用符号位补移出的高位

  算数左移 << , 符号位不变，低位补0

  无符号右移(逻辑右移) >>>(没有逻辑左移)，运算规则：低位溢出，高位补0





## 1.3 控制语句

和C语言一致

java也遵循 整数/整数 == 整数 原则，所有在 3/2时结果为 1 ，这时需要把3改为3.0或2改为2.0即可得到正确的值

+ 顺序，分支(选择)，顺序

  顺序，程序从上往下执行，变量先定义后使用

  分支，if..else , if ..else if ... else , ， switch(){case:...;}

  循环，for,while,do while

  break,用在switch或者循环中，用来终止循环或switch执行 。break可以通过标签指明要终止的语句块，如果没有写标签，默认退出最近的循环体 。 标签名称以 aca:(可以是任意数值) 方式出现在循环前，使用break aca即可推出对应的循环体

  continue,终止本次循环开启下次循环

  return ,结束本次函数执行，同时返回结果给调用函数者



+ 增强for 循环

  ```java
  /*
   使用  for(int i : nums){} 定义，在输出时会把 nums里的内容一个一个交给i,这时我们在循环体里打印i即可完成遍历操作
  */
  ```

  



## 1.4 数组

+ 数组基础

  ```js
  /*
   创建数组
   	(数据类型)[] 变量 = {1,23,32.3,...xxxx}，如 double[] a ={1.0,2.3}; 其中double[]表示double类型的数组，数组名为a，{1.0,2.3}是数组体
   	
   获取数组值
   	使用 a[1],a[2]下标方式访问对应数组下标的内容
   	
   数组定义方式
   	（1）使用 int[] a = new int[5]; 方式来限制数组长度，此表达式意思为 创建了一个可以存储5个变量的int类型数组，下标是0-4
   	（2） 使用 int[] a ; a = new int[5]; 先声明变量在分配空间
   	（3）使用 int[] a ={1,2,3,4}; 此方法也叫静态初始化法，已经给定了存储空间，且都被占用完毕
   注意
   	数组下标从0开始排序
   	创建数组时[]可以写在变量类型后面，也可以写在变量后面
   	当一个数组变量声明时，必须要给他传入参数，否则这个数组不可用
   	数组可以管理基本类型和复杂类型(String),数组存储的值要和其数据类型可存储值保持一致
   	数组里的值也存在隐式转换，如 double[] a = {1,2,3};此时1,2,3默认会转化为1.0,2.0,3.0
   	数组创建后，如果没有赋值，short,byte,long,int,float,double类型都会给0 ，char类型会给\u000,boolean类型会给false,String类型会给null
   	数组本质上也是对象
  */
  ```

  

+ 参数传递机制

  ```js
  /*
   基本数据类型 值传递(值拷贝，创建了一个新地址) 。 复杂数据类型地址传递(地址拷贝，指向的是同一个地址)，
   jvm内存由堆，栈，方法区组成，变量和普通数据类型都存放在栈区，复杂数据类型存放在堆区
   
   数组拷贝
   	int[] arr = {1,2,3};
   	int[] arr2 = new int[arr.length]; 
   	for(int i = 0;i<arr.length;i++) arr2[i] = arr[i];
   	实现复杂类型的数据拷贝本质上就是重新开辟一个新的地址，然后做赋值操作即可
  
   数组扩容
   	int[] arr ={1,2,3};
   	int[] arr2=new int[arr.length+1]; 
   	for(int i=0;i<=arr.length;i++)arr2[i]=i;
   	arr=arr2; // 让原数组指向一个新数组
   	数组扩容后本质上就是创建一个新的数组，这个数组长度为指定扩容长度，当完成扩容后，让原数组变量指向这个扩容数组地址即可
   注意
   	没有被变量引用的基本数据或复杂数据类型会被垃圾回收机制处理
  */
  ```

  ![image-20211015151450292](D:\typora_import_images\typora-user-images\image-20211015151450292.png)





+ 排序

  ```java
  /*
   冒泡排序
   	挨个比较冒泡，找到最大数
   	public class Scan{
  	public static void main(String[] args){
  		int[] a = {3,5,1,2,6};
  		int temp;
  		for(int i = 0 ; i < a.length-1; i++){
  			for(int j = 0; j < a.length-1-i; j++){
  				if(  a[j] > a[j + 1] ){
  					temp = a[j];
  					a[j] = a[j+1];
  					a[j+1] = temp;
  				}
  			}
  		}
  		for(int i = 0; i < a.length ; i++){
  			System.out.println(a[i]);
  		}
  	}
  	}
  */
  ```

  

  



+ 二维数组

  ```js
  /*
   定义方式
   	使用 int[][] a ={{}} 定义 ，第一个[]表示行数组，第二个元素表示该行有多少个数组
   	
   遍历二维数组
   	int[][] a = {
   		{0,0,1,2},
   		{0,0,1,1},
   		{0,3,1,2}
   	}
   	for(int i = 0; i < a.length; i++){ // 得到全部数组长度
   		for(int j = 0; j < a[i].length; i++){ // 得到每个行数组里的数组数
   			
   		}
   	}
   	
   动态设置二维数组初始值
   	(1) int[][] a = new int[2][3]
   	(2) int[][] a; a= new int[3][3]
      (3) 当知道行数不确定列数时，可使用以下方法定义二维数组
      	int[][] arr = new int[3][];  //  开辟三个数组地址，没有确定执行
      	for(int i = 0; i < arr.length; i++){
      		arr[i] = new int[i+1]; // 给当前行数开辟列空间
      		for(int j = 0; j < arr[i].length; j++){
      			arr[i][j] = i +1;  // 给当前行的列数赋值
      		}
      	}
      	
   声明形式
   	一维数组形式：int[] x || int x[]
   	二维数组形式：int[][] y || int[] y[] || int y[][]
   注意	
   	遍历二维数组时，可以用arr.length得到全部行数组的长度，arr[i].length得到全部行数组里的数组的长度
   	二维数组表示它的每一个数组单元都需要是一个数组地址，否则就会报错，多维数组同理
  */
  ```

  

+ 一维数组和多维数组的存储

  ```java
  /*
   一维数组和多维数组都存放在堆区
   一维数组由独立的地址，里面存储单元是每个元素
   二维数组里面存放的是一维数组里的地址，在引用时会查找对应数组的值
   三维数组里存放的是每个二维数组的地址值，二维数组存放的是一维数组的地址值，一维数组用来存放数据
   数组.length方法是查看当前数组的存储长度(不管是地址值，还是元素)
   ...
  */
  ```

  ![image-20211016092534155](D:\typora_import_images\typora-user-images\image-20211016092534155.png)

  

+ 杨辉三角

  ```java
  /*
  import java.util.Scanner;
  public class Scan{
  	public static void main(String[] args){
  		int n1,constNum = 1;
  		Scanner scan = new Scanner(System.in);
  		System.out.println("enter a number");
  		n1 = scan.nextInt();
  		int[][] arr = new int[n1][];
  		// 杨辉三角主要代码
  		for(int i = 0; i < n1; i++){
  			arr[i] = new int[i+1];
  			for(int j = 0; j <= i; j++){
  				if(j==0||j==i) arr[i][j] = 1;
  				else{
  					arr[i][j] = arr[i-1][j] + arr[i-1][j-1]; //当前数等于前一行的同列数+前一个同列数
  				}
  			}
  		}
  		
  		// 打印呈现
  		for(int i = 0; i < arr.length; i++){
  			for(int j = 0; j < arr[i].length; j++){
  				System.out.print(arr[i][j]+"\t");
  			}
  			System.out.println();
  		}
  	}
  }
  
  注意
  	杨辉三角主要算法为：当前行中列数首位打印1，其他列等于上行的同列+同列-1的和
  */
  ```

  

  

## 1.5 类与对象

**封装，继承，多态是面向对象编程的三大特征**

+ **当一个实例对象使用属性或方法以当前实例对象为目标(指向当前实例对象地址)，改变的是当前实例对象(static除外，static为类和实例对象共有的)，不会影响其他实例对象**

+ 类是抽象的，代表一类事物，对象是具体的代表一具体事物，即 实例
+ 可以把生活中的思维带到代码里然后实现
+ 生成的实例类只有属性有独立的空间，其在使用类方法时实际上是去调用原型类上的方法，然后原型类会在栈区开辟一个方法空间进行执行
+ 实例对象调用原型类方法时，使用的属性都是该实例类上的属性。实例类只保存属性，方法都是存储在原型类上，但实例类可以使用xxx.xx()方法调用原型类上的方法
+ String是一个字符类，同时也是一个不可变数据类型，当他通过参数传递时，实际上是在堆区又重新创建了一个字符串，并把之前字符串的值赋给了新字符串，所以改变这个新字符串对原字符串没有影响


+ 静态属性是 原型类和实例对象都能使用的

  私有属性或方法是只有本类中可以使用的，每个实例对象拥有者自己对应的私有属性，只是访问范围受限

  公有属性会在对象实例是把属性存储该对象地址，从而实现每个实例对线上的属性各自独立





+ 对象创建流程

  ```java
  /*
   
   **主要**
   首先java会把所有的类以二进制形式加载到方法区，当new 创建该类的实例对象时，java会在堆区开一块空间（经过了，加载。连接，初始化后(clinit()方法收集完所有的static表达式)）,该空间存储着该类的类对象，同时方法区的类以地址方式引用该类对象，之后jvm会自动调用super去一个个加载该类对象继承的父类，(重复以上类加载)，当Object类被加载完毕后(顶级基类)，从Object到当前类对象开始调用clinit()方法里的内容，至此类对象初始化完毕。然后从Object类对象到当前类对象依次执行属性&代码块，构造器。并从Object类对象到当前类对象依次向该实例对象空间内压入属性和方法，(以栈的方式载入，当前类对象的属性和方法在顶层，第一级父类的属性和方法在其后，第二级父类的属性和方法在第一季父类其后....Object),在该实例对象使用属性和方法时优先从栈顶查找，栈顶没有在逐步往下查找
   
   
   **特性**--------------------------------------------
   根据继承的特性，我们在初始化时，可以传入子类没有的属性给构造器，在使用super()关键字传递给上级父级，来帮助我们初始化，在初始化完毕后，子类和继承链上的属性会存放到实例对象的地址中，我们想使用该属性，查找顺序为，子类->上一级父类->...Object基类（基类，父类，超类都指父类），会现在子类查找，子类没有则回去上一级父类查找，我们之前借助了super关键字调用了父类的构造器，所以上一级父类会有该属性，这样我们在上一级父类就查找到了需要的属性
   
   当一个初始化后的实例对象调用原型类中的方法时，是以当前实例空间为对象的，也就是说这时的this指向的是调用者，如果此时该空间的属性发生修改，则后续在使用该空间上的属性时，也是修改后的属性值（在new一个对象初始化完毕后，该空间所有的属性就确定了，此时在和原型没有关系，原型可以看作一个生成实例的模板，方法可以看作是个操作属性的函数）
   
   构造器初始化调用符合方法调用规则，先进后出，后进先出，即先调用的最后执行，最后调用的最先执行，在构造器初始化时，默认第一行调用super()，如有指定super()则覆盖默认调用的super()。this()和super()同一构造器只会出现一个，（this()调用同类中的其他构造器，如果该构造器没有this()默认在去调用super()实现构造器初始化）
  */
  ```

  

+ 对象在内存中的存储

  ```java
  /*
   类(模板)存在方法区，实例类存放在堆区，其内部的所存放的数据如果是普通数据类型则和该类地址一致，如果存放的是复杂数据类型则会放到方法区里的常量池中，同时该类的内部保存其常量池的地址 。同时栈区的变量指向堆区的类地址
  */
  ```

  ![image-20211016161930099](D:\typora_import_images\typora-user-images\image-20211016161930099.png)

  

+ 属性&成员变量

  ```java
  /*
   在类中定义的变量被称为 属性或成员变量，字段，它可以是一个基本数据类型，或复杂数据类型(引用数据类型)
   
   注意
   	如果生成了对象实例，不赋值，则 int,short,byte,long,float,double都为0,boolean为false,char为\u0000，String为null
   	当生成对象实例时，需要同时创建一个该对象类型的变量去指向这个实例的地址，如String a = xxx; ，只有以该类名变量类型生成的变量才能指向该变量的实例
   	属性在创建时可以直接赋值
  */
  ```



+ 访问修饰符

  用来控制属性的可被访问范围，有**public,protected,private,默认**

  





+ 成员方法

  ```java	
  /*
   在类中使用属性来表示一个对象的不同信息，使用方法来增加该对象的不同特性(功能)
   
   成员方法的定义
   	可访问类名 返回数据类型 方法名 (形参){语句;return 返回值} 
   	如 public int sum(int n1,int n2) {return n1+n2}
   
   public class Scan{
  	public static void main(String[] args){
  		Person p1 = new Person();
  		p1.speak();
  	}
  }
  
  class Person {
  	String name;  // 类的属性
  	int age;
  	public void speak(){ // 类的方法
  		System.out.println("I am good person");
  	}
  	public int getSum(int n1,int n2){ // 该方法返回的值为int类型，因此只有int类型的变量才能接收
  		return n1+n2;
  	}
  }
  
   类方法调用机制
   	当一个类实例调用类方法时，会在栈区开辟一个该方法的栈空间，待该空间执行完毕后立刻销毁，同时如果该函数有返回值则返会对应的值给他的调用者
   	
   注意
   	栈空间和栈空间是各自独立的
   	当方法执行完毕后，如有返回值则返回该值给他的调用者
   	当栈区的函数调用完毕后会被立刻销毁
   	当main方法执行完毕后，整个程序退出
   	如果一个方法要返回多个值 ，可在类型声明中 以 public int[] xxx(){return x} 数组形式返回
   	方法的返回值和接收返回值的变量要和函数的类型一致
   	如果方法类型为void，则方法体中可以没有return，也可以写return，但是不能返回数据(不能写值)
   	在传入实参时，只能传入与该方法形参对应类型的值(低精度往高精度兼容)，不能多传少传
   	在同一个类中，方法直接可以直接进行调用，不同类中的方法进行调用必须要有该类的实例类
   	在java中，变量类型可以是传统变量类型，也可以是类变量类型(首字母大写)
   	方法里的形参起接收实参的作用的同时也起参数声明作用，在该方法体中可以直接接收该类型的数据，而不用再次声明类型
   	基本数据类型值传递，复杂数据类型地址传递
  */
  ```

+ 父类与子类的交互

  ```java
  
  public class Scan{
  	public static void main(String[] args){
  		
  		// 返回一个 female 对象
  		Person p1 = new Person();
  		Female female1 = p1.female();
  		System.out.println(female1.name);
  	}
  }
  
  //person类
  class Person {
  	
  	public Female female(){
  		return new Female();
  	}
  }
  
  // female 对象
  class Female{
  	String name;
  	int age;
  	String sex;
  
  }
  ```

  

+ 类的克隆

  ```java
  /*
   本质就是生成两个不同的实例(这样就有不同的地址值)，然后在做赋值拷贝即可
  */
  
  
  public class Scan{
  	public static void main(String[] args){
  		Female f1 = new Person().female();
  		f1.age = 12;
  		f1.sex = "female";
  		f1.name = "张三";
  		Female f2 = new Person().cloneClass(f1);
  		f1.name = "李四";
  		f1.age = 15;
  		System.out.println(f1.sex+"__"+f1.age+"__"+f1.name);
  		System.out.println(f2.sex+"__"+f2.age+"__"+f2.name);
  	}
  }
  
  // person 类
  class Person {
  	
  	public Female female(){
  		return new Female();
  	}
  	public Female cloneClass(Female f1){
  		Female f2 = new Person().female();
  		f2.name = f1.name;
  		f2.age = f1.age;
  		f2.sex = f1.sex;
  		return f2;
  	}
  }
  
  // female 对象
  class Female{   
  	String name;
  	int age;
  	String sex;
  
  }
  ```

  

  

+ 递归

  栈调用先进后出，后进先出(栈区的内容执行完后，如果没有引用会被回收)。队列 先进先出。

  递归调用机制，函数自己调用函数本身，需要施加一层判断作为出口条件，否则他就会一直调用自身，变为死递归。在递归调用时，符合栈调用规则，如果方法有返回值，则返回给他的调用者

  ![image-20211017094524423](D:\typora_import_images\typora-user-images\image-20211017094524423.png)

  



+ 汉诺塔算法口诀

  如果只有1一层塔，直接a移动到c即可，如果有多层塔，需要把a所有的塔借助c的力量移动到b，然后打印a=》c ,然后在把 b所有的塔借助a的力量移动到c

  ```java
  public class HanoiTower {
  	public static void main(String[] args){
  		T t = new T();
  		t.move(3,'A','B','C');
  	}
  }
  
  class T{
  	public void move(int num,char a,char b,char c){
  		if(num == 1) System.out.println(a +" ==> "+ c);
  		else {
  			move(num - 1,a , c , b);
  			System.out.println(a + " ==> " + c);
  			move(num - 1, b , a , c);
  		}
  	}
  }
  ```

  



+ 方法重载

  ```java
  /*
   java允许同一个类中，多个同名方法存在，但要求形参不一样(函数类型随意)，如System.outprintln()即用到了方法重载才可以同时打印不同类型的数据(所以java中的打印一次只能打印一种变量类型数据)
   
   有了方法重载我们也可以用相同的方法实现不同的效果(如可以同时可选择的计算多个数的和)
   
   注意
   	方法重载的方法名一定要相同，接收的形参一定不同，(方法类型无要求)当此方法被调用后，java会自动匹配满足形参类型的那个重载方法
  */
  ```

  

+ 可变参数

  ```java
  /*
   java允许将同一类中多个同名同功能但参数个数不同的方法，封装成一个方法
   如 public int sum(int n){} public int sum(int n,int n3){} 可使用可变参数把他们同一接收，这样减少了重复函数的书写
   	使用方法
   		public int sum(int ... nums){} 即可接收多个参数，nums是一个数组(和js扩展运算符同一用法)
   	注意
   		可变参数的实参可以是0个或任意个，使用数组方式存储
   		可变藏书必须是最后一个形参
   		一个形参列表中只能有一个可变形参
  */
  ```

  

  

+ 作用域

  ```java
  /*
   java中主要的变量就是属性和局部变量，局部变量指在方法里定义的变量
   全局变量，也叫属性，作用域为整个类
   全局变量有默认值，局部变量没有默认值
   
   注意
   	属性和局部变量可以重名，访问时遵循就近原则
   	属性生命周期较长，伴随着对象的创建二创建，局部变量的生命周期较短，伴随着代码块的执行而创建，代码块执行完后便会被销毁
   	在同一个作用域中变量不可重复定义,在一个for循环或其他控制语句定义的变量外部无法访问，外部定义的变量内部可以访问
   	属性可以被他的实例所使用(无论在哪里生成的实例)
   	全局变量(属性)可以加修饰符，局部变量不可以加修饰符
  */
  ```

  

  

+ 构造方法(构造器)

  ```java
  /*
   定义一个构造器
   class Person{
   	int age;
   	String name;
   	public Person(String pName,int pAge){
   		age = page;
   		name = pname;
   	}
   }
  
   注意
   	构造器的修饰符可以是默认的，也可以是public,protected,private
   	构造器本质上是一个方法，但没有返回值，没有方法类型
   	方法名和类名必须一致
   	构造器用来完成对象的初始化，并不是创建对象，构造器的调用由系统完成
   	构造器用来快速常见一个类的属性，没有构造器只能一个一个命名
   	构造器也可以重载，一个类可以定义多个不同的构造器，如指定人名，年龄等
   	如果一个类没有指定构造器，则java底层会默认添加一个无参的构造器，里面的参数为空。如 new Person() 这个Person()就是调用了底层的无参构造器
   	如果有自定义构造器，默认的构造器会被覆盖，除非自己在重新定义一边 xxx(){}
  */
  
  
  ```

  

+ javap

  ```java
  /*
   javap是jdk提供的命令行工具，可以对class文件进行返汇编
   javap xxx.class 可以对一个类文件进行反编译，即可查看文件详细(否则就是加密文件)
   javap xxx.class -c 可以对该文件进行反汇编操作
   javap xxx.class -v 可以对该文件输出附加信息
   javap xxx.class -p 可以显示该文件的所有类和成员(属性)
   
   注意
   	可以不写.class 直接使用 javap xxx -p 也可以达到同样效果
  */
  ```

  

+ this关键字

  ```java
  /*
   this指的是当前引用的类原型的实例对象，即谁调用原型对象this指向哪个实例
   hashCode可以返回一个当前对象的虚拟地址，使用 xxx.hashCode()
   
  
  public class Test {
      public static void main(String[] args){
          Computer computer = new Computer("w","3","3");
          System.out.println(computer.CPU);
      }
  }
  class Computer{
      String CPU="1";
      String memory;
      String disk;
      public Computer(String e,String memory,String disk){
          // this指向调用这个原型类的实例类，this.CPU指该实例类的CPU属性，CPU等于该原型类作用域下的CPU，没有则会去原型类全局查找对应属性
          this.CPU = CPU;
          this.memory = memory;
          this.disk = disk;
      }
  }
  
   
   注意
   	this关键字可以访问本类的方法，属性和构造器
   	java中类的方法访问可以使用xxx()或者this.xxx()都是一样效果（js中在内部访问自己的方法必须要加this.xxx()）
   	在一个类中，如果重载了多个构造器，则可以在一个构造器中使用this()进行构造器调用，不过this()必须写在该构造器的首句
   	this()调用另一个构造器，该构造器执行完毕后，会回到this()所在的构造器执行剩余内容
   	this不能在类定义外部使用
   	类在访问属性时，如果没加this就会优先在该方法的内部查找变量，没找到才回去实例对象全局查找。如果加了this就会直接去实例对象全局查找
  */
  ```

  

+ IDE集成开发环境

  ```java
  /*
   IDEA支持HTML,CSS,PHP,MySQL,python等开发
   Eclipse是一个免费开源高效的java编辑器
   
   	IDEA使用
   		可以新键一个工程 new Project 到指定文件夹，然后系统会默认创建一个src文件夹，在该文件夹中，默认存放的就是.java后缀的文件，即 创建文件时 直接写文件名即可，系统会默认加,java后缀变为java文件
   		双击一个选项页可以全屏显示该选项页
   		在Idea中点击运行后可以直接编译运行(idea帮我们做了这些操作)，src文件存放.java文件，out文件存放着.class文件
   		
   	IDEA常用快捷键(可在setting --- mapkey中设置)
   		删除，复制  ，可在setting --- mapkey中设置
   		补全代码 alt + /
   		自动导入java用到的类 在 setting --- Editor --- General --- Auto import 中勾上 add..fly 和 optimize... 。 使用：在需要用到本地类的行中输入 alt + enter 即可展示用到的jvaa类，选中自动导入 
   		格式化代码 ctrl + shift + l   
   		自动分配变量名。在代码后面加 .vartab即可 如 new Scanner(System.in).var(tab) 即可快速分配一个变量名
   		generate..用法，可以开苏成功构造器和getxx,setxx方法 ，我的快捷键为ctrl+shift+j
   		ctrl + alt + t 可给选中表达式插入一个语句(if,for,try-catch等)
   		
   	配置java模板
   		file--settings--editor---live template 即可编辑自己的模板 ， 可以点击+号配置模板，同时选择模板应用范围
   		
  */
  ```
  
  



+ 包

  ```java
  /*
   包用来区分相同名字的类，当类多时，用一个包去管理一些类是一个好选择，包的本质就是创建不同的文件夹来保存类文件,这样即减少了命名冲突
   基本语法
   	package com.hspedu,其中package表示打包，com.hspedu表示包名
   	
   创建包 选中 src new --- package 然后 com.xiao  即可创建，此时会在src下生成一个com文件夹，com文件夹里包含一个xiao文件夹，即com是package下的文件夹，xiao是com下的文件夹，有一个.会在其当前文件夹下创建一个文件夹 。 当执行 com.xiao创建包后 文件夹排序为 src/com/xiao
   
    包名下类的引用
    	如果有多个同名的类在不同的包下时，可以使用 路径+对象作为查找方式进行指定对象创建 如 com.Xiao.Dog() dog1 = new com.Xiao.Dog();  ，此方法为对象的引用
    	同时java也提供了引入包名类的方式，如 import com.xiao.Dog; ，引入过可直接使用Dog原型类 直接创建Dog实例 Dog g1 = new Dog(); . 以此法引入的类相当于在该java下创建了该类，因为java规定一个文件内不允许同名的类出现，因此，java不允许重复创建dog类且不能引入其他文件夹下同名dog的类
    
    
    包的引入
    	import java.util.Scanner;引入java.util下的指定包（常用）
    	import java.tuil.*; 引入java.util下的所有包
    
    包和类的关系
    	每个被包管理的类必须要写package xx.xx.xx 声明当前类所在的包(package语句必须在所有语句前)，同时与该包形成链接
    	
    
    包名规则(必须遵守)	
    	只能包含数字，字母，下环线，小圆点(.)，不能以数字开头，不能是关键字保留字
    
    java中常用的包
    	java.lang.*   lang为基本包默认引入
    	java.util.*   util包为系统提供的工具包，如Scanner
    	java.net.*    网络包，网络开发
    	java.awt.*    java页面开发,GUI
    
    注意	
    	当创建包时，每有一个.就会在其最近的上级文件夹下创建一个文件夹，如 com.wor 即在src下创建com文件夹，在com下创建wor文件夹
    	import语句在当前java文件类不冲突的情况下，可以无限制的引入其他包下的类
    	同一路径下的包不能有两个相同的类，且所有类互通，即在另一个类文件可以调用同包下的其他类文件
   	
  */
  ```
  
  ![image-20211019113004565](D:\typora_import_images\typora-user-images\image-20211019113004565.png)





+ 访问修饰符

  ```java
  /*
   java有四种访问修饰符符号，用于控制方法和属性的访问权限(范围)
   	有 public,protected,无，private
   		被public修饰的方法或类 表示对外公开，即外部可以使用
   		被protected修饰的方法火雷，表示只有子类和同一个包中的类可以使用
   		无，表示没有修饰符号，只有同一个包的类可以使用
   		被private修饰的方法或类 表示只有类本身可以使用，类外不能使用 
   		
   	注意
   		修饰符可以用来修饰类中的属性或方法即类
   		只有默认和public才能修饰类
  */
  ```

  ![image-20211019142441575](D:\typora_import_images\typora-user-images\image-20211019142441575.png)



+ ​	封装(encapsulation)

  ```java
  /*
   定义
   	把抽象出的属性和对数据操作的方法封装到一起，数据被保护在内部，程序的其他部分只有通过被授权的方法才能对数据进行操作，这就是封装。即用户只能使用某些接口，不能访问该接口的内部数据
  	封装的好处是 隐藏实现功能细节，用户只用完成接口调用即可完成对应操作。可以对数据进行校验，保证数据安全性 
   
   封装的实现
   	将需要保护的数据使用 private(只允许在本类进行操作) 关键字修饰
   	封装安全性校验(public修饰的属性或方法可以在任何地方访问)
   		可以在本类中创建一个setxxx的public方法进行内部逻辑安全校验(把校验细节写在该函数中)，如果数据合法即可修改 。 （方法内部可以施加判断逻辑，从而提高了数据安全性，而属性则不能）
   		可以在本类中创建一个getxxx的public方法获取被保护数据，在获取的同时可以做权限安全校验，如果权限合法即可获取数据 。 （方法内部可以施加判断逻辑，从而提高了数据安全性，而属性则不能）
   	
   构造器和封装的交互
   	当给类设置构造器后，可以进行传参设置私有属性的值，这样就间接的破坏了私有变量的安全性，所以可以在构造器中调用其他的公共判断安全性方法(get...,set...)来对这些数据进行判断，进而提高安全性
   	
   总结
   	封装可以保证数据的安全性，在其他地方要获取被保护属性或方法时，可以创建一个公共方法作为权限判断方法，如果权限满足则返回或修改对应的属性或方法
   	任何需要判断是否合法的属性都需要使用private来修饰(这样才能判断是否合法，且外部无法直接获取和更改)
   	作为判断合法性的方法不是必须以get...或set...开头，只要是一个单纯的方法就行，但是为了见名知意，通常以get...或set...命名
   	私有变量和publi判断合法性方法的关系就像外国商品和代购员一样，代购员要判断你的商品是否合法，然后才给你代购东西
  */
  ```

  



+ 继承

  ```java
  /*
   继承可以有效解决代码复用问题，使用extends关键字即可继承父类的属性和方法，有效解决了代码复用问题
   
   一个类使用extends继承另一类后，这个完成自称的类称为子类或派生类 ， 被继承的类被称为父类，子类可以使用父类即父类继承的其他类的属性和方法
   
   使用继承给编程带来了遍历，提高了代码复用性和扩展性以及维护性
   
   继承的基本语法
   	class 子类 extends 父类{} , 子类会获取父类及父类继承的其他类的属性和方法， 父类又被称为 超类，基类 。 子类又被称为派生类
   	
   继承的本质
   	当子类对象创建好后，建立了查找关系，其中子类继承链的构造器符合栈调用顺序(先进后出，后进先出)，所以构造器执行顺序为基类 ...-->子类
      子类属性查找规则：先查找子类是否拥有该属性(该属性可访问),如果有则使用子类的，如果没有就往上一级父类查找,在判断其是否拥有，如果有则返回，如果没有，则继续向上查找，直到找到该属性 。 即子类有就使用自己的，没有就往上一层父类查找，如果该父类也没有，则继续往上查找，直到找到为止
   	
   注意
   	子类继承了所有属性和方法，但私有属性和方法不能在子类直接访问，要通过public方法访问
   	当子类继承了父类后，在生成子类的实例时，子类和父类(1级父类,2级父类...顶级父类)的构造器都会被调用(如果子类没有使用super()则super()关键字会被默认调用),如果父类没有提供无参构造器，则必须要使用super关键字去指定父类的构造器(父类的构造器先调用，子类构造器后调用)，否则编译无法通过 。 
  	如果想要指定调用父类的哪个构造器可以使用super("参数")指定,否则会默认调用无参构造器
  	super()在使用时，必须是子类构造器的第一条语句 ，且super关键字只能在构造器中使用
  	super()和this()都只能作为构造器的第一条语句，所以super()和this()不能同时使用，(super关键字用于调用父类的构造器，this关键字用于调用本类的其他构造器)
  	java所有类都是Object的子类，Object是所有类的基类(父类),使用ctrl + h 可以查询继承关系
  	父类构造器的调用不限于直接父类（最近一级父类），将一直往上追溯知道Object类
  	一个子类继承父类时，实际上是继承了父类及父类继承的类，直到继承到基类(Object)，其中关系如 子类-->一级父类-->二级父类 .... 基类(Object)，最终所有继承指向基类
  	子类最多只能继承一个父类(直接继承)，即java的单继承机制，如果一个类A想使用类B和类C的属性和方法，可以使 A继承B，然后B继承C 即可，这条继承链上A可以使用所有向上继承(即A---B----C---Object)的属性和方法
  	如果在继承查找中，查找对一个符合条件的属性，但该属性被private时(需要在给类内部封装一个public类把该私有变量作为返回值返回)，无法访问，且查找链终止，程序报错
  	实际上每个没有使用extends关键字的类都是Object对象的子类，所以没有写super关键字默认在构造器首行添加(所以所有类在每写super关键字的情况下都会调用super())
  	在生成一个类的实例对象时，其继承的所有属性和方法都会载入到该地址中
  */
  ```
  
  ![image-20211020143858259](D:\typora_import_images\typora-user-images\image-20211020143858259.png)
  
  ![image-20211020161453002](D:\typora_import_images\typora-user-images\image-20211020161453002.png)



+ 继承的应用

  ```java
  /*
   package com.extendsme;
  
  public class Test {
      public static void main(String[] args) {
          PC huaShuo = new PC("华硕", "锐龙 2600x", "金士顿 2666", "西数固态 8g");
          NotePad note = new NotePad("白色","华硕", "锐龙 2600x", "金士顿 2666", "西数固态 8g");
          System.out.println(huaShuo.CPU);
          System.out.println(huaShuo.CPU);
          System.out.println(huaShuo.getDetails());
          System.out.println(note.getDetails());
  
  
      }
  }
  
  class Computer {
      String CPU = "1";
      String memory;
      String disk;
  
      public Computer() {
  
      }
  
      public Computer(String CPU, String memory, String disk) {
          // this指向调用这个原型类的实例类，this.CPU指该实例类的CPU属性，CPU等于该作用域下的CPU没有则会去原型类全局查找对应属性
          this.CPU = CPU;
          this.memory = memory;
          this.disk = disk;
      }
  
      public String getDetails(String... str) {
          String str1 = "";
          for (int i = 0; i < str.length; i++) {
              str1 += str[i]+" ";
          }
          return str1;
      }
  }
  
  class PC extends Computer {
      private String brand;
  
      public PC(String brand, String CPU, String memory, String disk) {
          super(CPU, memory, disk);
          setBrand(brand);
      }
      public PC( String CPU, String memory, String disk) {
          super(CPU, memory, disk);
      }
  
      public String getBrand() {
          return brand;
      }
  
      public void setBrand(String brand) {
          this.brand = brand;
      }
      public String getDetails() {
         return getDetails(brand, CPU, memory, disk);
      }
  }
  
  class NotePad extends PC {
      private String color;
  
      public NotePad(String color,String brand, String CPU, String memory, String disk) {
          super(brand,CPU, memory, disk);
          setColor(color);
      }
  
      public String getColor() {
          return color;
      }
  
      public void setColor(String color) {
          this.color = color;
      }
  
      public String getDetails() {
          String brand = getBrand();
          return getDetails(color,brand, CPU, memory, disk);
      }
  }
  */
  ```

  

+ super关键字

  ```java
  /*
   super代表父类的引用，用于访问父类的属性
   super的使用
   	super.xx或super.xx()可以访问父类的非私有属性或方法(可以访问public 默认 protected修饰的属性)
   	super()可以调用父类(最近一级)的构造器，他必须在子类构造器中进行调用，且要以第一条语句的方式
   
   
   注意
   	super()方式调用父类构造器时，super会根据接收的参数调用对应的父类(最近一级)构造器	
   	super()调用父类构造器方式很好的诠释了 自己的属性自己初始化
   	当子类和父类进行重名时，为了访问父类的属性可以使用super.xx方式，如果没有重名则super,this,直接使用方法一致
   	 super()只能调用最近一级的父类构造器(直接继承关系的父类)
  	 super.xx和super.xx()获取属性和调用方法可以获取或调用任意父类(直接父类和间接父类)的属性和方法(就近原则)
  */
  ```

  ![image-20211021153329841](D:\typora_import_images\typora-user-images\image-20211021153329841.png)





+ 方法重写(override)/覆盖

  ```java
  /*
  定义
  	如果一个子类的方法和父类的方法名称，返回类型，接收参数完全一致，那么就可以说这个子类重写了父类的某个方法
  
   子类方法的参数，方法名称要，方法数据类型(除了父类方法类型是子类方法类型的父类，如父类 Object，子类String)和父类完全一致,这样就形成了方法重写
   
   注意
   	子类重写的方法不能缩小父类方法的访问权限，如父类是public，子类重写后必须也是public，如父类是protected，子类可以为public，即父类的方法的访问权限一定比子类的大
  */
  ```

  



+ 多态(polymorphic)

  ```java
  /*
   多态提升了代码复用性和维护性，重写和重载就体现了多态性(方法多态)
   
   定义
   	父类对象 变量名 = new 子类实例();  此时的父类对象在编译时是父类对象，在运行时是对应的子类实例，即被父类对象声明的变量可以指向它的所有子类
   	
   向上转型
   	即被父类数据类型修饰的变量指向了它的子类，在运行阶段该类型仍然是指定的类型，并可以完成继承查找，但到编译阶段，则变为父类数据，只能使用该父类(及它继承)的属性和方法。如果在运行阶段该引用继承到了一个非其类上(包括该父类的继承类)拥有的属性和方法,在编译时则会报错
   
   向下转型
   	语法： 子类类型  引用名 = （子类类型） 父类引用   ，（子类类型） 父类引用 意为将父类的引用类型转换为子类的数据类型(该转变原父类引用不受影响)，并赋之给一个新的子类(同类型名)引用。这时该子类引用就可以使用其子类的属性和方法了。
   	向下转型中，父类引用只能转换为其当前指向的子类对象，如有一个父类当前指向的是Dog子类，则该引用的向下转型只能转换为 Dog ,例 Dog d1 = (Dog) animal;
   
   instanceof的使用
   	可以用来判断一个实例对象是否是由该原型类所生成的，或继承继承于该原型类（即是否为该原型类的子类）
   	注意
   		在判断一个多态引用时，以运行类型为准
   
   多态数组
   	即创建一个多态数组，数组的每一个下标存放的是它子类对象实例，在使用时下标获得每个子类对象实例，仍然符合多态继承动态绑定机制原则
   
   多态参数
   	方法定义的形参类型为父类类型，实参类型运行为子类类型 。仍然符合参数查找原则
   
   多态属性/方法查找路径
     父类/接口在使用属性从该父类身上开始查找，使用方法时，从指向的子类实例对象开始查找(运行类型,动态绑定机制)，且使用的需要是父类及父类继承链上存在的方法(编译类型)，(此时该实例对象已初始化完毕，其空间内有其自己的属性和方法即其父类的属性和方法，越是上级父类它的属性和方法越在底部，自己的属性和方法在最顶部，其下是最近一级父类的属性和方法…最近二级….Object),(实例对象的存储像一个栈结构，先进后出，后进先出),此外在调用一个子类实例对象有的方法但其父类及父类继承链上没有的属性和方法时，需要向下转型
   
   注意
   	父类变量类型之所以可以指向子类对象，是因为其会进行继承查找，差会遭到父类和子类有一定联系，即可以指向
   	一个对象的编译类型和运行类型可以不一致，如 Anima a = new Dog();(Anima为Dog的父类)，此时a的编译类型是Anima但运行类型是Dog . 即允许父类数据类型接收子类对象
   	编译类型在定义对象时就确定了，不能改变。运行类型可以变化的，一般编译类型看 = 号左边，运行类型看 = 号右边
   	编译类型和运行类型概念只会出现在对象中，编译类型可以是子类原型的父类
   	规范：被普通变量类型修饰的变量称为 变量名，被类数据类型(对象)数据类型修饰的变量称为 引用名
   	多态和继承同时存在互不影响
   	实例对象和实例对象做 == 判断是判断他们各自指向的地址，如果相同则为true
   	
   	多态中，父类指向子类时，父类以.xxx的方法获得属性时，是以当前父类开始查找(没找到一直往他的继承父类查找)。当父类以.xxx()调用方法时，以子类开始查找(且父类或父类的继承链上拥有该方法)，如果子类有该方法就执行子类的方法，且查找终止，如果子类没有该方法则会一直沿着继承链查找，找到就调用此方法，没找到则报错。
   	多态中，每次方法调用都会从子类开始查找，然后在沿着继承链查找
   	在多态中，如果想调用子类的方法，但父类即父类的继承链中没有此方法，在一般情况下会报错。可以通过instanceof判断实例，然后向下转型解决  ，如  ((Cat)animal).say();(必须要加()，()优先级大于.)
   	参数引用(赋值)和形参接收实参都是相同的
   	多态是依附于继承的，没有继承就没有多态
   	多态：父类/接口及 引用 子类./实现接口的类的实例对象时，在使用属性时，从该父类/接口身上查找并使用，在使用方法时，分编译状态和运行状态(需同时满足)。编译状态指调用的方法在父类或父类的继承链上必须存在，运行状态指，实际被调用的方法从当前指向的子类实例对象开始查找(子类实例对象没有则往它的继承链上查找)。通常分为四种情况，一.子类实例对象存在(方法)，父类也有此方法，则调用子类实例对象上的方法且编译通过；二.子类实例对象没有此方法，父类有此方法，则(通过子类实例对象继承父类方法)调用父类的方法且编译通过；三.子类有此方法，父类没有此方法，则需要向下转型(否则运行类型通过，编译类型不通过)；四.子类和父类都没有此方法，则编译不通过
   	
   	
  */
  ```

  

+ 动态绑定机制（dynamic binding）

  ```java
  /*
   动态绑定机制一般出现在继承和多态中，当每个方法在类中被调用时，它的查找顺序一直为: 当前实例对象原型-继承链，每有一次函数调用就会重新执行一边该查找（子类-继承链），如 C 继承 B ，B 继承 A,当 new C().xx()调用一个C原型类中没有的方法时java会往它的继承父类查找，查找到后，如果在该方法内部再次调用了一个方法，则还是从C内开始查找。又如 new B().xxx()时，其类中和继承链中的函数调用都会从B开始查找
    属性的查找顺序为 当前所处原型类，如果没有则会查找它的父类
  */
  ```

  

+ Object对象

  ```java
  /*
   equals方法
   	== 和 equals的对象
   		== 是一个比较运算符，可以判断基本数据类型，和引用数据类型，判断基本数据类型时，判断其值是否相等，判断引用数据类型时，判断其地址是否相等
   		equals是Object类中的一个方法，只能判断引用类型，Object类中写的是判断地址是否相等，但String和Integer原型类对该方法进行了重写，从而可以判断值是否相等
   		
   hascode方法
   	 hascode方法提高了哈希结构的效率
   	 注意
   	 	两个引用，如果指向的同一个对象，则哈希值一定一致，反之则不一致
   	 	哈希值根据内存地址推算而来，但不完全等价与地址
   		
   toString方法
   	该方法会返回一个 该类名+@+哈希值的十六进制 ，通常在使用时会把toString重写(为了打印对象拼接后的属性) ， 在Idea中可以使用 ctrl + shift + j 创建一个toString,默认做拼接
   	注意
   		当直接打印一个实例对象是默认调用的为toString方法
   
   finalize方法
   	当垃圾回收机制要回收一个没有引用关系的实例对象时，java会调用finalize方法（该方法在Object对象下），所以，我们可以重写finalize方法来帮助我们完成一些收尾逻辑
   	垃圾回收机制的调用由系统决定(由自己的gc算法)，也可以通过System.gc()主动触发（在垃圾回收机制正在进行的时候调用可能失效）
   	注意
   		finalize方法在没有重写的情况下，为一个空方法
   		使用finalize在idea中即可快速创建代码块
   
   注意
   	以上的方法都在Object下，我们可以完成方法重写
   	所有的类有一个顶层的超类Object,所以类在创建时就确定了继承关系，即有继承查找规则，和多态执行规则
  */
  ```



+ 方法中if-else优化

  ```java
  /*
   因为方法碰到return 会立刻返回并且不会执行剩余语句，因此在函数中如果没有特定需要通常只写if判断，避免if-else嵌套冗杂
   如
   	public boolean equals(Person personObj) {
          if (this == personObj) {
              return true;
          }
          if(personObj instanceof Person){
              if (this.name.equals(personObj.name) && this.age == personObj.age && this.gender.equals(personObj.gender)) {
                  return true;
              }
          }
          return false;
      }
  */
  ```

  



+ Date类

  ```java
  /*
   Date类可以获得当前的时间，但这个时间式美国时间，我们还需要用到SimpleDateFormate一个对象，它可以格式化一个时间如  SimpleDateFormate("YYYY-MM-DD hh-mm-ss"); ,在传入Date即可
   
   定义方式
   	Date date = new Date();
   	SimpleDateFormate sdf = new SimpleDateFormate();
   
   基本使用
  	    SimpleDateFormat sdf = new SimpleDateFormat("YYYY-MM-dd hh-mm-ss");
          Date date = new Date();
          String time = sdf.format(date);
          System.out.println(time);
  */
  ```




+ 对象模型开发

  ![image-20211025115413695](D:\typora_import_images\typora-user-images\image-20211025115413695.png)



+ static（类变量/类方法）

  ```java
  /*
   被static修饰的变量或方法为类变量或类方法，在类被加载到方法区时，系统会检索该类中是否有static修饰的属性或方法，如果有，则会把该属性或方法放到堆区中，并给该属性或方法开辟一个独立的空间，该原型类和它的实例对象都可以使用该静态属性或方法，如果是属性还可以对其进行修改，修改后，他的原型类和它的实例对象在使用该值时，则是显示修改后的值
   
   基本定义
   	定义类属性：访问修饰符 static 数据类型 变量名
   		定义的变量可以使用 实例对象.属性 或 原型类.属性 来访问
   	定义类方法：访问修饰符 static 数据类型 方法名(){}
   		
   注意
   	jdk8以前 static通常存储在方法区，在jdk8以后则存储在堆区
   	静态变量仍然要遵守访问权限
   	类变量是该原型类和该实例对象所共同使用的，而实例变量是每个实例对象独有的，在实例对象调用类方法时，操作的是当前调用方法的实例对象的属性(包括继承的)
   	被static修饰的属性和方法通常被称为类变量或静态变量，否则为实例变量/普通变量/非静态变量
   	类变量推荐使用 原型类.属性名方式使用
   	实例变量或实例方法不能通过 原型类.属性名以及原型类.方法名() 使用
   	类变量或类方法的生命周期随着类的创建而创建，类的销毁而销毁
   	如果我们不希望创建实例就能在原型类内部使用属性或方法，则可以把他们定义为静态属性或静态方法
   	在类方法中我们不能使用this和super，在普通方法则可使用
   	类属性和类方法又叫静态属性和静态方法，实例对象上的属性有被称为实例属性和方法或非静态属性和非静态方法
  */
  ```
  
  ![image-20211026164311078](D:\typora_import_images\typora-user-images\image-20211026164311078.png)



+ main方法

  ```java
  /*
  main的定义方式
  	 public static void main(String[] args){}
  
  各参数意义
  	 main方法为java虚拟机调用，main的类修饰符一定为Public，因为调用它的虚拟机和该类不在同一个包
   	java虚拟机调用类的入口方法时，以该原型类.main()方式进行调用，因此 main方法需添加static关键字
   	形参String[] args可以在该程序被运行时，接收编译传入的实参（传入的每个参数需要用空格隔开），遍历每个数组即可清晰的看到每个值。传参格式为：Java 运行的类名 第一个参数 第二个参数 ...
   	
  注意
  	在main方法中，只能直接使用全局变量下的静态属性或静态方法，如果要使用非静态属性和方法必须生成实例对象后，通过实例对象.属性或实例对象.方法()去使用 
      在Idea集成编译中，可在编译器右上角锤子旁，点击下拉菜单，选择edit，program arguments携带参数，参数的携带同样需要空格隔开
   
  */
  ```

  

+ 代码块

  ```java
  /*
   基本语法
   	[修饰符]{
   		代码
   	};  在类被加载到堆区时，代码块会被隐式调用
   
   注意
   	修饰符只能写static
      ;可以省略
      逻辑语句可以为任何语句(输入，输出，方法调用，循环等)
      普通代码块在每次被触发都会被调用，static修饰的代码块和属性，方法只会被调用一次
      代码块和类的继承交互中，在创建实例对象时，会先调用继承链上的所有构造器，当调用到本类中的构造器时，先执行代码块内容，在执行构造器内容
      静态(static)代码块触发隐式调用的三种情况（静态代码块当类被加载时才会执行，且会立刻输出）
      	创建实例对象时，代码块会先于构造器触发
      	创建子类实例对象时，其继承链上的构造器和代码块同时也会被调用
      	使用类的静态属性时也会触发代码块
      普通代码块的触发情况（遵顼栈调用原则，只会咸鱼本类中的构造器先输出）
      	每次生成实例对象时，会被调用
      	创建子类实例对象时，其继承链上的构造器和代码块同时也会被调用
      在对一个子类进行初始化时，他会先执行Object到该子类的所有静态属性和静态代码块(优先级一致，根据代码顺序执行)，然后调用子类构造器，构造器第一行通常为super()，则去调用最近一级父类的构造器，该父类继续去调用他的父类构造器，...一直执行该操作，直到执行到Object对象，然后开始从上到下执行父类的普通代码块和普通属性到实例对象地址中，然后执行父类的构造器内容，在执行完后跳转到下一级父类...一直执行此操作，直到执行到子类，这时即初始化完毕
      因为继承的原因，子类原型使用静态变量时，如果在该类没找到则回去它的父类的静态空间进行查找使用
      在一个构造器中，实际上隐藏了两行代码，第一行为super()，第二行为调用普通代码块
      静态代码块只能调用静态属性和方法，普通代码块可以调用任意属性和方法
  */
  ```



+ 单例设计模式

  ```java
  /*
   单例设计模式指只允许某个类存在一个对象实例，且该原型类只提供一个取得其对象实例的方法
   单例模式分：饿汉式 && 懒汉式
   
   饿汉式
   	将构造器私有化，防止直接new生成构造器
   	在原型类中，new 构造器()得到私有对象
   	设置一个静态方法返回这个私有属性(私有属性也需设置为static，因为静态方法只能使用静态属性)
   	外部通过类型.静态方法()即可获得一个唯一的实例对象
   		该方法不管调用多少次返回的都是同一个实例对象，因为其内部只new了一次
   		之所以为饿汉式，指原型类在使用其他静态属性时，就间接了创建了该类的实例对象，如果这个实例对象没有使用，就可能造成资源浪费
   
   懒汉式
   	将构造器私有化，防止直接new生成构造器
   	在原型类中，设置一个静态属性 原型类名 属性
   	创建一个静态方法，在其内部做判断，如果之前设置的静态原型类名修饰的属性没有被赋值，则创建一个该类的实例对象，返回此静态私有变量。懒汉式解决了饿汉式的资源浪费问题
   	
   饿汉式VS懒汉式
   	二者创建实例对象实例时机不同，饿汉式在类加载时就会创建，而懒汉式需要在使用时才创建
   	饿汉式不存在线程安全问题，懒汉式则存在
   	饿汉式可能存在资源浪费，懒汉式则不存在
  */
  ```



+ final

  ```java
  /*
   被final关键字修饰的属性，方法或类不允许被其他类继承
   
   基本使用
   	final 修饰类
   		final Class Person{} // 此时该类不允许其他类继承
   	final 修饰方法
   		public final void hi(){} // 此时该方法不允许子类重写
   	final 修饰属性
   		public final int a = 99; // 此时该属性不允许修改
   
   注意
   	一般不允许改变的属性会用大写，被称为常量如  public final int AV = 123;
   	final修饰的属性在定义时	，必须赋初值，且以后不能修改，（也可在构造器，代码块赋值）
   	如果final修饰的属性是静态的，则初始化的位置只能在定义时和静态代码块中
   	如果一个类已经被final修饰，则在此类中的方法就不必添加final修饰了
   	如果final和static一起使用则不会导致类的加载，即被static修饰的属性或方法不会加载到堆区
   	final修饰格式一般为  类修饰符 final static 类型修饰符 属性或方法()
   	常见包装类有(即被final修饰的类，不允许继承) Integer,Double,Float,Boolean,String等
  */
  ```




+ 抽象类

  ```java
  /*
   当父类的某些方法，需要声明，但又不确定器内部的代码逻辑时，我们可以将其声明为抽象方法(使用abstract修饰)，同时该类也要声明为抽象类，那么这个类就是抽象类
   
   基本定义
   	abstract class {public abstract void random();}
   	
   基本使用
   	abstract class A{public abstract void printIO();}
   	class B extends A{public void printIO(){}} // 子类继承父类时，如果父类是一个抽象类，且子类不是抽象类，则子类就要对父类的抽象方法继续重写
   	
   	
   	
   注意
   	抽象类不能生成实例化对象
   	抽象类可以没有抽象方法，但又抽象方法的类一定是抽象类
   	abstract只能修饰类和方法
   	抽象类本质还是类，可以正常的设置属性和方法
   	如果一个类继承了抽象类，则它必须实现抽象类的所有抽象方法，除非该子类也为抽象类。即抽象类在被子类继承时，如果该子类不是抽象类，则该父类中的抽象方法体必须由子类完成声明（即对该方法进行重写）
   	抽象类不能使用private,static,final修饰，因为抽象类方法就是要指定子类对该方法进行重写，如果使用了以上关键字则子类无法完成重写
   	
  */
  ```



+ 抽象模板设计模式

  ```java
  /*
   	System.currentTimeMillis()可以获得当前时间
   	抽象模板模式提高了代码复用性，通过在父类中声明相同调用的方法，然后去调用
  */
  
  public class Dynamic {
      public static void main(String[] args){
          new B().computedTime();
          new C().computedTime();
      }
  }
  abstract class A{
      public A(){
          
      }
      public abstract void job();
      public void computedTime(){
        long start =   System.currentTimeMillis();
        job();
        long end = System.currentTimeMillis();
        System.out.println(end-start);
      }
  }
  class B extends A{
      public int count;
      @Override
      public void job() {
          for(long i = 0; i < 80000000;i++){
              count++;
          }
      }
  
      public B(){
  
      }
  }
  class C extends  A{
  
      public int count;
      @Override
      public void job() {
          for(long i = 0; i < 800000000;i++){
              count++;
          }
      }
  
      public C (){
  
      }
  }
  ```

  

+ 接口

  
  
  ```java
  /*
   结果就是把一些没有实现的方法封装到一起，当某个类使用implements调用该接口时，由该类实现接口中的方法
   
   基本语法
   	public interface 接口名{
   		// 属性
   		// 方法(1.抽象方法，2.默认方法，3.静态方法)
   	}
   
   接口 vs 继承
   	继承主要解决了代码的复用性和可维护性问题
   	接口主要结局了设计规范问题，让实现的方法命名统一
   	接口在一定程序上实现了代码解耦（接口规范性+动态绑定机制）
   	
   接口多态性
   	接口参数可以接收实现该接口的实例对象，在使用时可以调用该实例对象的方法，（和对象的多态性一致，由于该实例对象实现的就是接口中的方法，所以通常不需要向下转型，直接调用方法即可，如果要调用一个接口中没有的方法时，就要对该接口指向的实例对象做向下转型，这时该实例对西昂运行和编译状态都是该实例对象，就可正常的使用他们属性和方法了）
   	接口多态数组
   		和多态数组类似，接口多态数组可以指向实现了该接口的所有实例对象
   	
   	接口多态传递
   		当接口A继承了接口B，同时一个原型类实现该接口A时 ，那么该原型类需要同时重写A和B中的所用方法，在这时接口A声明的变量可以指向该实例对象，接口B声明的变量也可以指向该实例对象
   
   注意
   	jdk7以前接口不能由方法体，jdk8以后接口可以有静态方法，默认方法，默认方法需要时用default关键字修饰
   	在接口中的方法都是抽象方法，可以省略abstract关键字
   	接口起了统一命名的作用，在协同开发时，避免方法命名不统一问题
   	接口不能生成实例化对象，且接口内的方法默认被public和abstract修饰，如果使用使用了static和default则会覆盖abstract
   	一个普通原型类实现接口时，必须实现该接口中的所有抽象方法
   	抽象类实现接口，可以不实现接口方法
   	一个类可以实现多个接口，多个接口要用,号隔开，如 class P implements IB,IC{}
   	接口中所有的属性默认被 public static final修饰，且属性访问形式为接口名.属性
   	接口不能继承类，但可以继承其他接口（接口继承接口时，可以同时继承多个）
   	接口的修饰符只能为 默认 和 public
   	一个普通类在实现一个接口时，其原型类，实例对象都能使用该接口的属性
   	在一个类继承了父类的同时实现了接口，然后他们都有相同的一个属性时，这时需要明确的指出使用的是那个一个变量，如 super.xx,或A.xx
  */
  ```
  
  # 





+ 内部类

  ```java
  /*
   一个类内部又完整的嵌套了另一个类结构，被嵌套的类被称为内部类，嵌套类被称为外部类，另外的和该类没有任何关系的类被称为其他类
   
   内部类的分类
   	定义在外部类局部位置上（如 方法内）
   		分 局部内部类（有类名） ， 匿名内部类(没有类名)
   			局部内部类（本质上还是一个类，只不过是局部变量的身份）
   				可以直接访问外部类的所有属性
   				局部变量的属性不能添加访问修饰符，因为它的地位就是一个局部变量，但final可以修饰，因为final可以修饰类，方法，和变量
   				局部内部类的作用范围只在该方法或代码块内
   				局部内部类可以访问外部类(包括继承链上)的属性
   				外部类可以在该方法内创建局部类的实例对象
   				局部内部类可以定义在方法或代码块中
   				外部类只能访问本类中的局部类
   				
   				
   			匿名内部类（anonymous）
   				基于接口的匿名内部类
   					基本语法 
   						new 接口||方法(){ //接口不能生成实例对象，但在匿名内部类中可以创建一个匿名类
   							//接口||方法方法重写
   						} // 此时java底层会生成一个以该类名为开头的$1..2..3...n的类。该类只创建在底层，所以类中无法访问此匿名内部类
   					new xxx(){}实际上是在底层创建了一个以该类型开头+$1或2或3的一个对象($1,2表示创建的匿名内部类个数。如 在ATest中 执行new A(){重写方法}，是在底层创建了一个  ATest$1 extends A{重写方法}的内部类，该底层的类会写入传入的方法体，该类的实例对象会返回给new A()语句)，此时接收他的类型引用只能是它的父级(所以又有了一个多态性)，因为其外部类无法获得底层的原型类，通常匿名内部类用于重写方法(因为其不能向下转型，在使用时只有重写有意义)，该匿名内部类的运行类型是底层类，编译类型是父级引用，所以匿名内部类通常用来重写方法
   					
   					实际用法
   						作为方法参数传递（匿名内部类创建完毕后，会在改行返回一个该匿名内部类的实例对象），对比传统创建对象实例传递更加灵活
   					
   					注意
   						接口和抽象类不能创建实例对象，但可以创建匿名类
   						创建实例对象表达式为 new XX(); 创建匿名内部类表达式为 new XX(){};
   						底层类会接收new XX('xx'){//重写方法}传递给底层类的方法体，new XX('xx')携带的参数，如果是类，则回去调用对应的类构造器
   						当java识别到new XX(){}时会立刻在底层创建一个原型类，该类继承 XX（即该底层类继承了其传入的类）.该类的方法体为 new XX传入的方法体，同时会在 new XX(){}行返回一个该内部类的实例对象，该实例对象只有其父类可以引用(所以有了多态，继承，封装)，因为内部类无法使用底层类，此时该类的运行类型为底层原型类，编译类型为XX，所以匿名内部类专门解决方法重写问题
   						匿名内部类返回的实例对象可以选择接收(父类引用)，也可以不接收，接收时受多态限制，不接受则该实例对象的运行和编译类型都是该对象
   						如果new XXX(){};中的XXX是一个类，那么底层内部类则会继承它，如果是一个接口，那么该内部类就会实现它
   					
              	匿名内部类本质上是一个类
   	定义在外部类的成员上
   		分 成员内部类 ， 静态内部类
   			成员内部类
   				成员内部类定义在外部类的成员位置上，并且不能被static修饰，可以添加访问修饰符(本质上就是一个成员)
   				基本语法
   					class A{
   						public int a;
   						public class B{};
   					} 
   					当创建一个A的实例对象时，其class B{}也会被加载到该对象的地址上，这时我们可以在A类中的其他方法中直接使用 new B()创建(在A的实例对象调用该方法时，new B()创建的实例是以该实例对象为参照的)。如果外部其他类想调用该类的实例对象，需使用 new A().new B() 创建(new A()创建A的实例对象，然后执行.new B()对该实例对象上的类属性创建实例对象，同时需要使用 A.B xx来接收该对象实例)
   			
   			静态内部类
   				使用 static修饰的一个成员类 ，该类只能访问外部类的静态属性和方法
   				在生成该静态类实例对象时，需使用 new 原型类名.静态类(); 在接受该静态实例对象时，应该以 原型类名.静态类名 xxx 接收
   
   
   
   注意
   	类的五大成员为 属性，方法，构造器，代码块，内部类
   	xxx.getClass() 可以获得一个实例对象的运行类型
   	匿名内部类的出现就是为了简化开发
   	当外部类和局部内部类属性重名时，采用就近原则，如果在一个重名内部类中想访问外部类的属性，则要使用 外部类.this.xxx 其中外部类.this代表的是当前调用该类的实例实例对象 ，如果使用this表示当前内部类的实例对象
   	内部类可以访问外部类的所有属性和方法
   	局部内部类中，只是类不能加访问修饰符，因为该内部类是一个局部变量。但类里的内容可以加访问修饰符，因为它是局部类的成员
  */
  ```
  
   





## 1.6 断点调试

+ 断点调试可以快速查找出程序在那个模块出现了运行错误

+ 断点调试（debug）

  ```java
  /*
   断点调试指在程序的某一行设置一个断点，调式时，程序执行到改行会停止，然后即可一步一步往下调试，调式过程中可以查看各个变量的值，如果在这过程中出错，则会显示改错误
   
   设置断点：在选代码行的位置鼠标左键，此时会有一个红色小圆圈，再次点击取消断点
   
   断点调试快捷键
   	step over 逐条执行代码
   	Resume 逐条执行断点
   	step into && force step into 如果本行是方法，则进入方法体执行 。推荐使用step into 但需要配置 Settings->Build,Execution->Debugger-->stepping,取消掉java和javax的勾选项即可
   	F7跳入，逐条执行，若此行有方法则跳入到方法体内执行
   	Ctrl+F5 重写调式代码
   	Ctrl+F2 结束调式
   	F8 逐行执行代码
   	shitf+F8跳出，跳出方法
   	F9（resume） 执行到下一个断点（不进入到代码块，程序依然执行）&&开始debug调式
   	
   注意
   	在断点调试中程序处于运行状态，对象以运行类型来执行
   	调试中，可以帮助查看java的底层代码
   	想要进行debug调式，需要右键选择debug运行
   	debug在执行中也可以下断点
  */
  ```

  

## 1.7 枚举

+ 类实现枚举

  ```java
  /*
   类实现枚举
   	不提供setXX方法只提供getXX方法，因为枚举值通常是只读的
   	对枚举对象/属性使用 final + static 共同修饰，实现底层优化
   	枚举对象通常使用全部大写，即常量的命名规范
  
  public class Realize {
      public static void main(String[] args){
          System.out.println(Enumberate.getSPRING());
      }
  }
  
  class Enumberate{
      private String season;
      private String describe;
      private Enumberate(String season,String describe){
          this.season = season;
          this.describe = describe;
      }
      private final static Enumberate SPRING= new Enumberate("春天","香的");
      private final static Enumberate SUMMER= new Enumberate("夏天","热的");
      private final static Enumberate AUTUMN= new Enumberate("秋天","熟的");
      private final static Enumberate WINTER= new Enumberate("冬天","冷的");
  
  
      public static Enumberate getSPRING() {
          return SPRING;
      }
  
      public static Enumberate getSUMMER() {
          return SUMMER;
      }
  
      public static Enumberate getAUTUMN() {
          return AUTUMN;
      }
  
      public static Enumberate getWINTER() {
          return WINTER;
      }
  
      @Override
      public String toString() {
          return "Enumberate{" +
                  "season='" + season + '\'' +
                  ", describe='" + describe + '\'' +
                  '}';
      }
  }
  */
  ```

  

+ 枚举

  ```java
  /*
   枚举使用enum关键字替代了 class ,然后设置的所有常量要写在第一行，如果有多个常量使用逗号间隔
   
   枚举创建流程
   	枚举是一个类，可以拥有一些只读的常量属性（需要大写），外部使用 枚举类.常量即可使用，该常量必须写在第一行，多个常量用逗号隔开，最后的常量使用分号结尾，常量可以拥有属性，在定义时使用 XX(xx)即可调用对应的构造器绑定常量下的属性，也可以生成一个没有属性的常量，这时可省略实参列表使用XX创建即可（系统会去调用无参的构造器）
   
   枚举方法
   	xxx.name() // 即可获取属性名
   	xxx.ordinal() // 输出该枚举对象的下标
   	xxx.values() // 会取出该枚举类的所有常量，把他们存储到一个数组中
   	xxx.valueOf("str") // valueOf方法会把写入的str拿到当前枚举类中匹配，如果匹配到对应的常量则返回，没有匹配到则报错
      xxx.XXX.compareTo() // 比较两个枚举对象的下标,为0表示相等，负数表示小于，正数表示大于
   	
   枚举应用
   	public class Exercise {
      public static void main(String[] args){
          Day[] arr = Day.values();
          System.out.println("========所有星期如下");
          for(Day i : arr){
              System.out.println(i.describe);
          }
      }
  }
  
      enum Day{
          MONDAY("星期一"),TUESDAY("星期二"),WEDNESDAY("星期三"),THURSDAY("星期四"),
          FRIDAY("星期五"),SATURDAY("星期六"),SUNDAY("星期日");
          public String describe;
  
          private Day(String describe) {
              this.describe = describe;
          }
          Day() {}
  
      }
   	
   	
   注意
   	enum也是一个类，有自己的构造器和方法，只是所有常量要写在第一行，且用逗号间隔，最后一个常量用分号结尾	
   	enum关键字生成一个枚举类时（默认被final修饰），会继承Enum类
   	在生成有参枚举属性时，可以使用 XXX(xx)创建，在生成无参枚举属性时，可直接写为XXX , 在访问该属性时使用 枚举原型类.XXX访问即可
   	枚举常量不能被类访问修饰符修饰
   	enum不能继承任何类，但可以实现接口
   	enum类创建的常量会自动在它前面 隐式补齐 public static final 枚举类名 常量名 = new 枚举类名('实参列表')，所以我们只用写为 XXX('实参列表')即可，如果没有实参()构造器调用也可省略，直接写为 ，XXX，在使用时直接 枚举类名.常量即可，如常量下有属性，则可以使用枚举类名.常量.属性 即可获得该属性
  */
  ```
  
  

## 1.8 注解

+ 注解(annotation)

  ```java
  /*
   注解也被称为 元数据(Metadata),用于修饰解释 包，类，方法，属性，构造器，局部变量等
   注解和注释一样不影响程序逻辑，但注解可以编译和运行，相当于嵌入到代码里的内容
   
   基本annotation
   	@override 表示该类中重写了父类的方法  ， 若一个方法被@override修饰，则编译器会去检查是否真的重写了父类的方法，如果重写了即运行，没有重写则报错。    即使不写@override，java也会去派别是否重写代码，@override只做校验工作
   	@deprecated 表示某个类或方法已经过时 , 在版本升级时，可以做过渡使用
   	@suppressWarnings 抑制编码器警告 ，当我们不希望看到警告时，可以使用这个注解来抑制代码警告 。可以放在对象，方法前(用于限定抑制范围) ，如 @SuppressWarnings({"all"}) // 抑制所有警告，更多配置项，自行百度
   		@SuppressWarnings配置项
   			unchecked 忽略没有检查的警告
   			rawtypes 忽略没有指定泛型的警告
   			unused 忽略没有使用某个变量的警告
   			。。。。。
   	
   元注解
   	用于修饰其他的注解
   	元注解种类
   		Retention // 指注解作作用范围 
   			Retention有三种值，SOURCE(编译器使用后直接丢弃注解),CLASS(将注解写入Class文件中，运行时不会保留注解),RUNTIME(将注解写入Class文件中，运行时会保留注解,程序可以映射得到此注解)
   		Target // 指注解可以在那些地方使用
   		Documented // 指注解是否会在javadoc体现
   		Inherited // 子类继承父类的注解
   
   注意
   	修饰注解的注解为元注解，如 @Target
  */
  ```

  





## 1.9 异常

异常指在程序执行中发生的不正常的情况 

+ 异常处理机制

  ```java
  /*
  异常分为两大类 
  	Error(无法处理，比如说栈溢出)
  	Exception(可以处理，比如说指针访问到一个空的对象属性)
  		Exception又分为两大类
  			运行时异常(运行时出现的错误，比如 1/0)
  			编译时异常(编译器检查出的异常,查看使用的属性是否存在)
  
   使用 try{} catch(){} 语法 来处理可能出错的语句
   	如
   		 try {
              System.out.println(1/0);
          } catch (Exception e) {
              System.out.println("程序有误请检查");
          }
     
    常见运行时异常
    	空指针异常(NullPointerException)
    		当给一个字符串设置为null时，如果调用该字符串的方法就会抛出的异常
    		如
    			String a = null;
          	 System.out.println(a.length());
        
       数字运算异常(ArithmeticException)
       	当整数 / 0 时会抛出的异常
       
       数组索引越界异常(ArrayIndexOutOfBoundsException)
       	在访问数组下表时越界会抛出的异常
      
       类的类型转换异常(ClassCastException)
       	在多态中，一个父类引用了的子类实例对象，只能类型转换为该子类，如果转换为其他类则会抛出此错误
       
    	 数字格式不正确(NumberFormatException)
    		在将一个字符串转化为数字时，如果字符串不符合转换格式，则会抛出此错误
    		如
    			String n = "汉";
    			int a = Interger.parseInt(n);
    			
     常见编译异常（必须处理，否则无法编译）
     	  SQLException // 操作数据库时，查询表可能发生异常
     	  IOException // 操作文件时，发生的异常
     	  FileNotFundException   // 文件不存在
     	  ClassNotFoundException  //加载类，类不存在时
     	  EOFException  // 操作文件，到文件末尾，发生异常
     	  IllegalArgumentException  // 参数异常
  */
  ```

  ![image-20211103141440024](D:\typora_import_images\typora-user-images\image-20211103141440024.png)





+ 异常处理

  ```java
  /*
   异常处理方式
   	try-catch-finally,finally可写可不写按需求来,程序在发生异常时，自动捕获处理
   		try{
   			// 放入可能出现异常的代码
   		} catch(Exception e){
   			// 如果发生异常，try余下内容不再执行，立刻执行catch的内容
   			// e用来存储当前发生异常的类型
   		} finally {
   			// 无论是否发生异常都会执行
   			// 通常用来做首尾工作，如关闭网络连接，数据库连接等
   		}
   		
   	throws 将异常抛出，交给调用者来处理，最顶级的处理者就是JVM
   		throws写在方法体后（public void xx() throws Exception{}），表示如果出现异常，则抛出给调用者
   		在throws抛出异常时，可以指定抛出某个异常(如NullPointerException) 或直接抛出所有异常的父类(Exception)
   		在throws抛出异常时，可以同时抛出多个子类异常，如 throws FileNotFundException,NullPointerException,ArrayIndexOutOfBoundsException . 也可直接抛出父类异常Exception
   		注意
   			有编译异常必须处理，使用 try-catch处理或将异常继续抛出 
   			在方法重写中，如果父类抛出了一个异常，则子类必须抛出一个一致的异常或抛出一个父类型异常的子类型异常。如父类型抛出的异常为 Exception 则子类重写后可以为 Exception或其下的其余异常。如果父类抛出的异常为 ArithmeticException 则子类必须为 相同的异常 。 即子类重新方法抛出异常的范围要小于父类抛出异常的范围。通常抛出的异常都一致
   			在调用一个抛出异常的方法时，异常会返回给调用者，此时必须处理或抛出异常，否则会报错
   			抛出的编译异常必须处理(try-catch或throws抛出同类型异常)，抛出的运行异常可以选择处理(最终java会帮忙处理)
   			编译异常必须做处理(方法中抛出或使用try-catch)，运行异常不需要手动处理(出现异常java会自动抛出)
   			通常在出现异常时我们会去处理它，否则会一直抛出异常直到跑到jvm,由jvm处理(输出报错内容，终止程序)
   			
   	throw  在方法体内使用，用来抛出一个异常，语法 throw new RuntimeException("字符串"),(运行异常java会帮忙抛出)
   		
   		
   注意
   	当程序出现异常时，实际上会默认抛出该错误到jvm身上，然后jvm最终会输出此错误，并退出程序
   	如果发生异常，try余下内容不再执行，立刻执行catch的内容
   	try{} catch(){}中可以多个catch来对对应的异常做对应的处理，此时catch里的形参只能指定为对应的子异常，父异常必须写在最后 ，如 try{System.out.println(1/0)} catch(ArithmeticException e){} catch(NullPoniterException){} ... catch(Exception e){}
   	允许 try-catch ,try-catch-finally,try-finally 组合  ， 
   		try-catch组合在出现异常时会停止执行try的内容转到catch执行，catch执行完后，在执行剩余代码
   		try-catch-finally组合在出现异常时会停止执行try的内容转到catch执行，catch执行完后，再执行finally内容，在执行剩余代码 。 如果没有异常则会执行玩 try内容后执行catch，然后执行余下内容
   		try-finally组合在出现异常时 try内容停止执行，然后执行finally内容 ，最后 jvm抛出错误，程序停止
   	try-catch-finally和reutrn的交互，当try,catch,finally都有reuturn语句时，最终会返回finally下的那一个
  */
  ```

  ![image-20211103152057955](D:\typora_import_images\typora-user-images\image-20211103152057955.png)



+ exercise

  ```java
  /*
   判断用户是否出入正确的整数
  */
   public class Test2 {
      public static int getIntNum(){
         while(true){
             try{
                 Scanner scanner = new Scanner(System.in);
                 int num = scanner.nextInt();
                 return num;
             }catch(Exception e){
                  System.out.println("你输入错了，快点重新来");
             }
  
         }
      }
      public static void main(String[] args){
          System.out.println(getIntNum());
      }
  }
  ```

  



+ 自定义异常

  ```java
  /*
   自定义异常步骤
   	定义一个异常类，继承Exception或RuntimeException
   		注意
   			如果继承Exception，则属于编译异常
   			如果继承RuntimeException,属于运行异常(通常继承RuntimeException,这样会由java默认抛出)
   	
   	例
   		public class CustomException {
  
              public static void main(String[] args) {
                  int age = 30;
                  if (!(age >= 30 && age <= 60))throw new AgeException("年龄需要在30-60之间");
                  System.out.println("代码结束了");
              }
          }
  
          class AgeException extends RuntimeException {
              public AgeException(String message) {
                  super(message);
              }
          }
   		
  */
  ```

  

+ throws&&throw

  ![image-20211103190753150](D:\typora_import_images\typora-user-images\image-20211103190753150.png)

  



## 2.0 常用内置类

+ 更多方法查阅文档

### 包装类

+ 八种基本数据类型有响应的引用类型

  ![image-20211104124321653](D:\typora_import_images\typora-user-images\image-20211104124321653.png)

  引用关系![image-20211104124656373](D:\typora_import_images\typora-user-images\image-20211104124656373.png)

  



+ 包装类和基本数据类型转化

  ```java
  /*
  	jdk5前手动装箱和拆箱方法为：基本数据类型->包装类型，反之则为拆箱，jdk5以后实现了自动拆装箱
  	如
  		   int a = 1;   // jdk 5 需手动装箱
              Integer integer= new Integer(a); // 装箱
              int i = integer.intValue();  // 拆箱
              System.out.println(i);
              
              Integer i = 1;  // jdk 5 以后 自动装箱和揣想
              int a = i;
              System.out.println(a);
  	
  	注意
  		自动装箱底层调用的是valueOf()方法
  		其他包装类用法一致
  		对应的拆箱方法为 XXX.XXXValue(); 如 Double.doubleValue();
  		
  */
  ```

  

+ 包装类转String

  ```java
  /*
   包装类转String
   	使用 xxx+"" 方法  ,  如  1+"";
   	使用  toString() 方法  , 如 ,  Interger i= 1;  i.toString(); 
   	使用 valueOf() 方法  ，如 String.valueOf(1);
   	
   String转包装类
  	使用 Integer.parseInt("123");
  	使用 new integer("123");
  	
  	注意
  		其他字符串转其他类型同理
  */
  ```

  ![image-20211104132748504](D:\typora_import_images\typora-user-images\image-20211104132748504.png)

+ 同包装类的判断

  ```java
  /*
    1. 
    	Integer a = new Integer(1);
    	Integer b = new Integer(1);
    	System.out.pritnln(a==b);   // false , == 判断基本类型判断值是否相等，判断引用类型判断地址是否相等
    
    2. 
    	Integer a =1;
    	Integer b = 1;
    	System.out.pritnln(a==b);   // true ,Integer直接接收数值时，如果该值在 -128 - 127 之间，则会使用值接收，超出这个范围则会new 一个对象接收
    3. 
    	Integer a =128;
    	Integer b = 128;
    	System.out.pritnln(a==b);   // false,超出Integer直接接收范围，会在底层执行 new Integer(128);
  */
  ```

  



###  String

字符串为使用 ""号括起来的内容，它的存储方式采用 unicode存储(一个字符占两个字节)

+ 串行化：可以网络传输，可以文件写入

+ String结构为分析

  ```java
  /*
  	 String类实现了  Serializable,Comparable,CharSequence 接口 ， 继承了Object类
   	Serializable 有串行化功能，String实现了它，所以拥有了串行化（可以在网络传输）
   	Comparable 有不过i叫功能，String实现了它，所以拥有了比较（可以相互比较）
   	
   	注意
   		String类被final修饰(不能被继承),其下的final char value[]  用来存储字符串，且该字符串不能修改(地址不能修改，地址中单个字符内容可以变化)
   		final关键字修饰变量不能修改都是指的地址不能修改，如果final修饰一个数组，则数组里的内容可以修改，但数组的指向可以修改
  */
  ```

  

+ String创建方法

  ```java
  /*
  	String创建方式
  		方式一：直接赋值 String a = "123";
  		方式二：调用构造器 String s = new String("123");
  		注意
  			方式一查找规则：先从常量池查找"123"数据空间是否存在，如果有，直接指向，如果没有则先创建，然后指向。使用String a = "123"方式创建的字符串最终指向常量池
  			方式二查找规则：先在堆中创建空间，里面维护了value属性，指向常量池的"123"空间，如果常量池没有"123"，则重新创建并指向。如果有则直接指向
  			
  			
  	字符串构造器有很多，常用的构造器有
  		String()
  		String(String)
  		String(char[])
  		String(byte[],int ,int)
  		String(byte[])
  		
  */
  ```

![image-20211105132622756](D:\typora_import_images\typora-user-images\image-20211105132622756.png)

+ 字符串创建特性

  ```java
  /*
   字符串是不可变的，一个字符串对象一旦被分配，则其内容不能修改
  				如   String a = "he" ; String a = "she" , 运行流程：先去常量池中查看是否有值为he的String对象，如果有则返回常量池的该对象地址，没有则创建 he 的String对象，并返回地址。第二次又去常量池查找是否有 值为she的字符串对象，有则返回该对象，无则创建对象并返回其对象。最终常量池有 he和she两个字符串对象 . 即字符串在每次创建时都会先去常量池查找是否有相同值的字符串，如果有则返回常量池字符串地址，没有则创建一个该字符串的字符串地址，然后返回地址
  				
   当做字符串常量拼接时，如 "hello" +" world" , 字符串会先进行拼接，然后再去常量池中查找是否有匹配字符串，有则返回常量池的字符串地址，没有则创建一个字符串地址并返回
   
   当作字符串变量拼接时，如String a="he";String b="she",String c=a+b;实际上会先在创建一个字符串对象，然后用里面的方法对a和b做字符串拼接，然后在去访问方法区内是否有与之匹配的字符串，如果有则引用方法区里的地址，没有则创建一个字符串对象，并返回引用 。 当此字符串和其他相同值的字符串做==运算时为false,因为该方法是先创建一个字符串对象，再去方法区进行校验。 
   即 当在做字符串常量拼接时，是在常量池中进行(比较的是常量池的地址,常量池中如果有相同的字符串返回的都是同一个地址) 。 当在做字符串变量拼接时，是在堆中进行，堆中的属性value再去常量吃查找相同字符串(当一个栈中指向字符串和一个堆中指向字符串做 == 运算时，是比较常量池字符串地址和栈中地址是否相等。一个栈中字符串和一个栈中字符串 == 比较，比较的是常量池的String指向是否相等)
   
   
  */
  ```

  

+ String方法

  ```java
  /*
  	"str".intern()
  		把该字符串和常量池的字符串进行比较，如果有相同的字符串，则返回常量池中相同字符串的地址，否则把该字符串添加到常量池，并返回该String对象引用
  		
  	"str".equal("str") 
  		判断两个字符串的内容是否完全相等
  	
  	"str".equalIgnoreCase("str") 
  		判断两个字符串的内容是否完全,(忽略大小写)
  	
  	"str".length() 
  		获得字符串的长度
  		
  	"str".indexOf() 
  		获取字符在字符串中第1次出现的索引，索引从0开始，美债到返回-1
  	
  	"str".lastIndexOf()
  		获取字符在字符串中最后1次出现的索引，索引从0开始，美债到返回-1
  	
  	"str".substring(开始截取下标[,停止截取下标(停止下标不会被截取)]) ，
  		从n位置开始截取字符串
  	
  	"str".trim()
  		去掉整体字符串两边的空格
  	
  	"str".charAt(n)
  		获取某索引处的字符，注意：在java中字符串对象不能直接使用下标取值，要是用charAt方法取值
  	
  	"str".toUpperCase("str")
  		将字符串大写
  	
  	"str".toLowerCase("str")
  		将字符串小写
  	
  	"str".concat("str")
  		拼接字符串
  		
  	"str".replace("str1","str2") 
  		替换字符串中的字符,将所有的str1替换为str2 . 不改变原字符串，如果需要改变原字符串接收即可
  	
  	"str".split("str")
  		将指定的str改变为逗号，分割整个字符串变为一个数组
  		
  	"str".compareTo("str")
  		比较两个字符串大小,如果大于传入字符串则为正数，等于传入字符串则为0，小于则为负数
  		
  	"str".toCharArray()
  		把字符串转换为字符数组
  		
  	String.format("%d ,%c",a,b)  
  		和c语言的printf高度相似，左边为字符串%d,%f为变量转换符，右边为传入的变量 
  		如
              int age = 3;
              String name = "张三";
              System.out.println(String.format("我叫%s，我今年%d岁了",name,age));
      
      
  */
  ```
  
  
  
+ StringBuffer类

  ```java
  /*
  一.StringBuffer结构
   StringBuffer代表可变的字符序列，可以堆字符串内容进行增删
   	StringBuffer直接父类是 AbstractStringBuilder,AbstractStringBuilder实现了Serializable接口因此，Stringbuffer拥有了串行化
   	AbstractStringBuilder内的char value[] 没有被final修饰，因此存储在堆区
   	Stringbuffer被final修饰，因此不能被继承
  
  二.String Vs StringBuffer
   String vs StringBuffer
   	String保存的字符串是常量，里面的值不能修改，每次String类的更新实际上是改变地址指向
   	StringBuffer 保存的是字符串变量，里面的值在没有超出字符串长度的情况下可以改变，不用改变字符串地址，效率较高
   	
   三.StringBuffer构造器
  	new StringBuffer()  //  创建一个长度为16个字符的StringBuffer字符串
  	new StringBuffer(number)  // 创建StringBuffer字符串，该字符串大小为传入的数字
  	new StringBuffer("str")  // 创建StringBuffer字符串，该字符串大小为传入字符串长度+16的字符
  	
   四. StringBuffer和String的转换
   	String转StringBuffer
   		使用构造器传入该字符串
   			StringBuffer sf = new StringBuffer("we");
   		使用StringBuffer下的append方法
   			StringBuffer sf = new StringBuffer();
   			sf.append("edg");
   	StringBuffer转String
   		使用toString方法
   			StringBuffer sf = new StringBuffer("we");
   			String a = sf.toString();
   		使用String构造器
   			StringBuffer sf = new StringBuffer("we");
   			String a = new String(sf);
   			
   五. StringBuffer常用方法
   	append 追加字符串
   		sb.append(1).sb(true)  // 将1和true追加到该sb字符串中  
   	delete 删除字符串
   		sb.delete(3,5)  // 删除 sb中3-4下标，即[3,5)
   	replace 修改字符串
   		sb.replace(3,5,"我是字符串") 将sb中下标为3-4的字符替换为  "我是字符串"
   	indexOf 查找字符串
   		sb.indexOf("he") //查找sb中是否包含he有就返回第一出线的下标，没有返回-1
   	insert 插入字符串
   		sb.insert(2,"he") // 在下标为2的位置插入he，原下标的元素向后排列  
   	
  */
  ```

  ![image-20211105150601701](D:\typora_import_images\typora-user-images\image-20211105150601701.png)





+ StringBuilder类

  ```java
  /*
   StringBuilder是一个可变的字符序列，和StringBuffer拥有类似的api(同样存放在堆区)，但只用于单线程开发(速度更快)，多线程开发可能出现安全问题（没有使用互斥方法，可能出现线程安全问题）
  */
  ```

  ![image-20211105171851845](D:\typora_import_images\typora-user-images\image-20211105171851845.png)





+ StringBuilder，StringBuffer,String对比

  ```java
  /*
   StringBuilder和StringBuffer 极其相似，都标识可变序列，且方法一致
   String ，不可变字符序列，效率低，但复用率高
   StringBuffer,可变字符序列，效率较高，线程安全
   StringBuilder,可变字符序列，效率最高，线程不安全(单线程适用)
   
   注意
   	如果有大量的字符串修改，且不处于单线程中，建议适用StringBuffer,因为其不会创建大量字符串对象，只会在其堆中地址里扩容
  */
  ```

  

### Math

+ 常用方法

  ```java
  /*
   Math.abs(-9)  // 求-9的绝对值
   Math.pow(2,3)  // 求2得3次方
   Math.ceil(2.33) // 对2.33向上取整(往大的取整),为3
   Math.floor(-3.22) // 对-3.22向下取整(往小的取整)，为-4
   Math.round(2.32) // 对 2.32 进行四舍五入
   Math.sqrt(3) // 对 3进行开方
   Math.random() // 返回 0-1的随机数(不包含1)
   Math.max(2,1) // 求两个数的最大数
    Math.min(3,2) // 求两个数的最小数
  */ 
  ```

  

### Arrays

+ 常用方法

  ```java
  /*
   Arrays.toString(arr)  // 以字符串方式显示数组
   
   Arrays.sort(arr);  // 对数组进行排序，默认从小到大排序，改变原数组 
   	Arrays.sort()也可以从大到小排序  。需要使用以下方式
   	 Arrays.sort(a,new Comparator(){  // 第二个参数是一个匿名内部类，底层会创建一个该类并生成一个该类的实例对象返回到该位置
           @Override
           public int compare(Object o1, Object o2) {
               Integer i1 = (Integer)o1;
               Integer i2 = (Integer)o2;
               return i2-i1;  // sort底层是对i2-i1征服值做判断，如果是正就从小到大排序，如果是负值就从大到小排序
           }
       });
   
  Arrays.binarySearch(arr,3) // 在arr中查找是否有3这个字段(arr必须经过排序后的数组)，如果有返回下标，如果没有则返回一个负数
  
  Arrays.copeOf(arr,4) // 拷贝arr中的4个元素(按下标顺序拷贝),如果超出arr的范围则多拷贝的位置会置null值
  
  Arrays.fill(arr,9)  // 将arr里的所有元素替换为 9 
  
  Arrays.equals(arr,arr1) //  比较两个数组的值是否完全一致，如果一致则为ture,反之为false 
  
  Arrays.asList(1,2,3,4) // 将数组转换为list集合
  */
  ```

  

### System

+ 常用方法

  ```java
  /*
   System.exit(n) //退出当前程序,n为0表示正常退出，为-1表示出错 
   System.arraycope("原数组","开始拷贝下标","目标数组","从哪个数组开始拷贝","拷贝多少个") // 复制数组元素，一般是底层调用,如果超出拷贝范围则报错
   System.currentTimeMillens() // 返回1970-1-1 到当前时间的毫秒数
   System.gc() // 调用垃圾回收机制
  */
  ```

  

### Big

+ BigInteger和BigDecimal

  ```java
  /*
   BigInteger用来存储一个超大类型整数，(long存不下)
   	定义方式
   		BigInteger bi = new BigInteger("99999999999999999999999999999999");
   		该数不能直接修改，需使用其内部的方法实现 加减乘除
   		加减乘除方法
   			bigIntger.add(bigInteger) // 使用 实例对象.add(big)可以实现加法,被加数写在()内，且必须是bigInteger对象
   			bigIntger.substract(bigInteger) // 使用 实例对象.add(big)可以实现减法,被减数写在()内，且必须是bigInteger对象
   			bigIntger.multiply(bigInteger) // 使用 实例对象.add(big)可以实现乘法,被加乘写在()内，且必须是bigInteger对象
   			bigIntger.divide(bigInteger) // 使用 实例对象.add(big)可以实现除法,被除数写在()内，且必须是bigInteger对象
   	
   BigDecimal用来存储一个精度非常高的浮点数(不会缺省)
   	定义方式
   	BigDecimal bd =	new BigDecimal("199.2222222222222333333333311111");
   	该数不能直接修改，需使用其内部的方法实现 加减乘除
   	加减乘除方法
   			bigDecimal.add(bigDecimal) // 使用 实例对象.add(big)可以实现加法,被加数写在()内，且必须是bigDecimal对象
   			bigDecimal.substract(bigDecimal) // 使用 实例对象.add(big)可以实现减法,被减数写在()内，且必须是bigDecimal对象
   			bigDecimal.multiply(bigDecimal) // 使用 实例对象.add(big)可以实现乘法,被加乘写在()内，且必须是bigDecimal对象
   			bigDecimal.divide(bigDecimal) // 使用 实例对象.add(big)可以实现除法,被除数写在()内，且必须是bigDecimal对象 . 当除一个数后可能变为无限循环小数，这时程序会抛出异常，此时需要添加额外的实参指定精度，写为 bigDecimal.divide(bigDecimal,ROUND_CEINING) ， 此时如果除以一个无限循环小数java会保留当前设置的精度位(即初始小数位数 )
  
  */
  ```


###  Date

+ 基本使用

  ```java
  /*
  
  
  一. 第一代日期类
   得到当前中国时间
   使用   Date date =  new Date();   // 获取一个老外的日期格式，这时需要转换
   	SimpleDateFormat sdf = new SimpleDateFormat(yyyy年MM月dd日 hh:mm:ss E);// 规定一个日期格式
   	String ChineseDate = sdf.format(date); // 把老外的日期传入，经过加工就为中国日期格式了，格式为字符串，所以需要字符串接收
   	// sdf.parse(strDate) // 可以将字符串时间转换为老外时间，需要抛出异常 ， 字符串格式需和生成实例对象时传入的格式一致
   
   new Date() // 得到老外的当前时间
   new Date(123123) // 通过传入毫秒数，推测时间
   注意
   	Date是java的工具类，需要进行引入 java.util.Date,（java.sql.Date是数据库时间，不要引入错误）
   
  
  二. 第二代日期类
  	Calender是一个抽象类，并且构造器被私有化了(private修饰)
  	我们可以通过Calender.getInstance()来获取实例对象，这个实例对象下有一个get(Calendar字段)方法,方法中填入Calendar字段即可获取对应的时间
  	获取时间
  		Calendar c = Calendar.getInstance();
          c.get(Calendar.YEAR); // 获取年
          c.get(Calendar.MONTH)+1 ;// 获取月 ,获取月份+1为中国月份
          c.get(Calendar.DAY_OF_MONTH) ;// 获取日
          c.get(Calendar.HOUR); // 获取12进制的时间
          c.get(Calendar.HOUR_OF_DATE); // 获取24进制的时间
          c.get(Calendar.MINUTE); // 获取分
          c.get(Calendar.SECOND) ;// 获取秒
          
  三. 第三代日期类(常用)
  	jdk8加入的第三代日期类
  	有三个类 
  		LocalDate(得到日期，即年月日),LocalTime(得到时间，即时分秒)，LocalDateTime(得到日期时分秒)
  	常用方法
  		LocalDateTime ldt =  LocalDateTime.now(); //返回一个实例对象
          System.out.println(ldt.getYear()); // 得到年
          System.out.println(ldt.getMonth()); // 得到英文月
          System.out.println(ldt.getMonthValue()); // 得到月
          System.out.println(ldt.getDayOfMonth()); // 得到日
          System.out.println(ldt.getHour()); // 得到时
          System.out.println(ldt.getMinute()); // 得到分
          System.out.println(ldt.getSecond()); // 得到秒
          
      日期格式化类
      DateTimeFormatter dtf =	DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");  // 指定日期输出格式
      String format = dtf.format(ldt);  // 即可按设定格式输出
       
       时间戳
       	Instant now = Instant.now(); // 获取当前时间戳
       	Date date = Date.from(now); // 将时间戳转换为Date对象
       	Instant instant = date.toInstant(); // 将Date对象转换为时间戳
       
       常用方法
  		提供了 plus和minus方法可以对当前时间进行加或减操作
  		LocalDateTime ldt = LocalDateTime.now();
  		LocalDateTime targetIdt=ldt.plusDays(20) // 对当前时间增加20天
  		LocalDateTime targetIdt=ldt.minusMinutes(20) // 对当前时间减20分钟
  */
  ```

  SimpleDateFormat可填入字符串![image-20211107103409807](D:\typora_import_images\typora-user-images\image-20211107103409807.png)

  ![image-20211107111855035](D:\typora_import_images\typora-user-images\image-20211107111855035.png)

  
  
  

###  功能类

+ UUID

  ```java
  /*
   UUID.randomUUID() // 可以获得一个随机不重复的UUID
   	使用 UUID.randomUUID().toString();
  */
  ```

  

## 2.1 集合

+ 在使用迭代器操作迭代器里的数据时可能出现报错，此时使用普通for循环即可解决

+ 基本组成

  ```java
  /*
   集合对标数组扩容复杂(数组扩容需创建一个新数组，长度是原数组+1然后克隆原数组，并加上改变值，最后让原来的变量去指向新数组)而出现
   集合可以动态的保存任意种变量元素，并提供了一系列操作方法，如add,remove,set,get
   
   注意
   	集合分单列集合，和双列集合。单列集合由每个数据组成，双列集合由KEY-VALUE键值对组成
  */
  ```

  map和iterable

  ![image-20211108095302350](D:\typora_import_images\typora-user-images\image-20211108095302350.png)

  ![image-20211108095328275](D:\typora_import_images\typora-user-images\image-20211108095328275.png)

  

+ Collection接口

  ```java
  /*
   collection接口特点
   	collection实现子类可以存放多个元素，每个元素是Object
  	有些collection的实现类可以存放重复元素，有些则不行
  	有些collection的实现类是有序的(List)，有些是无序的(Set)
  	Collection接口没有直接的实现子类，是通过它的子接口Set和List实现的
  
   Collection接口方法
   	add 添加一个元素，add(Object) 只要是object的子类都可以放置，装载的是一个个对象
   	remove 可以指定删除元素或者索引,remove(Object)||remove(index)
   	contains 可以检查集合中是否包含某个元素，contains("jack") 
   	size 可以返回集合拥有的元素个数个数 ，size()
   	isEmpty 可以验证一个集合是否为空，isEmpty() 
   	clear 可以清空一个集合的全部内容，clear()
   	addAll 可以一次添加多个元素,addAll(集合)
   	containsAll 可以查找多个元素是否在集合中存在 containsAll(集合)
   	removeAll 可以一次删除多个元素 ， removeAll(集合)
   		注意
   			添加元素的时候只能添加对象，如果传入 1，true等，java会自动把他们转化为包装类
   
   Collection遍历元素(iterable下声明了一个iterator方法)
    iterator对象被称为迭代器，主要用于遍历集合中的元素
    所有实现了Collection接口的集合类都有(实现了)iterator()方法
    	迭代器原理
    	  挨个遍历集合中的元素，在遍历前会判断下一个元素是否存在(hasNext()),如果存在就使用next()方法指针后移，并取出数据，再次进行元素判断，直到所有元素都读取完毕
    	  基本用法
    	    Iterator iterator = col.iterator();
    	  	while(iterator.hasNext()){
    	  		System.out.println(iterator.next())
    	  	}
    	  注意
    	  	在使用next()指向下一个集合元素时，必须先使用hasNext()做判断，判断下一个集合元素是否存在(存在为true,不存在为false) , 如果没有配合使用则会抛出一个异常(NoSuchElementException)
    	  	在迭代完成一次后，迭代指针已被移动到集合末尾处，如需在次遍历集合，需重置迭代指针位置，使用iterator = col.iterator() 重置迭代指针
    	  	获取迭代器的方法： Iterator it = 集合.iterator();(该方法放回一个迭代器实例都西昂)此时就获得一个迭代器，可以使用next(),hasNext()遍历数据
    	  	增强版for循环也和迭代器具有相同功能(底层for :调用的就是迭代器)， 语法：for(元素类型 元素名:集合名或数组)
    	  	在方法块中使用 I + tab 即可快速生成一个增强版for循环
    	  	
   	注意
   		实现collection接口的普通类（List和Set都能使用），使用接口方法的用法都一致
   		以上方法都是集合类实现接口下的方法
   		在IDEA中可以使用 itit 快捷键快速生成一个迭代器
  */
  ```

  

+ list接口

  ```java
  /*
   List集合类中元素是有序的，且可以重复，可以使用索引取出元素(使用get(index)方法)
   
   List常用方法
   	add(int index,Object ele) // 在指定位置插入一个实例对象
   	addAll(int index,Collection ele) // 在指定位置插入一个集合，该集合会自动展开
   	get(int index) // 获得指定索引的元素
   	indexOf(Object obj) // 返回obj在列表中第一次出现的索引
   	listIndexOf(Object obj) // 返回obj在列表中最后一次出现的索引
   	remove(int index) // 移出指定索引元素
   	set(int index,Object ele) // 给指定索引(索引必须存在)设置一个元素，相等于替换
   	subList(int fromIndex,int toIndex) // 返回从fromIndex到toIndex索引的元素并组成一个集合，不包含toIndex
   
   注意
   	jdk api中实现list接口的类有很多(如 AbstractList,Stack)，最常用的为 Vector,ArrayList,LinkedList
   	遍历的三种方式，迭代器，增强for，for
  */
  ```

  

+ ArrayList

  ```java
  /*
   Arraylist实现了List和Collection接口，因此可以被List和Collection声明的变量所引用(遵守多态性)
   
   ArrayList特性
   	当使用无参构造器创建一个ArrayList对象时，默认会在底层创建了一个Object类型的数组，该数组大小为0，在传入一个数据后，如果数组大小不足以装下该值，则数组会以该接收数据大小*1.5方式扩容
   	当使用有参构造器创建一个ArrayList对象时，默认会在底层创建了一个Object类型的数组，该数组大小为给定大小，当该数组即将占满时，该数组会以给定大小的1.5倍扩容数组
   
   p511-512 Array底层分析
   
   注意
   	ArrayList可以放所有类型的元素，甚至是null
   	ArrayList线程不安全，因此只能用于单线程开发
   	ArrayList可以存放相同的元素
   	要经常去看源码
   	被 transient修饰的属性不会被序列化
  */
  ```

  

+ Vector

  ```java
  /*
   Vector底层也是一个对象数组(线程安全，拥有synchronized方法(可以实现线程同步和互斥))
   
   Vector VS ArrayList
   	ArrayList
   		可变数据，线程不安全但效率高，如果有指定参数大小，每次满了按参数的1.5倍扩容，如果没有指定参数大小，第一次为10，如果满了按当前大小的1.5倍扩容
   	Vector 
   		可变数组，线程安全但效率不高，如果有指定参数大小，每次满了按参数的2倍扩容，如果没有指定参数大小，第一次为10，如果满了按当前大小的2倍扩容
  */
  ```

+ LinkedList

  ```java
  /*
   LinkedList底层实现了双向链表和双端队列 ，可以添加任意元素(线程不安全，没有实现同步)
   Linked中维护了两个属性first和last他们分别指向首节点和尾节点，每个节点中(Node对象)里有维护了    prev,next,item三个属性，其中prev指向前一个Node对象，next指向下一个Node对象实现双向链表
   
   LinkedList VS ArrayList
   	ArrayList 可变数组，增删效率较低，改查效率较高
   	LinkedList 双向链表，增删效率较高，改查效率较低
   
  */
  ```

  

+ Set

  ```java
  /*
   set是无序的，没有索引，不允许重复元素(每个元素只能存放一个，对比的是该元素的地址，如果地址一致则不能存入，反之则可以存入)，所以最多包含一个null
   set的实现子类有 TreeSet,HashSet，AbstractSet等
   set在遍历时只能使用迭代器或增强for循环，因为它是无序的
   获得数值的方式只有 迭代器和增强for循环
  */
  ```

  

+ HashSet(HashSet的底层就是HashMap)

  ````java
  /*
  52-525源码精看
  
   HashSet实现了set接口，HashSet底层实际上是HashMap(HashMap底层是数组+单向链表+红黑树)存储调用putVal方法
   
   注意
   	HashSet可以存放null值，但只能有一个
   	set执行add方法后会返回一个boolean类型数据，如果添加成功会返回true，失败会返回false
   	HashSet底层是HashMap,在添加一个元素时，会先去调用hashCode方法得到hash值(得到一个被算法处理过的hash值)，通过hash值转换为一个索引值并存储 ， 如果多个hash值相同时，java则会调用equal方法比较值(程序员可自行重写equal方法)，如果不同则会以链表的方式存储在该索引位置的上一个元素后，如果相同则会放弃存储，会采取索引接链表方式存储数据，如果链表存储了足够多的数据时会树化
   	如果一个HashSet在一个索引上有超过8个元素，且tables表格的大小达到了64则该索引下的链表会进行树化(红黑树)，如果没有达到64则每在一个超过8个元素的索引下添加一个equal不同的值时，该tables就会扩容一次(2倍扩容)
   	hashCode本质上是以地址推算来的，因此在set存储对象时，会出现相同参数也可以存储的问题，这时我们要重写 hashCode和equal方法来帮助我们完成set对象去重，(可以使用idea准备好的模板，ctrl+j)
   	HashSet初始空间为16且到了临界值(当前空间大小*0.75)就会以2倍的方式扩容
  	每次添加一次元素都算做一次空间占用(无论是不是再一个索引上或是否添加成功),当添加的次数达到临界值时，tables便会2倍扩容
  */
  ````
  
  ![image-20211111202856541](D:\typora_import_images\typora-user-images\image-20211111202856541.png)
  





+ LinkedHashSet

  ```java
  /*
   LinkedHashSet是HashSet的子类，底层是一个LinkedHashMap维护了一个数组+双向链表(相互指向)+红黑树，存储调用的是putVal方法
   注意
   	LinkedHashSet根据元素的hashCode值决定存储位置，同时使用链表维护元素的次序，使元素看起来像是以插入顺序保存的
   	LinkedHashSet不允许添加重复元素
   	LinkedHashSet和HashMap的唯一区别是，LinkedHashSet为双向链表存储，HashMap为单项链表存储，两者的存储策略一致(调用的相同的方法)
  */
  ```

  ![image-20220101143711005](D:\typora_import_images\typora-user-images\image-20220101143711005.png)

  
  
+ TreeSet

  ```java
  /*
   treeSet以无参创建时，默认以第一位字符上的ASCII码进行排序，想要其按指定方式有序需要传入一个实现了Comparator(){}接口的匿名内部类，底层会调用该匿名内部类实例对象的方法进行比较，进而达到有序
   注意
   	再底层进行比较时，如果有两个或多个具有相等(相同)排序特征的值，则该值不会被成功添加到TreeSet中 。 如当设置比较字符时，如果两个字符串相等，则放弃添加。再比较长度大小时，如果长度相等则不已添加
   	TreeSet的底层就是TreeMap
   使用
   	        Set set = new TreeSet(new Comparator() {
                  @Override
                  public int compare(Object o1, Object o2) {
                      return ((String)o1).length()-((String) o2).length();
                  }
          	}
          );
  
  */
  ```

  





+ map接口

  ![image-20220102193706994](D:\typora_import_images\typora-user-images\image-20220102193706994.png)

  ```java
  /*
   map和set的一个底层结构有些类似，同样是存放一组K-V但，但set中只使用K值，V值用 PRESENT(Object对象)做了占位，map则K-V都进行了使用
   
   注意
   	K-V被称为双列元素
   	Map中的key和value可以是任何引用类型数据，会封装到HashMap$Node对象中
   	Map中的key不允许重复，value允许重复，如果添加了两个相同的key值，则后一个key会覆盖掉前一个key
   	Map中的key可以为null,value页可以为null,但key为null的个数只能有一个(相同的key会被覆盖)
   	所有类型都可以当作key值，但最常用的key类型为字符串
   	Node存储的所有k-v被Entry集合维护(引用k-v地址)，且提供了 getKey() getValue()方法获得对应的值
  */
  ```

+ map接口常用方法

  ```java
  /*
   put(k,v) 添加
   remove(k) 根据键删除映射关系
   get(k) 根据键获得值
   size() 获得元素个数
   isEmpty() 判断个数是否为0
   clear() 清除全部数据
   containsKey(k) 查找键是否存在
   keySet() 拿到所有的key组成一个数组
   values() 获得所有的value值，组成一个数组
   entrySet() 获得该map对象下的所有k-v , entrySet集合下有 getKey(),getValue()获得相印的值
  */
  ```

+ map接口遍历方法

  ```java
  /*
   1. 使用增强for循环
   	 for (Object o : hashmap.keySet()) { // o拿到每个key，再通过get方法得到对应key的value
            System.out.println(o+"_"+hashmap.get(o));
         }
   
   2. 迭代器
   	Iterator iterator = hashmap.keySet().iterator();
          while(iterator.hasNext()){
              Object key = iterator.next();
              System.out.println(key+"_"+hashmap.get(key));
          }
          
  3. entrySet()迭代取值
  	   Set entrySet = hashmap.entrySet(); //获得该集合下的所有k-v组成一个数组
          for (Object o : entrySet) { 
              Map.Entry e = (Map.Entry) o; // 此方法通过Entry下的getKey()和getValue独有方法获得值，所以需要向下转型
              System.out.println(e.getKey()+"_"+e.getValue());
          }
  */
  ```

  

+ HashMap

  ```java
  /*
  HashMap底层是数组+链表+红黑树，调用的是putVal方法() , HashSet中已经分析过了(HashSet底层就是调用的HashMap)
   总结
   	Map接口的常用实现类为:HashMap,Hashtable和Properties
   	HashMap是Map接口使用频率最高的实现类
   	HashMap是以k-v的方式存储的
   	HashMap没有实现同步，因此线程不安全
  */
  ```

  ![image-20220102143810288](D:\typora_import_images\typora-user-images\image-20220102143810288.png)



+ ConcurrentHashmap

  ```java
  /*
   和hashmap功能一致，但处理了线程安全
  */
  ```

  





+ Hashtable

  ```java
  /*
   Hashtable特点
   	存放的元素为键值对(K-V)
   	hashtable的键和值不允许为null
   	Hashtable的方法和Hashmap基本一致
   	HashTable线程安全，HashMap线程不安全
   	
   Hashtable底层
   	有一个 Hashtable$Entry[] 数组，初始大小为11
   	临界值 threshold 为 当前大小 * 0.75
   	扩容 ，按照当前大小两倍扩容(*2+1)
   	
   注意
   	Hashtable和Hashmap的底层结构基本一致(存储方式)，但一些参数有点差异(扩容方式，初始大小)，
   	Hashtable线程安全，Hashmap线程不安全
   	Hashtable效率低，Hashmap效率高
   	Hashtable不可以为null，Hashmap可以为null
  */
  ```
  
  

+ Properties

  ```java
  /*
   Properties类继承Hashtable类实现了Map接口，也是一种键值对的形式保存数据，使用特点和Hashtable类似，
   Properties可以从其他文件(包名 xxx.properties)加载数据到当前Properties对象
   注意
   	xxx.properties文件通常为配置文件
   	properties k-v都不能为空
  */
  ```



+ TreeMap

  ```java
  /*
  TreeMap以无参创建时，默认以第一位字符上的ASCII码进行排序(如果相同则顺延比较)，想要其按指定方式有序需要传入一个实现了Comparator(){}接口的匿名内部类，底层会调用该匿名内部类实例对象的方法进行比较，进而达到有序
   注意
   	再底层进行比较时，如果有两个或多个具有相等(相同)排序特征的值，则该值不会被成功添加到TreeMap中 。 如当设置比较字符时，如果两个字符串相等，则放弃添加。再比较长度大小时，如果长度相等则不已添加
   使用
   	       Map map = new TreeMap(new Comparator() {
                  @Override
                  public int compare(Object o1, Object o2) {
                      return ((String)o1).length()-((String) o2).length();
                  }
          	}
          );
  
  */
  ```

  



![image-20220102195431614](D:\typora_import_images\typora-user-images\image-20220102195431614.png)



+ Collections工具类

  ```java
  /*
   Collections是一个操作Set,List，Map的集合工具类,提供了一系列方法来对集合数据进行处理
   Collections.reverse(List); // 反转List中元素的位置
   Collections.shuffle(List); // 重新随机排列集合中的数据
   Collections.sort(List); // 按字母ASCII进行排序，也可自己传入一个匿名内部类重写Comparator方法。如果传入一个集合，没有传入排序规则，则该集合必须实现Comparable接口，且实现compareTo(),Collections会去调用传入集合的compareTo方法
   Collections.swap(List,int,int); // 指定List集合中的i位置元素和i位置元素进行交换
   Collections.max(List); // 按字母ASCII大小选出最大的元素
   Collections.max(List,new Comparator(){public int compare(){}}); // 按指定方式选出最大的元素
   Collections.min(List); // 按字母ASCII大小选出最小的元素
   Collections.min(List,new Comparator(){public int compare(){}}); // 按指定方式选出最小的元素
   Collections.frequency(List,Object);  // 查看List中 Object出现的次数
   Collections.copy("拷贝数据存储集合","被拷贝集合"); //  "拷贝数据存储集合"长度要大于"被拷贝集合"才能拷贝成功，否则会报错 
   Collections.replaceAll(List,"tom","jimmy"); // 将list表中所有的tom替换为jimmy
  */
  ```
  
  



## 2.2 泛型

+ 泛型(generic)使用场景：**一个相同逻辑代码，不同返回类型，这时就可以使用泛型**

+ 泛型分为 **泛型类，泛型接口，泛型集合，继承接口泛型，继承类泛型，泛型方法**

  泛型类，当一个泛型类要生成实例对象时，先将其泛型替换为指定的引用类型(不影响该泛型类生成其他实例对象)，在将其加载到该实例对象地址中

  泛型接口，当一个类要实现了泛型接口时，先将其泛型替换为指定的引用类型((不影响其他类实现该泛型接口)，在进行方法重写

  泛型集合，通过指定集合的数据类型，来限制输入(不影响其他集合)

  继承接口泛型，当一个接口要继承了某泛型接口时，先将其泛型替换为指定的引用类型((不影响其他接口继承该接口)，在进行继承

  继承泛型类，当一个类继承了某泛型类时，先将其泛型替换为指定的引用类型((不影响其他类继承该泛型类)，在进行继承

  泛型方法的数据类型由传入的形参决定

  ```java
  /*
   1. 泛型类
      public void demo(){
          class A<T>{
              T value;
              public A(T value) {
                  this.value = value;
                  System.out.println(value.getClass());
              }
          }
          // 泛型类
          A<String> stringA = new A<>("我1"); // 生成了该泛型类的实例对象，并指定其泛型类型为String类型，不影响该泛型类生成其他实例对象
          A<Integer> ee = new A<>(333);  // 生成了该泛型类的实例对象，并指定其泛型类型为Integer类型，不影响该泛型类生成其他实例对象
          A<Boolean> fe = new A<>(false);// 生成了该泛型类的实例对象，并指定其泛型类型为Boolean类型，不影响该泛型类生成其他实例对象
      }
      
  2. 泛型接口
  interface AAA<T>{
      T finish();
  }
  class BB implements AAA<String>{
   // 实现了该泛型接口并指定其泛型为String类型，不影响其他类实现该泛型
      @Override
      public String finish() {
          return null;
      }
  }
  class CC implements  AAA<Integer>{
   // 实现了该泛型接口并指定其泛型为Integer类型，不影响其他类实现该泛型
      @Override
      public Integer finish() {
          return null;
      }
  }
  
  3. 泛型集合
  	 public void demo2(){
          HashMap<String, String> hashMap = new HashMap<>(); // 限制该接口的key,value为String类型
          ArrayList<String> strings = new ArrayList<>(); // 
          ArrayList<Integer> in = new ArrayList<>();
      }
  
  4. 继承接口泛型
  
  interface AAA<T>{
      T finish();
  }
  interface BBB extends AAA<String>{ 
  // 该接口继承了泛型接口，并指定其泛型为String类型，不影响其他接口继承该泛型接口
      String f1();
  }
  interface CCC extends AAA<Integer>{
  // 该接口继承了泛型接口，并指定其泛型为Integer类型，不影响其他接口继承该泛型接口
      Double f2();
  }
  
  5. 继承泛型类
   public void demo3(){
          class One<T>{
              T value;
          }
          class OneP extends One<Integer>{
             // 该类继承了一个泛型类，并指定其泛型为Integer类型，不影响其他类继承该泛型类
          }
          class OneX extends One<String>{
               // 该类继承了一个泛型类，并指定其泛型为String类型，不影响其他类继承该泛型类
          } 
      }
  */
  ```

  



![image-20220131163300328](D:\typora_import_images\typora-user-images\image-20220131163300328.png)



+ 泛型优点

  ```java
  /*
   1，类型安全。 泛型的主要目标是提高 Java 程序的类型安全。通过知道使用泛型定义的变量的类型限制，编译器可以在一个高得多的程度上验证类型假设。没有泛型，这些假设就只存在于程序员的头脑中（或者如果幸运的话，还存在于代码注释中）。 
  
  2，消除强制类型转换。 泛型的一个附带好处是，消除源代码中的许多强制类型转换。这使得代码更加可读，并且减少了出错机会。
  
  3，潜在的性能收益。 泛型为较大的优化带来可能。在泛型的初始实现中，编译器将强制类型转换（没有泛型的话，程序员会指定这些强制类型转换）插入生成的字节码中。
  */
  ```

  

+ 泛型的本质

  **泛型可以接收一个引用数据类型，当其被一个实例对象(集合，继承接口，接口，继承类)指定了确定的数据类型后，它在(相对于)该实例对象(集合，继承接口，接口，继承类)地址中的数据类型就是传入的数据类型，如果没有传入类型则默认为Object**，

  ```java
  /*
   interface IA<T>{
      T str();
  }
  // 继承泛型接口
  interface  IB extends  IA<String>{
   // 此泛型接口被IB接口继承，并指定其为String, 当其他类实现这个继承接口时，也会以String类型实现该方法
  }
  
  public class Test {
  
      public static void main(String[] args) {
          //1. 泛型常见6类 泛型类  泛型集合  泛型接口 继承泛型类 继承泛型接口 泛型方法
          //2. 被泛型修饰的变量不能进行直接初始化，也不能被static修饰(不知道泛型的类型,只有在被传入引用类型时才知道类型)
          //3. * 在需要指定数据类型的泛型上，如果指定则再此地址中该泛型的类型就是你指定的类型，如果
          //没有指定再此地址中该泛型的数据类型jvm会自动填充Object，如 new HashMap(); 没有指定默认为Object,Object
          //4. 相同的泛型数据在同一个地址只能被指定一次
  
   
          // 泛型类
          InheritClass<String> str = new InheritClass<>(); // 此时泛型类中的泛型在此地址中为String类型，并生成了实例话对象
  
          // 泛型集合
          HashMap<String, String> hashMap = new HashMap<>(); // 该泛型集合在此集合中为String(key),String(value)
  
          // 泛型方法
          // 它的类型由调用此方法传入的参数确定，此方方法中我们传入的是 String,String ,所以此次调用该泛型的类型就是String,String
          // 如果我们再次调用传入 Integer,String,则他的类型就是Integer,String
          new MethodGeneric<String>().runMethod("key","value"); // String String
          new MethodGeneric<String>().runMethod(333,"value"); // Integer String
  
      }
  
  
  }
  
  // 继承泛型类
  class MethodGeneric<T> extends InheritClass<MethodGeneric>  {
      // 该泛型类再此类中的类型为 MethodGeneric 类型，在实例此类时，该泛型也会以MethodGeneric方法被实例化
      public T name;
  
  
  
      public <K, V> void runMethod(K key, V value) {
          System.out.println(key.getClass());
          System.out.println(value.getClass());
      }
  }
  // 泛型接口
  class InheritClass<T>implements IA<String>{
      // 该泛型接口在此类中为String类型，实现的是String类型的方法，在被生成实例对象时，也会以此类型被载入该空间中
      public T age;
      @Override
      public String str() {
          return null;
      }
  }
  */
  ```
  
  
  
  
  
+ 为什么要有泛型

  ```java
  /*
   不用泛型时，不能对加入集合的数据的类型进行约束,导致所有数据都可以添加，再做遍历向下转型时可能造成转换错误。且遍历时需要向下转型导致效率降低
  */
  ```

+ 泛型的好处

  ```java
  /*
   再集合中添加泛型，可以对添加的数据进行约束，再传入了不满足约束要求 的对象时，编译器会进行报错，提高了程序的健壮性
   再进行迭代器遍历时，可以直接指定传入泛型的类型(没有泛型时只能指定Object，再调用方法时可能需要向下转型导致效率下降)
  */
  ```

+ 泛型的定义

  ```java
  /*
   泛型对象
      public class Generic_ {
          public static void main(String[] args) {
              System.out.println(new Person<String>("张三"));
          }
  
          static class Person<E> {
              public E name;
              public Person(E name) {
                  this.name = name;
              }
              public E r(){
                  return this.name;
              }
              @Override
              public String toString() {
                  return "Person{" +
                          "name=" + name +
                          '}';
              }
          }
      }
  
  泛型集合
  	Set<Student> set = new HashSet<>();
       set.add(new Student<String,Integer>("张三",16));
       set.add(new Student<String,Integer>("张三",16));
       set.add(new Student<String,Integer>("张三",16));
       Map<String,Student> map = new HashMap<>();
       map.put("学生一",new Student<String,Integer>("张星彩",18));
       map.put("学生二",new Student<String,Integer>("张春花",22));
       map.put("学生三",new Student<String,Integer>("张非",28));                         
  
  泛型接口(泛型接口需要在该接口被继承或被是实现时传入)
  	    interface IW<T,E>{
          void take(T e,T w);
          E play(E pl, T qq);
      }
      static class A implements IW<String,Integer>{
          @Override
          public void take(String e, String w) {
              
          }
          @Override
          public Integer play(Integer pl, String qq) {
              return null;
          }
      }
    当一个接口继承了泛型接口时，需要传入对应个数的引用数据类型，在一个类实现该继承接口时，就无需传入引用数据类型，因为其已经传入
    interface II extends IW<String,String>{
          
      }
      static class B implements  II {
          @Override
          public void take(String e, String w) {
              
          }
  
          @Override
          public String play(String pl, String qq) {
              return null;
          }
      }
      注意
      	如果还没想好传递什么数据类型，可以写为static class A implements IW<Object,Object>
      
  泛型方法
  	方法中的泛型是再传入数据时确定的，该泛型由java自动完成判断，再传入的为int类型，则java会自动判断并把其类型包装为Integer
  	public static void main(String[] args) {
          new Car().run("ee",3);
      }
      static class Car {
          public<T,R> void run(T name,R age){
              System.out.println(name.getClass()); // String
              System.out.println(age.getClass()); // Integer
          }
      }
      注意
      	如果没有再该方法前添加<T,R>，就使用 T name,R age修饰，会被Java判断为在使用类的泛型
  		在泛型方法中，泛型可以使用类声明的泛型，也可以使用自己声明的泛型，如果两个泛型重名，则优先使用自己的泛型
  	
   注意
  	泛型接收的是一个数据类型，当指定后，只能接收该类型或该类型的子类型的实例对象
  	泛型通过用 E,T,K,V等单个字符表示
  	泛型可以接收一个引用数据类型，当其被一个类(集合，继承接口，接口，继承类)指定了确定的数据类型后，它相对于该类(集合，继承接口，接口，继承类)的数据类型就是传入的数据类型，且该数据类型和传统的数据类型使用方法一致(作为变量修饰符，作为方法返回值，作为形参修饰符)
  	泛型的本质是传递给一个集合或一个类一个或多个数据类型，该类或该集合可以使用这些数据类型(泛型)，这时再传入参数时，我们只能传入约定好的数据类型的数据
  	泛型再接收基本数据类型时，只能接收他们的包装类型，如 int 要接收为 Integer
  	被泛型修饰的数组变量不能进行直接初始化(不知道泛型的类型)
  	被static修饰的表达式不能为泛型类型 (static修饰的表达式在类加载时就会执行，此时泛型还未接收数据类型)
  	JDK在编译之后程序会采取去泛型化的措施。也就是说Java中的泛型，只在编译阶段有效。在编译过程中，正确检验泛型结果后，会将泛型的相关信息擦出，并且在对象进入和离开方法的边界处添加类型检查和类型转换的方法。也就是说，泛型信息不会进入到运行时阶段。
  */
  ```

  

+ 泛型使用细节(注意点)

  ```java
  /*
   泛型只能接收引用数据类型
   再给泛型指定具体类型后，可以传入该类型或其子类类型
   在实际开发中，往往会在对泛型进行简写，只在其数据类型旁指定泛型，java会自动获取
   	如 List<String> = new ArrayList<>();
   如果有些数据类型需要接收泛型，但程序员没有指定，则java会自动填充Object
   定义了泛型的接口，方法，类或集合中，再每次使用到该泛型时都可以传入对应个数的引用数据类型，此时这些泛型表示的就是你传入的引用数据类型，如果你没有传入东西，则这些泛型默认为Object
  */
  ```

+ 泛型的继承和通配符

  ```java
  /*
   泛型不具备继承性 ，即 不能写为 List<Object> list = new ArrayList<String>();
   泛型的继承和通配符书写在方法的形参上
   数据类型<?> 表示支持接收该数据类型的任意泛型类型
   数据类型<? extends A> 表示只能接收该数据类型，且该数据类型的泛型类型是的A类或A类的子类
   数据类型<? super A> 表示只能接收该数据类型，且该数据类型的泛型类型是的A类或A类的父类
   
   使用
   public static void main(String[] args) {
          ArrayList<Object> objects1 = new ArrayList<>();
          ArrayList<String> objects2 = new ArrayList<>();
          ArrayList<A> objects3 = new ArrayList<>();
          ArrayList<AA> objects4 = new ArrayList<>();
          ArrayList<D> objects5 = new ArrayList<>();
  
      }
      public  void printCol(ArrayList<? super AA> c){ // 接收ArrayList的A类及A类的所有父类的泛型类型
  			// 只有  objects1,objects3，objects4 能作为参数
      }
       public void printRow(ArrayList<? extends AA> c) {/ 接收ArrayList的A类及A类的所有子类的泛型类型
  			// 只有 objects4 能作为参数
      }
  */
  ```

  

## 2.3 java绘图

+ **只有在paint()方法中画下的元素(包括方法调用)才会出现在画布中，且最后效果以paint()中输入的值为准**

+ 坐标体系

  坐标原点以屏幕左上角为基准，X为横坐标，Y为纵坐标

  ![image-20220104202243527](D:\typora_import_images\typora-user-images\image-20220104202243527.png)



+ 绘图基础

  ```java
  /*
   想要实现画板绘图，就要引入java提供的一个画图类(JPanel),并重写其paint方法完成初始化画图，同时需要继承JFrame库完成界面显示
   
   public class Panel_1 extends JFrame{ // JFrame 为一个窗口库
      private Canvas canvas = null; // 声明一个画板
      public static void main(String[] args){
              new Panel_1();
      }
      // 初始化面板
      public Panel_1(){
          canvas = new Canvas(); // 生成一个画板(执行paint方法里的语句)
          this.add(canvas); // 把画板添加到窗口
          this.setSize(1600,900); // 设置窗口大小
          this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 点击红X退出程序
          this.setVisible(true); // 设置为窗口可见
      }
      static class Canvas extends JPanel{
          @Override
          // paint方法用于完成画板绘制
          public void paint(Graphics g) {
              super.paint(g);   // 重写paint方法执行该行完成画板初始化
              g.drawOval(100,100,300,300); // 画一个圆 参数:x,y,width,height
          }
      }
  }
  
   注意
   	继承了JFrame就相当于继承了一个界面，继承了JPanel就相当于继承了一个画板，画板的内容写在paint中，然后又载入界面即可
   	paint(Graphics g)方法用于绘制外观
   	paint()方法调用的情况
   		再组件第一次被挂载到界面上时会调用
   		窗口最小化再最大化时
   		窗口尺寸发生改变时
   		repaint方法被调用时
  */
  ```
  
  

+ paint绘图方法中画笔g下的常用方法

  ```java
  /*
   g.drawLine(int x1,int y1,int x2,int y2); // 绘制一条直线 x1 y1为开始点位，x2 y2 为结束点位
   g.drawReact(int x1,int y1,int x2,int y2);// 绘制一个矩形 x1 y1为开始左上角点位，x2 y2为结束右下角点位
   g.drawOval(int x,int y,int width,int height); // 再指定位置绘制出一个椭圆形
   g.fillRect(int x1,int y1,int x2,int y2); // 画一个填充颜色的矩形，可以设置画笔颜色		
   g.fillOval(int x,int y,int width,int height);// 画一个填充颜色的椭圆，可以设置画笔颜色	
   
   画图片
   	画图片首先要获取图片资源
   		Image img = Toolkit.getDefaultToolkit().getImage(Panel.class.getResource("/com/bg1.png")); //即可载入图片资源
   		g.drawImage(img, 100:x, 100:y, 200:width, 400:height, this:position); // 在画板中绘制图片
   		注意
   			路径查找规则：根目录为 out/production/自定义名目录 
   画字符串
   	在画字符串前可以设置画笔颜色和字体来达到自己想要的效果
   	g.setColor(Color.pink); // 设置画笔颜色
   	g.setFont(new Font("宋体",Font.BOLD,50)); // 字体:宋体，粗细：粗体，字体大小：50
   	g.drawString("北上广深",100:x,100:y); // 宽高默认定位在字体的左下角
   注意
   	所有填充颜色和画字符串操作(fill...)都可以设置当前画笔颜色，如果不设置默认用之前的画笔颜色(画笔一开始为无色)
  	画笔颜色设置方法：g.setColor(Color.blue)
  	g.setColor(Color.decode("#94dee6")); // Color.defcode方法可以传入一个#rgb值
  	
  */
  ```

  

## 2.4 事件监听

有鼠标事件，键盘事件，窗口事件等

+ 键盘监听

  ```java
  /*
   想要实现键盘监听要实现一个接口(KeyListener)，重写它的keyTyped,keyPressed,keyReleased方法
   	keyTyped方法
   	keyPressed方法
   		当某个键被按下(一次或多次)后，该方法会被java调用
   	keyReleased方法
   		当某个键松开后，该方法会被java调用
   注意
   	keyTyped,keyPressed,keyReleased方法中有一个环境变量(KeyEvent e), 存储着当前环境中的所有数据
   	在事件设置完成后，需要挂载到界面中，来完成监听(在JFrame接口方法中写下this.addKeyListener(canvas))
   	每次事件触发后需要调用repaint()方法来完成画板重绘，否则画板不会变化
   	KeyEvent.VK_W 表示 W键  
   	e.getKeyCode()可拿到当前按下的键位
  */
  ```

  



## 2.5 进程&线程

+ JConsole(在控制台输入jconsole)，可以动态监视线程执行情况（点击对应包名进行连接即可进行监视）

+ 进程线程基本概念

  ```java
  /*
  进程
  	 指正在运行中的程序，它随着程序的开始而开始，程序的结束而销毁
  
  线程
   	 就是指的一个个程序，它由进程创建，是进程的一个实体
  单线程
  	 同一时刻只允许一个线程运行
  多线程
  	 同一时刻，允许多个线程运行
  并发
  	指多个进程或线程在同一时刻间隔执行
  并行
  	指多个进程或线程在同一时刻一起执行
  
  扩展
  	使用java查看当前主机的cpu核心数
  		Runtime runtime = Runtime.getRuntime();
  		int cpuNums = runtime.availableProcessors(); // 得到当前cpu核数
  
  */
  ```

  

+ 线程基本使用

  ```java
  /*
    创建线程的方法
    	继承Thread类(THread类实现了Runnable接口)，重写run方法
    	实现Runnable接口，重写run方法
    
    
    // 继承 Thread完成线程创建
        public class Demo extends Thread {
          static  int count = 0;
          public static void main(String[] args){
              new Demo().start(); // 开启了一个线程
          }
  
          @Override
          public  void run() {
  
              //super.run();
              while(true){
                  count++;
                  System.out.println("我是小龙");
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  if(count >=8)break;
              }
          }
      }
      注意
      	此方法继承了Thread类可以直接调用start()方法(继承链查找)，该方法最终会把该实例对象的run()方法设计为一个线程并执行
  
  //由于java是单继承机制，所以我们通常需要实现接口来完成线程创建
      public class Runnable_ {
          static private int count = 0;
          public static void main(String[] args){
              Tiger tiger = new Tiger();
              new Thread(tiger).start(); // 将tiger实例对象维护到Thread中，调用其start()方法，帮助tiger建立线程并完成tiger的run方法调用
                                         
              while(true){
                  System.out.println("Runnable_-"+ ++count);
                  if(count >=20) break;
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
              }
          }
          static class Tiger  extends Animal implements Runnable {
              static private int count = 0;
              @Override
              public void run() {
                  while(true){
                      System.out.println("Tiger-"+ ++count);
                      if(count >=20) break;
                      try {
                          Thread.sleep(1000);
                      } catch (InterruptedException e) {
                          e.printStackTrace();
                      }
                  }
  
              }
          }
          static class Animal {}
      }
  	注意
  		 new Thread(tiger).start();实际上是代理设计模式，该构造器可以接收实现了Runnable的接口对象，帮助其创建线程并调用其实例对象下的run方法
  		 Runnable接口只提供了run接口方法
  
   进程&线程执行流程
   	当此程序运行时就开启了一个进程，首先程序会进入主线成main执行，通过主线程main去开启其他线程的执行，线程之间可以并发&&并行执行，当主线程执行完后被销毁后，其他线程任然会继续执行，直到所有线程执行完毕后此进程才会结束(此时JConsole的监控也会停滞,因为全部可以执行线程已经被销毁)
   	
   注意
   	所有线程都可以去开启其他线程(但入口主线程是main首先要由其去开启其他线程，这样其他线程才会执行从而进一步开启线程)，只有全部线程执行完毕后，进程才会结束
   	只有调用该实例对象下的start()方法才能给当前实例对象下的run()方法开辟一个线程并发执行(该线程什么时候执行由cpu决定,cpu有核心空闲时才会执行)，直接进行方法调用不会开辟线程，而是串行化执行(串行化指，在该调用方法执行完后才会继续执行main方法的内容) . 
   	真正实现多线程的方法是start0()方法，调用start()就是间接的在调用start0()然后该方法会给当前实例对象下的run()方法开辟一个线程并进行调用
   	一个实例对象下只有run方法能够设计为线程，其他特性和普通对象一致(使用属性或方法以当前实例对象为目标，改变的是当前实例对象，不会影响其他实例对象)
   	当run(&main)方法的内容执行完后，代表该线程执行完了(被销毁了)
   	建议使用接口方式来完成线程操作
   	Thread.sleep(1000),可以让线程休眠1秒钟，单位为ms
   	
   	
  */
  ```

  ![image-20220106101422231](D:\typora_import_images\typora-user-images\image-20220106101422231.png)

  

+ 多个线程超卖问题

  ```java
  /*
   即多个线程在同一时间段间隔访问了一个还未达到停止条件的一个变量，改变了其到了一个不正常的值从而导致操作
  */
  ```



+ 线程终止

  ```java
  /*
   当线程完成任务后，线程就会销毁
   通过使用变量来控制run方法退出，从而达到线程销毁
  */
  ```

  

+ 线程常用方法

  ```java
  /*
   setName() // 设置线程名称
   getName() // 得到线程名称
   start() // 使该实例对象下的run方法变为线程开始执行.jvm底层调用start0()完成线程创建
   run() // 线程对象方法
   setPriority() // 更改线程的优先级
   getPriority() // 获取线程的优先级  
   sleep(ms) // 在指定的毫秒数让当前正在执行的线程休眠
   interrrupt() // 中断线程 , 并没有真正终止线程。一般用于中断正在休眠的线程
   yield() // 线程强制空闲，不让cpu执行此线程(时间不确定由cpu来判断)，通常如果cpu空闲则不会礼让成功，不空闲则可成功
   join() // 线程插队，在执行到a.join()方法时,cpu会立刻去执行a线程，等a线程执行完后继续执行插队前的线程
   setDaemon() // 将线程设置为守护线程
   getState() // 获得当前线程的状态
   Thread.currentThread().getName() // 获得当前线程名 
   注意
   	线程优先级范围 MAX_PRIORITY(10) MIN_PRIORITY(1) NORM_PRIORITY(5)
   	set...方法都需要在线程开始执行前设置
  */
  ```

  

+ 用户线程&守护线程

  ```java
  /*
   用户线程: 又称工作线程，当线程的任务执行完或通知方法结束
   守护线程：一般为工作(用户)线程服务，当所有用户线程结束，守护线程自动结束。可以利用此机制把需要一起关闭的线程设置为守护线程
  注意
  	常见守护线程：垃圾回收机制
  */
  ```

  

+ 线程的生命周期

  ```java
  /*
   NEW(new)，当一个线程被创建时就处于该状态
   RUNNABLE(runnable),runnable又被细化为ready,running,表示该线程可以运行(是否运行由cpu决定)
   BLOCKED(blocked),又被细化为(ready,running)，线程被加锁或进入同步代码块后就可能处于该状态，如果获得了锁就重新进入runnable状态
   WAITING(waiting),当一个线程调用了wait(),join(),park()方法后，该线程通常处于该状态
   TIMED_WAITING(timed_waiting)，当一线程调用了sleep(),join(),wait(),park()方法后，该线程通常处于该状态
   TERMINATED(terminated)，当一个线程即将被销毁时就处于该状态
   
   注意
   	在runnable中的线程处于ready,还是running状态又cpu决定，cpu又最终调度权
  */
  ```

  ![image-20220106180228716](D:\typora_import_images\typora-user-images\image-20220106180228716.png)



+ 线程同步机制

  ```java
  /*
   定义
   	在多线程编程中，一些敏感数据不允许被多个线程同时访问，此时就使用同步访问技术，保证数据在任何时候最多只有一个线程访问，以保证数据的完整性
   	
   synchronized 关键字实现方法线程同步
   	synchronized可以给方法和代码块添加         I	
   	public synchronized void run() {} // 给方法添加线程同步
   	synchronized (this || Object||...){} // 给代码块添加线程同步
   		注意
   			当synchronized关键字给代码块添加时,()中的锁可以指为this或其他类(所有线程的锁必须是同一个，否则就是无意义锁，达不到同步目的)
   			
   注意
   	synchronized是一个非公平锁，每个线程谁快这个锁就归谁,只有拥有锁的线程才能进入该方法或代码块
   	同步机制使程序执行效率变低，每次只允许一个线程执行，所以我们应尽量最小化该效率影响范围
   	synchronized给一个静态方法或静态代码块添加锁时，该锁就是类本身(即 该类名.class) 
   	每个线程在争抢到锁后即可进入方法或代码块执行，执行完后归还锁，进入下一次争夺，直到程序执行完毕
   	synchronized在修饰一个非静态方法或代码块时，默认为this,也可以设置为其他对象(要求对象唯一，使线程争抢的锁是一个目标)
   	synchronized在修饰一个静态方法或代码块时，默认为该类名.class
   	在使用同步机制时，优先使用同步代码块
  */
  ```
  
  ![image-20220106202824022](D:\typora_import_images\typora-user-images\image-20220106202824022.png)

+ 线程死锁

  ```java
  /*
   指多个线程占用了对方的锁资源，各线程不肯让步释放资源，导致程序运行式停滞现象
  */
  ```

+ 释放锁

  ```java
  /*
   以下情况会释放锁
   	当前线程的同步方法，同步代码执行结束
   	当前线程在同步代码块，同步方法中遇到break,return
   	当前线程在同步代码块，同步方法中出现了未处理的Error或Exception,导致程序异常结束
   	当前线程在同步代码块，同步方法中执行了线程对象的wait()方法，当前线程暂停，并释放锁
   
   以下情况不会释放锁
   	线程执行同步代码块或同步方法时，程序调用Thread.sleep()，Thread.yield()方法暂停当前线程执行，不会释放锁
   	线程执行同步代码块时，其他线程调用了该线程的suspend()方法将线程挂起，该线程不会释放锁
   
   注意
   	应该避免使用suspend()和resume()方法来控制线程
  */
  ```

  

 

## 2.6 IO流

+ IO总异常 IOException

+ 文件

  ```java
  /*
  在计算机中所有东西都可以看做是一个文件，而文件在程序中是以流的形式来操作的
  流：数据在数据源(文件)和程序(内存)之间的路径
  输入流：数据从数据源(文件)到程序(内存)的路径
  输出流：数据从程序(内存)到数据源(文件)的路 径
  注意
  	输入输出操作是以程序(内存)为参照点的，数据源到程序(内存)就是输入流操作，程序到数据源就是 输出流操作  . 可以把程序(内存)比作一个门，进入这个门就是输入流，出来这个门就是输出流
  */
  ```

  

+ 常用文件操作

  ```java
  /*
   文件构造器
       new File(String pathname) // 根据路径创建一个File对象
       new File(File parent,String child) // 根据父目录文件+子路径创建一个File对象
       new File(String parent,String child) // 根据父目录+子路径创建一个File对象
       
   
   获得文件相关信息(方法)
   	createNewFile() // 创建一个新文件
   	getName() // 获得创建的文件名
   	getAbsolutePath() // 获得文件的绝对路径
   	getParent() // 获得最近一级父级的绝对路径
   	length() // 得到文件内容的字节数
   	exists() // 判断创建的文件在该文件夹中是否存在
   	isFile() // 判断是否为文件
   	isDirectory() // 判断是否为目录
   	mkdir() // 创建一级目录
   	mkdirs() // 创建多级目录
   	delete() // 删除空目录或文件
   注意
   	文件夹也可以看做是一个文件
  */
  ```

  

+ I(读)/O(写)流原理

  ```java
  /*
   I/O是input/output的缩写，I/O是非常使用的技术，用于处理数据传输(如读写文件，网络通讯等)
   java程序中，对于数据的I?O操作以 流(stream)的方式进行
   java.io包提供各种 流 接口，用以获取不同种类的数据，并通过方法输入或输出数据
   输入input:读取外部数据(磁盘，光盘等)到程序(内存)中
   输出output:将程序(内存)数据输出到磁盘，光盘等存储设备中
  */
  
  ```

+ 流的分类

  ```java
  /*
    按操作数据单位不同分为：字节流(8bit，专门处理二进制文件，减少文件损伤) ,字符流(按字符，专门操作文本文件)
    按数据流的流向不同放分为：输入流，输出流
    按流的角色不同分为：节点流，处理流/包装流
  	
  */
  ```

  下图表中四个都为抽象类，不能直接生成实例对象

| (抽象基类) |   字节流    | 字符流 |
| :--------: | :---------: | :----: |
|   输入流   | InputStream | Reader |
|   输出流   | OuputStream | writer |

![image-20220111095614005](D:\typora_import_images\typora-user-images\image-20220111095614005.png)



+ IO体系图常用类

  **read()方法每次读取一定量数据后，读取指针就会移动到该读取位置后，通常配合while来完成全部读取**

  **在写入或读取数据时通常使用指定数组部分读或写，因为最后一部分的数据无法覆盖整个数组(而是覆盖前几个索引，后几个索引不会被清空)，这样会导致读写错误**

  **写入文件时，如果没有该文件jvm会自动创建该文件**

  +  InputStream常用子类

    ```java
    /*
     FileInputStream 文件输入流(FileInputStream(File file) | FileInputStream(String name))
     BufferedInputStream 缓冲字节输入流
     ObjectInputStream 对象字节输入流
     	FileInputStream常用方法
     		read() // 从该输入流中读取一个数据字节(不能读取汉字),如果没有可读取的数据返回-1
     		close() // 输入流操作完毕后，关闭该流，不关闭则会造成资源浪费
     		read(byte[] b) // 一次读取b数组长度的内容，并放到该数组中，最后使用new 							String(buf,0,buf.readlen)来进行输出b数组里存入的内容(和while配合使用)
     				注意
     					readlen为本次读取内容的长度，可以使用new String(buf,0,buf.readlen)构造器读						出数据
     					
     	read()读取数据
     		 // 进行文件操作
            String filePath = "D:\\FileOperate\\hello.txt";
            FileInputStream fileInputStream = new FileInputStream(filePath);
            int readData = 0;
            while((readData = fileInputStream.read()) != -1){
                System.out.print((char)readData);
            }
            	注意
            		readData存储的是读取位上的每一个字符的ASCII码，为int值，输出时以char输出显示文件内容,在读取到后读取指针后移
        
        read(byte[])读取文件
        	 public void readFileByte() throws IOException {
                // 进行文件操作
                String filePath = "D:\\FileOperate\\hello.txt";
                FileInputStream fileInputStream = new FileInputStream(filePath);
                byte[] b = new byte[8];
                int readLen = 0;
                while((readLen= fileInputStream.read(b)) != -1){
                    System.out.print(new String(b,0,readLen));
                }
                fileInputStream.close(); // 读取完毕关闭此文件流
        	}
        		注意
        			fileInputStream.read(b)每次会读取b长度的字节，存放到数组中，并返回本次读取的长度			 给readLen,在配合new String(b,0,readLen)来完成本次读取输出
        			如果读取没有占满整个byte数组，则会占前几位的索引，new String(b,0,readLen)正好也能			   完成监听
        	
     	注意
     		文件操作会造成编译异常，使用try-catch捕获，或throws抛出即可
     		read()和read(byte[] b)无法读取汉字数据，会乱码(汉字一个字占2-3个字节，此方法为一个一个字节显示)
     			
    */
    ```

    ![image-20220111101353769](D:\typora_import_images\typora-user-images\image-20220111101353769.png)

  + FileOutputStream

    ```java
    /*
     构造器
     	FileOutputStream(File file)
     	FileOutputStream(FIle file,boolean append)
     	FileOutputStream(String name)
     	FileOutputStream(String name,boolean append)
     常用方法
     	write('c')  // 写入单个字符
     	write(byte[])
     	write(byte[]:target,0:start,1:end) // 限制写入的字符串长度
     	write("123".getBytes()) // 写入一个字符串(需要转换为Byte数组才能完成写入，调用getBytes()即可)
     	write("123".getBytes():target,0:start,1:end) // 限制写入的字符串长度
     注意
     	在声明构造器时，可以给把append置为true或false,如果为true表示追加写入，为false表示覆盖写入,默认为false
     	字符串下的getBytes()方法可以把字符串转换为一个byte数组
     	
    */
    ```

    ![image-20220111114914883](D:\typora_import_images\typora-user-images\image-20220111114914883.png)

  + FileReader&FileWriter

    ```java
    /*
     FileReader&FileWriter是字符流，即按照字符来操作io
     
     FileReade
         构造器
            new FileReader(File)
            new FileReader(String)
         FileReader相关方法
            read() // 每次读取单个字符(可以读取汉字)，返回该字符，如果到了文件末尾返回-1
            read(char[]) // 批量读取多个字符到数组，返回读取的字符数，如果到文件末尾返回-1
            相关API
                new String(char[]); // 将char数组转换为String
                new String(char[],start,end) // 将char[] 指定部分转换为String
     
     FileWriter
     	构造器
            new FileWriter(File/String)
            new FileWriter(File/String,true)
        FileWriter相关方法
            write(int); 写入单个字符
            write(char[]); 写入指定数组
            write(char[],start,end); 写入指定数组的指定部分
            write(string); // 写入整个字符串
            write(string,start,end); // 写入字符串的指定部分
        注意
        	FileWriter使用后，必须要关闭(close)或刷新(flush)，否则无法将内容写入(数据停留在内存中)
     
    */
    ```

    ![image-20220111143227385](D:\typora_import_images\typora-user-images\image-20220111143227385.png)

    ![image-20220111144620403](D:\typora_import_images\typora-user-images\image-20220111144620403.png)



+ 拷贝文件

  ```java
  /*
  FileInputStream&FileInputStream实现拷贝(按字节读取)
     public void copyFile() {
          // bomb_1.gif
          String fileURL = "D:\\FileOperate\\bomb_1.gif";
          String copyURL = "D:\\FileOperate\\copy_1.gif";
          FileInputStream fileInputStream = null;
          FileOutputStream fileOutputStream = null;
  
          try {
              fileInputStream = new FileInputStream(fileURL);
              fileOutputStream = new FileOutputStream(copyURL);
              byte[] b = new byte[1024];
              int readLen = 0;
              while ((readLen = fileInputStream.read(b)) != -1) {
                  fileOutputStream.write(b,0,readLen); // 写入读写长度，在每次读取时b数组会被装满，只有最后一次时，装不满，会以索引顺序占位，所以要以fileOutputStream.write(b,0,readLen);方式进行限制
              }
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              try {
                  fileInputStream.close(); // 关闭读流
                  fileOutputStream.close(); // 关闭写流
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
  
      }
      
      
   fileWriter&fileReader实现拷贝(按字符读取)
      public void copy(){
              String fileURL = "D:\\FileOperate\\hello.txt";
              String copyURL = "D:\\FileOperate\\hello_copy.txt";
              FileWriter fileWriter = null;
              FileReader fileReader = null;
              try {
  
                   fileReader = new FileReader(fileURL);
                   fileWriter = new FileWriter(copyURL);
                   char[] c = new char[16];
                   int readLen = 0;
                  while((readLen = fileReader.read(c)) != -1){
                      fileWriter.write(c,0,readLen);
                  }
              } catch (IOException e) {
                  e.printStackTrace();
              }finally{
  
                  try {
                      fileReader.close();
                      fileWriter.close();
                  } catch (IOException e) {
                      e.printStackTrace();
                  }
  
              }
          }
          
   // BufferedInputStream&BufferedOutputStream包装流完成拷贝
    public void playCopy() {
          String fileURL = "D:\\FileOperate\\bomb_2.gif";
          String copyURL = "D:\\FileOperate\\bomb_2_copy.gif";
          BufferedInputStream bis = null;
          BufferedOutputStream bos = null;
          try {
              bis = new BufferedInputStream(new FileInputStream(fileURL));
              bos = new BufferedOutputStream(new FileOutputStream(copyURL));
              int readLen = 0;
              byte[] buf = new byte[1024];
              while ((readLen = bis.read(buf)) != -1) {
                  bos.write(buf, 0, readLen);
              }
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              try {
                  bis.close();
                  bos.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
  
  
      }
  */
  ```

  

+ 节点流&处理流

  ```java
  /*
  定义
       节点流可以从一个特定的数据源读取数据(如FileReader,FileWriter)
       处理流(又叫包装流)是 “连接” 已存在的流(节点流或处理流)之上，为程序提供更为强大的读写功能(如			BufferedReader,BufferedWriter) 
   
   全部处理流(包装流)
     BufferedReader和BufferedWriter属于字符流，按照字符来操作数据
   	BufferedReader使用方法
   		BufferReader bufferReader = new BufferReader(new FileReader("D:\\e.txt")); // 为new 		  FileReader("D:\\e.txt") 创建了一个包装流，该实例对象下的私有属性Reader in 指向传入的实例对象
   		注意
   			此时bufferReader就是new FileReader("D:\\e.txt")的包装流，可以使用该包装流上的方法，高效			  的控制该文件的读操作，
   			BufferedWriter包装原理和BufferedReader一致,其内部使用 Writer out 指向传入的实例对象
   			BufferedWriter和BufferedReader可以包装所有继承了Writer或Reader的实例对象(多态)
   			
     BufferedInputStream和BufferedOutputStream属于字节流，按照字节来操作数据	
   	BufferedInputStream使用方法
   		BufferedInputStream bis = new BufferedInputStream(new FileInputStream("D:\\e.txt")); // 	  为new FileInputStream("D:\\e.txt") 创建了一个包装流，该实例对象下继承父类的属性 InputStream in	   指向传入的实例对象
   		注意
   			此时bufferReader就是new FileReader("D:\\e.txt")的包装流，可以使用该包装流上的方法，高效			  的控制该文件的读操作，
   			 BufferedInputStream包装原理和BufferedOutputStream一致,其内部使用继承的OutputStream out 		   指向传入的实例对象
   			 BufferedInputStream和BufferedOutputStream可以包装所有继承了InputStream或OutputStream		    的实例对象(多态)
   	
   	ObjectInputStream和ObjectOutputStream在保存语句时可以保存该数值的值和数据类型
   		ObjectInputStream，该操作被称为反序列化操作
   		ObjectOutputStream，该操作被称为序列化操作
   			注意
   				ObjectInputStream和ObjectOutputStream内部维护了一个 InputStream in和OutStream out			  	ObjectInputStream和ObjectOutputStream可以包装所有继承了InputStream或OutputStream		   	    的实例对象(多态)
   				序列化操作操作指，在保存数据时，保存其的数据值和数据类型
   				反序列化操作指，在恢复数据时，恢复其的数据值和数据类型
   				只有支持序列化的类才能进行序列化保存，(需要实现Serializable或Externalizable接口,推荐			  实现Serializable接口，因为其是标记接口，没有任何方法)
   				序列化后保存的格式不是纯文本的而是特定格式的(如.dat)
  	
  	InputStreamReader和OutputStreamWriter属于转换流(处理流的一种)，可以指定节点流的编码
  		注意
   			InputStreamReader和OutputStreamWriter内部维护了一个 InputStream in和OutStream out			  	InputStreamReader和OutputStreamWriter可以包装所有继承了InputStream或OutputStream		
   
   注意
   	数据源就是存放数据的地方
   	节点流是一个底层流，可以直接操作数据源
   	BufferedReader,BufferedWriter类中维护了一个私有属性，该属性可以指向reader类和writer类及他们的所有子类实例对象，提供了更为强大的读写功能
   	处理流(包装流)包装节点流，既可以消除不同节点流的实现差异，也可以提供方便的方法来完成输入输出
   	处理流(包装六)对节点进行包装，使用了修饰器设计模式，不会直接与数据源相连
   	关闭处理流时，只需要关闭外层流即可，此时内存流会自动关闭
  */
  ```

  ![image-20220111154631725](D:\typora_import_images\typora-user-images\image-20220111154631725.png)

+ BufferedReader&BufferedWriter

  ```java
  /*
  BufferedReader&BufferedWriter为单个字符读取，因此不要用来读取二进制文件(会造成资源损坏)
   BufferedReader常用方法
   	readLine() // 按行读取，返回该行的内容，如果为null表示读取完毕
   	close() // 关闭此包装流和被包装的节点流开启
   	read()
   	
   BufferedWriter常用方法
   	write()
   	newLine() // 插入一个换行符
   	close() // 关闭此包装流和被包装的节点流开启
  */
  ```

+ BufferedInputStream&BufferedOutputStream

  ```java
  /*
   BufferedInputStream常用方法
   	read()
   	close()
   BufferedOutputStream常用方法
   	write()
   	close()
  */
  ```
  
  ![image-20220111181253091](D:\typora_import_images\typora-user-images\image-20220111181253091.png)
  
+ ObjectInputStream&ObjectOutputStream

  ```java
  /*
  序列化操作
       ObjectOutputStream只能序列化实现了Secializable或Externalizable接口的类，当在存入int,char等基本数据类型时，jvm会自动装箱为Integer和Char包装类，这些类身上实现了Secializable因此可以序列化成功 。 在序列化一个自定义类时，需手动实现Secializable才能序列化成功，否则会抛出异常
   
   反序列化操作
   	注意
   		反序列化顺序需要和序列化存入顺序一致，入在序列化时顺序为 int,char,double,在反序列化时顺序应对应	  为int,char,double
   ObjectInputStream常用方法
   	readBoolan() // 反序列化读取一个boolean
      readInt() // 反序列化读取一个int
      readChar() // 反序列化读取一个char
      readDouble() // 反序列化读取一个double
      readUTF() // 反序列化读取一个String
      readObject() // 反序列化读取一个对象(该对象必须实现Secializable或Externalizable接口)
   	close() // 关闭  ObjectInputStream流
   		注意
   			在反序列化一个类时，该类的访问遵守public等访问修饰符的访问范围，且该类需要是一个独立的类文件
   			
   ObjectOutputStream常用方法
   	writeBoolan() // 序列化写入一个boolean
   	writeInt() // 序列化写入一个int
   	writeChar() // 序列化写入一个char
   	writeDouble() // 序列化写入一个double
   	writeUTF() // 序列化写入一个String
   	writeObject() // 序列化写入一个对象(该对象必须实现Secializable或Externalizable接口)
   	close() // 关闭 ObjectOutputStream流
   
   注意
   	读写顺序要一致
   	序列化或反序列化对象需要实现Serializable接口
   	序列化类中建议添加SerialVersionUID来提高版本兼容性
   	序列化对象时，默认将里面所有属性都进行序列化，单被static和transient修饰的除外
   	序列化对象时，要求里面的属性类型也需要实现序列化接口
   	序列化具备可继承性，即如果某类实现了序列化，则它的所有子类也默认实现了序列化
  */
  ```



+ 标准输入输出流

  ```java
  /*
   System.in 标准输入流 编译类型:inputStream  运行类型:BufferedInputStream
   System.out 标准输出流  编译类型:printSream  运行类型::printSream
   输入设备：键盘  输出设备：显示器
  */
  ```

  

+ 转换流

  ```java
  /*
   InputStreamReader&OutputStreamWriter可以包装节点流的读和写，指定其编码，也可以将字节流转换为字符流
   
   注意
   	InputStreamReader指定读文件时的编码
   	OutputStreamWriter指定写文件时的编码
   实现指定编码读
   	public static void main(String[] args) throws IOException {
          String fileURL = "D:\\FileOperate\\1.txt";
          BufferedReader bur = new BufferedReader(
                  new InputStreamReader(new FileInputStream(fileURL),"utf-8"));
          String readLen = "";
          while((readLen = bur.readLine() )!=null){
              System.out.println(readLen);
          }
          bur.close(); // 关闭最外层流即可，这样内层流也会一起关闭
      }
      
  */
  ```

  ![image-20220112163520672](D:\typora_import_images\typora-user-images\image-20220112163520672.png)





+ 打印流(输出流)

  ```java
  /*
   打印流只有输出流，没有输入流，我们可以修改它打印的位置
   打印流分为 PrintStream,PrintWriter
   指定内容输出位置
   	第一种方法
   		使用 System.setOut(new PrintStream("路径"||文件))修改写入位置(默认输出在控制台)
   			public static void main(String[] args) throws FileNotFoundException {
                  System.setOut(new PrintStream("D:\\FileOperate\\log.txt"));
                  System.out.println("我来2312");
              }
              
   	第二种方法
   		使用 new PrintWriter(new FIleWriter("路径")) 修改写入位置
              public static void main(String[] args) {
              PrintWriter printWriter = null;
              try {
                  printWriter = new PrintWriter(new FileWriter("D:\\FileOperate\\log.txt"));
                  printWriter.println("我来写入内容");
              } catch (IOException e) {
                  e.printStackTrace();
              }finally {
                  printWriter.close(); // 必须关闭此流，才能正常写入
              }
          }
          
   注意
   	打印流底层的print方法实际调用的是write()方法，我们可以使用write()直接在控制台进行打印
   	使用PrintWriter写入时，必须使用close()关闭流后输出内容才会进行你的指定文件
  */
  ```

  ![image-20220112171033710](D:\typora_import_images\typora-user-images\image-20220112171033710.png)

  ![image-20220112171138035](D:\typora_import_images\typora-user-images\image-20220112171138035.png)



+ properties配置文件

  ```java
  /*
   properties为HashTable下的一个子类，通常是一个配置文件(格式:键=值)
   常用方法
   	load() 加载配置文件的键值对到Properties对象
   		properties.load(new FileReader("src\\mysql.properties"));
   	list() 将数据显示到指定设备
   		properties.list(System.out:输出设备);
   	getProperty(key) 根据键获取值,在访问一个没有的key时，返回false
   		properties.getProperty("user");
   	setProperty(key,value) 根据键值对到Properties对象
   		properties.setProperty("age",18);
   	store() 将properties对象存入到指定文件中
   		properties.store(new FileOutputStream("src\\mysql.properties"):存储位置,"test":注释);
   注意
   	键和值不需要空格，值不需要用引号包裹，默认类型是String
   	将Properties中的键值对存储到配置文件，在idea中，保存信息到配置文件，如果含有中文，会存储为unicode码
   	xx:'测试' 为解释，引用了typescript中的注解特点
   	使用properties每次设置完值后需要使用store方法进行保存
  */
  ```
  
  

## 2.7 网络编程

+ **网络编程中，不能通过改变变量的方式控制状态的选择(它只能获得一开始值的状态)，要通过传输的数据来控制状态（使用序列化来传输状态）**

+ 在网络编程中，每次服务端或客户端read数据时，都会阻塞，等待另一方发起数据

  ```java
  /*
   ObjectOutputStream oos = new ObjectOutputStream(outputStream);
   oos.writeUTF(hashmap); // 序列化字符串传给服务端，来控制当下服务端的操作
   oos.writeObject(hashmap); // 序列化集合
   
   注意
   	如果网络发生阻塞，则在每次写入序列化后 调用 flush()方法进行缓存载入
  */
  ```




+ 报错处理

  ```java
  /*
   当 报错为 stream classdesc serialVersionUID = 2984128623122997193, local class serialVersionUID = -9069290151688372218
   则需要在序列化的类中加上 private static final long serialVersionUID = 1L; 即可解决
  */
  ```



+ 无异常退出

  ```java
  /*
   如果不做无异常退出处理，则哪里端(服务端&客户端)退出，就会给另一端造成一系列报错(服务端&客户端突然终止了)，为了避免这个问题，使程序安全退出，则需要做无异常退出处理
   
   在客户端需要结束时调用System.exit(0) 关闭进程(线程随之关闭)，同时关闭前发送给服务端一个信号，服务端根据该信号检索自己内部接口连接数量，如果没有连接则关闭该服务端
  */
  ```

  

  



+ 网络数据的传入需要知道对方的ip地址才能完成通讯

+ 网络通信

  ```java
  /*
   概念：两台设备间通过网络实现数据传输
   网络通信：将数据通过网络从一台设备传输到另一台设备 
   java.net包下提供了一系列的类和接口，可以完成网络通信
  */
  ```

+ 网络

  ```java
  /*
   概念：两台或多态设备通过一定物理设备连接构成了网络
   根据网络的覆盖范围不同对网络进行划分：局域网(范围小，一个教室或一个机房),城域网(范围适中，一个城市大小),广域网(范围最大，覆盖全休(万维网www就是广域网))
  */
  ```

+ ip地址

  ```java
  /*
   概念：用于唯一表示网络中的每台主机
   查看ip地址: ipconfig
   ip地址的表示：点分十进制 192.168.2.1
   每一个十进制数的范围:0~255
   ip地址组成=网络地址+主机地址，如 192.168.22.35， 192.168.22表示网络地址 .35表示主机地址
   ipv6是互联网工程任务组设计的用于替代ipv4的下一代ip协议，其地址可以为时间上每一粒沙子编上一个地址
   ipv4最大问题在于网络资源有限，严重制约了互联网的应用和发展。ipv6的使用，不仅解决网络地址资源数量的问题，而且解决了多种接入设备连 入互联网的障碍
   ipv6使用128位表示地址(16个字节)，地址数是ipv4的4倍
   
  */
  ```
  
  ![image-20220114115048556](D:\typora_import_images\typora-user-images\image-20220114115048556.png)



+ 域名&端口号

  ```java
  /*
   域名是ip的地址的一个映射，当访问本域名时访问的就是该ip地址
   域名的优点： 方便记忆 
   端口号用于表示计算机上某个特定的网络程序，范围在0~65535(0~1024以被占用，不要去使用，如ssh 22,ftp 21,smtp 25)
   常见的网络程序端口号：mysql:3306 ,oracle: 1521
   每个网络服务的端口是唯一的，不能出现重复
  */
  ```

  

+ 协议

  ```java
  /*
   协议指数据组织的一种形式
   TCP(Transimission Control Protocol)/IP(Internet Protocal)，中文为 传输控制通讯协议/因特网互联协议，又叫网络通讯协议
   数据传输过程：数据经过程序会在其头部添加App首部，经过TCP协议会在其头部添加TCP首部，经过IP会在其头部添加IP首部，经过以太网在其头尾添加以太网标识。
   数据接收过程：数据接收到以太网传入的数据后，会逐个以太网头(找到对应网络)，IP,TCP(找到对应用户)，和应用程序（找到对应应用），然后接收数据
  */
  ```

  ![image-20220114143258153](D:\typora_import_images\typora-user-images\image-20220114143258153.png)

  ![image-20220114144238782](D:\typora_import_images\typora-user-images\image-20220114144238782.png)

+ TCP&UDP协议

  ```java
  /*
   TCP协议(传输控制协议)
   	使用TCP协议前，需建立TCP连接，形成传输数据通道
   	传输前，采用"三次握手"方式，是可靠的
   	TCP协议进行通信的两个应用进程：客户端，服务端
   	在连接中进行大数据量的传输
   	传输完毕，需要释放已建立的连接，效率低
   三次握手工作过程：
   	第一握手
   		客户端向服务端发起接入请求
   	第二次握手
   		服务端接收到客户端请求并接通数据通道
   	第三次握手
   		客户端向服务端预警即将发送数据
   	
   
   UDP协议
   	将数据，源，目的，封装成数据包，不需要建立连接
   	每个数据包的大小限制在64K内
   	因无需连接，所以不可靠
   	发送数据结束时无需释放资源，速度快
   
    TCP协议传输类似于生活中两个人打电话，需要确认对方是否能听到，然后开始表达意思，最后挂断电话(不挂断下一个电话(连接)无法接入)，效率低，但可靠
     UDP协议传输类似于生活中一个人给另一个人发短信，不知道对方是否收到，但消息已经发出，且其他人可以同时发送，效率高，但不可靠
     
     注意
     	TCP协议可以传输占用空间大的数据(视频)。UDP协议只能一次传输占用空间小的数据(需小于64k)
  */
  ```

  

+ InetAddress类

  ```java
  /*
   常用方法(静态方法)
   	getLocalHost() // 获取本机ip和机名 LAPTOP-SLASEDGH/192.168.2.103
  	getByName("域名"|"主机名") // 根据传入的主机名或域名获取ip 
  	
  	
   常用方法(非静态方法)
   	getHostAddress() // 获得当前实例对象的ip地址
   	getHostName() // 或的当前实例对象的域名
  */
  ```

  

+ Socket(TCP)

  ```java
  /*
   套接字(Socket)在开发网络应用程序中被广泛使用，通信的两端都需要使用Socket当作端点连接传输数据
   
   常用方法
   	getInputStream() // 获得字节读流
   	getOutputStream() // 获得字节写流
   	shutdownOutput() // 字节写流结束标记
   	shutdownInput() // 字节读流结束标记
   	close(); // 关闭通道
   
   注意
   	网络通讯就是Socket间的通信
   	Socket运行程序把网络连接当成一个流，数据在两个Socket间通过IO流传输(I操作为从另一端获取数据，O为向另   一端发送数据)
   	Socket在每次完成一个读或写后需要一个结束标记(调用shutDownOutput()|shutDownInput())，否则网络无   法得知是否结束
   	Socket下开启的流也需要关闭
   	Socket以字符流方式开启一个读流时，可以使用该流的newLine()方法来当作结束标记，此时要求接收方使用       readLine()进行读取，才能完成结束，否则无法结束
   	字符流读写可以通过包装流 InputStreamReader()&OutputStreamWriter()转换为字符流，在字符流写入时，每   次写入需要调用flush()方法刷新，否则数据无法写入
   	主动发起通信的应用程序被称为客户端，等待通信请求的为服务端
   	Socket有两种编程方式，TCP,UDP
   	在使用完后，需要关闭Socket
   	只有通过该通道里的读写流，读写数据后才需要添加操作结束符(shutDownOutput(),shutDownInput())
   	
   客户端&服务端传输(字节)
   	
   	// 客户端
   	public static void main(String[] args) throws IOException {
          Socket socket = new Socket(InetAddress.getLocalHost(),8888); // 连接到服务端
          OutputStream outputStream = socket.getOutputStream(); // 得到该连接的写流
          outputStream.write("第一次使用Java建立网络连接！~~".getBytes()); // 写入数据
          // 关闭客户端
          outputStream.close();
          socket.close();
      }
      
      // 服务端
       public static void main(String[] args) throws IOException {
          ServerSocket serverSocket = new ServerSocket(8888); // 生成了一个服务器在8888端口
          // 一个服务端可以同时处理多个客户端的连接，每一次连接都会生成一个socket接收数据
          Socket socket = serverSocket.accept(); // 阻塞，等待客户端接入
          InputStream inputStream = socket.getInputStream(); // 已和接入的服务端开启通信，读取数据
          byte[] b = new byte[1024];
          int readLen = 0;
          while ((readLen = inputStream.read(b)) != -1) {
              System.out.println(new String(b, 0, readLen));
          }
          // 关闭socket和inputStream流
          socket.close();
          serverSocket.close();
          inputStream.close();
      }
      
   客户端&服务端传输(字符)
   	 // Server
   	 public static void main(String[] args) throws IOException {
          ServerSocket serverSocket = new ServerSocket(9999); 
          Socket socket = serverSocket.accept(); // 持续监听连接
  
          InputStream inputStream = socket.getInputStream(); //获得通道读流
          BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
          System.out.println(bufferedReader.readLine()); // readLine读取newLine作为结束标记
  
          OutputStream outputStream = socket.getOutputStream(); // 获得通道写流
          BufferedWriter bufferedWriter = new BufferedWriter(new 									OutputStreamWriter(outputStream)); // 包装成一个字符写流
          bufferedWriter.write("hello client");
          bufferedWriter.newLine();
          bufferedWriter.flush();
  
          bufferedReader.close();
          bufferedWriter.close();
          socket.close();
      }
      
      // 客户端
       public static void main(String[] args) throws IOException {
          Socket socket = new Socket(InetAddress.getLocalHost(), 9999); // 连接端口
  
          OutputStream outputStream = socket.getOutputStream();
          BufferedWriter bufferedWriter = new BufferedWriter(new 									OutputStreamWriter(outputStream));
          bufferedWriter.write("hello server!~");
          bufferedWriter.newLine(); // 增加结束符 ，接收方需要使用 readLine()才能生效
          bufferedWriter.flush(); // 字符写入必须刷新，否则无法写入
  
          InputStream inputStream = socket.getInputStream();
          BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
          System.out.println(bufferedReader.readLine());
  
          bufferedReader.close();
          bufferedWriter.close();
          socket.close();
      }
   
   服务端&客户端连接过程
   	服务端
   	使用 ServerSocket serverSocket = new ServerSocket(8888); 开启服务
   	使用 Socket socket = serverSocket.accept();  轮询监听是否有客户端接入，有则建立socket通道，无则等   待
   	InputStream inputStream = socket.getInputStream();获得该通道的字节写流准备传入数据
   	客户端
   	使用  Socket socket = new Socket(InetAddress.getLocalHost(),8888); 连接到服务端，并开通socket通道
   	使用 OutputStream outputStream = socket.getOutputStream(); 向通道内写入数据
   	
  */
  ```

  

+ Socket(客户端向服务端上传图片)

  ```java
  /*
   // 客户端
      public static void main(String[] args) throws IOException {
          Socket socket = new Socket(InetAddress.getLocalHost(), 9999); // 端口通道连接
  
          OutputStream outputStream = socket.getOutputStream(); // 得到写流
          InputStream inputStream = socket.getInputStream();
  
          // 向服务端写入图片
          byte[] buf = new byte[1024];
          int readLen = 0;
          BufferedInputStream bis = new BufferedInputStream(new FileInputStream("D:\\FileOperate\\copy_1.gif"));
          while ((readLen = bis.read(buf)) != -1) {
              outputStream.write(buf, 0, readLen); // 写入图片到服务器
          }
          socket.shutdownOutput(); // 写完毕
  
          // 接收服务端响应
          byte[] bRes = new byte[1024];
          int length = 0;
          while ((length = inputStream.read(bRes)) != -1) {
              System.out.println(new String(bRes, 0, length));
          }
          socket.shutdownInput();
  
          // 关闭流和通道
          bis.close();
          outputStream.close();
          inputStream.close();
          socket.close();
      }
      
    // 服务端
      public static void main(String[] args) throws IOException {
          ServerSocket serverSocket = new ServerSocket(9999); // 建立服务端
          Socket socket = serverSocket.accept(); // 轮询监听
  
          InputStream inputStream = socket.getInputStream(); // 得到读流
          OutputStream outputStream = socket.getOutputStream();// 得到写流
  
          // 拷贝客户端传输的图片到服务端指定位置
          String path = "D:\\aLearning\\java\\java_code\\middle\\src\\com\\socket_connection\\copy_1.gif";
          BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(path));
          int readLen = 0;
          byte[] buf = new byte[1024];
          while((readLen = inputStream.read(buf))!=-1){
              bos.write(buf,0,readLen);
          }
          socket.shutdownInput();
  
          // 返回给客户端
          outputStream.write("拷贝完成！~".getBytes());
          socket.shutdownOutput();
  
          outputStream.close();
          inputStream.close();
          bos.close();
          socket.close();
      }
  */
  ```

  

+ netstat指令

  ```java
  /*
   netstat -am 可以查看当前主机网络情况，包括端口监听和网络连接
   netstat -anb | more 可以分页显示
   注意
   	netstat指令必须在dos控制台执行
   	netstat中，分布为 协议(开启的服务方式)，本地地址(在本机开启的服务端口)，外部地址(连接到本服务的地址)，   状态(listening,established,close_wait)
   	0.0.0.0和127.0.0.1 标识本机
   	当客户端连接到一个服务端时，TCP/UDP也会给该客户端分配一个端口供其连接到服务端
  */
  ```

  

+ UDP网络通信编程

  ```java
  /*
   DatagramSocket和DatagramPacket实现了UDP协议网络
   UDP数据报通过数据报套接字 DatagramSocket发送和接收，系统不保证UDP数据安全到达和什么时间到达
   DatagramPacket对象封装了UDP数据报，在数据报中包含了发送端的IP地址和端口号以及接收端的IP地址和端口号
   UDP协议中每个数据报都给出了完整的地址信息，因此无需建立发送方和接收方连接
   
   注意
   	在UDP中没有明确的服务端和客户端，而是变成了 数据的发送端和接收端(发送端可以变为接收端，反之如此)
   	接收数据和发送数据通过DatagramSocket对象完成
   	发送端数据被封装到DatagramPacket对象中发出，接收方收到该对象，进行拆包
   	socket类负责接收和发送数据，datagramPacket负责声明发送和接收的信息
   	
   // UDP发送和接收数据
   
   	// 发送&接收端
   	public static void main(String[] args) throws IOException {
          DatagramSocket socket = new DatagramSocket(8888); // 声明接收数据端
  
          byte[] bytes = new byte[1024];
          DatagramPacket packet = new DatagramPacket(bytes, bytes.length); // 声明存放位置，最大存储长度
          socket.receive(packet); // 阻塞 等待接收数据，并存放到 packet 中
  
          // 对接收的数据进行解包
          int length = packet.getLength(); // 获取收到数据的长度
          byte[] data = packet.getData(); // 将获取的数据解包
          System.out.println(new String(data,0,length)); // 输出数据
  
          // 发送数据到指定端口
          byte[] bytes1 = "好~~~".getBytes();
          // 声明发送的数据及长度，和发送的地址
          DatagramPacket datagramPacket = new DatagramPacket(bytes1, bytes1.length, InetAddress.getLocalHost(), 9000);
          socket.send(datagramPacket); // 发送数据
           socket.close(); // 关闭端口
      }
      
      // 发送&接收端
      public static void main(String[] args) throws IOException {
  
          DatagramSocket socket = new DatagramSocket(9000); // 声明 接收端口
  
          // 添加发送数据信息
          byte[] bytes = "hello 你好~~".getBytes();
          // 声明发送的数据及长度，和发送的地址
          DatagramPacket packet = new DatagramPacket(bytes, bytes.length, InetAddress.getLocalHost(), 8888);
          socket.send(packet); // 发出信息
  
          // 接收发送的数据
          byte[] bytes1 = new byte[1024];
          DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length); // 声明存放位置，最大存储长度
          socket.receive(datagramPacket); // 端口阻塞，等待接收数据存放到 datagramPacket 中
          byte[] data = datagramPacket.getData();
          int length = datagramPacket.getLength();
          System.out.println(new String(data,0,length));
           socket.close(); // 关闭端口
      }
  */
  ```

  

![image-20220115161832352](D:\typora_import_images\typora-user-images\image-20220115161832352.png)



## 2.8 反射

+ 反射总结

  ```java
  /*
   引用反射就是为了满足ocp原则，通过反射构造出来的实例对象可以和普通实例对象一样使用，但这就违背了ocp原则，（需要通过改变源码来控制，因为每个属性和方法都是不可变的），即无意义，所以我们要使用反射收集其对象类的字段和方法，来达成一次配置多次复用目的
   
   注意
   	在使用反射从一个类对象中获得各属性，方法，实例对象时，其都是一个个独立的对象并无关联，因此需要使用java   提供的方法来操作(建立关联)
  */
  ```

  

+ 反射完美实现了ocp原则(开闭)：**不修改源码，扩展功能**

+ 反射

  ```java
  /*
   定义&作用
   	反射机制允许程序在执行期借助于ReflectionAPI取得任何类的内部信息(成员变量，构造器，成员方法等)，并能操作对象的属性及方法，反射在设计模式和框架底层都有用到
   	加载类完后，在堆中就产生了一个Class类型的对象(一个类只有一个Class对象)，这个对象包含了类的完整结构信息，通过这个对象可以得到类的结构。这个对象就像一面镜子，可以通过它看到类的结构，因此被称为 反射
   	
   注意
   	一个类的反射被称为class类对象
  */
  ```

  ![image-20220120183928270](D:\typora_import_images\typora-user-images\image-20220120183928270.png)

  java类运行步骤

  ```java
  /*
   在代码编码完毕后，由jvm完成编译(编译阶段)。运行阶段，当类被实例化时，会在堆区先开辟一个该类的类对象(加载阶段，有则指向，无则加载，此时被static修饰的属性和方法会开辟一个空间被执行，该类和它的实例对象都可以使用)，然后通过该类对象映射属性和方法，同时该类的实例对象和它的类对象互为映射关系
  */
  ```

  

+ 反射机制

  ```java
  /*
   反射功能
   	在运行时判断任意一个对象所属的类
   	在运行时构造任意一个类的对象
   	在运行时得到任意一个类所具有的成员变量和方法
   	在运行时调用任意一个对象的成员变量和方法
  	生成动态代理
   
   反射主要类
   	java.lang.Class 代表一个类，Class对象标识某个类加载后在队中的对象
   	java.lang.reflect.Method。代表类的方法
   	java.lang.reflect.Field. 代表类的成员变量
   	java.lang.reflect.Constructor,代表类的构造方法
   	
   反射优点
   	可以动态的创建和使用对象(框架底层核心)，使用灵活，没有反射机制，框架技术就失去了底层
   反射缺点
   	使用反射基本是解释执行，对执行速度有影响。优化:反射继承了Accessible类，可以通过该类来关闭反射检查，从   而稍微提高反射的效率，如 h.setAccessible(true);(true为关闭,false为不关闭) 即关闭了h的反射检查
  */
  ```

  ![image-20220120150851179](D:\typora_import_images\typora-user-images\image-20220120150851179.png)

  class类

  ```java
  /*
  基本概念
       Class也是类，也继承了Object类
       Class类对象不是new出来的，而是类加载器生成的(系统创建的。在实例化一个对象时，如已有该类的类对象在堆		区，则直接通过该类对象映射创建实例对象，如没有，则该类的类对象就会被加载到堆区，通过该类对象去映射实例   对象)
       一个类的Class类对象只有一份，因为类只加载一次
       每个实例对象都会记载自己是由哪个类对象生成的
       通过Class可以完整地得到一个类的完整结构，通过一系列API
       类的字节码二进制数据，存放在方法区
       当类对象被加载后，还会在方法区里生成一个类的字节码二进制数据，其会引用类对象里的字段或方法
           注意
              加载类对象时执行的是loadClass方法
              每个类的对象类只能加载一次
              Class对象存放在堆区
   	
   常用方法
   	Class<?> cls = Class.forName("类路径"); // 加载该类路径的类对象到本类中
    	System.out.println(cls); // 直接打印显示的是：class 该类的类路径（编译类型）
    	System.out.println(cls.getClass()); // 输出cls运行类型为:class java.lang.Class
    	cls.getPackage().getName() // 得到包名
    	cls.getName() // 得到全类名
    	Car car = (Car)cls.newInstance() // 返回一个该类对象的实例对象（默认类型为Object）
    	Field brand = cls.getField("brand"); // 从类对象中得到一个属性(字段)
    	brand.get(car); // 得到该属性(字段)的值
    	brand.set(car,"奔驰"); // 通过反射设置属性的值
    	Field[] fields = cls.getFields(); //得到所有字段放到一个数组中
    	注意
    		Class<?> cls = Class.forName("com.Car"); 中<?>表示接收不确定的java类型
    		在反射中，类对象上的每个属性(字段)和方法都是一个可获取的对象(独立)，最终通过该类对象实例得到值(受		到访问修饰的限制)
    		
   class类对象的获取方式
   	在编译阶段，通过 Class.forName() 获取，此方法多用于 配置文件，读取类全路径，来加载类对象
   	在类加载阶段，通过 类.class 获取(最安全，性能高),多用于参数传递,如通过反射得到对应构造器对象
   	在运行阶段，通过 对象.getClass() 获取 ,多用于获取实例对象的类对象
   	通过 ClassLoader cl = 对象.getClass().getClassLoader(); 拿到该类的类加载器， Class cl1 = cl.loadClass("类全名"); 获取类对象
   	基本数据类型获得类对象，int.class,double.class等，此时jvm会堆基本数据类型自动装箱为包装类型(Integer,Double),在打印时会自动拆箱位int,double等
   	基本数据类型对应的包装类，可以通过.TYPE得到Class类对象 ， Class cls = 包装类.TYPE
   	
   以下类型有Class类对象
   	外部类，成员内部类，静态内部类，局部内部类，匿名内部类
   	接口，数组，枚举，注解，基本数据类型，void
   
  	
  */
  ```

  

+ 类加载

  ```java
  /*
   反射机制是java实现动态语言的关键，通过反射实现类动态加载
   静态加载：编译时加载(用到)相关的类，如果该类不存在则报错，依赖性太强
   动态加载：编译时不会加载相关类对象，而是在运行时加载需要的类，如果运行时不用该类对象则不报错，如果使用了该类对象则且该类不存在则会报错，降低了依赖性
   
   类加载过程
   	编写的源码类(class)会以二进制的形式装载到方法区，当其首次被new或其他方式类引用后，会进入类加载状态，分三个阶段，加载，连接(连接阶段分，验证，准备，解析)，(连接操作完成后该类的二进制数据就被合并到jre中了)，初始化(由程序员控制类的内容)。然后类对象就被加载到堆区了，同时方法区的类字节码引用该类对象
   	加载阶段
   		jvm在该阶段的主要目的是将字节码从不同的数据源(可能是class文件，也可能是jar包，升值网络)转化位二进制字节流加载到内存中，并生成一个代表该类的java.lang.Class对象
   	连接阶段-验证
   		目的是为了确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身安全
   		包括文件格式验证(是否以魔数 oxcafebabe开头)，元数据验证，字节码验证和符号引用验证
   		可以考虑使用 -Xverify:none 参数来关闭大部分的类验证措施，缩短虚拟机类加载的时间
   	连接阶段-准备
   		jvm会在该阶段对静态变量，分配内存并初始化(对各数据类型进行默认初始化，如0,0l,null,false),这些变量所使用的内存都将在方法区中进行分配(如果属性被 static final修饰则立刻开辟空间并赋值)
   	连接阶段-解析
   		虚拟机将常量池内的符号引用替换为直接引用(地址引用)的过程
   	initialization初始化
   		到初始化阶段，才真正开始执行类中定义的 Java 程序代码，此阶段是执行 clinit() 方法的过程
   		clinit() 方法是由编译器按语句在源文件中出现的顺序，依次自动收集类中的所有静态变量的赋值动作和静态代码块中的语句，并进行合并 (如 static{ a=100} static int a = 300 ;此时静态代码块在前，先收录它，然后收录 a = 300,由于两个变量名相同再进行合并)
   		虚拟机会保证一个类的 clinit() 方法再多线程环境中被正确的加锁，同步，如果多个线程同时去初始化一个类，那么只会由一个线程去执行这个类的 clinit() 方法，其他线程都需要阻塞等待，直到活动线程执行完毕 
   		
   	注意
   		加载和连接阶段由jvm控制，程序员赋值控制初始化阶段
   注意
   	当创建实例对象(new方式)，该子类和所有父类的类对象都会被加载(继承)
   	new 类名(),类名.静态属性，继承等都会触发类加载
  */
  ```

  ![image-20220120184226001](D:\typora_import_images\typora-user-images\image-20220120184226001.png)

 

+ 通过反射获得类的结构信息

  ```java
  /*
  com.reflection & ReflectionUtils.java 包 常用api
       Class<?> p = Class.forName("com.Person"); // 加载Person类对象到本类中
       p.getName(); // 获得该类对象的全类名，结果： com.Person
       p.getSimpleName(); // 获得该类对象的简单类名，结果：Person
       p.getFields() // 获得该类对象及父类对象的被public修饰的所有属性
       p.getDeclaredFields(); // 获得该类对象的所有属性
       p.getMethods(); // 获得该类对象及父类对象的被public修饰的所有方法
       p.getDeclaredMethods(); // 获得该类对象的所有方法
       p.getConstructors(); // 获得该类对象的被public修饰的所有构造器
       p.getDeclaredConstructors();// 获得该类对象的所有构造器
       p.getPackage(); // 获得该类对象的包信息，结果：com.Person
       p.getSuperClass();  // 得到该类对象的父类的包信息 
       p.getinterfaces(); // 以 Class[] 形式返回接口信息
       p.getAnnotations(); // 以Annotation[] 形式返回注解信息
       Person p1=p.newInstance(); // 此方法会去调用p对象类的Public无参构造器并生成一个该类对象的实例对象
   	 Constructor<?> cons = p1.getConstructor(Class...); // 根据参数列表，获得对应的public构造器对象
   	 	cons.newInstance("value"); // 即可调用该构造器返回一个实例对象
   	Constructor<?> cons =  p1.getDeclaredConstructor(Class...); // 根据参数列表，获得所有构造器对象
   		cons.setAccessible(true) // 爆破该private使外部可以访问
   		Object user2 = cons.newInstance("value"); //使用该构造器创建实例对象
   		
  java.lang.reflect.Field 包 & java.lang.reflect.Constructor 包 常用api
  	field.modifiers()//以int形式返回修饰符(默认修饰符为0，public为1，private为2，protected为4,static为8，final为16)
  	field.getType() // 以 Class形式返回字段的数据类型
  	method.getReturnType() // 以 Class形式返回方法返回值类型
  	methods.getParameterTypes() // 以数组形式收集形参类型
  
  通过反射获得属性并设置值的方法(包括私有属性)
  	Class<?> PersonClass = Class.forName("com.Person");
  	Object o = PersonClass.newInstance();
  	Field name = o.getDeclaredField("name"); // 此方法可以或的所有类型的字段
  	name.setAccessible(true); // 设置爆破，此时就可以打破访问修饰符的限制
  	name.set(o,"www"); // 设置该实例对象的值
  	name.get(o); // 输出
  	注意
  		如果获得的属性是静态的则可以置为其的初始态，这样也可以获得数据,如 name.get(null),age.get(0),	  (默认name,age都被static修饰)
  		getField("xx"); // 获得被public修饰的xx属性
  		getDeclaredField("xx") // 获得xx属性(无论被什么修饰)
  
  通过反射获得方法并调用(包括私有方法)
  	方法
  		getMethod("方法"[,形参列表]) // 获得被public修饰的指定方法
  		getDeclaredMethod("方法"[,形参列表]) // 获得指定的方法(无论被什么访问修饰符修饰)
  		
  	Class<?> playClass = Class.forName("com.Play");
  	Object o = playClass.newInstance();
  	Method m = o.getDeclaredMethod("call",int.class);	
  	m.setAccessible(true);
  	m.invoke(m.invoke(o,2)); // 调用此方法
  	注意
  		方法如果被static修饰则调用时可以传入 o(实例对象)或null
  		如果方法有返回值，无论该方法的返回值是什么类型，默认都为Object，即方法的返回类型在运行阶段是该方		 法指定类型，在编译阶段为Object类型
  
  注意
  	Class形式就是类对象
  	以上大部分方法，在类，属性，方法中通用
  	基本数据类型再获取时会自动拆箱和装箱，复杂数据类型再获取时，会显示该类型具体的位置(包路径)
  	反射机制可以在类外部使用被private修饰的属性或方法(需要使用爆破 ,方法：setAccessible(true))
  	如果获得的属性是静态的则可以置为其的初始态，这样也可以获得数据,如 name.get(null),age.get(0),	     (默认name,age都被static修饰)
  */
  ```

  

## 2.9 JDBC



+ **当我们连接到指定数据库系统时，如果直接指定表名不管用，则指定 数据库名.表名 即可**

+ java数据类型和mysql数据类型对应关系

  | mysql        | java          |
  | ------------ | ------------- |
  | int          | Integer\int   |
  | varchar&char | String        |
  | datatime     | Date\|String  |
  | double       | Double\double |

  

+ 数据库对象类的字段应该和对应表里的字段相同

  ```java
  /*
   如 actor 表中的字段为 name sex 则 该数据库对象类的字段也要为name sex
   
   注意
   	数据库对象类可以看作是对应表里的一行数据
   	多行数据使用 List<数据库对象类> 方式存储
   	如果数据库里有数据，但java查询出来有一个或多个字段为空，则首先检查数据类型是否匹配，如果都没问题，重新定义一下该数据库对象类即可解决
  */
  ```

  

+ jdbc

  ```java
  /*
   jdbc为访问不同的数据库提供了统一的接口(由各个数据库厂商来实现该接口，并生成.jar文件)，为使用者避免了细节问题，通过使用jdbc可以连接任何提供了jdbc驱动程序的数据库系统，从而完成对数据库的各种操作
  */
  ```

  ![image-20220125093717353](D:\typora_import_images\typora-user-images\image-20220125093717353.png)

  ![image-20220125102121425](D:\typora_import_images\typora-user-images\image-20220125102121425.png)



+ JDBC 操作

  ```java
  /*
   JDBC API是一系列的接口，它同一和规范了应用程序与数据库的连接，执行SQL语句并得到返回结果等各类操作，相关类和接口在 java.sql与javax.sql包中
   
   JDBC程序编写步骤
   	注册驱动(加载Driver类)
   	获得连接(得到Connection)
   	执行增删改查（发送SQL 给mysql）
   	释放资源(关系相关连接)
       注意
          注册驱动时需要加入对应数据的.jar文件，加入方法：在当前包创建一个libs包，把该jar文件放入libs包		  内，又键选择 add librarys添加库,此时该jar包就在该文件夹下解压为类文件了
    
  JDBC基本操作
  	使用普通类来连接数据库
  	 public static void main(String[] args) throws Exception{
          Driver driver = new Driver();
  
          // jdbc:mysql: 表示连接的数据库类型
          // localhost 表示连接的主机地址
          // :3306 表示连接的端口号
          // mySQl 表示连接的数据库管理系统名
          String url = "jdbc:mysql://localhost:3306/mySQl";
          // 将 用户名和密码封装到Properties对象中
          Properties properties = new Properties();
          // user和password为mysql规定好的字段
          properties.setProperty("user","root");
          properties.setProperty("password","lyx");
          // 根据设置的值，连接到对应数据库,建立的是一个网络连接
          Connection connect = driver.connect(url, properties);
  
          // 设置需要指向的sql语句
          String sql = "insert into stu1.actor values(null,'周杰伦','男','1979-1-18','19021304801')";
          Statement statement = connect.createStatement(); // 创建sql语句传输通道
          int rows = statement.executeUpdate(sql); // 处理 传输的 dml语句(增删改)，返回影响的行数(0-n),0表示失败 n 表示成功
          System.out.println(rows > 0 ? "成功":"失败");
  
          // 如果不关闭则该连接一直存在，如果过多则导致后续用户连接不上数据库
          statement.close(); // 关闭此传输通道
          connect.close(); // 关闭此连接
      }
      
      使用反射来连接数据库(增加程序灵活性)
      public static void main(String[] args) throws Exception{
          Class<?> driveClass = Class.forName("com.mysql.jdbc.Driver");
          Driver driver = (Driver)driveClass.newInstance();
          String url = "jdbc:mysql://localhost:3306/mySQl";
          Properties properties = new Properties();
          properties.setProperty("user","root");
          properties.setProperty("password","lyx");
          String sql = "insert into stu1.actor values(null,'周毛','男','1989-3-18','19021342213')";
          Connection connect = driver.connect(url, properties);
          Statement statement = connect.createStatement();
          int rows = statement.executeUpdate(sql);
          System.out.println(rows > 0 ? "成功":"失败");
          statement.close(); // 关闭此传输通道
          connect.close(); // 关闭此连接
      }
      
      使用DriverManager建立连接
      private static void demo()throws  Exception{
          Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
          Driver driver = (Driver)aClass.newInstance();
          String url = "jdbc:mysql://localhost:3306/mySQl";
          String user = "root";
          String password = "lyx";
          DriverManager.registerDriver(driver);
          Connection connection = DriverManager.getConnection(url, user, password);
      }
      
      使用反射加载Driver类配合DriverManager完成连接(推荐使用)
      // 此方法底层会帮我们创建Driver实例对象并进行DriverManager注册，因此我们无需创建实例对象和注册
      private static void demo1() throws Exception {
          Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
          String url = "jdbc:mysql://localhost:3306/mySQl";
          String user = "root";
          String password = "lyx";
          Connection connection = DriverManager.getConnection(url, user, password);
          System.out.println("连接成功"+connection);
          connection.close();
      }
      
      使用配置文件+反射+DriverManager完成连接(推荐使用)
      private  static void demo2() throws Exception {
          Properties properties = new Properties();
          properties.load(new FileReader("src\\mysql_config.properties"));
          String user = properties.getProperty("user");
          String password = properties.getProperty("password");
          String url = properties.getProperty("url");
          String driver = properties.getProperty("driver");
  
          Class<?> aClass = Class.forName(driver);
          Connection connection = DriverManager.getConnection(url, user, password);
          System.out.println("连接成功~~"+connection);
          connection.close();
      }
      
     注意
     	mysql驱动5.1.6版本后可以无需使用Class.forName("com.mysql.jdbc.Driver")进行加载驱动,jdk1.5以后使用了jdbc4不再需要显示调用Class.forName()注册驱动,底层会自动调用驱动jar包下META-INF\service\java.sql.Driver文本中的类名去注册
     	在实际使用中建议写上 Class.forName("com.mysql.jdbc.Driver")
  */
  ```
  
  

+ 结果集(resultSet)

  ```java
  /*
  结果集表示数据库返回结果的一张表，通常执行select语句后生成
  结果集在取数据时是一个指针形式，第一次指向数据的表头，需要使用next()下移指针获取数据，并返回boolean值，如果为true表示该行有数据，如果返回false表示没有数据
  
  常用方法
  	next() 指针下移一行，如果有数据返回true，没有则返回false
  	previous() 指针上移一行，如果有数据返回true，没有则返回false
  	getInt('索引'|'列名') 获得该行的第一列数据，且该数据为int型
  	getString('索引'|'列名') 获得改行的第二列数据，且该数据为String型
  	getDate('索引'|'列名') 获得改行的第三列数据，且该数据为Date类型
  	
  使用
  	private  static void demo2() throws Exception {
          Properties properties = new Properties();
          properties.load(new FileReader("src\\mysql_config.properties"));
          String user = properties.getProperty("user");
          String password = properties.getProperty("password");
          String url = properties.getProperty("url");
          String driver = properties.getProperty("driver");
          Class<?> aClass = Class.forName(driver);
          Connection connection = DriverManager.getConnection(url, user, password);
  
          Statement statement = connection.createStatement();
          String querySql = "select * from stu1.actor";
          ResultSet resultSet = statement.executeQuery(querySql);
  
          // 第一次resultSet指向返回的表头，next()指针下移，指向第一行数据,判断第一行是否有数据，有则为true,无则false，
          // 如果第一行有数据执行循环体，继续指向第二行判断...往复，直到把指向表尾为止(表尾返回false)
          while(resultSet.next()){
              int anInt = resultSet.getInt(1); // 获取该行的第一列数据
              String string = resultSet.getString(2); // 获取改行的第二类数据
              String string1 = resultSet.getString(3); // 获取改行的第三列数据
              Date date = resultSet.getDate(4); // 获取改行的第四列数据
              String string2 = resultSet.getString(5); // 获取改行的第五列数据
              System.out.println(anInt+"\t"+string+"\t"+string1+"\t"+date+"\t"+string2); // 打印数据
              //注意：获取的类型要和sql表中的类型对应 ,varchar char为字符串类型,int等为int类型,date等为 Date类型
          }
  
          resultSet.close(); // 关闭结果集流
          statement.close(); // 关闭sql传输流
          connection.close(); // 关闭sql连接
      }
  
  注意
  	dml语句使用executeUpdate(sql) ,查询语句使用executeQeury(sql)
  	结果集需要关闭
  */
  ```

  ![image-20220126100227936](D:\typora_import_images\typora-user-images\image-20220126100227936.png)



+ Statement

  ```java
  /*
   Statement对象 用于执行静态SQL语句并返回其生成的结果的对象
   在连接建立后，需要对数据库进行访问，执行 命名或是SQL 语句，可以通过 		
   	Statement,PreparedStatement,CallableStatemnt 方式执行
   
   PreparedStatement
       PreparedStatement方式执行的sql语句中的参数用?表示，调用PreparedStatement对象的setXxx()方法来设置    这些参数，setXxx()方法有两个参数，第一个参数为要设置的SQL语句中的参数的索引(从1开始)，第二个参数是设置    的SQL语句中的参数的值
    使用
  	 public  void demo() throws Exception {
          //解析properties连接mysql
          Properties properties = new Properties();
          properties.load(new FileReader("src\\mysql_config.properties"));
          String user = properties.getProperty("user");
          String password = properties.getProperty("password");
          String url = properties.getProperty("url");
          String driver = properties.getProperty("driver");
          Class<?> aClass = Class.forName(driver);
          Connection connection = DriverManager.getConnection(url, user, password);
  
  
          String oldName = "周杰伦";
          String newName = "周深";
          String sql = "update stu1.actor set name=? where name=?";
          PreparedStatement preparedStatement = connection.prepareStatement(sql);
          preparedStatement.setString(1,newName); // 将sql语句中第一个?替换为newName
          preparedStatement.setString(2,oldName); // 将sql语句中第二个?替换为oldName
          int i = preparedStatement.executeUpdate(); // 执行此语句
          System.out.println(i>0?"成功":"失败");
        }
    优点
    	不在使用 + 拼接sql语句，减少语法错误
    	有效的解决了sql注入问题
    	大大减少了编译次数，效率较高
    注意
   	PreparedStatement有两个常用方法 
   		executeQuery() 做查询时调用，返回ResultSet对象
   		executeUpdate() 做增，删，改时调用，返回受影响的行数
   	PreparedStatement已经接收了一个sql语句，所以在调用executeQuery()&executeUpdate()方法时旧不需要传    入sql语句了
   	
   
   注意
   	statement对象执行SQL语句，存在SQL注入风险(通过用户输入来操作数据库)
   	常见 sql注入 登录类型  用户名: 1' or ,密码: or '1'= '1
  */
  ```

  ![image-20220126194413245](D:\typora_import_images\typora-user-images\image-20220126194413245.png)

  ![image-20220126194454896](D:\typora_import_images\typora-user-images\image-20220126194454896.png)

  

+ JDBC控制事务

  ```java
  /*
   前言
  	由于dml语句会自动开启和提交事务，因此如果在一个程序中进行多次dml操作，而此时程序异常终止，此时就会造成数据库数据错误，为了避免这一问题，需要使用事务来对这些dml操作进行统一管理，即(程序一旦出错则不会提交事务，只有程序正常运行才会t)
  	
   基本作用
       JDBC程序中当一个Connection对象创建后，在执行dml语句(executeUpdate)时，默认情况下是自动创建和提交事务(每次执行一个SQL语句，如果执行成功，会向数据库自动提交，而不能回滚)，当我们想要对一些dml语句统一管理时,(只有sql语句全部正常提交才能提交，有一个失败则不会提交),就需要用到事务。通过设置connect下的setAutoCommit方法来控制事务自动提交(connect.setAutoCommit(false))，当全部无误时，使用 connect.commit()来提交全部事务
       
   工作原理
   	基本原理
   		dml会自动开启和提交事务。开始事务可以看作是进行操作排队，提交事务可以看作是一个令牌，只有在排队和拥有令牌才能执行dml语句。而通过设置connect.setAutoCommit(false)则表示不自动提交事务,当所有dml正常开启事务且程序无误时，再使用connect.commit()统一提交事务(统一发放令牌),这样就可以保证多个事务在同一状态，避免了因为程序故障而导致数据丢失情况
   	实体化理解
   		dml操作(自动开始和提交事务)可以实体化为一座桥，当开始事务可以看作为再桥前等待，提交事务可以看作是穿过这个桥(操作数据库),如果第一个dml操作完成后出现bug则其后的dml操作无法执行，如果这些dml操作之前存在交互则数据会丢失，为了避免这个情况需要设置connect.setAutoCommit(false)(关闭自动提交事务)，表示事务再桥前等待，当出现bug时，因为都在桥前没有操作数据库所以不会造成数据损失，只要全部事务都在桥前，再使用connect.commit()统一提交事务来操作数据库，保证了数据的一致性，避免了数据丢失
       
    注意
       JDBC程序中为了让多个SQL语句作为一个整体执行，需要使用事务(只有全部都成功时事务才能提交)
       调用Connection的setAutoCommit(false)可以取消自动提交事务
       在所有的SQL语句都成功执行后，调用commit()方法提交事务
       在其中某个操作失败或出现异常时，可以调用rollback()方法回滚事务
       只有被提交的事务才能再数据库中执行并修改数据
       通常有两个或两个数据以上的交互时，需要用到事务，来保证数据一致性，避免因此程序错误而导致的数据丢失
       
   使用
   	public static void main(String[] args) {
          Connection connect = JDBCUtil.getJDBCConnect(); // 连接数据库
          String sql1 = "update stu1.balance set money=money-100 where name='张三'";
          String sql2 = "update stu1.balance set money=money+100 where name='李四'";
          try {
              connect.setAutoCommit(false); // 开启的事务不自动提交 (sql语句中的 dml语句每次执行都会自动开启和提交事务)
              PreparedStatement preparedStatement = connect.prepareStatement(sql1);
              PreparedStatement preparedStatement1 = connect.prepareStatement(sql2);
              preparedStatement.executeUpdate(); // 开启事务
              int i = 1 / 0; // 制造程序故障
              preparedStatement1.executeUpdate(); // 开启事务
              connect.commit(); // 提交全部事务
          } catch (SQLException throwables) {
              throwables.printStackTrace();
          }
      }
  */
  ```

  

+ 批处理

  ```java
  /*
   当需要成批插入或更新记录时，可以采用java的批量更新机制，该机制允许多条语句一次性提交给数据库批量处理，通常情况下比单独提交处理更高效
   
   常用方法
   	addBatch() 添加需要批量处理的SQL语句或参数
   	executeBatch() 执行批量处理语句
   	clearBatch() 清空批处理包的全部语句
   
   使用
   	public static void main(String[] args){
  
          Connection connect = JDBCUtil.getJDBCConnect();
          String sql = "insert into stu1.actor values(null,?,?,?,?)";
          PreparedStatement preparedStatement = null;
          try {
              preparedStatement = connect.prepareStatement(sql);
          } catch (SQLException throwables) {
              throwables.printStackTrace();
          }
          for(int i = 1; i <= 100;i++){
              String[] strings = MyUtil.randomDataOfPeople();
              try {
                  preparedStatement.setString(1,strings[0]);
                  preparedStatement.setString(2,strings[1]);
                  preparedStatement.setString(3,strings[2]);
                  preparedStatement.setString(4,strings[3]);
                  preparedStatement.addBatch(); // 添加数据到batch
                  if(i%20 == 0){ // 每 20 条数据 提交一次
                      preparedStatement.executeBatch(); // 提交数据
                      preparedStatement.clearBatch(); // 清空数据
                  }
                  System.out.println(strings[0]+"\t"+strings[1]+"\t"+strings[2]+"\t"+strings[3]+"\t添加完成");
                  if(i==100) System.out.println("100 条数据添加完毕");
              } catch (SQLException throwables) {
                  throwables.printStackTrace();
              }
          }
  
      }
   
   addBatch源码分析
   	第一调用次会创建 Object[],该数组中存放着sql语句，当其即将满后，会以1.5倍扩容(使用了synchronized因此   线程安全) 
   
   注意
   	PreparedStatement通常和PreparedStatement一起使用，既减少了编译次数(网络开销)，又减少了运行次数，效   率大大提高(5000条数据，一条数据数据添加需要10s，批量添加只需要0.1s)
   	如果使用批处理，则需要再url后加上: ?rewriteBatchedStatements=true ,此时批处理才生效
   	当使用executeBatch提交了addBatch里的sql语句后，需要clearBatch清空数据，否则addBatch里的数据不会清   除，这样在下一次提交时会提交重复的数据
  */
  ```

  

+ 数据库连接池

  ```java
  /*
   传统连接池
   	传统JDBC数据库连接使用DriverManager来获取，每次向数据库建立连接的时候都要将Connection加载到内存中，再验证IP地址，用户名和密码(0.05s~1s)，需要数据库连接时，需要向数据库请求一个，频繁的进行数据库连接操作将占用很多系统资源，容易造成服务器崩溃
   	每一次数据库连接使用完后都需要断开,如果程序出现异常而未关闭,将导致数据库内存泄漏，最终将导致数据库重启
  	传统获取连接的方式，不能控制创建的连接数量，如连接过多，也可能导致内存泄露，导致MySQL崩溃
  	解决传统开发中的数据库连接问题，可以采用数据库连接池技术(connection pool)
  	
   数据库连接池
   	预先再连接池中放入一定数量的连接，当程序需要建立数据库连接时,只需从 "连接池"中和一个连接建立映射关系,在使用完毕后再断开此映射关系即可
  	数据库连接池负责分配,管理和释放数据库连接,它允许程序重复使用一个现有的数据库连接,而不是重新创建一个
  	当程序向连接池请求的连接数超过最大连接数量时，这些请求将会倍加入到等待队列(等待其他程序使用完数据库连接，若有则按队列顺序连接)
  	注意
  		连接池在java中的接口为DataSource，只要实现了这个接口的类就能管理 "连接"
  
  	
  数据库连接池种类
  	JDBC的数据库连接池使用 javax.sql.DataSource表示,DataSource知识一个接口.该接口通常由第三方提供实现
  	*C3P0 数据库连接池，速度相对较慢，稳定性不错(hibernate,spring)
  	DBCP 数据库连接池，速度相对C3P0较快，但不稳定
  	Proxool 数据库连接池，有监控连接池状态的功能，稳定性叫C3P0差一点
  	BoneCP 数据库连接池，速度块
  	*Druid(德鲁伊)是阿里提供的数据库连接池，集DBCP,C3P0,Proxool优点于一身的数据库连接池
  	
  C3P0连接池使用
  	public static void main(String[] args) {
          Properties properties = new Properties();
          try {
              properties.load(new FileReader("src//mysql_config.properties"));
              String user = properties.getProperty("user");
              String password = properties.getProperty("password");
              String driver = properties.getProperty("driver");
              String url = properties.getProperty("url");
  
              // 设置连接池参数
              ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();//创建连接池
              comboPooledDataSource.setDriverClass(driver); // 指定数据库连接类型
              comboPooledDataSource.setJdbcUrl(url); // 指定连接路径
              comboPooledDataSource.setUser(user); // 设置登录用户
              comboPooledDataSource.setPassword(password); // 设置登陆密码
  
              // 设置连接池里的连接数
              comboPooledDataSource.setInitialPoolSize(10);  // 设置初始化连接数
              comboPooledDataSource.setMaxPoolSize(50); // 设置最大连接数，超过该数则会进入等待队列排队，一有连接释放就会连接
  
              // 建立一次连接
              Connection connection = comboPooledDataSource.getConnection(); // 和该连接池里的连接建立连接关系
              connection.close();
          } catch (Exception e) {
              // 以运行异常抛出此时，程序员可以接收此异常做进一步处理，也可以不处理最终由jvm处理
              throw new RuntimeException(e);
          }
      }
  
  C3P0提供了配置模板(c3p0-config.xml)，可以通过它快速创建连接池和配置连接
  	// c3p0-config.xml 模板内容
  	<c3p0-config>
          <!-- 数据源名称代表连接池 -->
        <named-config name="mysql">
      <!-- 驱动类 -->
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <!-- url-->
          <property name="jdbcUrl">jdbc:mysql://localhost:3306/mySQl?rewriteBatchesStatements=true</property>
        <!-- 用户名 -->
              <property name="user">root</property>
              <!-- 密码 -->
          <property name="password">lyx</property>
          <!-- 每次增长的连接数-->
          <property name="acquireIncrement">5</property>
          <!-- 初始的连接数 -->
          <property name="initialPoolSize">10</property>
          <!-- 最小连接数 -->
          <property name="minPoolSize">5</property>
         <!-- 最大连接数 -->
          <property name="maxPoolSize">50</property>
  
          <!-- 可连接的最多的命令对象数 -->
          <property name="maxStatements">5</property> 
  
          <!-- 每个连接对象可连接的最多的命令对象数 -->
          <property name="maxStatementsPerConnection">2</property>
        </named-config>
      </c3p0-config>
  	// 通过配置模板完成调用
      public void demo() throws  Exception{
              ComboPooledDataSource lyx = new ComboPooledDataSource("mysql"); 
              Connection connection = lyx.getConnection();
              System.out.println("ok");
              connection.close();
          }
      注意
      	new ComboPooledDataSource("mysql"); 中，查找路径默认从src目录下开始，且输入的值要和<named-config >标签的name属性值一致，这样才能正确的引用到该配置文件
      	
   
   Druid(德鲁伊)连接池(需要通过 druid.properties配置文件连接)
   	// druid.properties
   	#用户名
      username=root
      #密码
      password=lyx
      #=连接数据库路径
      url=jdbc:mysql://localhost:3306/mySQl?rewriteBatchesStatements=true
      #url=jdbc:mysql://localhost:3306/mySQl
      #连接的数据库启动包
      driverClassName=com.mysql.jdbc.Driver
      #初始连接数
      initialSize=10
      # 最小连接数
      minIdle=5
      # 最大连接数
      maxActive=20
      # 超时报错,单位ms
      maxWait=5000
      
      // 通过引入 druid.properties 完成连接
       public static void main(String[] args) throws Exception {
          // properties对象加载 Druid.properties包
          Properties properties = new Properties();
          properties.load(new FileReader("src\\druid.properties"));
          
          //  创建一个Druid连接池，(该连接池解析了properties加载的druid配置文件，并根据配置文件连接数据库并登录)
          DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
          Connection connection = dataSource.getConnection(); // 获得连接
          System.out.println("连接了Druid线程池");
          connection.close(); // 关闭连接映射
      }
  
  注意
  	druid连接池最快最高效，50万次连接只需要 500ms左右
  	连接池关闭连接指断开连接引用(映射),普通连接断开连接指关闭接口，两者调用的方法不同，实现也不同
  */
  ```
  
  ![image-20220127171245737](D:\typora_import_images\typora-user-images\image-20220127171245737.png)
  
  
  
  
  
  
  
+ Apache-DBUtils

  ```java
  /*
   Apache-DBUtils是以调用 xx.set方式映射数据到指定类的，且调用的是该数据库对象类的空构造器,所以query查找到的字段要和数据库对象中的属性要对应，这样才能调用正确的setXx方法并存储倒该属性上
   当从数据库获取(select)数据时，返回的是一个ResultSet，它的生命周期和连接的生命周期一致(最长)，当连接关闭时该ResultSet也就关闭了(ResultSet也可以自己先关闭，但生命周期最长和连接的一致)，因此,为了数据持久化，我们需要创建一个ArrayList<类名(表名)>和一个类用于持久化保存其每一个数据到ArrayList中。(Apache-DBUtils完美实现了这个策略)
   
  存储大致实现
   	 public static void main(String[] args){
          try {
              Connection connection = JDBCUtilDruid.getConnection();
              String sql = "select * from stu1.actor ";
              PreparedStatement preparedStatement = connection.prepareStatement(sql);
              ResultSet resultSet = preparedStatement.executeQuery();
              ArrayList<Actor> actors = new ArrayList<>();
              while(resultSet.next()){
                  int actor_id = resultSet.getInt("actor_id");
                  String name = resultSet.getString("name");
                  String sex = resultSet.getString("sex");
                  String birthday = resultSet.getString("birthday");
                  String telephone = resultSet.getString("telephone");
                  actors.add(new Actor(actor_id,name,sex,birthday,telephone));
              }
              JDBCUtilDruid.close(resultSet, preparedStatement,connection);
              for (Actor actor : actors) {
                  System.out.print(actor._id+"\t"+actor.name+"\t"+actor.sex+"\t"+actor.birthday+"\t"+actor.telephone);
                  System.out.println();
              }
              System.out.println("结束了...");
          } catch (Exception e) {
              e.printStackTrace();
          }
  
      }
      public static class Actor{
          private int _id;
          private String name;
          private String sex;
          private String birthday;
          private String telephone;
  
          public Actor(int _id, String name, String sex, String birthday, String telephone) {
              this._id = _id;
              this.name = name;
              this.sex = sex;
              this.birthday = birthday;
              this.telephone = telephone;
          }
      }
      
  Apache-DBUtils
  	基本介绍
  		commons-dbutils 是 Apache 组织提供的一个开源 JDBC工具类库，它是对JDBC的封装，使用dbutils能极		大简化jdbc编码工作量
  		QueryRunner类：改类封装了SQL的执行，是线程安全的，可以实现CRUD和批处理
  		ResultSetHandler接口：该接口用于处理 java.sql.ResultSet,将数据按要求转换为另一种形式
  			ArrayHandler:把结果集中的第一行数据转换成对象数组
  			ArraayListHandler: 把结果集中每一行数据转换成一个数组，并存放到List中
  			BeanHandler:将结果集中的每一行数据封装到一个对应的JavaBean实例中
  			BeanListHandler:将结果集中的每一行数据封装到一个对应的JavaBean实例中，并存放到List里
  			ColumnListHandler:将结果集中某一列的数据存放到List中
  			KeyedHandler:将结果集的每行数据都封装到Map中,再把这些map存放到一个map里,其key为指定的key
  			MapHandler:将结果集中的第一行数据封装到一个Map中，key为列名，value为对应的值
  			MapListHandler:将结果集中的每一行数据封装到一个Map里，在存放到List中
  	
  	使用
  		// 使用 queryRunner.query() 方法 完成查找(select)
  		 public static void main(String[] args) {
          try {
              Connection connection = JDBCUtilDruid.getConnection();
              QueryRunner queryRunner = new QueryRunner();
              String sql = "select * from stu1.actor where sex=?";
             
                //  query方法就是执行sql语句，得到resultset 并封装到 ArrayList 集合中返回
                //  第一个参数 为 connection 指把连接引用传递给query方法
                //  第二个参数 为 sql 语句 ，指要执行的sql语句
                //  第三个参数 为 new BeanListHandler<>(Xxx.class) 指要把数据赋予的对象并存入到集合中
                //   第四个参数 为 可变参数... 配合 sql 语句的 ? 进行数据插入，prepareStatements有几个?就可以传入几个插入
              List<Actor> list = queryRunner.query(connection, sql, new BeanListHandler<>(Actor.class), "女");
              for (Actor actor : list) {
                  System.out.println(actor.getId() + "\t" + actor.getSex() + "\t" +
                          actor.getName() + "\t" + actor.getBirthday() + "\t" + actor.getTelephone());
              }
              JDBCUtilDruid.close(null, null, connection);
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
      注意
      	用queryRunner.query()存储的数据，如果没有查询某一行数据则该数据对象属性中不会赋值(为初始值)
      	queryRunner.query()底层会得到resultSet和PreparedStatement,且把数据封装到集合中后会在query关		 闭它们，所以我们只用关闭(或断开映射)connect
      	通过向queryRunner.query()中的第三个参数传入不同的对象映射可以执行不同的搜索查找
      		当传入 new BeanListHandler<>(Xxx.class) 可以返回多行多列的select结果(集合) 使用场景:数据		    检索
      		当传入 new BeanHandler<>(Xxx.class) 可以返回单行多列的select结果(对象) 使用场景:登录验证
      		当传入 new ScalarHandler() 可以返回一个单行单列的数据(对象) 使用场景：通过信息查找人名
      	如果在进行查找时，没有查找到匹配的数据则返回的是一个null值
      	
      // 使用apache完成 dml功能
      public void dml(){
          try {
              Connection connection = JDBCUtilDruid.getConnection();
              QueryRunner queryRunner = new QueryRunner();
              String sql = "delete from stu1.actor where sex=? and telephone like ?";
  
              // update 最多可以填写 3 个参数，最后返回受影响的行数
              // (1) connection连接
              // (2) 需要执行的sql语句
              // (3) 为一个可变参数，根据 你sql 语句的 ? 数量，参数可以往后累加(最后由可变参数接收)
              // 注意: queryRunner底层使用的是 PreparedStatement
              int affectedRows = queryRunner.update(connection, sql, "女", "18%");
              System.out.println(affectedRows >0?"修改成功":"修改失败");
  
          } catch (Exception e) {
              e.printStackTrace();
          }
  
      }
      
  注意
  	和数据库名对于的类我们称为 javabean,domain或pojo
  */
  ```
  
  ![image-20220128161405300](D:\typora_import_images\typora-user-images\image-20220128161405300.png)



+ BasicDao

  ```java
  /*
   apache-dbutils+Druid 简化了JDBC开发，但还有不足：
   	SQL语句是固定的，不能通过参数传入，通用性不好，需要进行改进，以方便执行crud操作
   	对于 select 操作，如果由返回值，返回类型不能固定，需要使用泛型
   	以后表很多且业务需求复杂，不能只使用一个Java类完成
   	
  BasicDao
   	DAO(datga access object)称为 数据访问对象
   	通用的数据库对象类我们称为 BasicDao,专门和数据库交互，即完成通用的对数据库的crud操作
   	在BasicDao的基础上实现一张表对应一个Dao,更好的完成功能,如Customer表-Customer.java类-CustomerDao.java
   
   注意
   	BasicDao类可以看作是一个sql管理对象，管理着所有的通用的sql操作，XxxDao为各自表的特有操作。然后XxxDao继承BasicDao即可完成操作链路
   	domain-dao-test-util 包分别管理者BasicDao-XxxDao-测试类-工具类，类似vue中的组件分级
   	BasicDao-XxxDao 由程序员完成
   	BasicDao-XxxDao 可以看作是 apache-dbutils+Druid的封装，使用我们更简单，方便的复用crud
   	
   使用
   	// BasicDao类
   	// 泛型可以接收一个引用数据类型，当其被一个实例对象(集合，继承接口，接口，继承类)指定了确定的数据类型后，它相对于该实例对象(集合，继承接口，接口，继承类)的数据类型就是传入的数据类型
   	public class BasicDao<T> {
      private QueryRunner qr = new QueryRunner();
  
      // 返回多行数据
      public List<T> queryMulti(String sql, Class<T> clazz, Object... parameters) {
          Connection connection = null;
          try {
              connection = JDBCUtilDruid.getConnection();
              return qr.query(connection, sql, new BeanListHandler<T>(clazz), parameters);
          } catch (Exception e) {
              new RuntimeException(e);
          } finally {
              JDBCUtilDruid.close(null,null,connection);
          }
          return null;
      }
  
      // 返回单行数据
      public T querySingle(String sql, Class<T> clazz, Object... parameters){
          Connection connection = null;
          try {
              connection = JDBCUtilDruid.getConnection();
              return qr.query(connection,sql,new BeanHandler<T>(clazz),parameters);
          } catch (Exception e) {
              new RuntimeException(e);
          } finally {
              JDBCUtilDruid.close(null,null,connection);
          }
          return null;
      }
  
      // 返回单行单列数据
      public Object queryOneProperties(String sql,Object... parameters){
          Connection connection = null;
          try {
              connection = JDBCUtilDruid.getConnection();
              return qr.query(connection, sql, new ScalarHandler(),parameters);
          } catch (Exception e) {
              new RuntimeException(e);
          } finally {
              JDBCUtilDruid.close(null,null,connection);
          }
          return null;
      }
      // 执行dml操作
      public int update(String sql,Object... parameters){
          int row = 0;
          Connection connection = null;
          try {
              connection = JDBCUtilDruid.getConnection();
              return qr.update(connection, sql, parameters);
          } catch (Exception e) {
              new RuntimeException(e);
          } finally {
              JDBCUtilDruid.close(null,null,connection);
          }
          return row;
      }
  }
  	
  	// ActorDou 类
  	// 泛型在继承类,实现接口,实例化对象，生成集合时都可以传入，不传则默认为Object
  public class ActorDao extends BasicDao<Actor> { // BasicDao中的泛型相对于ActorDao为Actor类型，且在此类被实例化对象后，该父类的泛型也会以Actor类型被压入地址
      // 此类为细化的类，实现特有的sql操作
  }
  
      // 对 该BasicDao类的使用
      public class DaoTest {
      public static void main(String[] args){
          ActorDao actorDao = new ActorDao();
          List<Actor> actors = actorDao.queryMulti("select * from stu1.actor where name like ?", Actor.class, "汤%");
          System.out.println(actors);
          Actor actor = actorDao.querySingle("select * from stu1.actor where name like ?", Actor.class, "汤%");
          System.out.println(actor);
          int update = actorDao.update("update stu1.actor set name=? where name=?", "汤钰烟", "汤新");
          System.out.println(update >0 ? "修改成功":"修改失败");
          Object o = actorDao.queryOneProperties("select name from stu1.actor where telephone like ?", "176%");
          System.out.println(o);
      }
  }
  
  根据下图页面可分为多个包 ，DAO包下存储BasicDAO,ActorDAO等，domain包存储各数据库对象类(入对应表的字段类型一致),utils包 存储各个工具类，test包 存储测试代码
  */
  ```
  
  ![image-20220130112429910](D:\typora_import_images\typora-user-images\image-20220130112429910.png)
  
  ![image-20220129142442287](D:\typora_import_images\typora-user-images\image-20220129142442287.png)
  
  ![image-20220129143536414](D:\typora_import_images\typora-user-images\image-20220129143536414.png)
  
  
  
  

+ java多表查询(数据库对象类)

  ```java
  /*
   当java需要多表查询时，可以创建一个MuitiTableBean&&MuitiTableBeanDAO&&MuitiTableBeanService,其中在MuitiTableBean中创建可能用到的对应属性(提供getter&setter方法以及空构造器)，当apach-Utils进行查找时，会往该数据库对象下对应的属性里(和查询结果相同的字段下调用setXxx())填充查询信息(没填充的数据为初始值)，此时我们只需要在其类中提供方法返回需要用到的结果即可
   
   使用
   	class MultiTableBean {
          private String name;
          private Integer age;
          private Date time;
          // 如果有新的多表查询属性直接添加到该类下然后提供对应的getter,setter和获取需要的属性的方法即可
  
          // 返回姓名和年龄
          public String getMes(){
              return "mes:"+name+" "+age;
          }
          // 返回姓名，年龄和时间
          public String getAll(){
              return "mes:"+name+" "+age+" "+time;
          }
  
          public String getName() {
              return name;
          }
  
          public Integer getAge() {
              return age;
          }
  
          public Date getTime() {
              return time;
          }
  
          public MultiTableBean(String name, Integer age, Date time) {
              this.name = name;
              this.age = age;
              this.time = time;
          }
  
          public MultiTableBean() {
          }
  
          public void setName(String name) {
              this.name = name;
          }
  
          public void setAge(Integer age) {
              this.age = age;
          }
  
          public void setTime(Date time) {
              this.time = time;
          }
  }
   
   注意
   	没有被填充的数据默认为初始值(int为0,boolean为false,String为null),因此为了使初始值容易分辨，基本数据类型需统一用包装类声明(Integer,Double等，包装类型没有给初始值为自动为null)
   	当该MuitiTableBean中含有了过多的属性后，可以将MuitiTableBean进行拆分，分为MuitiTableBeanXxx,MuitiTableBeanZzz等
   	在apach-utils中，在使用query查找到数据到数据后，会根据查找到的数据的列名去调用传入对象的对应属性的setXxx()方法进行存储(如查找的字段为name,则调用的是该数据库对象下的setName()方法,即将该查找字段的数据保存到该实例对象的name属性中)
   	在多表查询中，如果查询结果有两个或两个以上的相同字段的列(此时他们调用的方法相同，相同的属性覆盖存储列中的数据，造成数据丢失)，此时就可以给这个些相同的列起一个别名，这样就改变了他们的列名(此时调用了不同的方法，数据库对象类中有不同的字段可以存储该数据)
   	在apach-utils中，使用query查找到数据时返回的是一个对象(单行)或者列表对象(多行)，所以每个查询语句用的实例对象互相独立，每个实例对象存储的内容由该查询语句决定
  */
  ```

  ![image-20220203211818640](D:\typora_import_images\typora-user-images\image-20220203211818640.png)





## 3.0 正则表达式

+ 正则的匹配一定是连续的满足匹配条件的字符

+ 正则表达式就是匹配字符串的一个公式，有了正则表达式可以快速的从一个字符串中提取出我们需要查找的信息

+ java中用使用 ```\\```表示一个```\```(因为其需要被String包裹,需要使用转义字符转移)，其他语言只需要一个\因为他们没用字符串包裹

  ```java
  /*
    //(  匹配一个(
    //.  匹配一个.
    //$  匹配一个$
    
   注意
   	. * + ( ) $ / \ ? [ ] ^ { } 都需要使用转义字符，但当他们在[ ]中时不需要转义,可以直接写为[.* + ( ) $ / \ ?  ^ { }]
  */
  ```

  



+ 正则表达式底层查找规则

  ```java
  
   public static void main(String[] args){
          String str = "钉宫理惠，出生于大阪府，成长于熊本县熊本市，日本的女性声优、歌手，所属事务所为I'm Enterprise。\n" +
                  "代表作有《钢之炼金术师》阿尔冯斯·艾尔利克、《灼眼的夏娜》夏娜、《龙与虎》逢坂大河、《旋风管家！》三千院凪、" +
                  "《零之使魔》露易丝、《银魂》神乐、《妖精的尾巴》哈比、《绯弹的亚里亚》 神崎·H·亚里亚、《偶像大师》水濑伊织、" +
                  "《染红的街道》片桐优姬、《露蒂的玩具》亚斯塔露蒂·尤各瓦尔、《京骚戏画》筝、《血界战线》玛丽·麦克白、《天才麻将少女》" +
                  "片冈优希、《乐园追放》 安吉拉·巴尔扎克 、《王者天下》河了貂、《心跳！光之美少女》圆亚久里、" +
                  "《Tokyo Ghoul》铃屋什造、《NO GAME NO LIFE》特图、《十二国记》泰麒、《隐王》六条壬晴、" +
                  "《女神异闻录4》久慈川理世、《战刻夜血》结月、《野良神》野良、《海猫鸣泣之时》纱音、" +
                  "《崩坏3》琪亚娜·卡斯兰娜、《Fate/Grand Order》 织田信长、《巴哈姆特之怒》斑比、《碧蓝幻想》碧。\n" +
                  "钉宫理惠出道十几年，诠释过上百种角色，她以音调高而带鼻音的声音特色，完美诠释多位有“傲娇”（高傲却会害臊）特质的" +
                  "动漫人物，如《灼眼的夏娜》的夏娜、《零之使魔》的露易丝，受封为“傲娇女王”。这些可爱的少女角色深深吸引动漫迷，日本" +
                  "甚至出现“钉宫病”这个名词，形容粉丝对钉宫声音着迷到就像生病般的心情。";
          Pattern pattern = Pattern.compile("(\\w\\w)(\\w\\w)"); // 设置正则匹配规则
          Matcher matcher = pattern.matcher(str); // 设置需要正则匹配的字符串
  
          /**
           * matcher.find()源码分析
           * matcher.find()用于匹配满足正则规则的字符串，当匹配到后，
           * 会将其索引放到 Matcher类下的一个属性 int[] groups数组里，
           * groups[] 中，groups[0] 存储着被匹配字符串索引的开始位置，
           * groups[1] 存储着被匹配字符串索引结束位置(实际为结束索引位置+1，因为其内部使用subString方法截取[1,n))
           * groups[2]-groups[3]存储着被匹配字符串(正则第一组)的开始和结束位置(实际为结束索引位置+1)
           *  groups[4]-groups[5]存储着被匹配字符串(正则第二组)的开始和结束位置(实际为结束索引位置+1)...以此内推
           *
           * 注意
           *      1.groups[] 数组初始大小为 20个索引 。每次正则匹配时groups[]只会存储本次查找的字符串索引信息
           *      2.matcher类下的 int first(记录开始位置); int last(记录结束位置); int oldLast(记录上一次结束位置);  会记住本次查找到的索引位置,下一次匹配就会从记录位置+1开始继续正则匹配
           *      3.分组指对传入的正则进行括号包裹，这时就完成分组，如"(\\w\\w)(\\d\\d)" ,(\\w\\w)为第一组，(\\d\\d)为第二组
           *    例 匹配了一个为 ew12的字符串 ，  matcher.group(0)为ew12,matcher.group(1)为ew,,matcher.group(2)为12 ，假设ew12
           *    的开始索引位置为0 ，则 groups[0] 为 0 groups[1]为 4   groups[2]为 0 groups[3]为2 groups[4]为2 groups[5]为4
           * */
          while(matcher.find()){
              /** group源码分析
               *    public String group(int group) {
               *         if (first < 0)
               *             throw new IllegalStateException("No match found");
               *         if (group < 0 || group > groupCount())
               *             throw new IndexOutOfBoundsException("No group " + group);
               *         if ((groups[group*2] == -1) || (groups[group*2+1] == -1))
               *             return null;
               *         return getSubSequence(groups[group * 2], groups[group * 2 + 1]).toString();
               *     }
               * 当传入0时拿到的就是 groups[0]和groups[1] 即得到本次正则匹配的结果
               * 当传入1时拿到的就是 groups[2]和groups[3] 即得到本次正则匹配的第一组的结果
               * 当传入2时拿到的就是 groups[4]和groups[5] 即得到本次正则匹配的第二组的结果...以此类推
               *
               * 注意
               *      在使用matcher.group(n) 拿到分组的查询结果时，不能越界
               * */
              System.out.println(matcher.group(0));
              System.out.println(matcher.group(1));
              System.out.println(matcher.group(2));
          }
  
      }
  ```
  
  
  
+ 正则表达式语法

  ```java
  /*
   元字符分为以下类
   	限定符，选择匹配符，分组组合和反向引用符，特殊字符，字符匹配符，定位符
   	
   	匹配符
          [], 可接收的字符列表,[abc] 表示可接收abc中任意一个字符
          [^],不可接受的字符列表,[^abc] 表示接收除了abc以外的字符
          -，连字符,a-zA-Z (表示a,b,c....z,A,B,C....Z)表示接收任意字符
          ., 匹配除了\n 以外的任何字符,a.b 表示接收以a开头以b结尾，中间接收任意一个字符，如a1b,a2b,awb
          \\d,匹配单个数字字符，相当于匹配0-9
          \\D,匹配单个非数字字符，相当于[^0-9]
          \\w,匹配单个数字，大小字母和下划线，相当于[0-9a-zA-Z_]
          \\w,匹配单个非数字，大小字母和下划线，相当于[^0-9a-zA-Z_]
          \\s,匹配任何空白字符
          \\S,匹配任何非空白字符
          |，匹配 | 之前或之后的表达式 ，ab|cd 表示匹配ab或cd
          
   	限定符
          *,表示前一个字符或字符串重复0次或多(n)次 
          +,表示前一个字符或字符串重复1次或多(n)次
          ?,表示前一个字符或字符串重复0次或1次
          {n},指定前一个字符或字符串出现的次数  [abc]{3} 匹配 aab bcc aaa 等
          {n,}指定前一个字符或字符串至少出现n次(最多无限次),[abc]{3,} 匹配 aaaa,baca 等
          {n,m}指定前一个字符或字符串出现的次数大于等于n次小于等于m次, [abc]{2,4} 匹配 aa abc ccac
              注意
                  如果是限定字符串需要将前面多个字符用()包裹，这时他们就是一个整体
   			
   	定位符
   		^ 指定开始字符 ，^[0,9]+[az]* 以数字开头后接任意小写字母, 匹配 123 6aa 55ef
   		$ 指定结束字符，^[0,9]\\-[a-z]+$ 表示匹配以数字开头以字母结尾的字符串 ,匹配 1-w,3-eew 等
   		\\b 匹配目标字符串的边界(边界指它的下一个字符为空格或为该字符在字符串末尾)，如 hh\\b , 在字符串 wwhh hh中， 匹配ww后的hh 和结尾的hh 
   		\\B 匹配目标字符串的非边界值
   注意
   	String regStr = "(?i)abc";表示匹配不区分大小写的abc ，如果字符不使用()或其他限制符包裹，并基于此使用(?i)或其他限制符，表示(?i)或其他限制符影响右边一系列非括号的字符 ，如 a(?i)bc 表示a匹配小写bc大小写都匹配，a((?i)b)c 表示ac小写 b大小写都匹配
   	使用 Pattern.compile(regStr,Pattern.CASE_INSENSITIVE); 也可实现不区分大小写
   	java匹配默认使用 贪婪匹配法，即尽量多匹配,如  reg ="\\w{3,5}" str ="2231we" 会默认多匹配为2231w
  */
  ```
  
  

+ 分组

  ```java
  /*
   常用分组方式有两种，非命名捕获和命名捕获，非命名捕获只能用索引获得分组(第一组为1，第二组为2...)，命名捕获可以用索引或者名称获得分组(第一组为1，第二组为2...)
   非命名捕获 (pattern)  (\d\d)(\w\w)
   命名捕获 (?<name>pattern) (?<g1>\d\d)(?<g2>\w\w)
   
   特别分组(非捕获分组)
   	?:pattern 匹配pattern但不捕获该匹配结果(即匹配结果但不分组)，通常充当 |，如 industr(?:y|ies),表示匹配 industry , industries，
   	?=pattern 匹配pattern但不捕获该匹配结果(即匹配结果但不分组)，如 老王(?=真帅|真丑) 在字符串中有 "老王真美，老王真丑，老王真帅" ，如果匹配到的结果包含老王真帅或者 老王真丑就返回老王
   	?!pattern 匹配pattern但不捕获该匹配结果(即匹配结果但不分组)，如 老王(?！真帅|真丑) 在字符串中有 "老王真美，老王真丑，老王真帅" ，如果匹配到的结果不包含老王真帅 或者老王真丑就返回老王 
   
  注意
  	使用()进行分组，且从左向右 第一个()为第一组 ，第二个()为第二组 ，如 ((\\d)\\d)((\\w)\\w) ((\\d)\\d)为第一组 (\\d)为第二组 ((\\w)\\w) 为第三组  (\\w) 第四组
  	matcher.group()方法是重载的 ，形参可以接收一个字符串(分组名)或一个数字(分组索引)
  */
  ```

  

+ 贪婪匹配&非贪婪匹配

  ```java
  /*
   正则匹配默认使用贪婪匹配(尽可能多匹配)，如果需要它尽可能少匹配，就需要使用非贪婪匹配(在限定符后加上?)
   	贪婪匹配
   		String str = "12junk 32e"; Pattern compile = Pattern.compile("\\d+"); 则匹配结果为 12 32
   	非贪婪匹配
   		String str = "12junk 32e";Pattern compile = Pattern.compile("\\d+?");则匹配结果为 1 2 3 2
  */
  ```

  

+ 正则表达式常用类

  ```java
  /*
   java.util.regex 包主要包括以下三个类 Pattern 类，Matcher类和PatternSyntaxException类
   
   Pattern类 常用方法
   	 matches(regStr,content) 该方法可以快速使用正则去匹配字符串中的内容，如果完整匹配则为true，否则为false。完整匹配指正则完美匹配字符串内容没有多余遗漏字符 如System.out.println(Pattern.matches("\\d+","123456")); // true
  System.out.println(Pattern.matches("\\d+","123456ee")); // false
   	 
  Matcher类 常用方法
  	start() // 获得当前正则匹配字符的开始索引位置
  	end() // 获得当前正则匹配字符的结束索引位置 ,有了这两个方法我们可以自己去从字符串中截取内容
  	matches() // 该方法可以快速使用正则去匹配字符串中的内容，如果完整匹配则为true，否则为false。完整匹配指正则完美匹配字符串内容没有多余遗漏字符 
  	replaceAll("指定字符串") // 将所有匹配结果替换为指定字符串 
  */
  ```

  

+ 反向引用

  ```java
  /*
   圆括号的内容被捕获后，可以在这个括号后被使用，从而写出一个比较实用的匹配模式，这个我们称为反向引用，这种引用可以在正则表达式内部，也可以在正则表达式外部，正则内部反向引用为 \\分组号，正则外部反向引用为 $分组号
   
   使用
   	匹配两个连续相同数字  (\\d)\\1   
   	匹配五个连续相同数字 (\\d)\\1{4}
   	匹配个位与千位相同，十位与百位相同 (\\d)(\\d)\\2\\1
   	
   	String str = "23412-111222333";
      System.out.println(Pattern.matches("\\d{5}-(\\d)\\1{2}(\\d)\\2{2}(\\d)\\3{2}",str));
      
      去掉连续重复字符
        String str = "223311234112333";
        String content = Pattern.compile("(.)\\1+").matcher(str).replaceAll("$1");
        System.out.println(content); // 231234123
   	
   注意
   	\\1 表示 1 组匹配的内容 \\2 表示 2 组匹配的内容 ... ，如 (\\d)(\\d)\\2\\1 在第一组和第二组确认匹配结果后 3 2,那其 \\2 \\1 引用 也确认了结果 ，最终的正则表达式为 3223
  */
  ```

  

+ String类使用正则表达式

  ```java
  /*
   String 类 
   	public String replaceAll(String regex,String replacement){} ,将满足正则规则的字符串替换为指定字符串
   	使用
   		String conent = "1239987".replaceAll("^123","w");
   	public boolean matches(String regex){} ,判断该字符串是否完整满足正则匹配条件
   	使用
   		System.out.println("13823128791".matches("1(38|39)\\d{8}")); // true
   	public String[] split(String regex){}，使用满足正则规则的字符串来分隔数组
   	使用
   		String content = "hello#avc-jwe12smith";
   		String[] split = content.split("#|-|~|\\d+");
  */
  ```

  
  
  

