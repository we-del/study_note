# TypeScript

js对变量没有限制类型，导致它一旦接收一个不符合其之后运算的逻辑时，会出现报错，且报错不明显，js代码变量的可读性也较差。所以为了解决这个问题出现了typescript

TypeScript以javascript为基础构建的语言，一个javascript超集，可以在仍和支持js的平台进行执行，扩展了js并添加了变量类型  。ts不能被js解析器直接执行，需要借助编译来转化为js

typescript可以兼容大多数ie浏览器，并可以指定编译js的版本，如es3,es5等

在一个确定值正确但ts提示报错时，**可以在句尾添加!取消该提醒**

+ TypeScript开发环境搭建

  ```js
  /*
   下载  npm i typescript -g ，即可创建ts文件进行编码
   编译ts文件： 在ts当前目录打开cmd或powersheel 使用 tsc xx.ts 进行编译
  */
  ```

typescript对函数变量接收的校验比js更严格，少传多传以及声明了多个相同的参数都会报错

## 1.0 基本语法



+ 类型声明

  ```js
  /*
  前言：
  	类型声明是TS非常重要的一个特性
  	通过类型声明可以指定TS中变量(参数，形参)的类型
  	指定类型后，当为变量赋值时，TS编译器会自动检测值是否符合类型声明，符合则赋值，否则报错
  	简而言之，类型声明给变量设置了类型，使变量只能存储某种类型的值
  
  js基本类型有：number string boolean undefined  null symbol
  1. 指定接收类型并限定接收类型
   let a :number  // 声明一个变量a,同时指定它的类型为number，此时a只能存储number类型，如果限制为:string :boolean 同理
   let c:boolean = true  //声明变量并赋值
   
  2. 变量声明时就赋值,自动指定对应接收变量类型
   如果ts变量在声明时就赋值，则ts会对该变量进行检测，以后该变量只能接收该类型的值
   
  3. 给函数形参添加类型声明
  	function sum(a:number,b:number):number{} // 表示a和b只接收number类型,函数返回值也是一个number
  	
  4. ts中能使用的类型
  	number(任意数字),string(任意字符串) boolean(任意布尔值) 
  	自面量(限制变量的值就是该字面量的值，字面量为其本身) 
  	any (任意类型 )  unknown (类型安全的any )  void(没有值)
  	never(不能是任何值) object(任意js对象)  array(任意js数组)
  	tuple(ts新增类型，固定长度数组)  enum(枚举,TS新增类型)
  	
  	字面量使用: let a:10 //设置a的字面量为10该值不能更改，和const属性相似
  字面量可以同时设置多个值是用 | 来设置  如 let a:'male'|'female'|'gender',表示a之恶能被赋值为'male','female','gender'赋值其它值报错
  	any使用(少用)：let d:any ;d=1;=d='1';使用了any表示对该变量关闭了ts类型检测，其可以为任意类型数据，和js中的变量接收范围相似
  	unknown使用：unknown 表示未知类型的值，和any效果类似，可接收任意变量类型，但其在作为值传递给其他变量时，会受到变量接收限制，如 unknow类型接收了一个string的数据，在赋值给number类型变量时会报错，而any数据不会
  	void使用：用来设置返回函数的返回值，表示该函数没有返回值(undefined)，如function():void{}  。如果设置为function():number{}表示返回的是一个数字，番薯返回值设置也可使用 |(或) 来根据需要设置不同的返回值(if else) 写成 function():number|:string{}
  	never使用：表示永远不会返回结果 ,没有任何数据，通常用来表示一段报错信息，如给函数声明一个never表示该函数不会返回任何数据(undefined也不会返回)，通常此函数用来打印报错信息 如 function fn2():never{throw new Error('报错！')}
  	object使用：object表示一个对象，在应用中较少使用(js万物皆对象) 通常使用 字面量 方式进行限制  如 let b: {name:string} 表示b必须是一个对象，有且只能有一个name属性该属性的值必须是一个字符串 。 在对象的属性前加上一个?表示该属性是一个可选属性，即可以有也可以没有 如 let c:{name:string,?age:number} 表示c必须是一个对象必须包含name属性,age属性可有可无。如果想设置一个字面量对象必须包含一个name属性其他属性随意可写成: let e:{name:string,[propsName:string]:any} ,其中propsName可以写成任意值只是用来占位，其中[propsName:string]表示可以接收任意个key，key的类型必须是string,:any表示这写key的值为任意值
  	function使用：function表示一个函数，其应用较少，因为函数定义是通常会赋值，在赋值的使用TS会自动检测其接收数据 。 通常使用字面量方式去设置函数可接收类型和形参类型  如 let f :(a:number,b:number)=>number 表示 f 变量只能接收形参为number类型数据(两个形参都只能接收number数据，如果b为string，表示第一个形参接受number数据，第二个形参接收string数据)，返回值也必须为number数据 
  	array使用：array表示一个数组，在声明时可以指定该数据可接收的变量类型，如let e:string[],表示e数组只能保存字符串数据，let e:number[]表示e数组只能接收number类型数据。还有另一种写法 let e:Arrary<number> 表示e数组只能接收number数据，其他类型同理
  	tuple使用：元组为固定长度的数组，如let h:[string,number]表示一个数组只能接收两个参数，第一个参数为字符串，第二个参数为数字
  	enum使用：在一个数组或对象中的一个值可能有多个可选值时，可以使用枚举类型，把这些可能出现的数据写在枚举类型中 ，这样更具有语义化，如 {
  		enum Gender{Male,Female}
  		let i:{name:string,gender:Gender} 
  		i ={
  			name:'李四',
  			gender:Gender.Male
  		}
  		console.log(i.gender === Gender.Male) // true
  		
  	
  	} 枚举类型数据会按排列顺序自动赋值 0 1 2 3.。。
  	
  	
  
  注意
  	只声明变量不指定类型，ts会自动把它当作any类型
  	any类型的变量可以赋值给任意数据类型(any不用的原因)
  	unknown实际上时一个类型安全的any，其变量不能直接赋值给其他变量，需要做if判断才行，如unknown类型数据赋值给number类型时 if(typeof a:unknown ==='number'){b=a} 。或者使用类型断言 s = a as string 表示该unknown类型a是一个字符串类型
  	any和unknown类型尽量少用，如果必须用，则使用unknown
  	在声明变量时可以使用 | 方式表示,如 number|string 表示可接收的类型是number或者string . 也可使用 &方式表示，&常用在对象中 如 let j:{name:string} &{age:number}表示对象要包含name和age .  | 表示同时满足条件中的一个即可，&表示同时满足所有条件
  	
  */
  ```

  

