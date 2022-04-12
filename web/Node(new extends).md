# Node(new extends)



+ node具有以下优势

  事件驱动

  非阻塞IO模型(异步)

  轻量和高效

  跨平台性

+ node和前端js不同之处

  没有document和windows对象

  只能进行文件和后端操作
  
  node是一个单进程单线程应用程序



## 1.0 模块操作

+ commonJS语法

  ```java
  /*
   require('路径') // 即可引入其他js文件导出module.exports 对象下的内容
  	注意
  		如果其他js文件没有导出内容，则默认会得到一个空对象
  		当导入js文件时，可以选择加上.js或不加，node会帮你补上
  		当使用 require进行引入操作后，会先判断该文件再本文件中是否执行过(根据其暴露的module.exports={}判断),如果没有执行则会先执行一遍该文件，并获得其暴露的module.exports模块 ，如果该文件已执行过，则直接获取该文间暴露的module.exports对象
  		引入插件库的查找方式：首先会去同级目录下的node_modules下查找对应的命名文件夹，如果有则进入该文件夹，并进入该文件的package.json文件执行该文件的main指令，最终找到导入的文件。如果没有package.json则会执行该包下的index.js文件
  
   module.exports.xx = '属性'
   	注意
   		 module.exports.xx 经过node优化后可以写为 exports.xx , exports.xx = '属性',其中 = 左边exports.xx是给导出exports对象下添加 一个任意key值 ， =后边的 '属性' 为导出的 加工后的地址
   		 每个.js文件都有一个自己独立的 exports对象，当再该对象下挂载导出的属性时，会找改对应的文件的exports对象下挂载 ， 当引入一个文件的exports对象时，会得到该文件的exports对象下挂载的所有内容
     特别注意
   		只有module.exports能改变 当前指向的对象地址(可以使用此特性来完成对多个元素进行暴露，exports只能往module.exports对象下单个添加数据)，当改变修改地址后只有使用 module.exports.xxx才能往新的对象地址下增加数据,使用 exports添加数据时会失效
   		let a = 11  
          let b = 22
          module.exports = {user: 'eee'}//修改指向的对象地址后，导出的数据为 { user: 'eee', age: 11 },使用exports.xx添加的数据会失效
          exports.name = b
          exports.ee = b
          exports.www = b
          module.exports.age = a  
          exports = {number: 'lyx'} 
   		 
  */
  ```

  

## 1.1 包管理器

+ npm常见命令

  ![image-20211218162821347](C:\Users\xx\AppData\Roaming\Typora\typora-user-images\image-20211218162821347.png)

  

## 1.2 文件操作

+ fs模块

  ```java
  /*
   同步读文件
   	fs.readFileSync(path[,{flag:,encoding:}])  
   		使用
   			let fsContent = fs.readFileSync('hello.txt',{flag:'r',encoding:'utf8'})
   			console.log(fsContent.toString())
   		注意
   			以此法获得的文件的文件内容为 Buffer数据，需要使用.toString()方法获取正确值
   		
   
   异步读文件
  	fs.readFile(path{,option},callback)
  		option由 flag,encoding等组成，用来完成异步读写操作
  		使用
  			fs.readFile("hello.txt",{flag:'r',encoding:"utf8"},(err,data)=>{
  				if(!(err)){
  					console.log('你出错了')
  				}else{
  					console.log('data')
  				}
  			})
  		注意
  			异步读写操作可以使用 promise进行封装
  			
   异步写文件
   	fs.writeFile(path,content[,option],callback)
   		使用
   			fs.writeFile('hello.txt','我来完成',{flag:'a',encoding:'utf8'},(err)=>{
   				if(err) console.log('111')
   			})
   
   删除文件
   	fs.unlink(path,callback)
   		使用
   			fs.unlink('1.txt',()=>{
   				console.log('删除完成')
   			}
   			
   读取目录
   	fs.readdir(path,callback)
   		使用
   			fs.readdir('../desktop',(err,files)=>{
   				if(err) console.log(err)
   				else console.log(files)
   			})
   		注意
   			files是一个数组，里面存放着该目录下的全部文件名
   			
    删除目录
    	fs.rmdir(path,callback)
    		使用
    			fs.rmdir('./ee',()=>{
    				console.log('删除成功')
    			})
    		注意
    			此方法删除的目录会直接删除，回收站也找不到慎用
   注意
   	flags为当前文件的操作有 r+(读写),a(追加),r(读) ,w(写),a+(读取追加)等
   	encoding为选择读取文件的编码形式，通常和文件的编码形式一致
  */
  ```

  

