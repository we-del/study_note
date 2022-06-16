# JS（ECMA）

## 规范

+ 注意

  ```java
  /*
  1.form表单包裹的内容默认会向指定地址发出请求，可指定其的 onsumbit 事件,使用其环境变量 e.preventDefault()阻止其默认行为(提交请求是浏览器默认行为)
  2. 一个事件绑定在一个标签后，只有在该标签内触发了这个事件，该事件才会出发
  3. js设置 css3样式时(transform等)，需要使用字符串包裹，否则会当作函数解析
  	如 img.style.transform = "scale(1, 1)";
  4. 解构赋值可以对 对象和实例对象进行解构
  5. js中对象里的key直接写时默认当字符串处理
  	如 obj={a:2} ;obj.a = 1 ; // 以上的a会被当作字符串处理，
  	如果想要其当作js表达式处理就需要使用[]包裹，如下
  		let d = "ddd"
          let obj = { a: 2, b: 3, [d]: 5 };
          let c = "we";
          obj[c] = 4;
  
  6. string类型转number类型的方式
  	使用 ParseInt() ， 入 parseInt("2")+1 === 3 true
  	使用 *1  , 如  "1" * 1  === 1 true
  
  7. js中 一个 = 是指向(赋值) , 两个 == 是判断隐式转换后的布尔值是否相等 , 三个 === 是判断隐式转换后的布尔值和两个值的数据类型是否相等
  */
  ```

  

+ 规范

  ```java
  /*
   1. IIFE 指匿名函数自调用
  */
  ```

  

+ 集合

  ```java
  /*
   在做if判断是尽量使结果为一个boolean值，而不是靠隐式转换
   	如  if(1){}  // 不推荐      if(1 == 1){} // 推荐
  =============================== 	
  js中函数也有地址，可以当作参数传递
  =================================
  */
  ```

+ 执行顺序

  ```java
  /*
     (1) 基本执行顺序
      js执行是单线程的，也就是说 它执行代码的顺序只能从上到下顺序执行,当执行到了一个表达式时会去判断它的任务类型，并放入指定的队列
      因为这个特性，js把不同的数据分为了不同的队列执行，比如简单任务队列，复杂任务队列,宏队列，微队列
      简单任务队列专门存储执行引入，赋值，函数调用等 ， 复杂任务队列专门存储 函数 ，事件函数，宏队列专门存储 定时器，微队列存储Promise(不包括 new Promise()方式)
     简单任务队列随进随执行，复杂任务队列任务在触发时才会执行，宏队列里的任务在达到了指定条件才会执行，微队列里的任务完成了一次调用就执行
      执行顺序 简单任务(函数调用，生成实例对象)->微队列->宏队列  
  
  注意
  	简单任务是随扫随执行，其余任务则会放入对应的队列等待执行，待上下文扫描完毕后(此时简单任务已经执行完毕)，在按顺序执行微队列，宏队列里的任务。如果微队列或宏队列里还有简单任务，则会按顺执行
  	相同优先级的语句按链级顺序执行，1-n的链级顺序，1先执行，执行完后到2 ...再到
      
  ==============================================
         // 执行顺序
      Promise.resolve(5).then(value => console.log(value)); // 4(微任务+函数调用)
  
      new Promise((resolve, reject) =>{ // (实例对象)
          setTimeout(()=>{ 
              console.log("我是 promise 里的定时器"); // 5
          });
          console.log("我是promise"); // 1
  
      });
      let a = ()=>{
          console.log("我是全局的a() 方法"); // 2
      }
      setTimeout(()=>{console.log("我是全局 setInterval()");}) // 6
      a();
  
      console.log("我是全局"); // 3
  ====================================================
   // 执行顺序 1 7 2 3 8 4 6 5 0
      setTimeout(()=>{
          console.log("0");
      })
      new Promise((resolve, reject)=>{
          console.log("1");
          resolve();
      }).then(()=>{
          console.log("2");
          new Promise((resolve, reject)=>{
              console.log("3");
              resolve();
          }).then(()=>{
              console.log("4");
          }).then(()=>{
              console.log("5");
          });
      }).then(()=>{
          console.log("6");
      });
      new Promise((resolve, reject)=>{
          console.log("7");
          resolve();
      }).then(()=>{
          console.log("8");
      });
  */
  ```
  
  

## 细节

+ 注意

  ```java
  /*
   1. es6的对象简写 是语法糖，当对象只指定属性指传入一个变时，底层会进行展开
   	如 name = "2", {name} == {name:"2"}
  */
  ```

  

+ 集合

  ```java
  /*
   es6 解构复制 
   	 	使用
   	 		let obj = {
                  a: {
                      b: {
                          price: 1.1
                      }
                  }
              }
              let {a} = obj;
              console.log(a);  // 对象的结构
  
              let arr = [[0, 1], 2, 3];
              let [index0, index1] = arr;
              console.log(index0, index1);
              console.log(index0 === arr[0]);
              console.log(index0,arr[0]);
              console.log(index1 === arr[1]);
              console.log(a === obj.a);
     注意
     		对象的解构只能进行一层解构(最外层的解构)，数组的解构可以多层解构
     		解构实际上是创建了一个变量直接引用该解构的点，所以他们的地址是相同的
     		数组的解构是按索引来的，对象的解构是按key值来的，因此对象解构是变量应和key值对象，而数组则可定义		 任意变量只需和数组中需要解构的值对应即可
     		如数组需要解构的点在数组的中间，则可以使用[,,,](,分隔占位)方式进行占位，来保证索引的偏移量	
     			如
     				let [,,two] = [0,1,2]; // 获取2的值
  				console.log(two);
     	
     	
  =====================================================================   
  2. undefined和null的判断 
  undefined == null // true   ， 因为其都没有地址指向在某种意义上来说是相等的
  undefined === null // false ， 其地址虽然相等，但数据类型不等
  注意
  	== 运算是判断 地址或值是否相等
  	=== 运算是判断地址或值，数据类型是否相等
  ====================================================================
  3. 在完成代码封装时，需要注意js特有箭头函数this的指向，如果形参需要接收一个函数，需要对其this指向做判断，再来适当编写。
  以下为promise封装时，箭头函数产生的问题
  // 简单 Promise重新写
  class CustomPromise {
      private state: any;
      private value: any;
      private reason: any;
      private curState: string = "pending";
  
      public constructor(fn: any) {
          this.state = {pending: "pending", rejected: "rejected", resolved: "resolved"};
          this.value = null;
          this.reason = null;
          
           // 因为js特有 箭头函数this会指向外层的包裹对象，
           //因此如果在创建实例对象时，使用箭头函数则需要保存一下this指向(因为其this会指向外层)
          // 在完成创建实例对象后，则不需要保存this,因为方法调用的this指向的是调用者
          
  
          let _this = this;
          // resolve状态
          function resolve(value: any): void {
              if (_this.curState == "pending") {
                  _this.curState = "resolved";
                  _this.value = value;
              }
          }
  
          // reject状态
          function reject(reason: any): void {
              if (_this.curState == "pending") {
                  _this.curState = "rejected";
                  _this.reason = reason;
              }
          }
  
          fn(resolve, reject);
      }
  
      // 写两个静态的方法
  
      // 返回一个resolve的回调
      public static resolve(value: any): CustomPromise {
          return new CustomPromise((resolve, reject) => {
              resolve(value);
          });
      }
  
      // 返回一个reject的回调
      public static reject(value: any): CustomPromise {
          return new CustomPromise((resolve, reject) => {
              reject(value);
          });
      }
  
  
      public then(fn1: any, fn2: any = null): CustomPromise {
  
       
        //当一次then完毕后 需要对返回值做判断，如果为 null 说明可以传递一个相同状态的为null的promise对象
        //如果又返回值需要对其类型做判断，如果是异常则需要调用Reject,。。。
        
       
          if (this.curState == this.state.pending) {
              return null;
          } else if (this.curState == this.state.resolved) {
              if (fn1(this.value) == null) {
  
                  return CustomPromise.resolve(this.value);
              }
  
          } else if (this.curState == this.state.rejected && fn2 != null) {
              if (fn2(this.reason) == null) {
                  return CustomPromise.reject(fn2(this.reason));
              }
          }
          return null;
      }
  
      public catch(fn1: any): CustomPromise {
          fn1(this.reason);
          return null;
      }
  }
  
  let cus: CustomPromise = new CustomPromise((resolve, reject) => {
      resolve(1);
      reject(2);
  }).then(value=>console.log(value));
  console.log(cus);
  ================================================
  4. 因为js特有技能(函数有地址且可以当作形参来传递)，而类中又只能用this.xx来表示当前实例对象的属性和方法，此时在该类方法当作形参传递时，this的指向也改变了(this指向的是调用者)，此时就需要将本实例对象也当作形参来传递(或者在类对象上维护一个当前实例对象的属性)，才能获取正确的值(或者在可能改变指向的局部保存一个当前实例对象的指向)
  
   具体解决办法
   class UIRouter {
      public router: any;
      public static _this:any; // 保存该实例对象的地址，以免其类方法在当作参数传递时，找不到该实例对象
  
      constructor() {
          let {Router} = require("express");
          this.router = new Router();
          this.setRoute();
          UIRouter._this = this; // 此時 _this
      }
  
      private setRoute(): void {
          this.router.get("/index", (req, res): void => {
              res.send("我接收到了消息");
          });
          this.router.get("/", (req, res): void => {
              res.send("I is index");
          });
      }
  
      public getRouter(): any { //该方法可能被其他对象调用，因此直接从类对象的静态属性上获得实例对象指向
          return UIRouter._this.router; 
      }
  }
  
  export {UIRouter};
  */
  ```
  
  

## 技巧

+ 实现深拷贝

  ```java
  /*
  // 利用 JSON拆装箱来实现深拷贝
   let arr = [1,2,3];
  let arr1 = arr;
  console.log(arr==arr1);
  arr1 = JSON.parse(JSON.stringify(arr));
  console.log(arr == arr1);
  console.log(arr1);
  console.log(arr);
  =======================================
  // Object.keys() 方法 接收一个 对象或数组，如果接收的是一个数组则返回数组的全部索引到一个数组中，如果接收的是一个对象则返回所有的key到一个数组中
  */
  ```

+ 集合

  ```java
  /*
   1. 当使用 LocalStore存储数据时，使用  JSON.stringify("")和JSON.parse有助于提高效率
   	localStorage.setItem("msg",JSON.stringify("吃了吗")); // 存
   	let msg = JSON.parse(localStorage.getItem("msg")); // 读
  */
  ```

