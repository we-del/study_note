# Typescript(new)

# typescript和java很像

## 细节

+ 集合

  ```java
  /*
   1.ts有具有多态特性，也能向下转型
   2.ts和java再功能上非常相似，有面向对象思维非常容易上手
   3.ts中被static修饰的属性或方法只能由类自身使用,如 Person.w()
   4.如果存在继承关系，则需要手动再第一行调用super()去调用父类(ts中如果没有手动调用则其会自动调用一个空的super())
   5.使用类型断言完成多态向下转型, 如 (personAndMan as YoungMan).toDown() 
   6.当使用ts的变量保存一个html对象时 使用 HTMLElement 数据类型保存，如 ele:HTMLElement
   7.当使用ts的变量保存一个html对象伪数组时 使用 HTMLCollection 数据类型保存，如 bodies:HTMLCollection
   8.数组的定义方法 number[] (定义了一个number类型的一维数组) string[][](定义了一个string类型的二维数组)
   9. typescript中的属性不会自己赋初值，需要自己添加
   10. 当某个方法返回值可能是多个类型时，需要使用断言来确认类型
   11. tsc -w 可以对当前目录的所有ts文件进行监听，一发生更改立刻更新js文件
   12. js对象和类创建的实例对象的区别
   	js对象返回的数据是一次性的，即只能返回指定字段的一个地址，
   	js类创建的实例对象是可以复用的，通过该类可以生成相同字段不同值的属性，且可以生成任意个实例对象
   13. ts在使用数组时需要进行初始化，否则无法使用数组方法
   14. ts中的接口，其属性和方法 ，实现该接口的类都需要进行实现
  */
  ```

+ ts类继承过程

  ```java
  /*
   继承和java类似(应该也存在类加载)，当将生成实例对象时，会从当前类的构造器开始向上去调用其父类，一直向上调用直到调用到顶级父类为止，然后从顶级父类开始到当前类执行属性，构造器，然后依次将属性和方法压入到该实例对象的空间，最先执行的部分再该实例对象地址的最底部(符合栈结构，最底部的是顶级父类,其上的是第二级父类...直到当前类)，此时一个实例对象就创建完毕了，其地址中最上层的是自己的属性和方法，第二层是一级父类的属性和方法...最下层是顶级父类的属性和方法，当使用属性或方法时，会从当前类开始查找，当前类没有才会向下查找
  */
  ```

  

**webpack.config.js和packge.josn必须再同级目录**

typescript和java很像，用起来很香

ts是js的超集，在使用时，经过转换可以变为js

## 前言

![image-20220315153413617](D:\typora_import_images\typora-user-images\image-20220315153413617.png)

![image-20220315153555834](D:\typora_import_images\typora-user-images\image-20220315153555834.png)

![image-20220315153938367](D:\typora_import_images\typora-user-images\image-20220315153938367.png)







## 1.0 基本类型

![image-20220315162644212](D:\typora_import_images\typora-user-images\image-20220315162644212.png)

![image-20220315162744462](D:\typora_import_images\typora-user-images\image-20220315162744462.png)

```java
/*
object 类型使用
	object可以用来限制一个对象中可有的属性，有三种表示方式
		第一种表示方式 let b: {name:string,age:number} , 表示该对象必须接收一个name(且该属性的值必须是string)和age(且该属性的值必须是number)属性
		第二种表示方式 let b:{name:string,age?:number}, 表示该对象必须拥有一个name属性(且该属性的值必须是string)，age属性可有可无	  (类似于正则表达式中 ? 表示 接收0或1哥)
		第三种表示方式 let b:{name:string,[propName:string]:any}.表示该对象必须拥有一个name属性(且该属性的值必须是string),和任意个属性值为string类型的属性

function 类型使用
	function类型可以指定一个变量的函数结构
		如
			let a:(num:number,num:number)=> number; // 表示a这个变量必须指向指定结构的函数

数组类型使用
	和java类似
		如
			let a: string[]; // 表示a数组只能存储的是string类型

元组类型使用
	元组类型相当于定长的一个数组
		如
			let h:[string,string]; // 定义了一个元组，该元组里保存两个string

枚举类型使用
	枚举类型的值从左到右(或从上到下)默认为0~n,如果再某个端点指定了数值，则其后的数字回以该数值开始累加
		如
			num Gender{male,female,no} // 分别表示 0，1，2

 注意
 	any类型的变量可以接收赋值给任意的其他类型，而unknown类型的变量不能直接传递给其他变量，再传递时需要做类型判断
	函数不写返回类型默认返回void类型
	never类型表示该函数没有返回值多用于函数抛出错误，使用场景，再抛出错误时可以使用该变量返回
		如
			function ():never{ throw new Error("出错了")}
*/
```