+ Buffer

  ```java
  /*
   Buffer可以在内存中开辟一个指定大小且连续的一个缓冲区
   为什么有Buffer?
   	因为js数组效率太低，Buffer可以满足数组要求的同时提高查询效率
   	js数组不能二进制存储，Buffer可以(以16进制显示，但以二进制存储)
   
   使用
   	创建Buffer
   		let buf = Buffer.alloc(10)  // 开辟了10个空间的Buffer,下标0-9
  			buf[0] = 2333 		
   		注意
   			此方法创建的Buffer是干净的，不受污染，但效率对比allocUnsafe较低
   		let buf1 = Buffer.allocUnsafe(10)
   			buf1[0] = 2323
   		注意
   			此方法创建的Buffer是可能会夹带内存其他信息，效率较高
   注意
   	Buffer一次最多只能显示255个字节超出255字节取末尾数，原存储数据不受影响
  */
  ```

  

+ 标准输入输出

  ```java
  /*
   使用 readline模块
   	let readline = require('readline')
      let r1 = readline.createInterface({
          output: process.stdout,
          input: process.stdin
      })
      r1.question('我来了', (answer) => {
          console.log('你是？' + answer)
          r1.close()
      })
      r1.on('close',()=>{
          console.log('你真棒')
          process.exit(0)
      })
    
   注意
   	标准输入输出在使用完后要使用process.exit(0)进行关闭管程，或使用  r1.close()事件来间接关闭管程，否则线程不会结束
   	
   轮询输入
   	let readline = require('readline')
      let r1 = readline.createInterface({
          output: process.stdout,
          input: process.stdin
      })
      let arr = []
      let flag = true
      function inquire(){
          return new Promise( (resolve,reject)=>{
              r1.question('我来了', (answer) => {
                  if(answer === 'n') {
                      reject('错误')
                  }
                  resolve(answer)
              })
          })
      }
  
      let loop = async()=>{
          try{
              while(flag)arr.push(await inquire())
          }catch(e){
              console.log(e)
              r1.close()
          }
      }
      r1.on('close',()=>{
          console.log(arr)
          console.log('你真棒')
          process.exit(0)
          if(flag){
              process.exit(0)
          }
          inquire()
      })
      loop()
  */
  ```
  



+ 文件流操作

  ```java
  /*
   在文件读取或写入中，如果有的文件占用空间较大，可能出现所有进程停滞等该文件读取情况，为了解决此问题，需要对较大文件进行流式操作
   
   创建写入流
   	fs.createWriteStream('路径'{,flag:xx,encoding:}) // 返回一个该文件的流对象,之后的操作都会对改文件为对象操作,该流对象下有write,end方法用于完成流式写入操作。事件open,read,close用于监听文件流是否开始
   	注意
   		使用write和end异步函数即可完成文件写入操作，使用事件是为了控制文件开始写入和结束写入的收尾工作
   		write函数中有一个形参err，使用该形参可以判断文件写入是否有误
   		write用于写入数据，end用于关闭写入流，在关闭写入流后，就不能继续进行写入操作
   		写入流执行顺序为：open -> read -> write -> end -> close
   	使用
   		let sw = fs.createWriteStream('./txtFile/stream.java', { encoding: 'utf-8', flags: 'a' })
          sw.on('open', () => {
            console.log('我打开文件了，快在此函数添加限制吧')
          })
          sw.on('close', () => {
            console.log('我即将写入完成了，快再次函数进行首尾工作吧')
          })
          sw.write('111', (err) => {
            if (err) {
              console.log(err)
            } else {
              console.log('输入成功')
            }
          })
          sw.end(() => {
            console.log('写完了！！，即将关闭流')
          })
   		
    创建读取流
    	let rs = fs.createReadStream('2.txt',{flags:'r',encoding:'utf8'}) // 此时就经过调用函数，返回了一个该文件路径的写入流对象，在之后的操作中都会已改文件进行对象操作
    	注意
    		rs读取流主要使用三个事件来对一个大文件进行流式读取，其中事件为 open,data,close
    		open在开始读取时触发， data在流式读取时触发，默认为65535(一个字节)作为一次进行读取 , close在结束读取时触发
    		data事件下其函数有个形参，用来接收流式读取的数据
    	
   
   流式读写操作
   	let ws = fs.createWriteStream('./txtFile/streamws.java', { encoding: 'utf-8', flags: 'a' })
      let rs = fs.createReadStream('./txtFile/stream.java', { flags: 'r' })
      rs.on('open', () => {
        console.log('开始读取文件了')
      })
      rs.on('data', (chunk) => {
        console.log('我正在已一个字节的速度流式读取文件')
        ws.write(chunk, (err) => {
          if (err) {
            console.error('读取错误')
          } else {
            console.log('此部分读取完成')
          }
        })
      })
      rs.on('close', () => {
        ws.end()
      })
  */
  ```

  