+ 细节

  ```java
  /*
   1. LocalStore按域名的方式互相隔离数据，同域名(端口)之间的LocalStore数据共享
   2. js中函数也有自己的地址(因为其可以当作参数传递)
  */
  ```
  
  

## 1.0 js组成

ECMAscript 负责js的语法部分

DOM(document object model)   文档对象模 型(操作元素)

BOM(browser object model ) 浏览器对象模型(操作浏览器)

js代码从上往下执行

注释/**/&&// ,js可以以分号结尾，或不以分号结尾，推荐分号结尾

再javascript里定义的变量可以看作是一个指针，它指向的值是一个对象或者数组类型，当重新赋值(指向)后，值也会发生更改。普通函数的函数名也可以看作是一个指针指向一个函数，调用这个函数名(指针)就是调用它里面的函数，相同的，也可以对这个函数名(指针)的值(指向)进行修改，修改后原有的数值会被丢弃(释放)。

```js
/*
 数字字符串*数字可以把哪个字符串数组的类型转换为number型 如 '1' * 1 === 1 
*/
```



## 1.1 标准函数

+ alert() ，函数内的内容以弹出框的形式输出内容

+ sonsole.log() 函数内的内容在控制台输出

+ document.write() 函数内的内容在页面上输出

+ console.dir() 可以查看函数原型

+ 行内样式 onclick,用来给单个样式设置事件



+ 数据类型判断函数

  typeof(a)或 typeof a ，打印当前变量的数据类型，如果是null打印的为对象(object)

  ```js
  /*
  typeof 打印的结果为字符串
  typeof 可以判断出的数据类型有,number string boolean undefined function Object
  */
  var a=2;
  console.log(typeof typeof a); //结果为string
  ```
  
  

## 1.2 js书写方式

+ 三种方式

  ```javascript
  /*
  1.行内插入
  局限性较大，只能对事件进行书写js,做不到结果和行为分离
  <div onclick="alert(1)"></div>
  2.内嵌方式
  使用较多，一般些项目储器都是内嵌，最后变为外链
  使用script代码块书写，<script></script>
  3.外部引入
  使用script标签引入 <script src="love.js">
  */
  ```

## 1.3 变量&&关键字

+ 变量(可以改变的量)
+ 常量(不可改变的量)

+ 变量命名规范,组成，大驼峰，小驼峰，下划线

+ 变量定义要使用var声明 ,不用var定义也可以操作，但要进行赋初值，变量建议先定义在使用
+ 变量要初始化，表示此变量要存储的数据类型

+ 变量由**数字，字母，下划线，$组成**,不能以数字开头，不能时关键字和保留字

+ 变量由大驼峰，小驼峰，下划线命名方式

  大驼峰 var ClassNumber   小驼峰 classNumber   下划线 class_number

+ 关键字和保留字

  ```javascript
  /*
  javascript关键字
  break case catch continue default delete do else finally for function if in instanceof new return switch this throw try typeof  var void while with
  
  javascript保留字
  abstract boolean byte char class const debugger double enum export extends final float goto implements import int interface long native package private protected public short static super syncharonized throws transient volatile
  */
  ```

  + 两种方式交换两个变量的值

    ```javascript
    /*第一种*/
    var num1 = 5;
    var num2 = 6;
    var temp;
    temp = num1;
    num1 = num2;
    num2 = temp;
    
    /*第二种*/
    var num1 = 5;
    var num2 = 6;
    num1+=num2;
    num2=mun1-num2;
    num1-=num2;
    
    /*第三种*/
    var num1 = 4
    var num2 = 3
     num1 = num1^num2
     num2 = num1^num2
     num1 = num1^num2
    ```
    
    

+ 数据类型

  基本数据类型和对象(复杂)数据类型

  ```javascript
  /*
  基本数据类型(5种) ，number ,string,boolean,undefined,null
  number: 整数  小数 科学计数法  2进制(0b) 8进制(0) 16进制(0x)
  
  字符串： 单引号或双引号包含 空字符串 和 空白字符串，如果字符串内出现单引号，外部包含需用双引号
  
  布尔：  true或false
  
  undefined  定义的变量没有赋值
  
  null 定义的变量赋值为null(对象操作使用)
  
  */
  ```

  

## 1.4 运算符和表达式

+ 表达式，由变量或常量和运算符组成的式子，表达式是有值的

+ 运算符，参与运算的符号

  ```js
  /*
   1.算术
   + - * / %(小数不能取余) ++(自增) --(自减)
   a++,++a。a++先运算后自增，++a先自增后运算
   2.赋值
    = = -= *= /= %=
    
   3.比较(返回的为boolean)
    > < >= <= == != ===(全等) !==(不全等)
    
    注意点：
   (1)string可以和number进行比较,在比较是string数据会自动转换为number值
   (2)===  !==是用来判断值和数据类型是否都相等(===也叫严格判断)
   
   (3)在比较中NaN和NaN没有可比性结果为NaN，其余的比较是先隐式转换为数值，然后结果转换为boolean
  
   4.逻辑(和c语言相同)
   && || ！  按位& | ^ ~
   5.三目(和c语言相同)
   ?:
  */
  ```
  
+ 数据类型转换

  ```js
  /*
   1.数据类型强制转换
   （1）Number()强制将一个其他类型转换为数据类型，转不了就为NaN(为数据类型，但不是数字) 
   如果字符串是一个数字则转换为对应数字类型的数字，如果是字符串转换为NaN,如果是空格转换为0。
   字符串可以为2进制，8进制，10进制，16进制数，若有其他值结果为NaN
   如果是boolean数据 true会转换为1，false会转换为0
   如果是undefined 会转换为NaN
   如果是null 会转换为0
   
   （2） String()强制将一个其他类型转换位字符串类型
   只要是什么值，都会转换为对应的字符串包括(undefined,null)
   
   
   （3）Boolean()强制将一个其他类型数据转换位boolean类型
   在转换字符串时，除了空字符串是false,其他全是true
   在转换数字时，除了0为false,其他全是true
   在转换undefined,和null是都是false
  */
  ```