+ 类型别名

  如果一个类型声明过长可以使用 type myType = xxx|xxx|xxx 来设置一个别名，使用myType就表示使用 xxx|xxx|xxx   如**type myType = xxx|xxx|xxx ;let m: myType**

## 1.1 ts编译选项

+ 选项集

  ```js
  /*
   1. 使用 tsc 1.ts -w   第一次运行后，会在当前cmd开启一个监视，每次该ts变化时都会实时更新到js中，终止该cmd监视功能也取消，缺点，一个命令行只能对一个ts文件监视
   2. 在当前ts文件创建tsconfig.json配置文件  在cmd中使用tsc(此时tsconfig为空内容) 即可监视所有ts文件
   	tsconfig.json配置项
      "include":[] 表示那些ts文件需要被执行 
   		如 "include":[".\src\**\*"]  // 表示src下任意目录任意ts文件都需要执行
   	"exclude":[] 表示那些ts文件不需要被执行 
   		如 "exclude":[".\src\hello\*"]  // 表示src下hello目录里的所有ts文件不执行
   	"files":[] 指定被编译文件的列表，只有需要编译的文件小时才使用
   		如"files":["1.ts","2.ts"]
   	"compilerOptions":{} 编译器配置选项 
   		选项包括(输入一个错误值会提示可选课项) 	
   			target用来指定ts被编译为的es版本，默认转换为 es3
   				如 "target":"ES5"
   			module指定要使用模块化的规范
   				如 "module":"commonjs" 
   			lab用来指定项目中要使用的库，一般不需要更改
   				如 "lab":["dom"] // 表示只能使用dom库
   			outDir用来指定编译后的ts文件放到那个文件夹
   				如 "outDir":"./dist"  // 把所有的js文件放到dist文件夹
   			outFile用来把全局作用域中的代码合并到同一个文件中(用的少)
   				如 "outFile":"./dist/app.js"
   			allowJs用来选择当前目录的js文件是否会被编译到指定文件夹，是一个布尔值
   				如 "allowJs":false // 表示当前目录的js会不被编译到dist文件夹,true则返回
   			checkJs用来检查js是否符合代码规范,是一个布尔值，默认为false
   				如 "checkJs"：true  // 表示检查js
   			removeComments表示是否移除ts注释，是一个布尔值，默认为false
   				如 "removeComments":true  // 移除注释
   			noEmit表示不生成编译后的文件 ,默认为false
   				如 "noEmit":true // 不生成ts编译文件
   			noEmitOnError表示在有错时不生成编译文件，默认false
   				如 "noEmitOnError":true // 有错误不编译
   			alwaysStrict用来设置编译后的文件是否使用严格模式，默认false
   				如 "alwaysStrict":true  // 使用严格模式
   			noImplicitAny用来声明ts文件不能使用隐式any，是一个布尔值
   				如 "noImplicitAny":true // 不能使用任何隐式any(没有限定类型时，默认为隐式any)
   			noImplicitThis表示不能使用不明确类型的this
   				如 "noImplicitThis":true //检查出所有this指向不明确的this(确定this的指向如 this:window )
   			strictNullChecks表示严格检查空值，是一个布尔值
   				如 "strictNullChecks":true   // 检查所有空值变量
   			strict 表示所有严格检查的总开关，如果它写为true，则严格检查的所有功能都为true,如果有一两个想要false，在下面写上覆盖即可
   				如 "string":true  // 所有严格检查都开启，推荐
   			
  */
  ```

  