+ 连接符

  ```java
  /*
    & 符号 
    	用于表示一个表达式要同时满足&左边和右边的表达式
    	let w: {name:string} & {age:number};
    	
    | 符号
    	用于表示一个表达式需要判断|左边或右边的一个表达式
    	let e: {name:string} | {age:number};
  */
  ```

+ 类型别名

  ```java
  /*
   ts支持给一个数据类型起别名
   	如  
   		type myString = string; // 此时myString 表示的就是string类型
  		let s:myString;	
  */
  ```

  

+ 类型断言

  ```java
  /*
   可以给一个我们知道的变量，但编译器检查不出来的变量直接声明其变量类型
   	语法
   		let ins:string = w as string;
   		let ins:string = <string> w;   
   	如
   		let e:unknown = 1;   let b:number = e as number;
  */
  ```
  
  



## 1.1 编译配置

![image-20220315192956927](D:\typora_import_images\typora-user-images\image-20220315192956927.png)

+ tsconfig.json选项

  ```java
  /*
   再tsconfig.json中可以配置需要编译的ts文件的配置项
   该文件中包含以下配置部分
   	"include":[],include可以配置需要编译文件的路径,该配置相中**表示任意目录，*表示任意文件
   	"exclude":[],excude可以配置不需要编译文件的路径，该配置相中**表示任意目录，*表示任意文件
   	"extends":"xx",可以继承其他配置文件的配置项，可以减少重复配置的书写
   	"file":[],file可以配置需要编译文件的文件集，然后改文件集下的文件都会被编译
   	"compilerOptions":{...},该选项下可以配置编译文件的各种输出限制，其下有非常多配置项(重要配置项~！)
   		"target":"xxx",用于指定转换的js版本 ,可以填入 ES5,ES4,ES3,ES6 等
   		"module":"",用于指定使用的模块化规范，可以填入 es2015,es2016,commonjs 等
   		"lib":[],用于指定项目中使用到的库，通常不需要更改
   		"outDir":"",用于指定转换为js后的文件输出位置，通常ts源码会和输出位置隔离
   		"outFile":"",用于将所有ts编译后的代码合并到一个文件中 
   		"allowJs":false, 用于限制js文件是否被编译(默认为false)，如果为false表示不把该ts目录中的js文件	  编译到指定目录;为true表示把该ts目录中的js文件编译到指定目录
   		"checkJs":false,用于检查编译后的js代码是否符合编译前ts设置的变量规范(默认为false),如果为false		 表示不对编译后的js进行检查，为true表示对编译后的js进行检查
   		"removeComments":false,用于移除ts被编译为js后的注释(默认为false,不影响ts中的注释),如果为false	  表示不移除，为true表示移除
   		"noEmit":false,表示不生成编译后的js文件(默认为false),如果为false表示生产js文件，为true表示不	  生成js文件;如果只希望语法检查可以使用，使用较少
   		"noEmitError":false,表示有错误时不生产遍历后的js文件(默认为false),如果为false表示出错也生成编	  译后的js文件，为true表示出错不生成遍历后的js文件
   		"strict": true, 以下严格检查的总开关，如果为true下面的都为true，如果以下某个配置项需要更改单         独设置即可(包括，alwaysStrict，noImplicitAny，noImplicitThis，strictNullChecks)
   		"alwaysStrict":false,用来设置编译后的js文件是否启用严格模式(默认为false)，如果为false表示不启	  用，为true表示启用
   		"noImplicitAny":false,表示不将ts中所有未声明类型的变量的类型设置为any类型(默认为false),如果为       false,表示将所有未声明类型的变量隐式设置为any;为true表示不对未声明的变量进行更改,从而暴露问题
   		"noImplicitThis":false,表示是否检查不确定类型的this(默认为false)，如果为false表示不检查，为	   true表示检查(如果this的类型不明确，也可以自己手动指定this的类型)
   		"strictNullChecks":false,表示严格检查null值，如果为false表示不检查，为true表示检查
   		
   注意
   	如果忘记某个配置项的可选配置项，可以输入一个错误的值，这样ts会进行可选值报错提示
  */
  ```

  

## 1.2 webpack打包ts

+ 安装依赖

  npm i -d webpack webpack-cli typescript ts-loader