+ 隐式转换

  ```js
  /*
   一.各种数据类型数据在进行运算是都会隐式转换对应可计算的值进行计算
   二.0不能做分母(被除数) 如 0/1 0  1/0 Infinity 0/0 NaN 。
   三.Number.MAX_VALUE 可以拿到最大值
   四.每个数在做运算时，都是优先去找可以是隐式转换的值然后进行运算，如没有找到则会运算错误显示NaN(No a number)
   五.所有的运算都为Number类型，除了字符串做加法时为字符串类型
   六.数据类型在做算术运算时，会转换为数值类型进行（特殊情况除外），在做关系判断时，比较值是一方面，比较底层逻辑分类也是一方面，只有两个都相同才为true
   七.小数运算会出现精度损失
   
  1.字符串转换
  (1) 字符串在做加法时，为拼接，做其他运算都为NaN(''空字符串为例外，''字符串做加法时，单纯的拼接，做其他运算时转换为0)
  
  (2) 字符串和任意类型在做加法时，都会把该数转换为字符串
  
  (3)字符串和字符串比较时，是比较每个位上的ASCII值，如果编码值相等，则比较下一位值
  
  (4)空字符串''在做+发时，优先按字符串处理，结果为一个字符串，在做其他操作时，会隐式转换为false也就是0进行运算
   
   (5) 字符串可以和其他数据类型做除加法以外的运算操作（前提是单纯的数字情况，可以是2，8，10，16进制），如果不是纯数字则无法隐式转换为NaN
   
   (6) 当字符串在和undefined做判断时，< > == 都为false,!=是为true
  2.boolean转换
   做运算时会隐式转换为0或1
   
   3.undefined
   做加，减，乘，除，取余时结果为NaN(除了和字符串做加法)
   做逻辑判断时，会隐时转换为0
   做关系判断时，为undefined值(结果(隐式转换)为NaN)，不会隐式转换
   
  4.null
   做运算会印时转换为0
   1>null true
   1<null false
   
   5.比较运算的特殊情况
   (1)null==0 false null虽然会隐式转换为0，但底层概念不同
   (2)null==undefined true null表示空对象，undefined表示对象为定义，在某种意义上说是同一个意思
   (3) null=='' false 特殊情况,空字符串和null不相等
   (4)0=='' true 都是基本数据类型
   (5)0==undefined false undefined为未定义值
   (6)NaN==NaN false
   (7) '0'做逻辑运算为true 0做逻辑运算为false
  */

+ 手动转换(在字符串中提取数字)

  ```js
  /*
  以下两个方法必须都是以数字开头且是连续的数字才会进行截取，一旦不是数字立刻退出
   1.parseInt()从字符串中提取整数
   
   2.parseFloat()从字符串中提取小数，如果取值取到第二个.则停止提取，立刻退出
   如(parseFloat("123.1234.23f")) 提取为123.1234
   
   3.prompt('字符串')，此函数会在页面中弹出一个可输入值的框，结果为string类型，通常配合parseInt()函数使用
  */
  ```

+ 小面试题

  ```js
  /*
  快速将一个字符串'23'转换为数字23
   Number('23');
   '23'-0;  这一种常用，不用调用函数，占用内部资源
   parseInt('23');
   parseFloat('23');
  */
  
  ```

  

## 1.5 流程控制语句

以下均和c语法一致

### 1.5.1 顺序结构

程序从上往下执行

### 1.5.2 选择结构

+ if

  ```js
  /*
   单分支
   if(),后好里的一般为表达式，最终只都会转换为boolean值
   双分支 
   if(){}else{}
  */
  ```

+ 多分支(if ...else if.... else if... else)

  ```js
  /*
  if的功能版，先判断if如果为真执行if，其后的else if....else不执行，如果if为假则执行其后的一个else if 语句，如果为真，执行此语句其后语句不执行，如果此语句也为假，继续向下执行，直到else if 都为假，执行else
  */
  ```



+ switch（和c语言语法一致）

  ```js
  /*
  1.switch(有值，最后的结果值不会进行转换，的出来的值会去对号入座case里的值)
  2.break,跳过离它最近的一层循环
  3.也有switch击穿
  switch(){
  	case 语句1: 代码块1;break;
  	case 语句2: 代码块2;break;
  	case 语句3: 代码块3:break;
  	default: 代码块;break;
  }
  
  switch(num){
  	case num>80&&num<100:alert(1);break;
  	case num>60&&num<80: alert(2);break;
  	default: alert(3);break;
  }
  */
  ```

  

  

### 1.5.3循环结构

+ for

  ```js
  /*
  for(表达式1；表达式2；表达式3)  ， 第一次运行执行表达式1，表达式2，如果表达式2为真，执行循环体和表达式3，在对表达式2进行判断，如果为真就执行循环体和表达式3，直到表达式2为假退出for循环
  */
  ```

+ while

  ```js
  /*
  while先判断后执行
  while(表达式)，如果这个表达式为真就执行下面得循环体
  */
  ```

+ do while

  ```js
  /*
  do-while先执行后判断
  do{
  	循环体
  }while(表达式);  
  限制性循环体一次，再去判断while,如果while为假退出do-while循环
  */
  ```

+ break和continue关键字

  ```js
  /*
  break退出离它最近得循环
  continue停止不执行continue下的语句，开始下一次循环
  */
  ```

  

## 1.6 数组

+ 定义

  数组概念，是一个具有相同类型或不同类型得数据有序集合

  ```js
  /*
  两种定义数组得方法
  1.arr=[1,2,3,4]; []创建
  2.arr = new Array(1,2,3,4); new 构造函数创建
  3. arr = Array(1,2); 构造函数当普通函数调用
  4. Array.of(3,2)
  如果你用构造函数方法定义一个数组，且这个数组只有一个变量，则这个变量代表定义的数组长度，而用[]创建的数组没有这种问题
  只要定义了数组，则就有默认的属性length,数组length属性，随着你长度得变化而变化
  */
  ```
  
+ 长度和索引

  ```js
  /*
  和c语言数组规则相似，就是在创建时不用指定创建空间大小，js的数组可以实时更新数组
   arr.length拿到长度
   arr[3]拿到指定下标
  */
  ```

+ 数组(增删改查)

  ```js
  //1.增
   //想要在数组末尾添加元素之间a[a.length]即可
  
   //再数组指定位置添加指定元素
          var arr = [1, 'wrong', 2, 'gender', 3, 'weight'];
          var i, j, x = 0;
          j = parseInt(prompt('Please Enter add position' + '<=' + arr.length));
          if (j == arr.length) arr[arr.length] = prompt("Please enter add's content");
          else {
              for (i = arr.length; i > j; i--) arr[i] = arr[i - 1];
              arr[j] = prompt("Please enter add's content");
          }
          console.log(arr)
  
  //2.删(使用arr.length--配合数组删除)
   //再数组指定位置删除指定元素
          var arr = [1, 'wrong', 2, 'gender', 3, 'weight'];
          var i, j, x = 0;
          j = parseInt(prompt('Please Enter add position' + arr.length + '<'));
          for (i = j; i < arr.length; i++) arr[i] = arr[i + 1];
          arr.length--;
          console.log(arr);
  
  //3.改和查
   // 查找&&修改元素
          var arr = [1, 'wrong', 2, 'gender', 3, 'weight'];
          var i, j, x = 0;
          j = parseInt(prompt('Please Enter add position' + arr.length + '<'));
          for (i = j; i < arr.length; i++){
             // if (arr[i] == 1) console.log(arr[i]);查
              //if(arr[i]==1)arr[i]=2; 改
          }
  
  //4.反转数组
          var arr = [1, 2, 3, 4, 5, 6];
          var i, j = 0, temp;
          for (i = arr.length - 1; i > j; i--) {
              temp = arr[i];
              arr[i] = arr[j];
              arr[j] = temp;
              j++;
          }
          console.log(arr);
  ```

+ 练习

  ```js
  //数组去重
  function arrayDeleteIdentical(arr) {
              var i, j = 0, count = 0;
              for (i = 0; i < arr.length; i++) {
                  for (j = i + 1; j < arr.length; j++) {
                      if (arr[i] != arr[j]) count++;
                  }
                  if (count != arr.length - 1) {
                      for (j = i; j < arr.length; j++)arr[j] = arr[j + 1];
                      arr.length--;
                  }
              }
          }
          var arr = [1, 1, 1, 2, 2, 3, 4, 5, 6, 6, 5, 4];
          arrayDeleteIdentical(arr);
          console.log(arr)
  
  // 合并数组
  ```

+ 二维数组

  ```js
  // 和c语言数组类似
  var arr[[0,1], [2,3], [4,5]];
  for(var i = 0; i < arr.length;i++){
      for(var j = 0;j<arr[i].length;j++)console,log(arr[i][j])
  }
  ```

  

## 1.7 函数

+ 定义

  ```js
  /*
   1.有某种特定功能的代码块
   2.函数可以解决代码复用问题
   3.函数可以把整个项目模块化
   4.function(){}定义函数
   5.函数的三要素，功能，参数，返回值(return),返回值如果没写，默认返回undefined,return是否需要有取决于函数的功能 .当执行到return语句时立刻返回此函数的返回值，函数不再执行
   6.函数本身就是一个变量当此函数被重新赋值后，原有的函数会被覆盖
   如function a() {  alert('1')}，直接打印a打印的是一个函数，当a=3时再打印此a得到的值为3，以函数调用的方式调用时会报错，表明此时a的原有函数值已被覆盖
   7. return 可以返回变量，数组，对象
  */
  ```

  ```js
  // 有两种定义方式
  //1. 普通函数
  function fib(){}
  //2. 匿名函数
  var fib = function(){}
  
  // 函数分为有参返回值和无参返回值
  // 函数调用时串入的是实参，函数声明时写的是形参
  // 有形参的函数如果没有用到此形参，也可以不穿值进行调用
  function a(a) { // 此时这个形参a是undefined
              console.log(1);
          }
          a();
  ```

+ 练习

  ```js
   function maxAndMin(arr) {
              var max = arr[0];
              var min = arr[0];
              for (var i = 0; i < arr.length; i++) {
                  if (max < arr[i]) max = arr[i];
                  if (min > arr[i]) min = arr[i];
              }
          //函数的返回值如有多个值，可以为数组类型作为返回对象，接收此返回值的变量会隐式转换为一个数组
              return [max, min];
          }
          var arr = [23, 23, 12, 5, 3, 8];
          var result = maxAndMin(arr);
          console.log(result);
  
  //1.函数在传递普通数据类型时是值传递，传递对象数据类型时是地址传递
  // 数组(对象)在调用函数时，是地址传递，函数内部改变其值时，数组内部也会改变
  function maxAndMin(arr) {
              // var max = arr[0];
              // var min = arr[0];
              for (var i = 0; i < arr.length; i++) {
                  if (arr[i] < 10) arr[i] = 1;
              }
          }
          var arr = [23, 3, 12, 5, 3, 8];
          var result = maxAndMin(arr);
          console.log(arr); // 23 1 12 1 1 1
  
  //在传递普通数值时，为值传递，函数内改变值，外部调用的值不受影响
  function maxAndMin(arr) {
              arr = 5;
          }
          var arr = 2;
          var result = maxAndMin(arr);
          console.log(arr); // 2
  ```

  

## 1.8 作用域

+ 定义

  ```js
  /*
   1.指变量再作用域中得影响范围
   2.函数外部的变量称为全局作用域，全局作用域中，全局都可以使用(包括函数,for)
   3.函数内部的变量称为局部变量，局部变量只能再局部在使用
   4.变量的取值采用就近原则，及自己作用域里没有的变量才会去往上一级作用域查找
   5.块级作用域(ES6){}再花括号里的变量只有{}里才能使用(使用let定义)
   6.如果内部函数使用了一个没有定义的变量，全局变量也没有定义时，系统自动会在全局顶部定义一个var
   7.函数内部如果没有定义用到的变量，就会去她的上层寻找，当函数内部改变全局(上层)变量的值后，则其他值再使用这个变量就是改变后的值
  */
  ```

+ 作用域链([scopes])

  ```js
  /*
   变量的使用采用就近原则，如自己有需要使用的变量就使用自己的，如果自己没有就会往上层找。再你找到你需要的变量后，你对其变量的值进行更改，则再改作用域下的那个值也会更改
  */
  var a = 1;
          var b = 2;
          function e() {
              var a = 10;
              var b = 10;
              d();
              console.log(a, b); // 12 23
              function d() {
                  a = 12;
                  b = 23;
                  console.log(a, b); // 12 23
              }
  
          }
          e();
          console.log(a, b); // 1 2
  ```

+ 堆，栈，队列，局环境

  ```js
  /*
  一.再栈上面的函数可以使用栈下面的变量(再自己没定义的情况下)
   1.栈就像一个桶，先进后出，后进先出
   2.队列就像一个管子，先进先出，后进后出
   3.堆就像一个链表结构，随意增删
   4.局环境：先有全局环境，再有局部环境，并且都在栈区管理，创建完全局环境后，程序会把所有全局环境变量收集起来并且进行执行开辟空间。再栈上面的变量可以使用栈下面的变量(所以局部变量可以使用全局变量)
   5.当函数执行完成后，也就是函数返回返回值以后，这个函数开辟的栈空间会立马销毁。当整个程序执行结束以后，全局环境最后也会弹出栈，也就是销毁全局环境，释放占用内存
   6.js代码从上往下执行，所以全局变量必须写在函数调用的上面，这样才能保证全局环境变量比函数先进入栈，因而再函数局部使用变量时如若没定义，则回去全局找，从而然函数拿到全局变量的值
  
  */
  ```

+ 预解析(在正式执行代码之前运行)

  ```c
  //一. 预解析只会对有var及普通函数进行预解析，没有var不会解析,当执行到没有var的变量(且此变量再全局没有定义)，则此变量会再之前预解析完成的var和函数的下面添加一个var (变量)来进行再解析
  // 二. 预解析优先级  先去解析函数，函数如果有会发生覆盖，再去解析var变量，如果变量有同名，则是忽略。(可理解为，var是先上升的变量，然后函数跟在它其后上身，且会被后面同名的函数覆盖)
  // 三. 全局预解析再定义函数的时候不关心函数是否被使用(仍会提升)，函数句部预解析时如果内部函数没有被使用不会提前定义
  alert(a);
  		// var先上升，函数后上升，会被后面上升的覆盖
          function a() { alert('我是变量') };
          var a = '我是变量';
          alert(a);
  //--------------------
   a()
          function a() {
              a = 3;
              alert(a) // 3
          }
          alert(a) // 3
  
  // 1. 使用var创建的变量会有一个变量提升，提升至程序顶部，全局环境中提升至全局的顶部。局部环境中，提升至局部的顶部。但这个提升只是提升变量本身(值为undefined),而不会提升赋给它的数值
  //带var的数值
   e();
          var a = 1;
          var b = 2;
          function e() {
              a = 10;
              b = 10;
              console.log(a, b); // 10 10
              function d() {
                  a = 12;
                  b = 23;
                  console.log(a, b); // 12 23
              }
  
              d();
          }
          console.log(a, b);// 1 2
  
  //不带var的数值
  console.log(a); //报错，没有var不会预解析，当在执行到a时，才会给改表达式添加var
      a=2
          
  // 函数的预解析
  //1.function f1(){} 函数整体会提升(提升至程序顶部)  
  //2. var f1=function(){}只会提升f1，函数本质上也是一个值 
  //3. 当一个没有定义的值执行++操作时值为NaN   
          
  alert(a); //打印function a()函数
  a++; 
  alert(a);//NaN
  var a='我是变量';
  function a(){alert('2')}
  alert(a);//我是变量
  ```

+ IIFE(Immediataly Invoked Function Expression)立即调用的函数表达式

  ```js
  /*
  立即执行函数再html加载好后立即执行
  1.IIFE(立即执行函数)再执行到本行时，才会执行
  
   2.function(){};为匿名函数，本质上是一个没名字的表达式，语法正确，但不能再直接外部调用(需要用特殊的方法调用) 。(function(){})();可以调用匿名函数，此调用方法不会触发预解析，且只能调用一次。通常用来做项目的初始化 。 
   匿名函数自调用有以下好处，封装代码，不把代码暴露到全局，防止外部命名被污染。
   
   3.(function(a){console.log(a)})(a);如果函数需要接收变量，第二个括号里可以传入实参，让内部函数进行接收
   
  4.Arguments(函数实参伪数组)
  每个函数都会有arguments伪数组，即使函数没写(或写)形参，但在传参时传入实参时，可以时用arguments伪数组拿到参数。
  arguments功能上和数组相似，但原型对象不同，且没有数组的常用方法
  arguments通常用再判断中使用，根据实参长度不同，让函数实现不同功能
  
  */
  ```

+ 回调函数

  ```c
  /*
    回调函数，函数定义后，自己没调用，最后会执行
  */
  ```

+ 全局变量&局部变量

  ```js
  /*
   1. 在函数内部得变量都是局部变量，局部变量改变得值，对全局(外层)变量不影响。局部变量包括，函数内定义的参数，和接收得形参，如果自己内部没有定义，则会一直往上级寻找，多高可找至全局变量下，当在本函数内改变该变量时，该变量所在得值也会改变
  */
  ```

+ 执行上下文

  ```js
  /*
   1. js引擎在正式执行js时会创建一个执行环境，进入该环境后创建一个变量对象，该对象收集当前环境下的包括，变量，函数，函数形参，this(找关键字var,function(类似变量提升))可在浏览器sources下的global查看
   2. 在函数中，没有被调用的函数不会执行上下文
   3. 一般流程，初次加载把全局全局环境变量压入栈底，包括函数，循环等，第二次调用函数,第三次调用函数...
  */
  ```

+ 静态(词法)作用域&&动态作用域(bash)

  ```js
  /*
   静态作用域是在代码定义的时候决定的，动态作用域是在代码执行的时候决定的
  */
  ```

  

## 1.9 对象

 判断一个对象是否为空对象 a.JSON.stringify() ==='{}'

Object.key(obj) ，可以得到该一个数组，该数组里的每个值都是对象下的key值

+ 概念

  ```js
  /*
   1. 在js中，以切都是对象，为无序的名值对的集合
   2. 在js中，名可以加单引号，双引号，或不加引号
   3. 在js中，语句必须用函数进行封装，要不然在调用对象时就会执行
   4.Obj.prototype.xxx给实例对象使用  Obj,xxx 给原型对象使用
   5. obj.toString()会转换为[object Object]
  */
  ```

+ 基本使用

  ```js
  // 1.定义对象 
  // 常规定义
  var obj = {
              'name': 'zly',
              'age': 32,
              gender: 'female',
              sing: function () {
                  console.log('I like sing');
              }
          };
          console.log(obj.sing())
  // new 关键字定义
  var obj2 = new Object({name;'ym',age:33})
  
  // 工厂函数(本质上时使用构造函数)
  function createObject(name,age){
      var obj = new Object();
      obj.name = name;
      obj.age = age;
      return obj;
  }
  console.log(createObject('rb',26))
  
  // 2. 对象操作及遍历(增，删，改，查)
  // obj[]只有中括号中的变量会隐式转换
  // 增
  var obj = {}
  obj.name = '小黄';
  obj.age = 12;
  //obj['str']此方式添加的名值对需要加引号，否则系统会解析为变量，如果该变量指向的是一个字符串，则可以成功创建，不存在会报错。在属性名是不合法的标识符和属性名不确定的情况下可以用此方法定义
  // 以obj[a]的方式添加的名值对，如果a是对象类似数据如，a={},a=[],function a()等，系统会把这些名隐式转换为对应类型的对象名
  obj["color"]='gray'; 
  
  // 改
  a= 'color';
  obj.color = skyblue;
  obj["color"] =blue;
  obj[a] = red;
  console.log(obj.a) // 不会隐式转换 为 color 
  console.log(obj[a]) // 会隐式转换 为 color
  
  // 查(如果名值不存在，会输出undefined)
   console.log(obj.color)
   console,log(obj[a])
  
  // 删
   delete obj.color;
   delete obj[a];
  ```

+ 对象的遍历

  ```js
  // for in 枚举对象的时候除了能够枚举自身的属性外，还会枚举原型对象上的属性，通常配合对象下的 obj.hasOwnProperty(i)使用(返回boolean值)
  // for in 可以遍历数组和对象，遍历的每个值都是以字符串形式存储，且当遍历的对象为数组时，i表示的是下标。是对象时，i表示的是key值
  var obj ={
      name: 'gray',
      category: 'we'
  }
  // key 拿的属性名，通过属性名.的方式，或属性名['str']方式拿到值
  // key 是属性名, obj是对象
  var obj = {
              'name': 'zly',
              'age': 32,
              gender: 'female'
          };
          for (var key in obj) {
              console.log(key, obj[key])
  
          }
  ```

+ 构造函数

  ```js
  // 构造函数本质上也是一个函数，使用大驼峰命名，在js中(5版本没有类)，构造函数可以理解为类
   function Person(name, age, gender) {
       // 1. this指向问题
       // 在通常情况下，构造函数都会有this值，this本质是一个对象，代表调用这个函数或方法的对象
       // this代表window ,在函数当中，函数也可以叫做是window对象的方法
       // 在事件当中，回调当中的this，代表的是事件对象
       // 在对象的方法中，this代表的是这个对象
       // 在构造函数中，this代表的是实例化出来的对象
       //this指向是看函数调用者，如果是全局都是window，如果是new就是构造函数,事件，则指向事件调用者，对象，则指向对象
       
              this.name = name;
              this.age = age;
              this.gender = gender;
              this.eat = function () {
                  console.log('eating')
              }
          }
  // 2. new关键字
  // 在调用此函数时使用关键字 new 则为创建一个对象，不加new 则代表一个普通的函数
  // new 关键字代表在堆区开辟一个对象空间，然后this指向此空间
  function Person(name,age){
      this.name = name;
      this.age =age;
  }
  p1 = new person(2,3);
  ```

+ 原型对象，原型链

  ```js
  /*  
    prototype(显示原型(属性)) __proto__(隐式原型(属性))
    1.任何函数对象在定义的时候都会伴随的一个原型对象的出现，原型对象默认是Object实例对象
    2.在创建一个函数后，它都有一个同时产生原型对象(链)，proto指向原型(object),里面由一些属性和方法，constructor指向创建者，同一个构造函数创建的实例对象可以使用该构造函数(包括原型)的属性和方法
    3.对象在调用方法时，首先会在自己的对象空间去查找，如果没找到，则会去自己的构造函数原型对象空间去找，如果还没找到，则会去原形对象的原型对象空间去找
    4.总结：简单理解为，给原型对象(数组，函数)或原型对象的Object对象添加方法，让使用这个原型对象的实例对象都可以调用添加的此方法
    5. Object.prototype 为当前对象内存储的数据
  */
  function BlueCat(name, age, gender) {
              this.name = name;
              this.age = age;
              this.gender = gender;
  
          }
  BlueCat.prototype.__proto__.stretch = function () {
         console.log('it can stretch with itself thinking');
  }
  // 或
  BlueCat.prototype.stretch = fucntion(){
      
  }
  ```

+ apply，call(会改变this的指向(指向这个函数))

  ```js
  //apply和call 可以改变一个函数的执行对象
  // apply,需要两个参数，第一个参数代表的改变的执行者(对象)，第二个参数是函数需要的参数，这个参数必须是数组形式
  function Cat(name){
      this.name = name;
  }
  c1 = new Cat('jimmy')
  function add(a,b){
      return a+b;
  }
  // 可以用在不同的构造函数下生成的实例对象调用同一个方法
  console.log(add.apply(cat1,[10,20])) // 30
  
  //call，第一个参数为调用者，....后面的参数为函数所需要的参数
  console.log(add.call(cat1,10,20)) // 30
  
  ```

+ typeof && instanceof

  ```js
  //1. typeof 在判断 number,string,boolean,undefined,function时可以正常判断.  array,object,null不能判断(结果为object) . typeof 返回数据类型
  //2. instanceof 判断一个实例对象是不是 数组或对象得实例。instanceof 返回结果 true&&false
  //3. ==运算符可以用来判断 null值
  //4. instanceof 在判定时，现在类型判断，如果类型无法判断，在做原型判断。Object,Function,Array,时，Function，Array有原型链对象Object所以，在和Object进行判定时为true，Function,Array为false. instanceof 主要是对变量得constructor来进行判断
  var a = 12;
  console,log(typeof a);
  a = {};
  console.log(a instanceof Object); // true
  console.log(a instanceof Array); // false
  var b = null;
  console.log(b == null) // true
  
  //4.判断数组时，数组内部有Array.isArray()方法，可以判断是否时数组
  var c = [];
  console.log(Array.isArray(c)) // true
  ```

  

## 2.0 值类型

+ 堆和栈

  ```js
  /*
   堆和栈是内存得视距结构，内存被开辟使用，就一定会被计算机回收
   1. 栈（先进后出）
   栈结构内存一般较小，计算机自动分配内存，存取速度快
   
   2. 堆（随意进出）
   堆结构内存一般较大，底层需要程序员自己分配，堆里存储得都是一些比较复杂得栈空间较大得数据
  */
  ```

+ 数据的存储

  ```js
  /*
   基本数据类型：number string boolean undefined null
   复杂数据类型：object function(object) array(object)
   1. js在预处理是首先会把全局变量放到栈区做一个声明，如果是基本数据类型，则可以在变量提升后直接进行赋值(占用空间小)。如果是复杂数据类型，则会先去堆区开辟一个对象空间然后把地址返回给栈区的变量进行存储(类似指针)
   2. js栈区在切断和堆区的指向时，js会在内部堆堆区的数据进行一个垃圾回收(3种方式)。常见的方法是，把一个指向对象的变量赋null值
   
  */
  ```

+ 数据类型的传递方式

  ```js
  /*
   和c语言类似
   1.在函数传参时，普通数据类型 值传递， 复杂数据类型 地址传递(在堆区开辟的内存)，复杂数据类型传过来的参数，函数内部也可以改变地址的指向，这样不会对原来的值造成影响
  */
  ```

  

## 2.1 内置对象

+ JSON

  ```js
  /*
   JSON是js种的一个内置对象，里面封装了json格式操作方法
   json是一种数据格式，是前后端目前数据交互的主要格式，格式为对象，或者对象的数组
   在json里的数据必须用引号括起来，除了数字
  */
  
  // 1.JSON转换  JSON.stringify()
  var dateTest = {
      name: 'za'.
      age: 32
  }
  var dateStr = JSON.stringify(dataTest);
  console.log(dataStr); // 转换为json字符串
  
  // 2.JSON解析 JSON.parse()
  var dataJson = JSON.parse(dateStr) //转换为 正常对象
  ```

+ Math

  ```js
  /*
   1. Math对象方法有，round,floor,cell,random,max,min,PI,pow
   Math.round(1.5) 代表四舍五入
   Math.floor(0.8) 表示向下取整
   Math.ceil(1.8) 表示向上取整
   Math.random() 表示随机数 返回0到1的随机数
   Math.max(1,18,15) 返回多个数最大值
   Math.min(1,18,15) 返回多个数最小值
   Math.sqrt() 对一个数进行开方(开根号)
   Math.PI 表示圆周率
   Math.pow(2,3) 表示求平方,第一个参数表示数，第二个参数表示平方
   Math.sin(Math.PI/2) 三角函数相关的都用弧度
  */
  
  // 1. 打印随机数
  var n = Math.random() * 3 + 5;
  console.log(n)
  var num = Math.ceil(n);
  console.log(num)
  
  // 2. 打印区间范围随机整数
  function getRandomInt(min, max) {
       return Math.floor(Math.random * (max - min + 1)) + min;
              //或
       return Math.ceil(Math.random * (max - min + 1)) + min - 1;
  }
  
  // 3. 打印随机验证码
  // 字符串也有自己的下标，和length方法，可以获得长度，字符串是不可变数据类型下标不可更改
  var Mastr = 'QWRTYUIKAJLZXCVBNM123465789aweiuqopitvxklcnmz';
          var strarr = '';
          for (var i = 0; i < 8; i++)
              strarr += Mastr[Math.floor(Math.random() * Mastr.length)];
          console.log(strarr)
  ```

+ Date对象

  ```js
  /*
  Date.now() 拿到当前时间(1970-现在的时间，毫秒数)
   1. 需要实例化一个日期对象  
   var date = new Date()
   2. 实例化后 直接打印 date 可以拿到当前时区的标准时间码
   console.log(date)
   3. date.getFullYear() 拿到当前年份
   4. date.getMonth() 拿到值 0-11 代表1-12月份  0代表1月份 11代表12月份，使用时需+1
   5. date.getDate() 拿到日期
   6. date.getHours() 拿到时
   7. date.getMinutes() 拿到分
   8. date.getSeconds() 拿到秒
   9. date.toLocateTimeString() 拿到当地时间的时分秒
   10. date.toLocateDateString() 拿到当地年月日
   11. date.getTime() 拿到 1970年1月1日的毫秒数
  */
  ```

+ 包装对象

  ```js
  /*
  简单数据类型 number string boolean null undefined 
  简单数据类型，直接在栈区存放数值，所以是值传递
  复杂数据类型 object function array
  复杂数据类型，内存在堆区开辟，栈区指向的是它们的地址，所以在函数传参时是地址传递
   1.当创建一个简单数据类型时，它可以调用数字，字符串，布尔值的方法，这时系统会把这个值包装成一个临时对象去调用原本的对象的方法，当调用完成后，这个临时对象会立即清除，从而蜕变为简单数据类型
   */
  // 2. 创建一个复杂数据类型的number string boolean 
  var str = new String('123');
  var num = new Number(123);
  var boole = new boolean(true);
  ```

  

## 2.2 ES5&&ES6

+ 严格模式

  ```js
  /*
  1.作用
   消除javascript语法的不合理，不严谨之处，减少怪异行为
   消除代码运行的不安全性，保证代码稳定运行
   提高编码器效率，增加运行速度
   为未来新版本javscript做铺垫
   开启严格模式，把 "use strict"写在js脚本顶部 开启严格模式
   2.严格模式下的一些改变
   变量声明必须写var，不写var报错
   禁止自定义函数this指向window,如果构造函数忘记写new那么this不会影响全局
   禁止随意删除变量
   对象不能有重复的属性，函数不能有重复的参数
   
   eval()函数可以把字符串解析为可执行代码(现在被淘汰)
  */
  ```

+ let&&const

  ```js
  /*
   1.let 作用及特点
   块级作用域声明变量
   let定义的变量不会进行预解析
   let不允许重复定义
   
   2.const 作用及特点
   声明一个变量，变量的值无法更改，且是块级作用域
   const定义的变量不能修改，且必须初始化
  */
  ```

+ string方法 ES5&&ES6

  ```js
  /*
   1.ES5方法([]为可选参数)
   （1）str.charAt(8) 返回在指定位置的索引字符
   （2）str.concat('ew') 拼接字符串，返回拼接后的字符串
   （3）str.indexOf('123'，[指定开始检索位置]) 返回指定字符串所在原字符串中第一组字符串的起始下标，查到返回位置，没查到返回-1
   （4）str.lastIndexOf('123',[指定开始检索位置]) 从后往前查找，返回指定字符串所在原字符串中第一组字符串的起始下标，查到返回位置，没查到返回-1
   （5）str.length 返回字符串长度
   （6）str.localeCompare('2') 字符串和指定字符串比较，逐个比较编码值，如果相等返回0，str大于指定字符串返回1，小于返回-1
   （7）str.split('12') 以 12 为分隔点(把12替换为，)，转换为数组
  
   
   
   正则用：
   str.replace('12'); 正则表达式用，正则匹配，替换字符串
   str.match('12') 正则表达式用，正则匹配，寻找子串
   str.search('1')正则匹配寻找字符串，返回子串位置
   
   2.以下了解
   str.charCodeAt(1) 返回指定位置的unicode码(或ASCII)
   String.fromCharCode(48) 只能用于函数对象，根据unicode码返回值
   str.slice(6,-2) 从字符串中抽取指定字符串位置，包含起使位置，不包含结束位置 
   str.substr(6,3) 抽取字符串，起始位置，和抽取长度
   str.substring(6，9) 截取字符串和slice很像.包含起使位置，不包含结束位置 ，但不能传递负数 
   str.toLocaleUpperCase() 转本地大写
   str.toLocaleLowerCase() 转本地小写
   str.toUpperCase() 通用大写
   str.toLowerCase() 通用小写
   str.toString() 转换为字符串
   str.valueOf() 显示字符串
   
   3.ES6新增方法
   (1)str.includes('WW') 查询指定字符串是否在原字符串中，存在返回true，不存在返回false
   (2)str.startsWith('lw') 判断原字符串是否以指定字符串开头，是为true,不是为false
   (3)str.endsWith('ew') 判断原字符串是否以指定字符串结尾，是为true,不是为false
   (4)str.repeat(2) 指定字符串执行次数，只能填正整数
  */
  ```

+ 数组方法

  ```js
  /*
  ES5
   1. arr.concat(4) 在末尾插入一个新值，返回一个新数组，不会对原来数组产生影响。并且拼接的对象如为多个数，可以用数组表示，还是照常拼接
   2. arr.length 拿到数组长度
   3. arr.join('') 把数组转换为字符串，逗号转换为你指定的字符串
   4. arr.reverse() 会返回经过反转的数组，会改变原数组
   5. arr.slice(1,4) 截取数组，第一个参数为起始位置，第二个参数为终点位置(不包含终点位置) ，返回新数组
   6. arr.sort() 会对数组进行排序，更具unicode码，会影响原数组 。 补给回调函数默认从小到大排序，给回调函数可以指定顺序
    arr.sort(function(a,b){return a-b})从小到大排序
    arr.sort(function(a,b){return b-a}) 从大到小排序
   7. arr.push(2) 给原数组最末尾添加一个元素(可以为多个元素或数组，如果添加的是一个数组，则取值需要用二维数组方式取值)，返回添加后的数组长度，会改变原数组
   8. arr.pop() 从数组末尾删除一个元素，返回删除的元素，原数组改变
   9. arr.unshift() 给原数组头部添加一个元素，返回添加后的数组长度，会改变原数组
   10. arr.shift() 删除数组头部的一个元素，返回被删除的数组元素，会影响原数组
   11. (删)arr.splice(1,3)从原数组中[1,3]开始截取，返回一个新数组，可对数组增删改，会影响原数组
    (增)arr.splice(1,0,'zz') 在原数组的下标1中插入一个zz，改变原数组，返回一个空数组 。(splice返回值是一个删除后的字符串，本意会改变原数组，第三个元素可以做添加，可以一次加多个元素)
    (改)arr.splice(1,1,'ew') 返回被删除的数组形式数据，然后插入一个字符串数据
    12. arr.toString()  按通用字符串转换为字符串
    13. arr.ToLocaleString() 按本地字符转换为字符串
    
    ES6
    1.arr.indexOf(value) 从前往后找，得到数组中第一个指定元素下标，没有返回-1
    2.arr.lastIndexOf(value) 从后往前找，返回第一个指定元素的位置，没有返回-1
    3.arr.forEach(item,index) 遍历数组,item可以拿到数组里每一个元素，index可以拿到下标。没有返回值
    4.arr.map(function(item,index){})map会遍历数组每一个元素并可以对数组里的元素的值进行修改，需要return返回修改后的数组，map方法不会改变原有数组。
    arr.map方法不能做if判断因为它每个数值都会返回一个指定值，如果没有指定值则会返回undefined
     var arr = [2, 1, 3, 5, 4];
          var result = [];
          result = arr.map(function (item, index) {
              item += 2;
              return item;
          })
          console.log(result)
     5.arr.filter(function(item,index){}) 会按指定条件返回数组值，不会对原有数组产生影响
     6. arr.from(arr1) 把一个字符串数据中的每一个字符转换为数组，不会影响原来的字符串
     7. arr.of(1,2,3) 可以传入多个值，别这些值转换为数组
     8.arr.findIndex(function(item,index) return item>10) 返回第一个满足条件的变量返回下标
  */
  ```
  
  

## 2.3 正则表达式

+ 正则对象

  ```js
  /*
   正则表达式可以用来做判断，查找，修改字符等操作
   使用/正则/来表示，对象下有一个test()方法，可以检测一个字符串内是否有相同的字符串，有为true，无为false
   1.创建方式(在所有语言中\都是转义字符)
    (1) var a = /\d/
    (2) var a = new RegExp('\\d')
  */
  ```

+ 正则表达式规则写法

  ```js
  /*
   1. 修饰符
   i：忽略大小写
   g: 执行全局匹配(查找所有匹配而非找到第一个匹配后停止)
   m: 执行多行匹配
   
   2. 方括号
    [abc] 查找abc任意一个
    [^abc] 查找不是abc的任意一个
    [0-9] 查找任意一个小数
    [a-z] 查找任意一个小写字母
    [A-Z] 查找任意一个大写字母
    
    3.元字符
    . 匹配任意字符不包含\n(换行和结束符)
    \d 任意数字 等价[0-9]
    \D 任意非数字 等价[^0-9]
    \w 任意字母数字下划线 等价[a-zA-Z_]
    \W 非字母数字下划线
    \s 任意空白字符 
    \S 任意非空白字符
    \b 单词边界
    \B 非单词边界
    \n 换行符
    \f 换页符
    \r 回车符
    \t 制表符
    \v 垂直制表符
    
    4.量词
    + 一个或多个前一个字符
    * 0或多个前一个字符
    ? 0或1个前一个字符
    {n} n个前一个字符
    {m,n} m到n个前一个字符
    {m,}至少m个前一个字符
    正则是用来匹配一段符合标记的字符串，如果像定义开头和结尾字符需加^$
    $结尾
    ^开头
    
    5.分组
    ()
    6.贪婪，非贪婪
    量词后加?代表非贪婪,非贪婪尽量少取，贪婪模式(默认)尽量多取
  */
  ```

+ 正则方法

  ```js
  /*
   1. Reg.test()
    用于检测一个字符是否匹配某个模式，如果字符串中含有匹配的文本则返回true，否则返回false
    判断是否包含数字
    var str = 'askdjioawjhirhwajkw';
      var patt = /\d/;
      console.log(patt.test(str))
      判断是否包含abcd
      var str = 'askdjioawjhirabcdkw';
      var patt = /abcd/;
      console.log(patt.test(str))
      
   2. Reg.exec()
    用于检索字符串中正则表达式的匹配，该函数返回一个数组，未找到返回null，此方法只会返回一个结果，如果需要找到所有的值，需要配合循环去调查找值，且用必须全局匹配修饰g
    注意
    	exec匹配后返回一个数组，第一个索引为正则匹配后的全部内容，第二个索引为被修饰符所匹配到的内容，第三个索引是一个对象存放着匹配的位置，input被匹配的全部内容
     var str = 'FS1232sf312451askdj%io#aw&jhirabcdkw12325123124512';
      var patt = /\w+/g;
      var result = patt.exec(str);
      while (result != null) {
        result = patt.exec(str);
        console.log(result)
      }
      
   3. str.match()
   在字符串中搜索符合规则的内容，搜索成功返回内容，格式为数组，失败返回null.如果不加g，返回第一次符合的结果，加g返回所有结果的数组
   var str = 'FSew1232sf3124#$51askdj%io#aw&jhirabcdkw12325123124%512';
      var patt = /\d+/g;
      console.log(str.match(patt));
   
   4. str.search()
    在字符串中搜索符合正则的内容，搜索到返回出现的位置(下标从0开始)，返回第一次出现的位置，失败返回-1，只能返回一次
    
   5. str.replace()
   查找符合正则的字符串，替换对应的字符串，返回替换后的内容
   str.replace(正则，字符串)
   var phone = '18045437140';
  console.log(phone.replace(/4/, '*'));
  
  6.  /[0-9a-zA-Z_]{0,}/此写法拿到0-9a-zA-Z_无数位
  	/[0-9a-zA-Z_]/此写法只能拿到0-9a-zA-Z_1位
  */
  ```
  
  

# JS高级

js内存分配只有堆内存，堆内存里分堆和栈结构

## 1.0 闭包

使用完闭包后要及时清除，最后赋一个null值(系统会自动回收)

+ 概念

  ```js
  /*
  函数内部的函数是私有函数，全局访问不到，需要全局函数来返回内部函数
  闭包的作用：延长外部函数的生命周期(优缺点，如果不及时清除容易造成内存溢出泄露)，函数外部可以访问函数内部变量
  1. 闭包产生的必要条件：函数嵌套,内部函数引用外部函数的局部变量,使用内部函数
   2. 闭包是一个引用关系，该引用关系存在于内部函数中，引用的是外部函数的变量的对象
   3. 程序运行中，先执行上下文，把所有的全局函数变量等压入栈底，然后调用函数时会再栈区进行引用，当引用到函数内部的函数时，这个内部函数会执行，且他会回调一个指针去指向他外部的一个函数，从而获取他外部的变量值，这个调用简称为闭包
   4. 再正常函数调用中，是在函数调用执行完成后，立刻销毁这个变量对象，但有闭包产生的函数中，此函数的销毁周期会延长(供给一个地址让内部闭包能指向外部函数)
  */
  ```
  
+ 闭包解决同步异步问题(通过循环一次自调用一次解决)

  ```js
   var divs = document.querySelectorAll('div');
      var i = 0;
      for (i = 0; i < divs.length; i++) {
  
         // 立即调用函数会立刻调用()里面的最外层的函数，因此需要先写一个匿名函数接收i，然后再添加点击事件。直接写单个点击事件那么会立刻触发
        (function (i) {
          divs[i].onclick = function () {
            alert(i + 1)
          }
        }
        )(i)
      }
  //---------------------------
  function fun(){
      var i = 1;
      function fun2(){
          i++;
          console.log(i);
      }
      return fun2;
  }
  fun()()
  ```

+ this指向

  ```js
   // 1. this指向问题
       // 在通常情况下，构造函数都会有this值，this本质是一个对象，代表调用这个函数或方法的对象
       //  在函数&&全局变量当中，函数也可以叫做是window对象的方法,this代表window
       // 在事件当中，回调当中的this，代表的是事件对象
       // 在对象的方法中，this代表的是这个对象
       // 在构造函数中，this代表的是实例化出来的对象
  
   // 2. 再通常情况下，我们会人为的定义一个that变量去指向this最开始的指向，以免后续认为修改this指向，导致this指向迷失
  ```

  

## 1.1 对象补充

+ 名值添加方法

  ```js
  // 添加名值可以用 tep.a = 1 || tep[a] = 3方式添加，a系统会自动隐式转换，一般转换为字符串
  var a = [1, 2]
      var b = {
        a: 3,
        b: 4
      }
      function c() { console.log(1) }
      var tep = {}
      tep[b] = 1;
      tep[a] = 2;
      tep[c] = 4;
      console.log(tep)
  /*
  输出为
  {[object Object]: 1, 1,2: 2, function c() { console.log(1) }: 4}
  1,2: 2
  [object Object]: 1
  function c() { console.log(1) }: 4
  */
  ```

+ 对象创建方式的优缺点

  ```js
  /*
   1. obj = new Object(); 可以快速创建对象，但不能创建时赋值，需要一个一个添加，代码冗杂
   2. obj = {} 字面量创建，可以创建时赋予初值，且创建快速，但如果创建过多可能造成对象名值重复过多
   3. 工厂函数创建 ，可以简化代码冗杂，批量创建对象，但不能区分创建对象的构造者
   function Person(name,age){
   return {
   	name: name,
   	age: age
   }
   }
   4. 构造函数 ，可以简化代码，且对类别进行管理
   
   function Person(name,age){
   this.name = name;
   this.age = age;
   }
   
  */
  ```

+ 原型继承

  ```js
  /*
  原型继承只能继承一个父类
   再构造函数中，想要使用自己没有的方法时，可以使用原型继承的方法来获取其他构造函数下的方法，但继承后，函数的constructor属性会丢失需要手动补上
    function Person(name, age) {
        this.name = name
        this.age = age
      }
      Person.prototype.eat = function () {
        console.log('我会吃饭')
      }
      function Child(name, age) {
        this.name = name
        this.age = age
      }
      // 让子类的原型成为父类的实例，需重新指向Child构造者，然后系统会自动继承父构造函数里的属性，如果再生成实例时没有给它的属性传入参数，则会显示undefined(不影响)
      Child.prototype = new Person();
      Child.prototype.constructor = Child;
      var p1 = new Person(1, 2)
      var c1 = new Child(2, 3)
      p1.eat()
      c1.eat()
      console.log(c1, p1, c1.name, c1.age)
  */
  ```

+ 借用继承

  ```js
  /*
   再有多个构造函数时，它们的形参一定会有重合，此时使用call或apply就可以达到继承调用属性的目的。主要是让this指向需要得到值的实例对象
   function Person(name, age) {
        this.name = name
        this.age = age
      }
      function Child(name, age, sex) {
        Person.apply(this, [name, age]) || Person.call(this,name, age)
        this.sex = sex
      }
      var p1 = new Person(1, 2)
      var c1 = new Child(2, 3, 3)
  */
  ```

  

## 1.2 进程与线程

JS是单线程(主线程)，代码会阻塞,js分同步和异步

+ 进程&&线程

  ```js
  /*
   1. 进程： 程序的一次执行，它占有一片独有的内存空间
   2. 线程： CPU的基本调度单位，是程序执行的一个完整流程
   3. 进程与先程
   一个进程中至少有一个运行的线程：主线程
   一个进程中也可以同时运行多个线程，我们会说程序时多线程运行的
   一个进程内的数据可以共七种的多个线程直接共享
   多个进程之间的数据不能直接共享
  */
  ```

+ 浏览器内核

  ```js
  /*
   Chrome,Safari: webkit
   firefox: Gecko
   Ie: Trident
   360,搜狗：Trident + webkit
  */
  ```

+ 同步&&异步

  ```js
  /*
   1. 同步代码： 同步会阻塞后续代码的执行，且没有回调。alert()证明
   异步代码(满足条件执行)：异步是非阻塞的 ，且有回调 
  
   
   2. js是主线程语言，同步代码先执行，异步代码后执行，队列会轮询等待(询问)代码执行，当达到代码执行条件时，钩子(回调函数，生命周期函数)函数会拿出此数据，然后进入循环队列(callback queue)排队执行
   
  3. 解释型语言程序从上往下运行，当执行到同步代码时，立刻执行，当执行到异步代码时放到异步模块等待触发执行。只有再同步代码执行完后异步代码才会执行。再异步代码中，定时器是特殊的，当执行到定时器后，先把这个定时器放到定时器模块开始执行(计时)，当执行完成后，会由钩子函数勾入到队列中，等待同步代码执行完后才会执行，因此，再定时器后不要写大量复杂的(耗费时间长的)代码，这样会导致定时器有误差
  
   
  */
  ```

## 1.3 Web Workers

+ H5提供了js分线程的实现，取名为：Web Workers

  ```js
  /*
   相关API
   worker: 构造函数，加载分线程执行的js文件
   worker.prototype.onmessage 用于接收另一个线程的回调函数
   worker.prototype.postMessage 向另一个线程发送消息
  */