## 1.2 面向对象

指通过对象进行操作

类中的this指向它的调用这， this.xx 可以使用该原型类给该实例类的属性

+ 类的定义

  ```js
  /*
   使用 class Person{  定义
    a:numer = 1    // 可以写定类型并赋值，此后a只能接受number型
    b = 2 // 也可直接赋值由ts自动判断接收类型，此后b只能接受number型
    readonly name= '李四'  // 设置该属性为只读类型，不能修改
    static age = 18  // 只有Person原型可以使用的属性
    static readonly sex = 'male' // 只有Person原型能只读的属性
   }
   
   注意
   	被static修饰的值只有Person原型可以使用，没被static修饰的值只有实例可以使用
   	一般使用类创建对象都是经过constructor()函数，其中用到的属性需要在全局类下声明，如name:string,age:number，然后在constructor中使用 如 constructor(name:string,age:number){ this.name=name;this.age=age}
  */
  ```

  

  

+ 抽象类

  抽象类和普通类效果一致，不过不能使用抽象类创建对象(可以继承)，在class前使用abstract即可变为抽象类



+ 类中的属性修饰符

  ```js
  /*
   类中的属性修饰符用来限定属性在什么时候生效 
   包括  
   	readonly 只能读取 不能修改   
   	static 只能原型类使用
   	public 都可以使用
   	producted 受保护的属性，只能在当前类和当前类的子类中使用和修改(即继承也可以使用)
   	private 类中的私有属性，该属性只有在类中可以使用 。但可以通过函数返回值方式返回，也可通过函数传值方式修改私有属性 。 也可以使用内置方法 get xx(){reutrn this._xx}返回，当该类的实例使用了 per.xx时实际上是得到了get xx()的返回值 ， 当在设置内置属性时可以使用 set xx(value){this._xx =value }在类中定义 , 实例则可以使用 per.xx='我来操作' 直接调用set xx(value)方法，从而间接的设置内置属性
   	如 {
   		class Per{
   			private _name:string;
   			constructor(name:string){
   				this._name = name
   			}
   			get name(){
   				return this._name
   			}
   			set name(value){
   				this._name = value
   			}
   		}
   		let p1 = new Per('张三')
   		console.log(p1.name)  // 实际上调用的是get name()并得到返回值
   		p1.name='李四' // 实际上调用的是 set name(value) 设置了_name私有属性
   	}
   	
   	语法糖创建类
   	{
   		class C{
   		constructor(public name:string,public age:number){}
   		}
   		const c = new C('x',11)
   		console.log(c)
   	}
  */
  ```

  



## 1.3 接口

+ 基本使用

  ```js
  /*
  1.概念
   接口使用 interface定义，用来定义一个类结构中应该包含的属性和方法，同时接口可以当成类型声明去使用
   接口在定义类的时候限制了类的结构，其所有的属性和方法都不能由实际的值
   多个相同的接口会对其声明的属性进行合并
   	如{
   		interface Inter{name:string};
   		interface Inter{speack():string;}
   	} // 最终会进行合并为 interface Inter{name:string;speack():string}
   
   2.使用
   	定义类时，可以时类去实现一个接口，即使类满足接口要求
   		如  {
   			interface myInter{name:string;age:number}
   			class My implements myInter{
   				constructor(name:string,age:number){
   					this.name =name
   					this.age = age
   				}
   			}
   		}
  */
  ```

  

## 1.4 泛型

在定义函数或类时，如果遇到类型不明确的情况就可以使用泛型

+ 使用

  ```js
  /*
   function fn<T>(a:T):T{
   	return a
   }
   当一个函数接收的数据类型可能包含很多最终数据类型性时可以把该函数指定为泛型，方便接收
   泛型的定义方法<'任意值'> 同时形参变量声明时的值要和这个值相等，相当于一个占位符用来接收传入形参的数据类型，当传入的值为number类型时(不指定数据类型的情况)，该函数和形参的类型就是number。（指定数据类型的情况）当调用该函数时还可以指定泛型如 fn<string>('12') 表示该函数接收的类型为string
   	如 { 
   		// 单参数泛型
   		function fn<T>(a:T):T{
   			return a;
   		}
   		let r1 = fn(10) //  没有指定数据类型，由ts自动判断数据类型传给T
   		let r2 = fn<string>('12') // 指定数据类型，直接把该数据类型传给T
   		
   		// 多参数泛型
   		function fn2<T,K>(a:T,b:K):T{
   			return a;
   		}
   		fn2<number,string>(123,'hello')
   		
   		
   	}
   	
  */
  ```

  