+ 管道流

  ```java
  /*
   管道流(pipe)是对流式读写的一个封装，可以快速完成一个文件到另一个文件的流式写入操作（即流式读取一个文件的所有内容并把该文件内容流式写入到另一个指定文件）
   使用
       let ws = fs.createWriteStream('./txtFile/streamws.java', { encoding: 'utf-8', flags: 'a' })
       let rs = fs.createReadStream('./txtFile/stream.java', { flags: 'r' })
       rs.pipe(ws)  // 即可完成复制内容操作
  */
  ```

  

+ 自定义事件

  ```java
  /*
   自定义事件完成函数调用
   // 自定义事件绑定，在使用on时可传递绑定函数
  // 在使用 emit时 触发函数
     let myEvent = {
        event: {
        },
        on(eventName, eventFn) {
          if (this.event[eventName]) {
            this.event[eventName].push(eventFn)
          } else {
            this.event[eventName] = []
            this.event[eventName].push(eventFn)
          }
        },
        // flag用来占位
        emit(eventName, ...args) {
          this.event[eventName].forEach(fn => fn(...args))
        }
      }
      myEvent.on('bind', (w, e) => {
        console.log('我来输出内容 bind')
        console.log(w, e)
  
      })
      myEvent.on('bind', () => {
        console.log('我来输出内容 bind2')
      })
      myEvent.on('bind', () => {
        console.log('我来输出内容 bind3')
      })
      myEvent.emit('bind', '打豆豆', '玩代码')
  */
  ```
  
+ 自定义事件模块

  ```java
  /*
   自定义事件模块是对自定义事件的一个封装，它也由on,emit等组要函数构成
   使用
      const { EventEmitter } = require('events')
      let ee = new EventEmitter()
      ee.on('hello', () => {
        console.log('hello1')
      })
      ee.on('hello', () => {
        console.log('hello2')
      })
      ee.on('hello', () => {
        console.log('hello3')
      })
      ee.emit('hello')
  
  */
  ```

  

## 1.3 爬虫相关

+ 内置模块path

  ```java
  /*
   引入 path 模块 
   	let path = require('path')
   	path 模块的常用方法
   		path.extname('str') // 获取扩展名
   			使用：let info = path.extname('111233.222.jpg')  // 结果为: .jpg
   		path.resolve('str1','str2',...) // 以当前绝对路径为前缀获取当前路径,可以使用数组收集,然后结构数组
   			使用: let info = path.resolve(...['/path','em']) // 结果为: 当前绝对路径\path\em
   		path.join(’str1‘,'str2',..) // 用法和resolve相同，但resolve会补全当前绝对路径,join不会
   			使用: let info = path.join('111','222')  // 结果为: 111\222
   		path.parse(__namefile) // 对一个绝对路径进行解析，解析结果为一个对象
   			使用： let info = path.jon(__namefile) // 结果为一个对象,其内属性有：root(解析盘符),dir(当前文件最近一级目录),base(当前文件),ext(当前文件后缀),name(当前文件名)
    补充
    	不适用path就能获取当前路径的node全局环境变量
    		__dirname // 获取到当前目录的绝对路径，不包含当前文件
    		__filename // 获取到当前文件的绝对路径
  */
  ```
  
+ os模块

  ```java
  /*
   获取当前cpu的情况
   引入OS模块
   	let os = require('os')
   	os模块常用方法
   		os.cpus() // 查看当前cpu详细情况
   		os.totalmem() // 查看总内存条容量
   		os.freemem() // 查看当前可用内存容量
   		os.arch() // 查看当前系统架构 ，(x64或x86)
   		os.platform() // 查看当前主机使用的系统
  */
  ```

  