# JS ES5-ES7

ECMAjavascript由ECMA组织指定发布的脚本语言

ES5(09年发布) ES6(ES2015) ES7(ES2016)

## 1.0 Object扩展

+ object.create(prototype,[descriptors])

  ```js
  /*
   作用： 以指定对象为原型创建新的对象。为新的对象指定新的属性，并对属性进行描述
   [descriptor]数值： value:指定值，
   writable:标识当前值是否可以修改默认false
   configurable: 标示当前属性值是否可以删除，默认false
   enumerable: 标示当前属性是否能用for in 枚举 默认false
   
   var obj = {
        name: '张三',
        age: 18,
        behavior: function () {
          console.log('我是一个行为')
        }
      }
      // 以obj为原型对象创建一个新的对象
      var obj2 = Object.create(obj, {
        a: {
          value: 'e',
          enumerable: true,
          writable: true,
          configurable: true
        }
      })
      obj2.behavior()
  */
  ```

+ Object.defineProperties(object,descriptors)

  ```js
  /*
   [descriptors]: get 获取变量属性值时自动调用
   set 监视当前扩展属性，改变变量属性值的时候自动调用，必须写形参，形参会自动接收修改后的值
    value:指定值
   writable:标识当前值是否可以修改默认false
   configurable: 标示当前属性值是否可以删除，默认false
   enumerable: 标示当前属性是否能用for in 枚举 默认false
   
   // 数据绑定
   var obj = {
        name: '张三',
        age: 18,
        behavior: function () {
          console.log('我是一个行为')
        }
      }
      var obj1 = {};
      for (var i in obj) {
        (function (i) {
          Object.defineProperties(obj1, {
            [i]: {
              get: function () {
                return obj[i]
              },
              set: function (msg) {
                obj[i] = msg;
              }
            }
          })
        })(i)
      }
      obj1.age = 12;
      console.log(obj1, obj)
  */
  ```