+ 简单配置

  ```java
  /*
  此时webpack就能自动打包编译ts文件为js文件并放到指定目录，但为了完整性还需要扩展配置
  =====================================
  // package.json
  {
    "name": "ts",
    "version": "1.0.0",
    "description": "",
    "main": "helloWorld.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "build": "webpack"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
      "ts-loader": "^9.2.8",
      "typescript": "^4.6.2",
      "webpack": "^5.70.0",
      "webpack-cli": "^4.9.2"
    }
  }
  
  ===============================================
  // tsconfig.json
  {
    "compilerOptions": {
      "module": "es5",
      "target": "es5",
      "strict": true,
      "noEmitOnError": true,
      "allowJs": true,
      "removeComments": true,
      "checkJs": true
    },
    "exclude": [
      "node_modules"
    ]
  }
  ===================================================
  //webpack
   // 引入路径包
  const path = require("path");
  
  
  module.exports = {
      // 指定入口文件
      entry:"./src/index.ts",
  
      // 指定打包文件的目录
      output:{
          path:path.resolve(__dirname,"dist"),// 输出文件的位置
          filename:"bundle.js" // 输出文件类型
      },
  
      // 指定webpack使用到的模块
      module:{
          rules:[
              {
                  // test之地那个该插件作用的文件范围
                  test:/\.ts$/,
                  use:"ts-loader", // 使用的插件
                  exclude: /node_modules/   // 指定排除的文件
              }
          ]
      }
  }
  */
  ```

  

+ 自动引入所有js到指定页面

  npm i  -d html-webpack-plugin  (该插件可以自动识别所有js并嵌入到指定html文件中)

  ```java
  /*
   plugins:[
          // 自动创建html页面，并嵌入所有的js文件
          new HTMLWebpackPlugin({ // 可以传入一个对象，对象包含该页面的自定义信息
              //title:"myHtmlPage"  // 设置创建的html页面的标题
              template: "./src/index.html" // 以该路径的html文件为模板生成一个html文件
          }),
      ]
  */
  ```

  

+ 代码更改后自动运行webpack打包，无需手动执行的插件

  npm i -d webpack-dev-server (该插件再每次目标ts文件发生更改后会立刻自动打包webpack)

  ```java
  /*
  
  // 再package.json中的配置(只需再package.json中配置即可)
  {
  
   "scripts": {
      "build": "webpack",
      "start": "webpack serve"
    },
  }
  */
  ```

  

+ 每次代码更新前，先进行输出文件位置的文件清空，保证数据为最新数据的插件(没用此插件前默认为覆盖)

  npm i -d clean-webpack-plugin

  ```java
  /*
    plugins:[
          new CleanWebpackPlugin() // 再每次向输出路径输出文件时，先清空该目录的内容
      ],
  */
  ```

  

+ 引入语法转换插件(babel,babel专门用于解决兼容性问题)

  npm i -d @babel/core @babel/preset-env babel-loader core-js

  ```java
  /*
   // 局部完整版webpack配置
   // 引入路径包
  const path = require("path");
  // 引入 自动嵌入js的插件
  const HTMLWebpackPlugin = require("html-webpack-plugin");
  // 引入 清空目标位置文件的插件
  const {CleanWebpackPlugin} = require("clean-webpack-plugin");
  
  module.exports = {
      // 指定入口文件
      entry:"./src/index.ts",
  
      // 指定打包文件的目录
      output:{
          path:path.resolve(__dirname,"dist"),// 输出文件的位置
          filename:"bundle.js" // 输出文件类型
      },
  
      // 指定webpack使用到的模块
      module:{
          rules:[
              {
                  // test之地那个该插件作用的文件范围
                  test:/\.ts$/,
                  // loader里的内容从右向左(或从下到上)执行
                  use:[
                          // 如果时简单的loader配置向直接用数据包含即可(默认为loader)
                          // 如果右较复杂的loader配置项则可以使用对象形式具体配置
                          {
                              // 指定加载器
                              loader:"babel-loader",
                              //该loader的配置项
                              options:{
                                  // 预设配置项，通常用于指定转换器和浏览器兼容版本
                                  presets:[
                                      [
                                          // 使用的转换器
                                          "@babel/preset-env",
                                          {
                                              // 指定浏览器版本
                                              targets:{
                                                  "chrome":"88"
                                              },
                                              // 指定编译器版本
                                              "corejs":"3",
                                              // 使用corejs编译的方式，如果为usage为按需加载
                                              "useBuiltIns":"usage"
                                          }
                                      ]
                                  ]
                              },
  
                          }
                  ,"ts-loader"], // 使用的插件
                  exclude: /node_modules/   // 指定排除的文件
              }
          ]
      },
  
      // 配置webpack使用到的插件
      plugins:[
          // 自动创建html页面，并嵌入所有的js文件
          new HTMLWebpackPlugin({ // 可以传入一个对象，对象包含该页面的自定义信息
              //title:"myHtmlPage"  // 设置创建的html页面的标题
              template: "./src/index.html" // 以该路径的html文件为模板生成一个html文件
          }),
          new CleanWebpackPlugin()
      ],
  
      // 此配置项用来配置可以引用的模块类型
      resolve:{
          extensions: [".ts",".js"]
      },
      mode:"development"
  }
  */
  ```

  