+ url模块

  ```java
  /*
   url模块用于引入网络资源
   引入url模块
   	let {URL},url = require('url')
   	url模块常用方法
   		let urlNew = new URL('网络路径')
   			使用: let info = url.parse('https://www.baidu.com') // 结果：返回一个当前域名解析后的一个对象,包含 protocol(协议名),slashes,host(主机域名),port(端口),hostname(主机域名),hash(获取#的内容),search(得到?的内容),query(获得?后携带的参数),pathname(获得路径后缀),path(或的文件后缀，包括携带的参数),href(整体网络路径)
   		url.resolve('路径一','路径二') 
   			使用: let info = url.resolve(’https://www.baidu.com','eee') // 结果为https://www.baidu.com/eee .  如果路径一有协议名，路径二没有，则路径二拼接路径一。如果路径一没有协议名，路径二有协议名显示路径二，如果都没有协议名显示路径二
   		
  */
  ```



+ axios

  ```java
  /*
  	使用axios第三方库请求页面，等待返回数据
  	注意
  		axios请求网页返回数据
  		axios请求图片视频等(需要流式请求)  axios.get(baseURL, { responseType: 'stream' })
  */
  ```




+ cheerio包

  ```java
  /*
   和jquery类似，jquery用于web页面，cherrio用于爬虫 ， 两者语法相同
   cheerio爬取资源总策略：在返回的页面数据中，使用cheerio选择出需要获得的标签的属性
   	引入 cheerio
   		const cheerio = require('cheerio')  // 通过 $('ele') 即可选择需要的元素
   		使用
   			let $ = cheerio.load(res.data)  // 载入需要爬取的网页字符串
   			$('.ele img') // 载入后即可使用和jquery一样的查询方式
   			
   注意
   	把网络数据写入到本地即是下载数据
   	引入网络资源时要以此方式引入let res = await axios.get(baseURL, { responseType: 'stream' })，有了{ responseType: 'stream' }选项就能完成流式写入(用于下载图片，视频等)
   	只有被$()包括的元素才能使用jquery相关语法
  */
  ```



+ 爬取数据策略

  ```java
  /*
   1. 所有数据直接暴露在浏览器上
   	直接使用 sheerio包抓取数据即可
   2。 前端ajax渲染
   	在XHR中获取分页列表，对其解析即可
  */
  ```

  



+ 反爬虫策略

  ```java
  /*
   找一个代理帮助发起请求，防禁ip
   如果为前端渲染，则着重查看XHR,请求路径可能加密，可以通过反加密来获取
   
   注意
   	Network下的XHR可以快速查看网络请求的重要信息
  */
  ```

  