+ Object.defineProperty()

  ```js
   var obj = {};
      // 外定义变量做存储
      var sex = '男'
      Object.defineProperty(obj, 'sex', {
        get: function () {
          return sex
        },
        set:
          function (msg) {
            console.log(msg)
            //只能用三方变量存储，不能使用对象方式修改值，否则会反复调用
            sex = msg
          }
      })
      obj.sex = '女';
      console.log(obj.sex, obj)
  ```

  

## 1.1 Function 扩展

+ bind()

  ```js
  /*
  bind和call,apply相似，可以改变指向和传参，但apply和call都是立即调用立即执行的，bind则返回的是改变指向后的'新'的函数需要重新调用
   obj.bind专门用于修改定时器this的指向
   function sum(a, b) {
        return a + b
      }
      let c = 3;
      let d = 4;
      let h = document.querySelector('h1')
      h.onclick = function () {
        this.e = sum.bind(this, 4, 2) // 此时this指向sum并把新的函数赋值给this.e
        console.log(this.e()) // 再次调用仍可以得到返回值
        console.log(sum(3,1)) // 调用原来的函数仍有效，只不过这两个函数的调用对象不同
      }
  注意
  	使用bind改变一个函数this指向时，是返回一个当前传入this指向的该函数到当前行，在使用此函数时，它的this就指向你传入的对象。而 bind和call调用是一次性的，它没有返回值
  */
  ```
  
  