## 1.3 面向对象

**面向对象基本和java的对象类似，有访问修饰符，static关键字，接口，抽象类，泛型，重载等，思想和java类似**

![image-20220316131602829](D:\typora_import_images\typora-user-images\image-20220316131602829.png)

![image-20220316132654115](D:\typora_import_images\typora-user-images\image-20220316132654115.png)



+ 可以给一个属性添加readOnly修饰符，表示该属性只能读，不能修改

+ 使用断言完成向下转型

  ```java
  /*
   abstract class Person {
      public state: string = "我是父类的属性";
  
      public eating(): void {
          console.log("是人都会吃饭");
      }
  
      public static unique(): void {
          console.log("我是独特的");
      }
  
      public abstract run(): void;
  }
  
  class YoungMan extends Person {
      private personality: string = "欢乐的";
  
  
      public run(): void {
          console.log("年轻人会奔跑");
      }
  
      public getPersonality(): string {
          return this.personality;
      }
      public toDown():void{
          console.log("使用类型断言完成多态的向下转型");
      }
  }
  
  let personAndMan: Person = new YoungMan();
  
  // 使用类型断言 完成都西昂向下转型
  console.log("======="+(personAndMan as YoungMan).toDown());
  */
  ```

  



## 1.4 ts实战

+ 写算法题

  ```java
  // 图的深度优先广度优先遍历
  class Graph {
      private readonly vertexes: string[];
      private vertexAssociate: Array<Array<number>> = new Array<Array<number>>();
      private readonly vertexState: Array<boolean> = new Array<boolean>();
      private readonly size: number;
      private result: string = "";
  
      constructor(vertexes: string[]) {
          this.vertexes = vertexes;
          this.size = this.vertexes.length;
          for (let i = 0; i < this.size; i++) {
              this.vertexState[i] = false;
          }
          for (let i = 0; i < this.size; i++) {
              this.vertexAssociate[i] = [];
              for (let j = 0; j < this.size; j++) {
                  this.vertexAssociate[i][j] = 0;
              }
          }
      }
  
      // 遍历列表
      public list(): void {
  
          let tmp: string = "";
          for (const vertex of this.vertexAssociate) {
              for (const ele of vertex) {
                  tmp += ele + " ";
              }
              tmp += "\n";
          }
  
          console.log(tmp);
      }
  
      // 拿到指定节点的第一个邻边节点
      private getFirstVertex(row: number): number {
          for (let i = 0; i < this.size; i++) {
              if (this.vertexAssociate[row][i] > 0) {
                  return i;
              }
          }
          return -1;
      }
  
      // 拿到指定节点的下一个邻边节点
      private getNextVertex(row: number, col: number): number {
          for (let i = col + 1; i < this.size; i++) {
              if (this.vertexAssociate[row][i] > 0) {
                  return i;
              }
          }
          return -1;
      }
  
      // 深度优先遍历
      private dfs(i: number): void {
          this.vertexState[i] = true;
          this.result += this.vertexes[i] + " -> ";
          let w: number = this.getFirstVertex(i);
          while (w != -1) {
              if (!this.vertexState[w]) {
                  this.dfs(w);
              }
              w = this.getNextVertex(i, w);
          }
      }
  
      public entryDfs(): void {
          // 重置点位
          this.result = "";
          for (let i = 0; i < this.size; i++) {
              this.vertexState[i] = false;
          }
          for (let i = 0; i < this.size; i++) {
              if (!this.vertexState[i]) {
                  this.dfs(i);
              }
          }
          console.log(this.result);
      }
  
      // 广度优先遍历
      private bfs(i: number): void {
          let queue: number[] = [];
          this.vertexState[i] = true;
          this.result += this.vertexes[i] + " -> ";
          queue.push(i);
          while (queue.length > 0) {
              let w = queue.shift() as number;
              let a = this.getFirstVertex(w);
              while (a != -1) {
                  if (!this.vertexState[a]) {
                      this.vertexState[a] = true;
                      this.result += this.vertexes[a] + " -> ";
                      queue.push(a);
                  }
                  a = this.getNextVertex(w, a);
              }
          }
      }
  
      public entryBfs(): void {
          this.result = "";
          for (let i = 0; i < this.size; i++) {
              this.vertexState[i] = false;
          }
          for (let i = 0; i < this.size; i++) {
              if (!this.vertexState[i]) {
                  this.bfs(i);
              }
          }
          console.log(this.result);
      }
  
      // 建立顶点连接关系
      public connect(): void {
          this.associate(0, 3);
          this.associate(0, 4);
          this.associate(3, 1);
          this.associate(1, 4);
  
  
      }
  
      // 实际做事点 , 讲究一个开闭原则 ， 后序再更迭
      private associate(n1: number, n2: number): void {
          this.vertexAssociate[n1][n2] = 1;
          this.vertexAssociate[n2][n1] = 1;
      }
  }
  
  // KMP查找算法
  class KMP {
      private next: Array<number> = [];
      private readonly searchTarget: string;
      private readonly size: number = 0;
  
      constructor(str: string) {
          this.searchTarget = str;
          this.size = str.length;
          // 根据传入的字符串得到对应的字符串匹配值
          this.next = this.getNextPattern();
      }
  
      public startSearch(str: string): number[] {
          for (let i = 0, j = 0; i < str.length; i++) {
              while (j > 0 && str[i] != this.searchTarget[j]) {
                  j = this.next[j - 1];
              }
              if (str[i] == this.searchTarget[j]) j++;
              if (j == this.size) {
                  return [i - j + 1, i];
              }
          }
          return [-1];
      }
  
      private getNextPattern(): Array<number> {
          let next: Array<number> = [];
          next[0] = 0;
          for (let i = 1, j = 0; i < this.size; i++) {
              while (j > 0 && this.searchTarget[j] != this.searchTarget[i]) {
                  j = next[j - 1] as number;
              }
              if (this.searchTarget[j] == this.searchTarget[i]) j++;
              next[i] = j;
          }
          return next;
      }
  
  }
  
  // 堆排序
  class Heap {
  
      public heapSort(arr: number[]) {
          // 3 4 5 2 1
          for (let i = arr.length / 2 - 1; i >= 0; i--) {
              this.adjustHeap(arr, i, arr.length);
          }
          for (let j = arr.length - 1; j > 0; j--) {
              arr[0] ^= arr[j];
              arr[j] ^= arr[0];
              arr[0] ^= arr[j];
              this.adjustHeap(arr, 0, j);
          }
      }
  
      // 堆调整
      private adjustHeap(arr: number[], i: number, length: number): void {
          let val: number = arr[i];
          for (let j = i * 2 + 1; j < length; j = j * 2 + 1) {
              if (j + 1 < length && arr[j] < arr[j + 1]) j++;
              if (arr[j] > val) {
                  arr[i] = arr[j];
                  i = j;
              } else {
                  break;
              }
          }
  
          arr[i] = val;
      }
  }
  
  // 入口类
  class Entry {
      public static start(): void {
          //  this.entryGraph(); // 开始图
          //this.entryKMP();
          this.entryHeapSort();
      }
  
      // 开始进入图
      private static entryGraph(): void {
  
          let vertex: string[] = ["A", "B", "C", "D", "E"];
          let graph: Graph = new Graph(vertex);
          graph.connect();
          graph.list();
          console.log("深度优先=======");
          graph.entryDfs();
          console.log("广度优先=======");
          graph.entryBfs();
          console.log("改变搜索条件");
  
      }
  
      // 开始进入KMP算法
      private static entryKMP(): void {
          let kmp: KMP = new KMP("aab");
          let position: number[] = kmp.startSearch(" aa bb aaw aab");
          console.log(position);
      }
  
      // 开始进入推排序
      private static entryHeapSort(): void {
          let heapSort: Heap = new Heap();
          let arr: number[] = [6, 2, 3, 5, 4, 1];
          heapSort.heapSort(arr);
          console.log(arr);
      }
  }
  
  // 开始主任务
  Entry.start();
  ```

  

## 1.5 ts环境配置

+ ts编写node需要的环境配置

  ```java
  /*
   下载依赖库
   	npm i -d @types/node // 此库用于引入node模块库 ，ts配置和之前配置无差异
   	
  */
  ```


+ 安装ts-node包可以直接编译ts文件

  ```java
  /*
   npm i -g ts-node  
  */
  ```

  