+ Puppeteer 库（前端自动化，爬虫）

  ```java
  /*
   由Google维护的一个库，该库可以通过代码自动模拟用户做相应操作(如生成PDF页面等)
   下载puppeteer库
   	npm i puppeteer
   引入
   	let puppeteer = require('puppeteer')
   	let browser = await puppeteer.launch({headless:false}) // 开启一个浏览器，其中{}为配置项，headless表示该浏览器是否由界面(false为有界面,true为无界面)，(无界面浏览器性能更高，有界面一般用于调试)
   常用类和方法
   	let page = await browser.newPage() // 打开一个新页面
   	await page.goto('网址') // 访问一个网页
   	
   	// 选择全部指定元素标签并把它作为函数的第一个形参传入
   	page.$$eval('a', ele => {  // 可得到指定标签的属性，选择一个元素标签，将其的所有元素都放到ele形参上
      ele.forEach(i => console.log(i.innerHTML))})
      
      // 选择一个指定元素标签并把它作为函数的第一个形参传入
   	page.$eval('#a', ele => {  // 可得到指定标签的属性，选择一个元素标签，将其的所有元素都放到ele形参上
      ele.forEach(i => console.log(i.innerHTML))})
      
      // 选择所有指定元素
    	let eleHandler = await page.$$('a') // 获取可自动执行DOM事件的标签对象
      eleHandler[0].click() // 对指定下标的元素执行点击事件
      
      // 选择一个指定元素
      let inputEle = await page.$('input') // 获得所有的input标签
      await inputEle.focus() // 使第一个索引的input获得鼠标聚焦
      await page.keyboard.type(222) // 设置该选中表单的内容
      await page.keyboard.press('Shift') // 按一次下Shift键
      
      page.waiFor... 系列方法，等待指定内容出来后才会被调用
      	如 page.waitForSelector(selector[, options]) // 等待指定元素回来后才会执行
      await page.waitForSelector('a[title = "JOJO的奇妙冒险第四部 不灭钻石"]'); // 等待选择器
      
    	page.on('console',(e)=>{}) // 监视控制台变化,并可以通过e来接收控制台的变化
   	
      // 拦截google请求
       await page.setRequestInterception(value) // value为boolean值默认为false,为true表示启用拦截器
         page.on('request', res => { // 配合 request事件监听来做处理,每有一次请求该事件就会执行一次
      if (res.url())
        res.abort();
      else
        res.continue();
    });
    	注意
    		环境变量res是一个实例对象，包含了对请求的处理和获取
    			res下的常用方法
    				request.abort([errorCode]) // 放弃接收本次请求
    					
      
   	page.emulate('手机型号') // 用来模拟不同的手机进行登录
   		const iPhone = puppeteer.devices["iPhone 6"]
   		await page.emulate(iPhone);
   	
   	page.evaluate(()=>{}) // 在evaluate中可以写DOM语法 ，如果该函数为promise语法则会等待其执行，帮把值返回，不是则返回undefined
   	
   注意
   	当一个文件通过点击下载(且该元素身上没有该下载的资源时)，可以通过network观察多出来的xhr请求，从而找出下载源
   	请求拦截器需要在进入网站前开启
   	在按鈕或其他的元素导致刷新或页面刷新时，可以使用page.waiFor... 系列方法等待数据同步，防止报错
   	puppeteer库，实际上是下载一个浏览器插件，通过对此浏览器进行事件方法操作，来完成浏览器的页面访问
   	puppeteer每次返回的都是一个promise对象，因此每个操作前都需要添加await
   	官网：http://www.puppeteerjs.com/
   	在puppeteer中的函数中的this指向的都是window
   	$,$$,$eval,$$eval都是选择dom中的标签节点元素，但只有$eval,$$eval里的回调函数中可以操作标签节点的属性，$,$$则是用来获取标签对象配合click,focus函数执行的
   	puppeteer中的所有回调函数和node中的作用域相互隔离，都不能访问其外部的数据，puppeteer中的回调函数只能通过return的形式把数据返回给node
   	如：	  let books = await page.$$eval(".thumb-img>a", (books) => {
              let bookObj = {};
              books.forEach(async item => {
                  let title = item.title;
                  bookObj[title] = item.href;
                  console.log(item.title,item.href)
              })
              //fs.writeFile('./books/list',JSON.stringify(bookObj),()=>{}) 默认fs模块引入，因为相互隔离所以fs失效，且此时函数的this指向window
              return bookObj // 把window环境下收集的数据通过return返回给node环境
          })
  */
  ```

  

+ puppeteer获取a链接和其标题

  ```java
  /*
  	const puppeteer = require('puppeteer');
      const URL = 'https://www.bilibili.com/';
  
      let start = async () => {
        let action = async (url) => {
          const OPTION = {
            headless: false,
            // defaultViewport: {
            //   width: '3200px',
            //   height: '3200px'
            // }
          }
          const browser = await puppeteer.launch(OPTION); // 打开浏览器
          const page = await browser.newPage(); // 创建一个新页签
          await page.goto(url); // 进入一个网页
          let result = await page.$$eval('a', ele => {
            let eleArr = []
            ele.forEach(i => {
              let aMsg = {
                href: i.getAttribute('href'),
                text: i.innerText
              }
              eleArr.push(aMsg)
            })
            return eleArr
          })
          page.on('console', (e) => console.log(e)) // 监视控制台变化
          console.log(result)
        }
  
        action(URL)
      }
      start()
  
  */
  ```

  

+ 自动化搜索内容(基于Puppeteer)

  ```java
  /*
  1.
   let ele = await page.$('#nav_searchform>input');
      await ele.focus();
      await page.keyboard.type("我被选中了");
      let btnEle = await page.$('.nav-search-btn');
      btnEle.click();
      
  2.
   let ele = await page.$('#nav_searchform>input');
      await ele.focus();
      await page.keyboard.type("我被选中了");
      await page.keyboard.press("Enter");
  */
  ```

  