## 1.2 ES6 新增

接口==API==方法

+ let用法，特点

  ```js
  /*
  let
   特点：再块作用域内有效，不能重复声明，会预处理，有变量提升(放到script对象里,不会放到global全局里，且是window的下级)，但不能提前使用提升的变量
   有块级作用域就可以解决同步异步问题(不需要使用之前的立即执行函数解决)，let每次使用都会生成一个自己专属的块级作用域(var没有此作用域)
   
  var 
  特点：有变量提升，作用域提升，创建的属性和方法直接放到global对象，没有自己的作用域
  */
  ```

+ const

  ```js
  /*
   const 关键字专门用来定义常量，设置为常量的变量的值不能更改，如果更改会报错
  */
  ```

+ 变量的解构赋值

  ```js
  /*
  解构赋值为值传递，值得改变不会影响原型
   1.对象的解构赋值
    解构赋值按需索取，左边需要用{}来表示为解构赋值，右边需要写解构的对象
    用指定的key得到对应的value
    
    let obj2 = {username:'kobe',age:33}; 
    let {username,age} = obj2;
    console.log(username,age)
    
    2.数组的解构赋值
    用指定的下标的到对象的value
    let arr= [1,2,3]
    let [a,b,c] = arr; //会对位赋值，如果想拿其他得一个值可以用逗号进行占位(定义了空对象获取原位上得值)
    let [,,e]=arr; // 此时拿到得为3
    
    
    
    3. 对象再函数中得解构赋值
    let obj = {
        name: '张三',
        age: 3
      }
      function fun({ name, age } = obj) {
        console.log(name, age)
      }
      fun(obj)
      
     4. 数组再函数中的解构赋值 
     let divs = document.querySelectorAll('div')
      function fun([a, b, c] = divs) {
        a.onclick = function () {
          alert('我被点击了')
        }
        b.onclick = function () {
          alert('我是b')
        }
        c.onclick = function () {
          alert('我是c')
        }
      }
      fun(divs)
  */
  ```

+ 模板字符串

  ```js
  /*
    使用``符号表示，需要拼接的值用${}进行表示
    let a = 1;
      let str = 'lyf'
      console.log(`我是${str}`)
  */
  ```

+ 简化对象写法

  ```js
  /*
    如果在全局中事先定义了对象中需要的键值可只写键，系统默认会把它的键值同名引用。函数可直接省略function直接使用 xxx(){}创建
    ----- 之前写法
    let userName = '张三'
      let age = 12
      let obj = {
        name: userName,
        age: age,
        showName: function () {
          console.log(this.name)
        }
      }
      //--------------- es6写法
      console.log('---------------')
      let obj1 = {
        userName,
        age,
        showName() {
          console.log(this.userName)
        }
      }
      console.log(obj, obj1)
      obj1.showName()
      obj.showName()
  */
  ```

+ 箭头函数

  ```js
  /*
   
   
  1. 常规函数
   let fun2 = function(){
   	console.log(1)
   }
   
  2. 箭头函数 
  （1） 在单行箭头函数中，如果函数是一条语句，且没有默认返回值，则箭头函数的返回值为undefined(由语句发出)，如果函数是多条箭头函数，且没有写返回值，返回值为undefined(由箭头函数发出)
  （2）只有一行时可以以;结尾(和循环选择结构一致)，且没写return情况下会自动return一条语句或者表达式
   let fun = () => console.log(2);
  （3） 当箭头函数只有一个形参时，箭头函数()可以省略,其他写法都需要写上()，因为形参代表一个整体
   let fun3 = a => ocnsole.log(a);
   
   3. 箭头函数特点
   （1）箭头函数没有自己的this,箭头函数的this不是调用的时候决定，而是在定义的时候由上层对象决定的，它的上层对象的this指向谁，它的this就指向谁，如果在上层同时出现多个箭头函数，则它们的this的指向为最外层箭头函数的this的指向
   （2）在构造函数中，不能使用箭头函数，因为this要指向实例对象，如果使用箭头函数this则指向的是window
   let divs = document.querySelectorAll('div');
      let [a, b, c] = divs;
      a.onclick = function () {
        let fun = () => {
          console.log(this) //a
          let fun1 = () => {
            console.log(this) //a
            let fun2 = () => console.log(this); //a
            return fun2();
          }
          return fun1();
        }
        return fun();
      }
  */
  ```

+ 点点点(...)运算符

  ```js
  /*
  ...运算符会自动调用iterator接口(方法)
  用途1： 点点点运算符和arguments一样可以收集传入的形参变量，但是一个真数组，和arguments相比有更多的方法可以使用。此外，点点点运算符比arguments更灵活，可以和普通的形参搭配使用,如果普通形参没有收集到的变量全都会放到点点点运算符里，点点点运算符必须放到形参的最后，且不能跟额外的普通形参
   let fun = (a,...values) => console.log(values);
      fun(1, 2, 3)
  
  用途2： 可以插入数组
  点点运算符可以对指定的数据进行拆包，从而插入或改变另一个对象，拆包不能直接遍历对象
  let arr1 = [2, 3, 4]
      let arr = [1, ...arr1, 5]
      console.log(arr) 1 2 3 4 5
  */
  ```

+ 形参默认值

  ```js
  /*
   形参默认值，在函数没有传入参数的时候，函数在使用形参时，会使用形参设置的默认值作为参数。形参默认值的类型一般根据需要的实参类型决定
    let distance = (x = 0, y = 0) => console.log(x, y);
      distance(5, 10)
      
    在没有形参默认值时的写法
    let distance1 = (x, y) => {
        if (!x) x = 0;
        if (!y) y = 0;
        console.log(x, y)
      }
  */
  ```

+ Symbol数据类型

  ```js
  /*
  Object.keys(obj)原型方法可以把Obj的所有键存在一个数组里
   1. ES6中添加了一种原始数据类型Symbol(现有6种数据类型，number,string,boolean,undefined null,symbol)
   typeof 返回值7种(number,string,boolean,undefined,function,object(null,object,array),symbol)
   2. Symbol特点：Symbol的属性是唯一的，解决了命名重复问题，它的值不能和其他数据类型进行计算，包括字符串拼接。此外，for in,for of 也不能遍历symbol属性
   3. Symbol类型的引用，symbol类型可以使用内置对象Symbol()进行引用转换
   4. Symbol === symbol false 地址不一样
   5. 内置Symbol值
   （1）Symbol.iterator(遍历器，迭代器)，为一种接口机制，为各种不同的数据结构提供统一的访问机制，使数据结构成员按某种次序排列
   （2）ES6创造了一种新的遍历命令，for...of循环，iterator接口主要使用for...of
   for...of工作原理：内部会创建一个指针对象，只想数据结构的起始位置，第一次调用next方法，指针自动指向数据结构的第一个成员，接下来不断调用next方法，指针会一致往后移动，直到指向最后一个成员，每调用一次next方法返回的是一个包含value和done的对象，{value:当前成员值，done:boolean}.value表示当前成员的值，done对应的布尔值表示当前的数据的结构是否遍历结束，当遍历结束的时候返回的value值是undefined,done值为false
   
   重写内部方法(适用对象及数组) Object.prototype[Symbol.iterator]
   // 此函数必须用普通函数，因为要确定this的指向
   let iteratorTool = function () {
        let index = 0;
        // 箭头函数会指向外层函数变量
        let that = this;
        if (this instanceof Array) {
          return {
            next: () => {
              return {
                // that 接收外部this指向
                value: that[index],
                done: Boolean(!that[index++])
              }
            }
          }
        } else {
        // Object.keys(obj)原型方法可以把Obj的所有键存在一个数组里
          let objArr = Object.keys(that)
          return {
            next: () => {
              return {
                value: that[objArr[index]],
                done: Boolean(!that[objArr[index++]])
              }
            }
          }
        }
      }
      Array.prototype[Symbol.iterator] = iteratorTool;
      Object.prototype[Symbol.iterator] = iteratorTool;
      let arr = [5, 4, 8, 5, 8];
      let obj = {
        name: 'zs',
        age: 14,
        sex: 'male'
      }
      for (let item of obj) {
        console.log(item);
      }
      for (let item of arr) {
        console.log(item);
      }
  */
  ```

  

+ class

  ```js
  /*
  类是构造函数的上层封装，属性和方法的查找采用就近原则，自己有用自己的没有用上层的
  class的this指向类的实例对象
    一.定义类
    和构造函数写法相似，但此类才是真正的类，构造函数的类是一种非正式的类。相比构造函数创建类对象，定义类创建可以划分创建块，且可以直接再此原型对象上添加方法
   
  class Person {
    // 类的构造方法，生成一个类必定会调用此方法，constructor项为一个真正的类
        constructor(name, age) {
          this.name = name
          this.age = age
        }
        showInfo() {
          console.log(this.name, this.age)
        }
      }
      let p1 = new Person('张三', 14)
      Person.msg = 'personProperty';
      console.log(p1, Person)
      p1.showInfo()
  
  // 构造函数创建类，constructor本质上是一个函数
      function People(name, age) {
        this.name = name
        this.age = age
      }
      People.prototype.fun = function () {
        console.log(1)
      }
      let pe1 = new People(1, 2);
      console.log(pe1)
      pe1.fun()
      
   // 由上两种创建方法可知，构造函数创建类时，它的constructor选项是一个函数，而不是真正的类，而后，想要添加共有的方法需要到外部的原型对象上绑定。相反，类创建类时，它的constructor选项是一个class类，而后，想要添加攻游的方法直接再类对象内(constructor外)添加即可，系统会自动绑定到原型对象下
   
   二.给类&&构造函数添加属性
   本质上是给constructor下添加属性，可以调用类名，函数名.的方式使用
   类添加属性方法 ，再内部用 static num=1 或 Person.a =1方式添加
  static 声明的属性和方法只有其本身大类可以调用
   class Person {
        static num = 1;
        constructor(name, age) {
          this.name = name
          this.age = age
        }
      }
      let p1 = new Person('张三', 14)
      Person.msg = 'personProperty';
   
   构造函数添加
   function People(name, age) {
        this.name = name
        this.age = age
      }
   People.a = 123
   
   三.类的继承
   1.使用 class 类名 extends 类名 继承父类 ，此时可以使用父类的属性和方法，此时子类的原型对象从object变为父类的原型(相当于再原型链中插入一个类对象)，且不会改变constructor构造者，不会有原型命名污染(构造函数继承会改变constructor构造者，需要重新指向，且会污原型对象池)
   2.子类可以通过super来调用父类的constructor来帮助自己创建实例对象，当调用super时，super会自动指向父类的constructor通过传入的形参，返回对应的值
   class Father {
        constructor(name, age) { // 用于接收外部变量并把它们存在类中进行赋值
          this.name = name
          this.age = age
        }
        fire = true  // 在constructor外定义的类全局变量也会被作为键值对添加给它所构造出来的实例或继承的类构造出来的实例，类全局创建的属性或方法this指向实例对象
        showInfo() {
          console.log(1)
        }
      }
      // 当一个字类继承了一个父类时，必须再其构造函数类写下super关键字，super(),此时可以传递参数让父类帮助声明参数
      class Children extends Father { // 没有污染的继承，不需要进行额操作
        constructor(name, age, sex) {
          super(name, age) // super关键字调用父类的constructor帮助创建实例,此时传入的参数的位置要和父级constructor位置一致，如果子级传参少，和父级的construcotr位数不匹配，而又会按位进行赋值，导致有些值会显示为undefined，所以为了避免此情况，要和父级传入的值个数相等(想要传入的参数和父级的参数对应)，然后delete删除多余的属性
          this.sex = sex
        }
      }
      let c1 = new Children(1, 2, 3)
      let f1 = new Father(1, 2)
      console.log(c1, f1)
      
      function P1(name, age) {
        this.name = name
        this.age = age
      }
      function P2(name, age) {
        this.name = name
        this.age = age
      }
      P2.prototype = new P1(); // 有污染的继承
      P2.prototype.constructor = P2; // 需要重新指向构造者
      let p2 = new P2();
      let p1 = new P1();
      console.log(p1, p2)
  */
  ```

+ 数值扩展

  ```js
  /*
    Number.isFinite(i)判断是否是有限大数
    Number.isNaN(i)判断是否是NaN
    Number.isInteger(i)判断是否是整数
    Number.parseInt(str) 将字符串转换为对应整数
    Number.trunc(i) 去掉小数部分
  */
  ```

+ 数组扩展

  ```js
  /*
   arr.from(v) 将为数组对象或可遍历对象转换为真数组
   arr.of(v1,v2)将一系列值转换为数组
   arr.find(function(value,index,arr){return true})找出第一个为true的元素
   arr.findIndex(function(item.index){return true}) 
  */
  ```

+ 对象扩展

  ```js
  /*
  1.
   Object.is()判断2各数据是否完全相等
   Object.is(0,-0) //false
   Object.is(NaN,NaN) // true
   
   2.
   Object.assign()将一个或多个源对象的属性复制到目标对象上，会改变目标对象，且返回一个拷贝后的对象
   let target = {};
   let source1 = {name:1};
   let source2 = {age:3};
   let result = Object.assign(target,source1,source2)
   
   3.
   直接操作 __proto__ 属性(es6属性)
   let obj ={};
   obj.__proto__ = target;  // 让别的对象成为我原型链中的一个隐式原型
  */
  ```

+ Set和Map数据结构

  ```js
  /*
   set容器：无序(不能通过下标获取)不可重复的多个value集合体，可以数用for of 遍历每个元素
      let arr = [2, 3, 1, 3, 5, 4]
      let set = new Set(arr);
      for (let i of set) console.log(i, set, set[i])
   set.add(value)添加一个属性
   set.delete(value)删除一个属性
   set.has(value)查看set中是否有指定元素，返回boolean值 
   set.clear()清空整个set
   
   map容器：无序的key不重复的多个key-value的集合体(需要传入一个二维数组)
      let arr1 = [[1, 2], [3, 4], [2, 3]];
      let arrObj = new Map(arr1);
      console.log(arrObj) // {1 => 2, 3 => 4, 2 => 3}
      arrObj.set(5, 6) // 再末尾添加一个键值对
      console.log(arrObj.get(1)) // 得到对应键的值
      arrObj.delete(1) // 删除对应的键值对
      arrObj.has(1) // 判断此键值对是否再对象中存在
      arrObj.clear() // 清除整个对象
  */
  ```

+ for of

  ```js
  /*
   数组有iterator接口，对象没有iterator接口，Set,Map又iterator接口
  */
  ```

+ 数组去重

  ```js
  /*
  es4
   let arr = [1, 2, 3, 1, 2, 3, 1, 2, 3, 5];
      let newArr = [];
      arr.forEach((item) => {
        if (newArr.indexOf(item) == -1) newArr.push(item)
      })
      console.log(newArr)
      
   es5
   let arr = [1, 2, 3, 1, 2, 3, 1, 2, 3, 5];
   let newArr = arr=>[...new Set(arr)]
   console.log(newArr)
  */
  ```

+ 通过对象toString方法拿到其他对象的数据类型

  ```js
  /*
  
   let obj = {};
    1. 因为object.toString()拿到的是[object Object],所以通过apply,call,bind可以改变方法的调用者，从而拿到不同类型的数据 
   let obj = {};
      let arr = [1, 2, 3];
      let num = 1;
      let str = '12';
      let objType = obj.toString().slice(8, -1)
      let arrType = obj.toString.apply(arr).slice(8, -1);
      let numType = obj.toString.apply(num).slice(8, -1);
      let strType = obj.toString.apply(str).slice(8, -1);
      console.log(obj.toString.apply(arr), numType, objType, strType)
  */
  ```

+ 深度克隆(浅拷贝，深拷贝)

  ```js
  /*
  浅拷贝深拷贝的判断应看变量里的对象表示方式，而不是看变量本身，一般来说只有对象里嵌套对象或数组，或者数组里嵌套数组或对象才需要判断是浅拷贝还是深拷贝，因为嵌套的数组或对象也是地址传递，如果不进行深拷贝，则还是操作的同一块内存
  如： let = {name:1,age:3,sex:{opition1:'male',opition2:'female'}}这种写法就要判断使用深拷贝还是深拷贝,单一的简单数据类型不用判断，一定是浅拷贝
   
   1.浅拷贝，修改拷贝后的数据，被拷贝数据也会修改,浅拷贝传递的是地址，操作的都是同一个对象
    str.slice() str.concat()都是浅拷贝
   2.深拷贝，修改拷贝后的数据，被拷贝数据不会修改，深拷贝是重新开辟一个相同的内存(数组或对象遍历赋值重新开辟内存)，返回这个内存地址值
    JSON.parse() && JSON.stringify()可以实现深拷贝，但转换的对象里不能包含函数
    
    3.实现深拷贝
    普通数据类型值传递，复杂数据类型地址传递，所以再遍历到复杂数据类型时，需要使用递归来复制里面的每个值
          // 深克隆
      function clone(obj) {
        if (!(obj instanceof Object)) return obj;
        let tmp = null;
        if (obj instanceof Array) {
          tmp = [];
        } else {
          tmp = {};
        }
        for (let i in obj) {
          if (obj[i] instanceof Object) tmp[i] = clone(obj[i]);
          else tmp[i] = obj[i];
        }
        return tmp;
      }
  */
  ```
  
+ es7

  ```js
  /*
   (幂)**
   数组方法，Array.prototype.includes(value)判断数组中是否包含指定value
  */
  ```

  

 

