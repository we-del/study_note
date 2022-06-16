#   React

+ js代码执行顺序

  ```java
  /*
   认识
      js中 为单线程运行，因此其把任务分为同步任务(简单任务，复杂任务)，异步任务(异步任务又分宏任务，微任务)
   名词解释
   	同步任务
   		在调用或运行时可以直接运行(简单任务就是同步任务)
   	异步任务
   		需所有同步任务执行完后，异步任务才能执行(宏任务和微任务就为异步任务)
   	简单任务
   		同步任务的一种，一类可以直接执行的任务，如，参数定义，函数调用（函数回调属于同步任务，不管是否被包裹）
   	复杂任务
   		同步任务的一种，一类需要触发才能执行的任务，如 函数调用
   	宏任务
   		异步任务的一种，如定时器,ajax回调
   	微任务
   		异步执行的一种，如promise的函数调用(then方法,catch方法)setState 微任务
   		
   所有任务的执行顺序
   	同步任务 > 微任务 > 宏任务
   
   js代码执行流程及任务队列划分
   	首先，js分为简单任务(直接执行)，复杂任务(读取到时在堆区开辟一块空间，等待调用执行)，微任务(等待当前上下文(作用域)所有简单任务执行完毕后执行)，宏任务(等待当前上下文(作用域)所有简单任务和微任务执行完毕后执行) ， 其中 复杂任务，微任务，宏任务，通常使用函数包裹，有自己的上下文(作用域)，因此在执行到这些内容时，其上下文的所有任务还会进行划分，在执行时进入对于的任务队列等待执行
   	js的任务队列分为 简单任务队列，微任务队列，宏任务队列，遵守队列原则(先进先出，即先放到队列的数据，先执行) ， 宏任务如果是定时器，则在进入宏队列的同时开启定时，以便在调用时可以直接被钩子函数勾出
   
   js执行规则
   	简单任务直接执行;复杂任务读取到堆中，等待调用执行;微任务进入微队列，等待当前上下文中(当前上下文(作用域)指，如果此时在全局执行代码则当前上下指的就是全局，如果在函数中执行，则当前上下文指的就是当前函数)所有简单任务队列执行完毕后开始执行;宏任务进入宏队列，等待当前上下文所有简单任务队列和微队列执行完毕后开始执行
   
   注意
   	简单任务直接执行，复杂任务则进入堆中等待调用执行，宏任务和微任务则进入对应的队列，等待执行，待所有简单任务执行完后，在执行微队列里的内容，然后执行宏队列里的内容
   	在执行复杂任务，宏任务和微任务里的内容时，还会对该上下文内容的进行任务类型的划分，依然遵守执行规则,如此微任务或宏任务中，又包简单任务，宏任务或微任务，则在进入对应的队列等待执行，直到全部任务执行完毕
   	return 默认会当作简单任务执行不管其被什么东西进行包装，我们可以返回一个promise实例对象(同步执行)，返回给其调用者(promise实例的函数内容也有自己的上下文，执行时会进入对应的队列等待调用，如果再promise的状态确定点是一个微任务或宏任务，打印其(console.log()同步执行),则拿到的是一个pendding状态的promise，因为其的状态还没确定)
   	使用了 await关键字后，会等待目标promise的执行(即等待该promise获取resolve或reject)，此时阻塞await后的代码执行，当其执行完后(该promise获取了resolve或reject),await才会执行，如果此时resolve的结果被一个宏任务包裹，则表示此时promise的状态在该宏任务执行时才变更，如果此时resolve的结果被一个微任务包裹，则表示此promise的状态在此微任务执行时才变更，如果是一个简单任务则直接执行(await会等待promise状态变更后执行)
   	 await会阻塞接下来的代码执行(除了return)等待目标  Promise确认状态后，await才会拿到对应的数据，并放行，因此await的执行队列 前一个promise的状态结束时机确定(await再其确认了状态后立即执行，如果此时前一个promise状态是再同步队列确认的则这个await就同步执行，如果promise状态是再微队列或宏队列确认的则这个await就再其后执行(因此返回时也需要进行层级包裹，返回一个同步的promise，然后其的状态再await确认状态后执行，此时才能拿到想要的数据)
   	 return是同步执行的，因此如果我们想要返回一个我们需要得到的值(该值是微任务或宏任务)，就必须要返回一个同步的promise，然后该promise的状态的确认是在需要得到的数据之后的，此时外部再使用await关键字等待返回的promise确认状态，此时就能拿到需要的返回值
   
   return+promise返回使用(拿到一个我们需要得到的返回值)
       export let getCategoryList = async ()=>{
          // await会阻塞接下来的代码执行(除了return)等待目标  Promise完成状态后，await才会拿到对应的数据，并放行，因此await的执行是在目标promise确认状态之后的
          // 前一个promise的状态结束时机确定(await再其确认了状态后立即执行)
         let res =  await getCategoryLitReq();
          if(res && res.data){ // 获取到服务器数据
              let tableArr:TableContent[] = [];
              res.data.data.forEach((item:any)=>{
                  let showObj:TableContent = {name:"",key:""};
                  showObj.name = item.name;
                  showObj.key = item._id;
                  tableArr.push(showObj);
              })
              // return为同步返回，因此我们必须返回一个promise实例(也是同步执行)，
              // 然后调用方使用 await 等待此promise结果来达到阻塞后序代码执行，等该任务(宏任务)执行后，await拿到成功的回调 。 因此此时只需要将该promise实例的状态确认点，在需要的数据之后即可
              // return new Promise((resolve)=>{
              //     setTimeout(()=>{resolve(tableArr)});
              // })
              // return new Promise((resolve)=>{
              //     console.log(tableArr);
              //     setTimeout(()=>{resolve(tableArr)});
              // })
              return tableArr;
              // setMsgArr({loading: false, msgArr: tableArr});
          }else{
              message.warning("当前网络不稳定，请售后再试");
          }
      }
   
   await调用示例
      function fn1() {
        setTimeout(() => {
          console.log(0); // 5
        });
      }
      function fn2() {
        console.log(1); // 2
        new Promise((resolve, reject) => {
          setTimeout(() => {
            resolve(2);
            console.log(2); // 6
          });
          console.log(3); // 3
        });
        return new Promise((resolve, reject) => {
          setTimeout(() => {
  
            resolve(10);
            console.log(8); // 7
          });
        });
      }
      async function fn3() {
        let res = await fn2(); // 阻塞执行，等待fn2()promise的状态变更
        console.log("res:", res); // 8 res=10
      }
      console.log(4); // 1
      function fn4() {
        let resP = new Promise((resolve, reject) => {
          setTimeout(() => {
            console.log(9); //9
            resolve(5);
          });
        })
        // 同步执行，此时因为resP状态没变更，所以打印Pendding状态
        console.log("resP:", resP); // 4 resp=new Promise<Pendding>
        return resP;
      }
      fn1();
      fn3();
      fn4();
   	
   
   调用示例
   	// 此函数是一个经典的await then调用
   	export const loginReq = async (username: string, password: string) => {
          console.log(1); // 同步执行
           // 使用了 await关键字 ，其后的代码会被包装成 微任务进行微任务队列等待执行(待执行到该任务后又对这些任务进行在分配，如下为一个定时器，会被放到宏任务队列)，如果同步任务return依赖到这个res变量，则直接返回一个同步的res （一个promise对象）
          let res = await axios.post(HOST + PORT + LOGIN, {username, password});
          console.log(res);
          console.log("www"); 
          setTimeout(()=>{
              console.log("wo");
              return res; // 同步任务， 拿到 axios.post回调的值，直接返回 因此为一个promise对象
          },1000)
      };
      
      
      // promise状态调用
      const { log } = console;
      function a() {
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            resolve(1);
          }, 0);
        }).then((value) => {
          console.log(value) // 3
          return 3;
        }, (reason) => { console.log(reason) })
      }
      const b = async () => {
        let res = await a();
        log("res=", res); // 4
      }
      const p = a();
      log(p); // 1  此时是pendding状态的promise
      b();
      log(2); // 2
      new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(3);
          log(4); // 5
        }, 0);
      })
      
      // promise 调用示例
      执行顺序   
      	1-3行为宏任务进入宏队预执行，并等待其他任务执行完毕在开始执行
      	5-7行为简单任务直接执行 ，输出 1
      	8-17行为其第一个微任务，进入微任务队列等待执行
      	21-23行为一个简单任务，直接执行，输出 7
      	24-26行为一个微任务，进入微任务队列等待执行
      	此时所有的简单任务都已经执行完毕，开始执行微任务队列里的任务
          =================
      	执行8-17里的微任务，对其任务内容再次进行划分
      	9-12行为简单任务直接执行，输出 2，3
      	13-14行为微任务，在进入微任务队列排队
      	================
      	此时8-17行的then已经执行完毕
      	18-20行为外部promise的第二个then调用，进入微队列等待执行
      	================
      	所有任务都已经进入队列里，开始执行宏队列和微队列(微队列全部执行完毕才执行宏队列)内容(所有执行小于1ms执行完毕)
      	执行24-26行的微任务调用，并对其进行任务拆分，此时为一个简单任务，输出 5
      	执行13-12行的微任务调用，并对其进行任务拆分，此时为一个简单任务，输出 4
      	15-17行为一个微任务，进入微任务队列等待调用
      	执行18-20行的微任务调用，并对其进行任务拆分，此时为一个简单任务，输出 6
      	执行15-17行微任务调用，并对其进行任务拆分，此时为一个简单任务，输出 5
      	=====================
      	此时所有微任务执行完毕，开始执行宏任务队列内容
      	执行1-3行的宏任务调用，并对其进行任务拆分，此时为一个简单任务，输出 0
    1  setTimeout(() => { // 宏任务 
    2    console.log("0"); 
    3  })
    4  //  1 7 2 3 8 4 6 5 0
    5  new Promise((resolve, reject) => {  //  简单任务 
    6    console.log("1"); //  简单任务  
    7    resolve();
    8  }).then(() => { // 微任务
    9    console.log("2");  // 简单任务
    10    new Promise((resolve, reject) => {  //  简单任务
    11      console.log("3"); // 简单任务 
    12      resolve();
    13    }).then(() => { // 微任务
    14      console.log("4");  
    15    }).then(() => { // 微任务
    16      console.log("5"); // 简单任务
    17    });
    18  }).then(()=>{//微任务(在第一次扫描上下文时,因为其还需要依赖前一个then的返回值所以不会被放到微队列)
    19    console.log("6"); // 简单任务
    20  });
    21 new Promise((resolve, reject) => { // 简单任务
    22   console.log("7"); // 简单任务 
    23    resolve();
    24  }).then(() => { // 微任务
    25    console.log("8"); // 简单任务 
    26  });
  */
  ```






+ 概念

  ```java
  /*
   纯函数
   	定义
   		一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用(即增加值的不确定性)，我们就把这个函数叫做纯函数,即返回值是我们可以预测到的，只依赖其函数内部的参数的函数是纯函数(相同代码相同传参在同一时间多次调用的返回值相同)
   	实例
   		// 纯函数，返回值可以预测到的
   		const a = 1
          const foo = (obj, b) => {
            return obj.x + b
          }
          const counter = { x: 1 }
          foo(counter, 2) // => 3
          counter.x // => 1
          
          // 不纯函数，返回值无法预测，内部更改了数据
          const a = 1
          const foo = (obj, b) => {
            obj.x = 2
            return obj.x + b
          }
          const counter = { x: 1 }
          foo(counter, 2) // => 4
          counter.x // => 2
          
   	注意
   		不能调用Date.now()或Math.random()等返回值不固定的函数
   高阶函数
   	定义
   		如果一个函数接受一个函数作为参数，或者一个函数的返回值是一个函数则这个函数就是一个高阶函数
   	注意
   		函数是一个复杂数据类型，有自己的地址，在形参接收一个函数时，实际上是指向其函数地址
   高阶组件
   	定义
   		一个组件类其属性接收的是另一个组件类或其方法返回的是另一个组件类时，这个组件就是高阶组件
   
   有状态组件
   	使用state(setState)属性来重绘界面的就是有状态组件
   无状态组件
   	没有使用state(setState)属性来重绘界面的就是无状态组件
   
   一般组件
   	自己引入的组件就是一般组件(通常会重复使用) , 如 <Component/>
   路由组件
   	由路由器渲染的组件为路由组件,如 <Route path="/" eleemnt={<Component/>}>
   
   
   注意
   	每个复杂数据类型都会被分配一个地址，然后变量去引用它们的地址，在传递时也是将它们的地址进行传递(形参去执行该函数的地址)，没有被引用的复杂数据类型的数据 会被js 给释放掉
  */
  ```

  

+ 项目描述(面试可能问)

  ```java
  /*
   描述 方面
   	此项目是一个前后台分离(前台负责展示，后台负责给东西(数据))得后台管理得SPA(single page apption 单页面应用)
  	包括 ....等功能模块
  	前端使用...等技术 ，如React+Antd+Axios+ES6+Webpack等
      后端使用...等技术, 如 Node+Express+Mongodb(Mysql)
      采用 模块化，组件化，工程化模式开发(React特性)
   
   接口测试
   	在写项目前，需先测试每一个接口得返回数据是否正确，以此来完成协调更改
   	
   注意
   	mock.js 可以模拟后端返回得数据完成开发
  */
  ```

  ![image-20220419173508761](D:\typora_import_images\typora-user-images\image-20220419173508761.png)

**react讲的是单页面开发,学习如何多页面开发(看文章即可)**

+ 特别注意

  ```java
  /*
  ==================================================
   // 关于js
   1. 在js中要注意this指向问题，this一般指向其调用者，但js中箭头函数this指向和外层this指向一致，函数的返回值无异常(返回给调用者)， 因此在特殊使用我们可以使用call,apply,bind 去改变函数this的指向
   2. 在js事件调用时，如果需要接收参数来完成功能，就需要使用静态代理模式来解决问题(因为js底层在调用时，接收的需要是一个函数地址，且只会传入一个环境变量参数，因此需要使用静态代理模式帮助完成参数传递)
   	 如：a.onclick = (e)=>{ fun(uuid,e)} // 当点击事件触发后，js底层调用此箭头函数(代理函数)，然后由箭头函数(代理函数)去调用另一个真正需要执行的函数(实现函数传递) 
   3. js中复杂数据类型地址传递，简单数值类型值传递，在 new Array()或new Object()时(js可以简写为 []||{}创建一个地址)就表示在堆区创建了一个地址	 
   ==================================================
   // 关于react	 
   1. react在调用了一个函数没有传递正确的参数时，会阻塞程序运行，且不会提示错误
   2. react的运转是生命周期函数和事件驱动的，render函数多用于返回虚拟dom到引用该组件实例的位置，componentDidMount多用于启动整个组件的运行(类型于java的main函数),getDerivedStateFromProps 多用于将外部Props数据挂载到状态中，componentDidUpdate 多用于页面在更新后做处理 ， 事件多用于来开启 更新生命周期函数流(观察者模式)
   3. npm run eject 可以拿到当前react配置的webpack文件(webpack.config.js,我们可以再此基础上做扩展)
   4. react中的样式最终都会统一加载到全局(先由各组件引入然后由app汇总到全局)，因此所有的页面的标签的类名或id不能重名，否则会导致样式串流(可以用子类选择器 #title>li)
   
    ==================================================
   // 前端路由
   1.再使用新版本react脚手架后，使用 react-router-dom中的Route标签可能跳转会失效，去掉index.js中的strict组件即可
   	如 root.render(          
         //  <React.StrictMode>  // 去掉这个组件即可
              <BrowserRouter>
                  <App/>
              </BrowserRouter>
        //  </React.StrictMode>
      );
      
   2. 在react-router-dom 5.2.0版本以前,前端路由为模糊匹配，在以后为精准匹配; 模糊匹配 是前端路由去匹配地址(该地址为 www.xx.com/ 后的部分)，且前端路由需和地址的首部的某个片段完整匹配，精准匹配指 前端路由指只有全部匹配才能匹配成功
   	模糊匹配实例  // 因此在模糊匹配中，需要把难匹配的放前面，好匹配的放后面，或在其行内添加 exact
   	   <Switch>
           <Route path={"/"} component={RootPage}/>//每次路由匹配时都被此路由抢占,因此/路由应该滞后放置
           <Route path={"/index"} component={Index}/>
           <Route path={"/items"} component={Items}/>
         </Switch> 
         
   3. 前端路由中，多级路由的显示，需同时出现在一个界面中(如点击一个三级路由，其三(当前子路由)，二(直接父路由)和一级路由(顶级路由)都会出现在此界面；不是有几个/index/xxx 就是几级路由，而是其父子路由关系说成路由层级， 如 设为 /doc/react 为一级路由，/doc/react/detail/app 为二级路由   )
   4. 路由大多都是无状态的？因此推荐函数渲染？
   5. v6版本的钩子函数只能在函数组件中使用
   
   ==================================================
   // 组件选择 
   1. 新版本react更推荐使用函数组件(因为占用体积小，且拥有类的大部分重要功能，如 state,props,ref,生命周期) ,类组件占用体积大，功能更庞大，如果只需满足常用需求使用函数组件即可，如果有额外需求使用类组件即可
   2、类组件  以 <Component/> 载入到虚拟dom中时，react底层创建了一个实例对象
   3. 函数组件 以 <Component/> 载入到虚拟dom中时，react底层实际上是调用了这个组件函数，创建了一个单独的函数实例(当以 fn1()调用时只是单独的一次调用)
   4. 类组件 以 <Component  name={"张三"}/> 传递的参数会被放到该实例对象的props属性下，以<Component>张三</Component> 接收的参数会被放到 该实例对象下的props属性下的children属性下；
   	函数组件 以 <Component  name={"张三"}/> 传递的参数会被放到该实例函数组件的第一个参数上(为一个对象，key value为传入的组件参数)，以<Component>张三</Component> 接收的参数会被放到 该实例函数组件的第一个参数下的children属性下 
   5. 创建类组件实例 就相当于一次把所有类组件相关配置集成到了一个实例对象中，而创建函数组件可以把自己需要的组件相关配置进行引入，来达到一个按需引入目的(所以函数组件比类组件在日常使用中更具有优势)
   6. 函数组件在使用useState更新状态后，会重新调用一次该函数组件(其中所有非钩子函数都会进行初始化) 。 类组件在使用setState改变状态后会第哦啊用一次更新时的生命周期函数(使用PureComponent后会对当前更新做判断)
    ==================================================
   // axios请求发送
   	1. axios发送post请求后台可能无法正常接收，进行一下配置代码即可正常接收
   
      import {HOST, PORT, LOGIN} from "../../../config";
      import Qs from "qs";
      // 设置post请求头消息
  	axios.defaults.headers.post["Content-Type"]="application/x-www-form-urlencoded";
      export const loginReq = async (username: string, password: string) => axios.post(HOST + PORT + LOGIN, Qs.stringify({ // 传输的数据使用Qs模块进行转换
          username,
          password
      }));
   ==================================================
   // 其他
   1. 在引入资源时，该资源会被先执行一遍(已经引入的资源在引入时不会在执行，因为其已经加载过)
   2. 文档注释非常重要，每一个函数都应配置一个文档注释
   3. 组件包裹组件可以把被包裹组件当作props参数挂载到包裹组件的props属性上
   4. 只需一次显示，无需重复渲染得组件推荐些为函数组件(无状态)；需要重复渲染得组件推荐类组件(有状态)
   5. 因版本node npm 和 yarn 不能同时使用？
   6. 当执行 <Component/>时，如果该Component是一个类，实际上是创建了一个该类的实例对象(类声明周期钩子轮询render返回虚拟dom到该行)，如果该Component是一个函数，则是调用其函数(函数返回虚拟dom到该行) 
   7. react中的钩子函数 喜欢用 useXxx命名
   8. 分别暴露，统一暴露最终都会放到一个对象中，因此可以解构获取，而默认暴露是暴露变量本身，因此只能变量指向获取
   9. 当箭头函数想返回一行数据的时候最好使用小括号包裹，这样看着像一个整体(返回的如果是一个对象必须包裹)
   10. 扩展运算符 ... ,本质是展开运算符，可以将一个数组或对象的最外层进行展开，依此法可以放到另外一个数组或对象中(对象展开后是一个键值对，在控制台无法输出，因此只能放到一个对象中，，可以直接使用或放到数组中)，实现浅拷贝(只对最外层实现拷贝),如果想实现深拷贝则需要使用 JSON.parse(JSON.stringify())或对每层的数组或对象进行展开
   11. instanceof 的作用：判断一个实例对象的地址中是否是否包含该类的一个构造部分(也就是继承)，即判断一个实例对象是否由该类构建 ，如 (Function instanceof Object true),Objcet是所有引用类型的基类 
   12. 解构赋值原则：读取值可以进行解构，写值(修改数据)不应该进行解构(，因为在解构后，简单数据类型是值传递，修改后不会作用子原对象或数组上，只有复杂数据类型的解构才会影响原地址(因为是地址传递))
   13. 已经开启的定时器，在没有关闭的情况下，只有其执行完毕了才会自行取消，因此因即使关闭没有使用的定时器，避免内存浪费
   14. 解决跨域的方式有 ,jsonp,cors,前端代理(再package.json中 添加 "proxy":"url")
   15. 注意组件渲染的位置，如果组件在错误的位置渲染可能会产生找不出位置的报错(通常检查配置文件，是否有错误调用，如router,config等配置文件)
   16. 钩子函数只能使用在函数组件中，且钩子函数的初始化(调用)的外侧包裹的必须是函数组件，如外侧包裹的函数不是函数组件则会报错(被外部以组件<Component/> 引入的函数被称为组件函数)
   17. react 16以上 再index.ts中新增了<React.StrictMode> 组件包裹。再dev开发模式下一个组件初次加载和更新时会执行两次(为了防止side effect引起的bug) ， 不使用该组件时，组件加载和更新时只会执行一次
   18. 再编写完react代码后 ，运行 npm run build 即可构建静态资源，此时webpack打包的代码会被当作生产模式运行(需要部署到服务器，可以使用 serve包快速搭建本地服务器，或使用node部署)
   19. 在写函数组件时，只需用到一次的属性，不用重复获得的属性可以写在函数组件体的外面(即在，该组件被引入并使用时会执行一次，以后该函数发生更新，只会执行函数体李的内容)
   20 . React独有，如果需要将后台返回的string类型 html标签转换为 dom类型的html标签 可以使用 React中的一个属性 dangerouslySetInnerHTML，其接收的是一个对象，该对象的属性为__html 值为 string类型的html标签
   	如 <p dangerouslySetInnerHTML={{__html: content}}/> // 在虚拟dom中写js表达式需要使用 {}包裹
   21.需要改变的状态,且其全局大多数组件中都需要使用的组件才需要存储在redux里，否则只用存储到localstorage里
  */
  ```
  



+ react脚手架配置less,sass

  ```java
  /*
  注意
  	scss 是 sass的一个超集，支持了sass的所有写法，不过再属性编写的时候不同，scss为括号包裹，且每个样式必须以;结尾。而sass是缩进表示包括，每个样式不能以;结尾
  	缩进显示层级和{}包裹显示层级效果一致，再当作判断使用时，前一个条件为true是才会进入，为false不进入；再当作包裹层级使用时，默认进入一次
   配置sass
   	react脚手架默认支持sass(已经完成了sass的webpack配置，我们使用只需要下载对应的包即可使用)
    	安装sass依赖 ： npm i sass sass-loader -D
    	注意
    		sass和scss都是sass的子集
    
   配置less
   	安装less依赖： npm i less less-loader -D
   	配置方法
   		前言
   			参考网站 https://blog.csdn.net/m0_59324574/article/details/120046927
   		1.查找 node_modules 下面的react-scripts/config/webpack.config.js 进行打开
   		2.在sassRgex 和sassModuleRegex下添加以下常量
              const lessRegex = /\.(less)$/;
              const lessModuleRegex = /\.module\.(less)$/;
   		3.在sass-loader的配置上一句添加以下配置
   		{
                test: lessRegex,
                exclude: lessModuleRegex,
                use: getStyleLoaders(
                    {
                      importLoaders: 3,
                      sourceMap: isEnvProduction
                          ? shouldUseSourceMap
                          : isEnvDevelopment,
                      modules: {
                        mode: 'icss',
                      },
                    },
                    'less-loader'
                ),
                sideEffects: true,
              },
              {
                test: lessModuleRegex,
                use: getStyleLoaders(
                    {
                      importLoaders: 3,
                      sourceMap: isEnvProduction
                          ? shouldUseSourceMap
                          : isEnvDevelopment,
                      modules: {
                        mode: 'local',
                        getLocalIdent: getCSSModuleLocalIdent,
                      },
                    },
                    'less-loader'
                ),
              },
  */
  ```
  

## tsx

+ 注意

  ```java
  /*
   1. ts 严格限制数据类型，如果该数据在原数据里不存在可预定义，无法预定义可设置为any类型
   2. ts使用 | ? 来完成 方法的重载， 如 fn1(content:string[]|string ,comment ?:string) , 再使用时再去
   3. 直接指定一个对象的接收的所有属性和其值的数据类型，相当于给定了一个接口进行限制，不过此接口在对对象时起限制作用。在对类时，是接口作用(类需要实现接口的所有属性和方法(java中的接口只用实现方法，共享属性))
   4. 作为形参的变量必须要生命变量类型(因为其不知道指向的是谁)，一个变量在指向其第一个常量时(也可以自己指定)，该变量的类型就该常量的类型，在后续指向中，中能指向其相同的类型(如 let a = 1; 这个a的类型是number，在其下程序中，a只能指向number类型数据)
  */
  ```

+ 方法(函数)重载

  ```java
  /*
   介绍
   	ts使用 | ? 来完成 方法的重载
   补充
   	重载指，相同函数(方法)不同形参个数或类型即可构成重载
   注意
   	在重载后，一个形参的数据类型如果可能接收多个值时，再实际使用时，需要对他的类型进行p断言(即强制转换)，便于代码提示和语法检查
   	
   示例
    const handlerOpposite = (title: string, content: string[]|string): React.ReactNode => {
          let res: React.ReactNode = null;
          if (title === MERCHANDISE_IMAGES) {
              content = content as string[]; // 此时指明content为string[]类型 ,便于代码提示和类型限制
              res = (
                  <>
                      {
                          content.map((url:string) => (
                              <img src="" alt={"不见了"}/>))
                      }
                  </>
              );
          } else if (title === MERCHANDISE_DETAILS) {
  
          }
          return res;
      }
  */
  ```

  

+ 接口的使用

  ```java
  /*
  ts中接口的使用
        export interface LoginStateLimit {
          loginState:number,
          loginToken:string,
          entry():void,
      }
      // 普通对象可以把接口当作对象的key和类型约束，且其对象内的属性和方法必须为接口的设定的属性和方法
      const initialState:LoginStateLimit = {
          loginState: -1,
          loginToken:""，
          entry(): void {
      	}
      }
      // 类需要实现接口里的所有属性和方法
      class Login implements LoginStateLimit{
         public loginToken:string = "";
         public loginState:number = -1;
         entry(): void {
     		}
      }
   注意
   	tsx可以在形参上设置 ? 来完成函数的重载(?表示接不接受都行),如 fn(a:number,b?:number):void
   	使用tsx编写程序时，必须要写返回值，和数据类型，否则可能编译报错
   	react默认不支持tsx，如果需要使用，则需在创建项目时添加如下
   		npx create-react-app 项目文件名 --template typescript
  	ts无法引入识别图片资源，需要在 项目 src目录下 创建 image.d.ts 文件，并写入如下内容
  	    declare module '*.svg'
          declare module '*.png'
          declare module '*.jpg'
          declare module '*.jpeg'
          declare module '*.gif'
          declare module '*.bmp'
          declare module '*.tiff'
          declare module '*.pdf'
  */
  ```


+ 泛型的使用

  ```java
  /*
   泛型和java中的泛型类似，填写后，可以限制一个属性的接收限制
  */
  ```

+ 组件自定义状态

  ```java
  /*
  注意
  	每个组件类再使用前应该指定其对应的props和state的接口泛型(表示可接收的props属性和组件的状态)，并传入到组件类中，以便更好的代码提示和类型检查，当没有指定props参数时，外部无法传入props参数到props对象下
  总结
     类组件只有指定了props泛型后才能从外部接收对应的props数据,否则会报错(因为该类没有指定props属性个数，导致tsx无法识别要接收的个数)
   例子
   	export default class A extends PureComponent {
          constructor(props: any) {
              super(props);
              this.state = {
                  count: 1
              }
          }
  
          render(): React.ReactNode {
              return (
                  <>
                      <h1>我是A组件</h1>
                      <B count={12} render={(props:any)=> (<C name={props} />)}/>
                  </>
              );
          }
      }
      interface Props {
          count ?:number;
          render ?:any;
      }
      interface State {
  
      }
      // 此时B的实例对象可以从外部接收 count和render到props对象下
      class B extends PureComponent<Props,State> { // 指定了props和State
          render(): React.ReactNode {
              console.log(this.props);
              return (
                 <>
                  <h1>我是B组件</h1>
                 </>
              );
          }
      }
  
      interface CState {
  
      }
      interface CProps {
          name ?:string;
      }
      //指定了props和State，此时外部可以传入一个 name属性到props对象下
      class C extends PureComponent<CProps,CState> { 
          render(): React.ReactNode {
              return (
                  <>
                      <h1>我是C组件</h1>
                  </>
              );
          }
      }
  */
  ```

  

## 技巧

+ 编码技巧

  ```java
  /*
   1. 在做逻提交时，任何需要传输的数据可以放到一个对象里，最终在校验时，只需校验该对象即可
   2. 函数组件在更新时，其非钩子函数得数据会被全部重新初始化，因此需要注意此时存储数据得对象或数组是否会被初始化而导致存储的数据丢失(如果会丢失则可以定义在全局(只会初始化一次),或以状态的方式定义(状态只会在页面首次加载时执行一次（采取了享元模式？ 无状态管理时获取状态，有状态则返回之前的状态）))
   
   注意
   	localstore适合存储该网站上的通用信息（即每个用户相同的信息），不同的信息应该存到redux或状态里
  */
  
  /**
   @description: 记录更新用户信息， 此数据要么以常量得形式定义在全局，要么以状态得方式定义在函数组件中
   (状态得值在初始化后不会因为组件得重绘而再次初始化 (useState得状态采取得是享元模式？即没有状态时运行初始化状态，
   有状态时使用已有得状态))
   @tips: 此信息得初始化必须在文件首次被加载时完成，不能以无状态数据定义在函数组件内部(，因为其得数据是在点击后获取，此时会触发一次
   函数组件得重绘，导致该值又进行一次初始化，此时目标组件无法获得更新后得数据(得到得是初始化数据)
   函数组件得重绘会把所有非钩子函数在进行一次初始化赋值 (虚拟dom采取diff算法方式重绘)
   */
  ```

  

+ 对象在jsx中的遍历方式

  ```java
  /*
   	 {
   	 	// Object.keys(categoryObj) 收集对象中的所有属性，使用对象[属性] 获取对应的值
           Object.keys(categoryObj).map((key: any) =>
                   Option value={key} key={key}>{categoryObj[key]}</Option>)
       }
  */
  ```

  

+ 解决react报错问题

  ```java
  /*
   18.0 后 react不支持 ReactDOM.render()方法需要使用下列方法
  import React from 'react';
  import ReactDOM from 'react-dom';
  import App from './App';
  import { createRoot } from 'react-dom/client';
  
  // 对函数调用进行了解耦
  const container = document.getElementById('root');
  const root = createRoot(container);
  root.render(<App />);
   
   
  // 被注释的是之前ReactDOM.render的代码
  // ReactDOM.render(
  //   <React.StrictMode>
  //     <App />
  //   </React.StrictMode>,
  //   document.getElementById('root')
  // );
   
  */
  ```

  

+ date

  **说明： 技巧，前言部分为 2022/4/2日添加，旨在认真复习react和vue，之前做的笔记仅供参考，此后新增笔记才是本体**

+ 解决jsx文件无法快速生成html标签

  ```java
  /*
   在WebStorm中打开File–>Setting–>Languages & Frameworks–>JavaScript–>Libraries，点击右边的Download按钮，选择TypeScript community stubs，下载react和react-native。
  */
  ```

  

+ 组件实例对象创建到页面渲染过程(发生了什么)

  ```java
  /*
  注意
  	js的类也存在 封装，继承和多态
  
  
  实例对象创建过程
   		js底层会去调用该组件类的构造器 , 然后再其中隐式调用super()关键字去调用其父类的构造器(如果父类中使用了constructor就需要自己手动调用super()),一直往复如此，直到加载到顶级父类，然后从顶级父类类到一级父类,...,到当前子类，执行static修饰的语句，并放入一个地址空间中，该空间和子类存在映射关系 ，此时类加载完毕，然后从从顶级父类类到一级父类,...,到当前子类，执行属性，方法和构造器，执行完后将其属性和方法压入(堆结构)该实例对象的地址内，往复如此直到把当前子类压入该地址内时，此次实例对象就创建成功了，该地址空间内，从最底部到最顶部依次为 顶级父类的属性和方法 ，一级父类的属性和方法,...,当前父类的属性和方法。当该实例对象使用属性和方法时优先从顶部开始查找，如未找到，则查找指针下移去查找此上一级父类的属性和方法，直到找到为止。此外，每次方法的调用会从当前子类实例对象开始查找，也就是该空间的最顶层开始查找。
  
  React页面渲染过程
  	以<Component/>方式引入的组件实例，react底层会创建一个该类的实例对象，并执行一次初始化生命周期函数，最后render生命周期函数返回虚拟dom到引入组件实例的位置；当该组件实例的状态改变后(通过setState({})),react底层会重新执行一次更新时的生命周期函数(包括render)然后把最新的虚拟dom返回到该行
  React初始化过程
  	当编写完组件类后，引入该组件实际上是创建了一个该组件类的实例对象，然后react底层会进行一次实例对象的参数补充，它会将props接收的属性，载入到该实例对象的props对象下，将ref属性映射的虚拟dom或组件 载入到指定属性上，同时进行一次页面渲染(初始化，和更新时渲染)，此时组件实例对象一次工作完毕
  
   react 生命周期函数得调用(间接render()重绘页面)方式常见有:
   	初始化时(render())
   	componentDidMount()生命周期函数调用导致其他函数调用setState()时
   	事件触发，导致其他函数调用setState()时 (setState()会执行更新时生命周期函数)
  */
  ```


+ react前端路由挂载过程

  ```java
  /*
   当跳转至对应的前端路径后(可以使用Link或NavLikn)，前端路由会对该路径进行匹配，如果匹配成功就按顺序去渲染路由，如此时为一个三级前端路由，则react会依次渲染 ，一二三级路由(1ms时间，几乎同时渲染),如果失败则不做处理
   注意
   	react的每一级路由就对应一个组件，对该组件状态的修改只会对该组件某个修改的部分局部更新
   	一个多级路由是由前级路由渲染出来的，如 一个此时是一个三路由，则该界面一定包含 一二三级路由的组件
  */
  ```

  



+ 技巧

  ```java
  /*
  ==================================================
   // 关于前端路由
   1. 可以把所有同属性的路由放到一个数组中，并进行export暴露，外部想使用时，引入该路由然后map遍历即可
  */
  ```

  

+ 集合

  ```java
  /*
  ==================================================
   // 关于组件
   1. 在组件拆分时，把可能在多个页面用到的部分拆分成一个组件(js的模块)，以便在多个页面进行引入，大大增加了其可维护性。如果一个功能在其他页面不可能被引用则不需要拆分
   2. 每引用一个组件就是创建了该组件类的实例对象，然后执行初始化生命周期函数
   3. 组件分受控组件和非受控组件,使用state来控制重绘的组件就是受控组件，没有使用state的就是非受控组件，受控组件(改变状态后需要重绘页面)使用类方式编写，非受控组件使用函数编写(不需要重绘页面)
   4. react是组件化开发的，也就是说react构建的页面也是基于对象来开发的，通过某个组件实例对象的state改变来完成页面局部更新
   5. 一个组件在其他组件被引用时，被引用组件被称为子组件，主动引用其他组件的组件被称为父组件
   6. 如果一个组件的内容过于庞大，可以对该组件的局部功能进行拆分(变为该组件的一个子组件)，然后再引入到该组件中(增加了可维护性，和复用性)
   ==================================================
   // 关于react diff算法
   1. react使用虚拟diff算法来增加重绘重排效率，且该算法通过每个属性不同的key来完成识别
   
    ==================================================
   // 类
   1. js 在生成了一个类的实例对象后，通过对类方法进行调用或手动在该实例对象下挂载属性，仍然可以在该实例对象地址中追加属性(java不行)
    ==================================================
   // react脚手架
   1. react脚手架是facebook公司搭建的一个webpack脚手架，可以高效的运行react环境，react脚手架具有模块化，组件化，工程化特点
   2. 脚手架整体的架构为：react+webpack+es6+eslint 
   ==================================================
   // 其他
   1. 因为js中函数也可以当作参数传递,因此为了保持该函数的this指向不变,可以使用 apply,call或bind 来完成调用
   2. import引入其他资源，和java中引入其他类相似，都是把目标的一个映射引入到本文件中
   3. React.Component 在 react 包中 ，ReactDOM.render() 在 react-dom 包中
   4. props 属性可实现 父类向子类通信，ref属性可实现 子类向父类通信
   5. 在一个组件实例对象生成完毕后，其地址内底层已经隐式挂载了react底层的生命周期函数，当它们调用时，是从该地址顶层开始查找，(因此我们可以重写这些生命周期函数，来完成生命周期函数的过程封装),如果没找到则 寻址指针下移 最终会移动到该地址的底部去调用react底层封装的生命周期函数，找到后调用，寻址指针重新回到该地址顶部，准备下一次属性或方法调用
   6. react(或js中)在事件调用时如果想携带参数，可以写为 onClick={(e) => this.removeUserContent(uuid, e)} ,该语句表示，当这个按钮点击时，传入一个函数到react底层，然后react调用传入了一个环境变量，此时通过此调用来完成另一个方法调用
   7. 函数也可以返回虚拟dom
   8. 在完成基础布局时可以使用flex完成，然后再此基础上填写目标元素，不要不布局就开始画页面，这样可能产生bug
  */
  ```

+ 注意

  ```java
  /*
  ==================================================
  // 关于虚拟dom
   1. 虚拟dom存储在内存中
   2. jsx中 分 js语句和虚拟dom标签,js语句在虚拟dom标签外可以正常解析,在虚拟dom标签内使用需要加{}，且{}内的语句必须是js表达式(或虚拟dom标签，如果该虚拟标签内还需要解析js表达式则要嵌套{})，(有值在本行得语句就是js表达式，包括变量，有返回值的函数等)
   3.虚拟dom标签和dom标签语法，结构和功能大致相同，虚拟dom也可以通过css来改变样式，标签包裹的内容默认当字符串处理，标签行内的内容默认当属性处理，唯一不同点是虚拟dom可以使用js表达式而真实dom不行，如果在虚拟dom标签中的某个位置需要使用js语句，则需要使用{}包裹(在被包裹的内容中也可以写虚拟dom，如果该虚拟dom也需要使用js表达式，则继续需要使用{}包裹，在虚拟dom内的注释也需要使用{}包裹)
   4. 虚拟DOM绑定事件和原生绑定事件基本一致，不过虚拟DOM绑定事件采用小驼峰命名，且通常写在行内
   5.为了简化开发，使用js渲染的虚拟dom如果超过5行则可以单独开一个属性方法，然后由改属性方法会返回虚拟dom(属性指向一个函数，react无法自定方法，因为react底层干煸了方法的this指向)
  ==================================================
  // 关于组件
   1. 当引入一个组件时(如 <He/>)，react底层会帮助生成一个该组件类的一个实例对象，并进行生命周期函数初始化调用（最终会去调用render方法(返回虚拟dom)），我们只用写该组件标签即可
   2. 当我们创建了一个组件类后，react底层将我们自己定义的方法的调用者更改成了undefined(重写父类已有的方法时，调用者正常,即该组件实例对象),因此 我们需要将其修改指向并挂载到该实例对象的属性上，这样就能正常解析使用
   	如  this.xx = this.xx().bind(this)  //此代码因写在constructor 内
  ==================================================
  // 关于state
   1. setState只会改变当前组件实例对象的状态，因此只会对此组件的局部进行重绘(diffs,即单个组件状态改变，之后导致页面局部更新)
   2.使用 this.setState()方法能重新渲染页面，底层使用了观察者模式？(state)属性只能对单个表达式的指向产生修改 所以不能重新渲染页面
   3. 每次引用组件就是创建了该组件的实例对象，,react底层经过生命周期函数的轮询调用，最终会去调用该实例对象的 render方法，然后把虚拟dom作为返回值返回到该行(引用该组件的位置)
   4. 在 webstorm中 不要选择 Change setState() signature 选项 ，否则state属性无法使用
  
   ==================================================
  // 关于props
   1. 在引用一个组件(实例对象)时，可以在该组件标签中 添加 行内属性，此时这些行内属性会被收集并放到该组件类(实例对象)的props下(以对象形式)
   2. 在实际使用中，通常把props需要传入的数据 放入一个对象中，然后再传入时使用 {...obj} 即可完成展开，实现props属性传入，此方式简化了props传递(react+babel会转换 ...obj 帮助该对象展开(react+babel独有) )
  
   ==================================================
  // 关于ref
  1. ref属性可以获取当前组件类中虚拟dom的一个dom节点或使用的组件，并把该节点或节点维护到该组件类的一个属性下
  	将遗弃的写法  
  		<div ref="divNode">123</div> // 把该虚拟dom节点维护到 该组件类的ref属性对象下，key为divNode，value 为该虚拟dom节点
  	函数写法 
  		<div ref={node => this.divNode = node}>123</div> // 将该虚拟dom节点维护到组件类的divNode属性下 ； 此方式是向node底层传入一个函数，然后其进行调用并将该虚拟dom节点放到该函数的第一个形参上
  	专人专用写法(每一个ref实例只能存放一个虚拟dom节点)
  		myRef = React.createRef();  // 创建一个可以存储虚拟dom节点的一个属性
  		<div ref={this.myRef}>123</div> // 把该虚拟dom节点存放到myRef属性下
  
   ==================================================
  // 关于生命周期函数
  1. 生命周期函数为react底层定义的特有函数，它们由react统一来完成调用，我们可以通过对生命周期函数进行重写来完成自己封装生命周期钩子的过程
  2. 每个组件实例对象继承了这一套生命周期系统，组件间的生命周期过程(函数)互相隔离，即不会影响彼此(当生成A组件实例对象时，是调用A组件实例对象上的初始系列方法)
  3. 当初始化时，按顺序调用 componentWillMount() -> render() -> componentDidMount() 方法
    当更新状态等操作时，按顺序调用为
    componentWillReceiveProps() -> shouldComponentUpdate()->componentWillUpdate() -> render() -> 	   componentDidupdate()
  4. 每个组件类都隐式继承了这些生命周期函数
    
   ==================================================
  // 关于diff算法 (百度查看源码)
  	1. 创建虚拟DOM树是 使用了文档碎片技术
  	2. 每一个虚拟dom都会被react赋予一个唯一的不冲突的key值(不包括js表达式生成的虚拟dom标签，js生成的标签需要自定义key属性(如map遍历出来的虚拟dom标签需要自己指定key))，然后react通过该key值给每一个虚拟dom标签生成对应一个文档碎片，最后组成一颗虚拟dom树，并进行页面初始化渲染，当状态改变时，会去检查虚拟dom树结构和每一个文档碎片的前后数据，前后一致的文档碎片不做处理，前后不一致的文档碎片和虚拟dom树内容，则会进行内容替换或更新，并重新渲染页面，即局部更新
  	3. 在进行diff算法时，只会进行文档碎片的内容比较，而非标签比较，在替换时，也是替换标签内有差异的部分
  	
   ==================================================
  // 关于react脚手架
  	1. 使用 npm i -g create-react-app 全局创建脚手架(全局安装后，该命令在此主机上就可以使用)
  	2. 使用 create-react-app 项目名，会在该路径下创建一个react脚手架项目
  	3. 在react脚手架中，css和图片资源当作 "模块" 引入，且这些资源默认为 默认暴露
  	4. 在idnex.js页面渲染时,可以直接获取public下的index.html里的标签(React内部完成链接,文件夹不能更改)
  	5. 成型的第三方库或者包需要放在public目录下(如 bootstrap.css)，由index.html引入
  	6. react v5.0.0 后不再支持本地安装的create-react-app(本地有则需要卸载),而要使用npx 进行临时安装(npm 6版本的产物) 语法: npx create-react-app 项目名 ,如 : npx create-react-app app
  	
  	
   ==================================================
  // 关于前端路由
  1.Route再路由匹配时，会从上到下对所有的前端路由进行匹配，渲染满足要求的路由，因此需要使用Switch(老版本5.2.0以前)或Routes(新版本5.2.0以后)约束
  2. (老版本5.2.0以前)应该把经常匹配的路由(或多级路由)放到最前面(因为是模糊匹配)，(新版本5.2.0以后)则不需要(因为是精准匹配)
  3. 再一个前端路由匹配后，浏览器可以对此路由展示的内容进行收藏，下次从可以收藏中获取该路由
  4. 在进行多级前端路由匹配后，可能导致第三方库在public目录下的index.html中丢失问题，此时可能是你设置的是相对路径，改成绝对路径或使用HshRouter即可 , 如 %PUBLIC_URL%/xxx 或者 /xxx (此方式会自动补全绝对路径)
  5. 在路由匹配中，是路由去匹配地址，且前端路由需和地址的首部的某个片段完整匹配(在 v.5.2.0以前,模糊匹配)，在v6.0以后则是精准匹配，前端路由需和地址的全部片段完整匹配
  6. 每次前端路由切换时，会卸载掉前一个展示的路由组件实例对象，因此可以在ComponentWillUnmount()钩子函数中关闭流，请求接口等
   ==================================================
  // 其他
  	1. render返回的虚拟dom最终会由react底层渲染到其引用的位置上，最终会由index.js挂载到public下的html页面上(期间还会进行diff算法差异比较)
  	2. 再引入一个组件类时，如果没有指定组件名，则react会默认引入该组件下的index.jsx 文件
      3. 再引入一个组件类时，如果是jsx或js文件,则后缀可以不写
      4. 主流后台验证接收消息的方式有 ，cookie,session-cookie,token
      5. 使用 <Component {...obj} /> 可以展开一个对象的内容并放到一个新对象中，如果此时是写在组件属性参数上，则这个props属性会指向这个新对象
  */
  ```
  
+ 规范

  ```java
  /*
   ==================================================
  // 关于虚拟dom
   1. 为了使多级虚拟dom标签为一个整体，通常使用()进行包裹
   2. 在react中 虚拟dom标签的 class 属性 必须写为 className
   3. 虚拟dom标签写行内样式时(style)，必须使用写为对象形式(是js表达式)，如{{width:300+"px",height:0}}
   4. 函数也可以以返回值得方式返回虚拟dom，利用这个特性可以向类中render()方法的return中的合适位置载入虚拟dom，如state渲染时，我们把需要使用state渲染的部分封装成一个属性方法，然后再render() return中的合适位置调用即可
   ==================================================
  // 关于组件
   1. 组件的状态改变只能在该组件中进行改变，如果其他组件想改变此组件的状态，可以向外暴露一个方法，其他组件只用调用该方法即可(迪米特法则)
   2. 组件必须大写字母开头，且文件名必须和组件名一致(借鉴java)
   3. 组件必须写render()方法并返回一个虚拟dom标签，否则其在被引入时，会报错
   4. 组件分为 一般组件(我们自己引入的组件)，路由组件(前端路由匹配后，由路由底层帮助渲染) ；一般组件放入components目录中，路由组件放入page目录中(且在 react-router-dom v5版本中，底层会往路由组件下挂载history,match和location对象；在react-router-dom v6版本则需要自己获取)
  ==================================================
  // 关于state
   1. state中的复杂数据类型的数据不要直接指向该地址并且修改该地址的数据,而应该先深拷贝(使用JSON下的stringify和parse方法)一个state地址里的数据，然后在对这个新地址进行改变，最后使用 setState()维护新数据 ， 如果不这样做可能导致状态改变但页面不更新(因为有些时候state里的值变化了但是由于地址没有改变可能导致页面不更新)
   2. 在一个类中需要使用的state最好在一开始统一声明(，后面向该对象下挂载属性也可以，但是对于属性来说不直观)
  ==================================================
  // 关于jsx
   1. jsx只允许有一个根标签
   2. jsx 就是 js+xml ，即使用js和xml的语法规范，所有的虚拟DOM标签必须闭合
   
  ==================================================
  // 关于react脚手架
   1. react脚手架中，js交互直接写在对应的模块类的方法中 ； 在引入一个资源时必须见名知意，在引入一个组件类时，必须和该类名同名
   2. react脚手架中,public目录是服务器的根目录
   3. 每有一个组件类就会随即给该组件类配置一个文件夹(一般为该组件类名的小写形式)，该文件夹下放入该组件类使用的资源(图片，样式等)
   4. 组件类一般使用jsx文件(即该文件专门用于存储组件类)，App.js,index.js一般为包装文件，即对这些组件进行总和并做处理
   5. 在react中，不要使用a标签 ,且使用window下的confirm方法时，需要写为 window.confirm()
   ==================================================
  // 关于前端路由
  ==================================================
  // 其他
   1. 标签的key值需选择唯一不冲突的值(减少页面怪异行为)，此时在状态改变时有效的减少页面重绘重排，大大提高了渲染效率
   2. 因尽可能的少使用ref属性
   3. SPA ,为 single page Application
   4. react模式(结构)和java非常类似
   5. 生命周期函数setState()可以更新状态的同时，开启一轮更新生命周期函数的调用，因此state通常被认为是用于页面渲染的属性
   6. 关注点分离原则(软件工程中得一个理念)，即把状态和结果区分开，在返回状态时，就知道这次操作得结果
   7. 在react组件间通信中，能使用props和ref完成的通信则尽量使用它们(迪米特法则，不要将组件的数据过多的暴露给外部)，实在无法完成可以使用 消息订阅与发布 和 redux
   8. 再jsx文件中只书写组件类，配置项等因写入js文件
   9. 一个react文件的引入顺序为，第三方库，本地组件，样式，图片
   10. 在 ts编写react时，组件类写为 tsx,其他文件写为ts
  */
  ```
  

### 常见布局

+ flex完成左右布局

  ```java
  /*
   <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
      html,
      body,
      #root {
        width: 100%;
        height: 100%;
      }
  
  
      .app {
        display: flex;
        width: 100%;
        height: 100%;
      }
  
      .sider {
        flex: 1;
        background-color: #177ddc;
      }
  
      .show-content {
        display: flex;
        flex-flow: column;
        flex: 3;
        background-color: #146262;
      }
  
      .main-header {
        flex: 1;
        background-color: #58d1c9;
      }
  
      .content {
        flex: 3;
        background-color: #1d3712;
      }
  
      .main-footer {
        flex: 1;
        background-color: #112123;
      }
    </style>
  </head>
  
  <body>
    <div id="root">
      <div class="app">
        <div class="sider">sider</div>
        <div class="show-content">
          <header class="main-header">
            header
          </header>
          <div class="content">content</div>
          <footer class="main-footer">footer</footer>
        </div>
      </div>
    </div>
  </body>
  </html>
  */
  ```

  ![image-20220507110203337](D:\typora_import_images\typora-user-images\image-20220507110203337.png)

## 知识集中点

+ 组件联通图

  ```java
  /*
   流程
   	当一个组件被引入时，其组件所在的文件的所有代码会被执行，并分配一个独立的空间，外部只能获取其被暴露的部分，而无法访问其文件内的其他代码部分，如需访问，则需要被引入部分充当一个代理，帮助转发信息
  */
  ```

  ![image-20220517170904231](D:\typora_import_images\typora-user-images\image-20220517170904231.png)

+ 传递props参数时的使用

  ```java
  /*
   使用行内属性传递参数到一个组件中时
   	该参数会被放到组件的props参数下
   使用组件包裹传递参数到一个组件中时
   	该参数会被放到组件的props的children属性下
   		
   注意
   	在传递props参数时，如果传递的是一个组件或者标签，传递时会被react映射为一个对象 此时这个对象中存储着虚拟dom的包裹层级，我们可以选择合适的部分将其载入，然后react底层会转换为对于的虚拟dom
  */
  //使用 
  // guide.tsx
  import React from "react";
  import {useSelector} from "react-redux";
  import {Navigate, Outlet} from "react-router-dom";
  import {LoginStateLimit} from "../../redux/slice/loginStateSlice";
  import {useMatch} from "react-router-dom";
  import Menu from "./MyMenu";
  import MenuLayout from "./MenuLayout";
  import logo from "../../assets/img/logo.png";
  import "./css/menu.less";
  
  export default () => {
  
      const loginState: LoginStateLimit = useSelector(({loginState}: LoginStateLimit): any => loginState);
  
      /**
       @description: 当匹配到 输入的路由时给出该配置新的配置对象
       */
      const match = useMatch("/guide"); // 解决重复渲染问题
  
      /**
       @description: 路由拦截，根据当前登录状态导航到对应页面
       @tips: antd配合react时，不能渲染在一个组件中同时渲染多个相同的组件，要注意逻辑编写严谨，否则报错很难定位位置
       */
      let loginInterceptor = () => {
          if (loginState.loginToken.trim() === "") { // 过滤掉没登录的情况
              return (<Navigate to={"/login"}/>);
          } else if (match !== null) { // 过滤掉误入情况
              return (<Navigate to={"/guide/home"}/>);
          }
      }
  
      /**
       @description: 最近一级子路由展示
       */
      let nextChildRouterShow = () => {
          return (<Outlet/>);
      }
  
      /**
       @description: 做一个身份检查，如果已经登录且命中的是该路由则引入主路由
       */
      return (
          <>
              {loginInterceptor()}
  
              <MenuLayout>
          		// 将虚拟dom传入给该组件，会被放置到该组件的props的children参属下
                  <>
                      <div className="main-nav-menu">
                          <div className="main-nav-menu--title">
                              <img src={logo} alt="xxx待定"/>
                              后台管理系统
                          </div>
                          <Menu/>
                      </div>
                      <div className="content-show">
                          {nextChildRouterShow()}
                      </div>
                  </>
              </MenuLayout>
  
          </>
      );
  }
  // MenuLayout.tsx
  import {Layout} from "antd";
  const {Sider,Header,Content,Footer} = Layout;
  export default (props:any) => {
      return (
          <>
              <Layout>
          		/*
          		props.children.props.children[0] 表示的为下列部分
          		  <div className="main-nav-menu">
                          <div className="main-nav-menu--title">
                              <img src={logo} alt="xxx待定"/>
                              后台管理系统
                          </div>
                          <Menu/>
                      </div>
                   props.children.props.children[1] 表示的为下列部分
                   <div className="content-show">
                          {nextChildRouterShow()}
                      </div>
          		*/
                  <Sider>{props.children.props.children[0]}</Sider>
                  <Layout>
                      <Header>Header</Header>
                      <Content>Content</Content>
                      <Footer>Footer</Footer>
                  </Layout>
              </Layout>
          </>
      );
  }
  ```

  



+ 前端代理

  ```java
  /*
   介绍
   	当后端不配合我们使用 jsonp,cors解决跨域时，前端的一个解决跨域的方法(访问接口),他还可以共享指定代理后端的public文件下的内容，如图片，音频文件等
   作用
   	帮助转发请求，和获取后端public文件下的资源
   注意
   	前端代理会先去当前域名下查找，如果没找到指定资源才会去后端的public下查找，因此指定域名要和当前域名的地址保持一致，否则无法完成代理查找(端口使用不受限制)
   	
   配置方法
   	再package.json最外层中添加 "proxy":"url"
   工作过程
   	会先在当前端口下查找是否可以解决，解决不了会由代理转发请求，去另一个端口下查找解决
  */
  ```




+ 装饰器(date:2022.5.5)

  ```java
  /*
   介绍
   	装饰器是 Es7的一个提案语法(可能发生更改)，它可以对一个类，方法(类中的方法或对象中的方法)或者变量进行装饰
   	
   普通类装饰器
   	工作流程
   		相当于调用该装饰器函数并将装饰类以形参的方式传入，因而可以向类中装饰信息
   	使用	
          function withRouter(target) {
            console.log('withRouter:', target);
          }
  
          @withRouter 
          class App {} // 相当于将 A这个原型类当作 withRouter 的第一个参数传递，往其身上挂载的方法最终会以static属性或方法的形式挂载到该原型类身上(js中只有类能使用static属性和方法)
      
   带参装饰器使用
    	工作流程
    		相当于调用该装饰器函数并将装饰器参数以形参方式传入，当调用完毕后，底层会去检查是否有返回值，及返回值是否为函数，如果为函数则调用此函数并把装饰对象传入，完成装饰
    	function withRouter(params) { // 接收参数的函数
        console.log('withRouter.params:', params);
        return function(target) { // 装饰器函数
          // 给被装饰的类添加一个静态属性
          target.params = params;
          // 也可以给原型添加函数和属性，例如：target.prottotype.name = 'Jameswain';
            console.log('withRouter.target:', target);
        }
      }
  
      @withRouter('Jameswain')
      class App {
      }
      console.log(App) // 向App类中挂载静态属性
      
   多个装饰器嵌套使用时的执行顺序
    	顺序
    		从上到下先执行有参构造器，然后从上到下执行无参构造器
    	实例
          function log(name) { // 接收参数层
            console.log('log.name:', name)
            return function logDecorator(target) { // 装饰器层
              console.log('log.target: ', target);
            }
          }
  
          function connect(name) { // 接收参数层
            console.log('connect.name', name);
            return function connectDecorator(target) { // 装饰器层
              console.log('connect.target: ', target);
            }
          }
  
          function withRouter(target) { // 装饰器层
            console.log('withRouter.target: ', target);
          }
  
          @log('日志')
          @withRouter
          @connect('连接器')
          class App {
          }
     
      结果
      	log.name: 日志
          connect.name 连接器
          connect.target:  [class App]
          withRouter.target:  [class App]
          log.target:  [class App]
          
    方法装饰器
    	工作流程
    		和以上有参无参构造器原理一致(无参则直接调用并将装饰对象传入，有参则先传入装饰参数，如果返回值是一个函数则在传入装饰对象)，只不过，方法装饰器接收的是三个形参，第一个形参为传入的实例(通常是类的实例)，第二个形参为被装饰的函数名，第三个形参为 被装饰函数的描述对象
    	实例
          function log(target, name, descriptor) {
            console.log('log.target: ', target);
            console.log('log.name: ', name);
            console.log('log.descriptor: ', descriptor);
  
          }
  
          class App {
            @log
            onClientList() {
              console.log('App.onClientList');
            }
          }
  
          const app = new App();
          app.onClientList();
    	执行结果
    	    log.target:  {}
          log.name:  onClientList
          log.descriptor:  {
            value: [Function: onClientList],
            writable: true,
            enumerable: false,
            configurable: true
          }
          App.onClientList
  注意
  	装饰器函数的返回值可以不写，如果要写，必须为 函数,原传入值或null
  	装饰器和装饰器模式的理念一致，让一个需要装饰的类或对象通过函数传递的方式向这个类或对象中添加属性或方法
  */
  ```

  

+ 生命周期钩子函数(旧)

  ```java
  /*
  注意
  	此为react生命周期函数调用顺序，react在每次调用生命周期函数时，会去检查该组件实例对象是否重写该生命周期函数，如果重写则进行调用。如果某个生命周期钩子我们没有重写，react会依据情况去底层调用其自己的钩子函数(往复如此，直到初始化或更新的函数顺序调用执行完毕)
  	componentDidMount() 生命周期函数 一般用于开启 定时器，发起请求，等初始化操作
  	componentUnmount()生命周期钩子在一个组件实例对象将要被从页面卸载时,进行调用,通常在该函数中做首尾工作
  	只有调用render()方法才会进行页面重绘
  	每个组件实例对象都有自己的生命周期过程(函数)，它们之间互相不影响
  	componentDidMount()和componentUnmount()是两个常用的生命周期函数
  	当我们使用this.setState()更新状态时，react底层会从 shouldComponentUpdate()开始进行调用(,此方法返回值如果为true表示按顺序调用下一个生命周期函数，为false表示终止更新)，我们可以对shouldComponentUPdate()进行重写来完成延时更新等功能(如延时更新等)
  	调用this.forceUpdate()方法也可以完成页面重绘，该方法接收一个函数作为形参，起强制更新作用
  	当父组件向子组件传递props参数时，componentWillReceiveProps() 生命周期函数开始顺序调用
  
  组件实例对象挂载过程
   	当一个组件实例对象被创建(先遵循js对象创建原则)后，react底层会顺序去调用该组件实例对象的 componentWillMount() -> render() -> componentDidMount() 方法 完成初始化显示 。
   	
  组件实例对象更新过程
   	当一个组件做了状态更改操作时，如果调用的是 setState() ,则react会去调用 shouldComponentUpdate() -> componentWillUpdate() -> render() 方法 ;如果调用的是 forceUpdate() 则 react会按顺序调用 componentWillUpdate() -> render()
  */
  ```

  ![image-20220406195601900](D:\typora_import_images\typora-user-images\image-20220406195601900.png)



+ 生命周期钩子函数(新)

  ```java
  /*
  新增钩子函数
   static getDerivedStateFromProps(props,state)   // 此钩子需要使用static关键字装饰
     此生命周期函数起一个拦截作用可以在函数中对state的值进行更改，根据此钩子的返回值react会做判断，如果为     null则不会对state发生更改，如果返回一个对象，则表示更新state里的数据，如果如果返回对象的key在state里存   在，说明做覆盖操作，如果不存在说明做添加操作
     应用： 可以获得父组件向该组件传递的属性，然后维护到该组件的状态中
     使用
     	static getDerivedStateFromProps(props, state) {
              console.log(props, state);
              return {contentList: [], data: 0};
          }
    
    getSnapshotBeforeUpdate(prop,state)
    	此生命周期函数需要和componentDidUPdate配合使用,该函数返回一个结果，并作为参数出现在componentDidUpdate第三个参数上，通常用于界面临时信息(使用较少)
    	
  组件实例对象初始化过程(新钩子函数)
   getDerivedStateFromProps(props,state) -> render() -> componentDidMount
   
  组件实例对象更新过程
   	getDerivedStateFromProps(props,state) -> shouldComponentUPdate()(使用force(fn)强制更新时不会触   发此方法) -> render() -> getSnapshotBeforeUpdate() -> componentDidUpdate()
   	
  新，旧生命周期函数对比
   	新生命周期函数中废弃了 componentWillMount(),componentWillReceiveProps,componentWillUpdate()钩子   函数，新增了 getDerivedStateFromProps()，getSnapshotBeforeUpdate()钩子函数
   	getSnapshotBeforeUpdate 介于 componentDidUpdate和render之间
       
  */
  ```

  ![image-20220407172339059](D:\typora_import_images\typora-user-images\image-20220407172339059.png)





+ 虚拟dom与diff算法

  ```java
  /*
   当初始化一个组件实例对象时(引入组件)，页面渲染顺序为：创建虚拟DOM树 -> 真实DOM树 -> 绘制界面显示
   当对组件实例对象的state属性进行更改时，页面渲染顺序为: setState()更新状态(导致页面重绘) ->重新创建虚拟DOM树 -> 新/旧 dom标签对比 (通过对key的对比)-> 更新差异位置的虚拟DOM树 -> 重绘界面
  */
  ```

  ![image-20220407202046650](D:\typora_import_images\typora-user-images\image-20220407202046650.png)



+ fetch

  ```java
  /*
   前言
   	fetch是 h5 新增得一个js发起异步请求的一个语法，它遵循了关注点分离原则,即把状态和结果区分开，当返回状态时就知道请求得结果
   	
   语法
   	 sendRequestByFetch = () => {
          let q = "v";
          let s = "stars";
          let url = `https://api.github.com/search/repositories?q=${q}&sort=${s}`;
          fetch(url) // 向url地址发送fetch请求(异步请求)，并返回一个 fulfill状态的promise实例(存储着请求的状态)
  
              // 得到该请求的状态，如果接收请求的地址不存在，则立刻抛出一个rejected状态的promise实例(其存储着错误信息)
              .then(response => {  // 查看请求是否成功
                  console.log(response);
                  // 如果接收请求的地址存在则解析其结果并抛出一个fulfill状态的promise实例(存储着响应结果)
                  return response.json();
              })
              // 拿到该请求的结果。如果匹配路由异常(未匹配到路由)则立刻抛出一个 rejected状态的promise实例(存储着错误信息)
              // 如果有返回数据，则继续执行该函数
              .then(data => {  // 查看响应是否成功
                  console.log(data);
                  this.changeStateByFetch(data);
  
              })
              .catch((reason) => {
                  console.log(reason);
                  let {responded} = this.state;
                  responded = true;
                  this.setState({error: true, responded})
              });
      }
   
   注意
   	fetch向指定位置发送请求后，会返回一个 fulfill状态的promise实例对象,其中的value为此次请求的状态;当我们使用then方法拿到该结果后,如果地址匹配失败则立刻抛出一个状态为rejected的promise实例(值为报错信息),如果匹配地址成功需要手动返回一个结果(response.json(),js底层会自动将这个结果包装成一个fulfill状态的promise)
   	fetch发送的请求，如果请求的地址不存在则直接抛出一个 rejected状态的promise实例对象(该状态中的值为错误信息);如果存在则抛出一个 fulfill状态的promise实例对象(它的值是本次请求结果)，当我们使用then方法调用时，js底层会对该请求进行解析(检查是否匹配到路由和是否由结果)，如果该路由有返回数据则继续执行本then方法的第一个函数，如果该路由没有返回数据(说明路由匹配失败)，会立刻抛出一个 rejected状态的 promise,里面存储报错信息
   	fetch把依次状态的请求分成了两个节点，第一次then方法调用是看地址是否存在，第二次then方法调用是看路由是否匹配(是否有返回值)
  */
  ```

  

+ 组件间通信

  ```java
  /*
  注意
  	包括 props,ref,context,PubSub,redu
   1. props 可以实现 父向子通信 
   
   2. ref 可以实现 子向父通信
   
   3. 消息订阅与发布
   	原理
   		订阅者指定端口(变量方式)进行监听(订阅),当发布者每次发送数据时,订阅者就会被(PubSub底层)调用从而获		  得数据
   
   	使用
   		// 订阅者
   		 subscriber = () => {
          // 对 test 端口进行监听，获取每一次向该端口传递的数据
          // subscribe由底层自动调用，第一个参数为监听的端口(变量形式)，第二个参数为一个函数，用来给底层调用将信息载入到该函数身上
          // _ 是一个占位符，表示该变量基本不被使用 (原变量为msg为监听的端口,如 此时该值为 test )
          PubSub.subscribe("test", (_, data) => { // _ 进行占位
              console.log(data);
              data("world");
          }); 
      	}
      
      	// 发布者
      	pubsubPublish = ()=>{ // 该函数每调用一次就会向指定端口发送一次数据
          // 向test 端口发送数据一次数据
          PubSub.publish("test",this.testPubSub);
      	}
   
  4.简单的消息订阅与发布（PubSubd）
  	class PubSub {
          public static subscribe(str: string, data:(string,any)=>void): void {
              this.pool[str] = data;
          }
  
          public static publish(str: string, data: any): void {
          	if(this.pool[str]){
              	this.pool[str](str,data);
          	}
          }
          private static pool:any= {};
      }
  
      PubSub.subscribe("test",(_,data)=>{
          if(typeof data ==="function"){
              data();
          }
          console.log(data);
      })
      PubSub.publish("test","hello");
      PubSub.publish("test",()=>{
          console.log("your can invoke me");
      });
   
   	注意
   		订阅者需要先指定端口(变量方式)进行订阅，然后发布者才能向该端口(变量方式)发送数据,前后顺序不能颠	    倒,否则此时发布者的信息没有订阅者进行监听，从而导致数据丢失
   		发布者每次向订阅者发送一个数据时，订阅者就会被调用一次(pubsub底层完成调用)
   		PubSub的订阅与发布机制是全局都有效的，因此可以完成跨组件通信
   		PubSub机制和网络通信原理类似，每次发布者向指定端口发布请求，监听该端口的订阅者方法就会被调用拿到		 本次发布者发送的数据
   		发布者可以向所有订阅该端口的订阅者发送数据，即在发布者向该端口发送数据时，所有订阅该端口的订阅者	  都能够接收到
   
  
  */
  ```
  

## 路由

+ 注意

  ```java
  /*
   1. 手动改变路由会破坏网站结构，导致redux重新更新状态(需要持久化)
   2. <Navigate /> 做路由重定向时，只有返回到虚拟dom中才能完成重定向
   3. 使用 useNavigate()钩子函数拿到的是一个路由导航，使用 navigate("路由"); 即可跳转到对应的页面，如果路由路径为 /guide/xx 表示以guide为根路由改变当前url路径，如果路由路径为 guide/detail 表示guide以拼接前级路由改变前端路径(每次前端路径改变后，路由器就会监听此变化，并挨个去和路由进行比对，最后渲染对应的路由组件)
  */
  ```

  

+ 路由

  ```java
  /*
   前端路由 vs 后端路由
   	前端路由
   		响应由前端决定
   		路由是模糊(或精准v6以后)匹配
   	后端路由
   		响应由后端决定
   		路由是精准匹配
   
   浏览器存储页签机制(双栈结构)
   	浏览器的双栈机制可以细化为 回退栈，前进栈
   	使用栈结构来存储网页路由
   		回退栈
   			点击一个不产生新页的页签时,页面显示新内容，且会把上一个路由地址压入回退栈顶,当点击回退时从			后退栈顶拿一个路由地址进行显示，且将上一个内容压入前进栈栈顶
   		前进栈
   			在点击回退后,从后退栈栈顶拿出一个路由进行显示，前一个展示的页面会被压入前进栈栈顶,当点击前			进时,从该栈顶拿一个路由地址进行显示，前一个展示的页面会被压入后退栈栈顶
   	注意
   		已经加载过的路由在回退或前进时，不会重新加载，因为该栈会记录该路由访问的文件
   		当栈顶没有路由时，则无法点击(chrome浏览器长按 前进或后退可以选择路由进行进入)
   
   
   前端路由
   	前言
   		react-route-dom v6特性参考:https://blog.csdn.net/WAIXINGHAIKE/article/details/123672472 
  	安装前端路由依赖
   		npm i react-router-dom
   	
   	注意
   		Link组件最终呈现到html页面中时，是以a标签方式载入的 href为前端路由跳转
   		React实现浏览器相关操作使用到了history库
   		
   	获取路由属性
   		v5以及以前版本
   			使用一个路由组件后,该组件下的props属性下router底层会自动挂载history,location和match对象
   				history对象下常用属性或方法
   					goBack() // 相当于点击后退按钮
   					goForward() // 相当于点击前进按钮
   					push("url") // 执行push操作，将该路由压入栈顶
   					replace("url") // 执行替换栈顶路由操作
   				location对象下常用属性或方法
   					pathname //拿到当前前端路由的路径
   				match对象下常用属性或方法
   					params // 该属性存储着前端路由的params参数
   					params 参数的使用
   						多用于从数据库中获取数据,并进行前端展示,为了页面看起来有跳转玩的一个帽子戏法
   					注意
   						params参数和后端路由的params方式一致,匹配指定前缀的任意路由，并挂载到params						 对象参数下，key为占位的参数，value为匹配到的路径。
   							如  <Route path="/home/:id" component={HomeMsg}/>//匹配/home/任意
   								<Link to="/home/123">getMsg</Link> // 此时点击此Link会先去匹配								   Route中的/home/123 路由，如果没有则会去匹配前端参数路由，最终									会参数路由匹配的路由挂载到该组件的params对象参数下，key为:后的								   字符串 value为匹配的路径，如上例会挂载为 {id:123} 
   				
              
   		
   		
   	多级路由匹配
   		v6前多级路由匹配写法
   			// 匹配路径需要当前前端路由路径相同
   			render() {
                      return(
                          <div>i am a items
                              <ul>
                                  <li><Link to={"/items/msg"}>msg</Link></li>
                                  <li><Link to={"/items/news"}>news</Link></li>
                              </ul>
                              <Switch>
                                  <Route path={"/items/msg"} component={ItemsMsg}/>
                                  <Route path={"/items/news"} component={ItemsNews}/>
                              </Switch>
                          </div>
                      );
                  }
           
   		注意
   			 多级路由匹配时，多级路由的路由前缀必须继承前级路由的地址，否则路由会失效
   				如 一级前端路由为 /index ，二级前端路由必为 /index/e ,三级前端路由必为 /index/e/w
      	
      	v6后多级路由匹配写法
      		// 父路由组件中写法 ,如果有子路由需要在其后加 /*
      		<Routes>
                      <Route path={"/move/by/*"} element={<MoveBy/>}/>
                      <Route path={"/target"} element={<Target/>}/>
                      <Route path={"/move"} element={<Move/>}/>
                      <Route path={"/"} element={<Redirect to={"/move"}/>}/>
              </Routes>
      		
      		// 子路由组件中写法,Link写全当前前端路由路径，Route匹配时，只需匹配当前新增的前端路由路径
      		render() {
                  return(
                      <div>
                          <h2> i am a moveBy</h2>
                          <ul>
                              <li><Link to={"/move/by/swift"}>swift</Link></li>
                              <li><Link to={"/move/by/toggle"}>toggle</Link></li>
                              <Routes>
                                  <Route path={"/swift"} element={<Swift/>}/>
                                  <Route path={"/toggle"} element={<Toggle/>}/>
                              </Routes>
                          </ul>
                      </div>
  
                  );
              }
      
      
   	常用组件(再react-router-dom中的)
   		HashRouter
   			前言
   				HashRouter是H5提出的新路由，当使用HashRouter后，前端路由前会出现/#/进行间隔
   			使用
   				root.render(
                      <HashRouter>
                          <App/>
                      </HashRouter>
                  	);
   			url状态
   				http://localhost:3001/#/move
   		
   		BrowserRouter
   			注意
   				使用前端路由必须使用BrowserRouter或hashRouter进行包裹，可直接包裹再index.js中，避免				多次引入
   			使用
   				root.render(
                      <BrowserRouter>
                          <App/>
                      </BrowserRouter>
              	);
              	
   		Link 
   			作用
   				再该Link组件被点击后,会再当前url地址中加入特殊的前端路由(由to属性的内容决定),当url地				 址改变后Route组件会进行该前端路由的比对，如果找到了对应的前端路由则把该组件进行渲染，				   否则不做处理
   				
   			参数
   				replace|| replace = {true} 
   					可以设置前端路由为replace状态(replace指在进行路由点击操作时，每次替换栈顶的路					  由，默认为push 指保留每次路由点击操作，可以回退到上一个路由)
   			使用
   				<Link to="/target">点击</Link> //点击后再url地址中添加该前端路由,如果该路由再Route			  组件中匹配成功则进行该路由的渲染,如果没有匹配则不做处理(只是url地址中添加了该前端路由)
   			注意
   				设置的前端路由后，再没经过特殊处理，可以使用浏览器的回退和前进功能进行路由穿梭	
   				当此标签被点击后，会重写前端路由地址为你当前to属性指定的地址
   					如 to属性为  "/touch" ,而当前显示为 /home/work,当被点击后路由会变为 "/touch"
          
   		NavLink 
   			和Link作用类似，当它被点击后有特殊的高亮效果(由背景颜色决定)
   			使用
   				<NavLink to="/move" className={"demo"}>动作</NavLink
   			注意
   				使用此组件挂载样式时，如果样式渲染异常，尝试提高样式的优先级即可解决(!important)	
   				再 react-router-dom 6.0版本前使用activeClassName={"demo"} 方式设置高亮，以后只需要			  className={"demo"} 即可设置高亮
   		
   		Routes
   			前言	
   				react-router-dom 5.2版本以后使用路由需要此组件进行包裹，完全替代了Switch(继承了				  Switch的功能，并作出了优化)
   			使用
   				<Routes>
                          <Route path={"/target"} element={<Target/>}/>
                          <Route path={"/move"} element={<Move/>}/>
                          <Route path={"/"} element={<RootPage/>}/>
                   </Routes>
               
               属性
               	exact 
               		在5.2.0版本以前默认模糊匹配，因此如果想要某个路由精准匹配可以在其路由行内添加exact
   			
   		Route
   			作用
   				用于前端路由匹配,当匹配到对应的Route组件后,将其component(或element)属性下的组件渲染				 到该组件中
   			兼容问题
   				再 react-router-dom 5.2版本以前写法
   					<Route path={"/index"} component={Index}/>
                  	 <Route path={"/items"} component={Items}/>
   				再 react-router-dom 5.2版本以后写法(需要Routes组件包裹)
                      <Routes>
                      	// 如果此时前端路由为/target 则匹配此路由，渲染 <Target/> 组件
                          <Route path={"/target"} element={<Target/>}/> 
                          // 如果此时前端路由为/move 则匹配此路由，渲染 <Move/> 组件
                          <Route path={"/move"} element={<Move/>}/>
                      </Routes>                                   
          
          Switch
          	前言
          		Route再路由匹配时，会从上到下对所有的前端路由进行匹配，渲染满足要求的路由(模糊匹配),即				 可能会同时匹配多个路由。因此需要使用Switch做限制
          	作用
          		再一次路由查找中一旦匹配成功，不会继续向下继续匹配路由
          	兼容问题
          		再 react-router-dom 5.2版本以后不支持Switch，转而被Routes替代
          	使用
          		<Switch>  // 一次只会匹配一个满足要求的路由
                      <Route path={"/index"} component={Index}/>
                      <Route path={"/items"} component={Items}/>
                      <Route path={"/"} component={RootPage}/>
                  </Switch>
                  
          Redirect
          	作用
          		用于页面重定向,在没有前端路由匹配时可以自动调转到重定向页面，多放在路由的末尾，在所有的			 前端路由都没匹配的情况下，做出重定向操作
          	
          	兼容问题
          		Redirect在 v6版本后被移除，需要使用useNavigate来完成重定向
          	
          	使用
          		<Switch>
                      <Route path={"/index"} component={Index}/>
                      <Route path={"/items"} component={Items}/>
                      <Redirect to={"/index"}/> // 都没匹配上时，重定向到index前端路由
                  </Switch>
          
          Navigate
          	作用
          		v6新组件，可以完成页面重定向,该组件用于代替Redirect
          	使用
          		<Routes>
                      <Route path={"/move/by/*"} element={<MoveBy/>}/>
                      <Route path={"/target"} element={<Target/>}/>
                      <Route path={"/move"} element={<Move/>}/>
                      // Navigate需要在react-router-dom 中引入
                      <Route path={"/"} element={<Navigate to={"/move"}/>}/> //重定向到/move
                  </Routes>
          
          Outlet
          	作用
          		在使用Routes路由嵌套子路由时(使用useRoute解析的就是嵌套路由)，给直接子路由进行放行(该				组件需挂载到父组件中)，此时，在前端路由匹配到此子路由路径时，可以顺利渲染到放行处(即					 Outlet在哪里,其子路由渲染的组件实例对象就会出现在哪里),否则不会匹配
          	使用
          		export default class Activity extends Component {
                      render() {
                          return (
                              <div>
                                  <Outlet/> // 其子路由渲染组件的内容会被放到此处
                                  <h2>activity</h2>
                                  <li>
                                      <Link to={"/activity/basketball"}>toBasketball</Link>
                                  </li>
                                  <li>
                                      <Link to={"/activity/football"}>toFootball</Link>
                                  </li>
                              </div>
                          );
                      }
                  }
                  
          
                  
  前端路由兼容问题(版本迭代)
  	在react-router-dom 5.2.0版本以后不支持 Switch转而被 Routes替代，且Routes功能更强大
  	Route中不支持component写法，转而写为 element，传入的参数也稍有不同
  	在react-router-dom 5.2.0版本以前,前端路由为模糊匹配，在以后为精准匹配; 模糊匹配指 前端路由中只要前面有一级路由相等就会匹配，而不是检查后面是否相等，精准匹配指 前端路由指只有全部匹配才能匹配成功
  */
  ```


+ react-router-dom(v6版本)

  ```java
  /*
  注意
   	浏览器移除了WithRouter(因此无法隐式在路由组件挂载)，我们可以引入新增的钩子函数来完成这些功能，有 useParams,useLocation,useSearchParams(使用这些函数时，不能使用类的方式返回，而是需要使用函数的方式)
   	可以使用<Route path="*" element={<NotFound />} />去匹配任意路由,且 * 优先级最低,可以使用该值完成页面再无路由匹配时的页面显示
   	当前路由如果有其子路由，必须在其Route下的path方法中以 /* 结尾，表示其有子路由，如果不添加则无法正常显示，使用后，其子路由在Route匹配时，react底层会补齐父类的前端路由路径，自己只用写当前路径路由即可
   	使用navigate("url") 默认执行push操作，如果想变为replace操作添加配置项即可
   		如 navigate("url",{replace:true})
   	v6中路由只有一级路由需要 添加 / 其以后的路由不需要
  	使用嵌套路由必须指定出口(使用react-router-dom提供的Outlet组件)
  	v6踢狗的钩子函数可能需要在函数中使用
  	在路由嵌套中，当匹配到了一级路由后，在其一级路由组件中设置Outlet组件出口，二级路由才能够匹配，且二级路由组件会被渲染到一级路由组件的Outlet位置，在其二级路由组件中设置Oulet组件出口，三级路由才能够匹配，且三级路由组件会被渲染到二级路由组建的Outlet位置 ...(往复如此，后级路由需要前级路由组件设置Outlet组件放行才能够匹配，且其后继路由组件会被渲染到前级路由组件的Outlet位置)
  	
  嵌套路由
   	前言
   		v6版本中可以使用嵌套路由来管理路由的关系，但要设置一个出口Outlet组件
   	使用
   		// 嵌套路由
   			<Routes>
   				<Route path="/" element={<Layout/>}>
   					<Route path="board" element={<Layout/>}>
   					<Route path="article" element={<Layout/>}>
   				</Route>
   			</Routes>
   		// 在Layout组件中设置出口，此时其直接子路由才能进行匹配，渲染的内容会被放到<Outlet/>位置
   			<div>
   				ayout
   				<Outlet/> // 需要从 react-router-dom 引入
   			</div>
   			
  钩子函数的使用(以下用到的函数，都需要从react-router-dom进行引入)
  	useInRouterContext
  		作用
  			查看一个组件是否在 react-router-dom 上下文环境中,返回布尔值,如果在则为true，否则为false
  			
  	useRoutes
  		作用
  			创建路由表,useRoutes接收数组生成路由表.根据路由表(嵌套路由),动态创建<Routes>和<Route>
  		注意
  			使用该属性导出一个配置对象时，需要给有子路由的父路由设置放行(Outlet)，这样才能在前端路由跳			转到该子路由时，顺利匹配该子路由
  		使用
  			//路由表配置：src/routes/index.js
              import About from '../pages/About'
              import Home from '../pages/Home'
              import {Navigate} from 'react-router-dom'
  
              export default [
              {
                  path: "/activity",
                  element: <Activity/>,
                  children: [ // 因为 /activity有子路由，因此需要在其对应的组件类中设置Outlet进行放行，此时才会正确匹配路由，匹配到的组件类会被放置到 OUtlet 组件位置
                      {
                          path: "basketball",
                          element: <ActivityBasketball/>
                      },
                      {
                          path: "football",
                          element: <ActivityFootball/>,
                          children: [ // 因为 football 有子路由，因此需要设置Outlet放行
                              {
                                  path: "item",
                                  element: <ActivityFootballItem/>
                              }
                          ]
                      }
                  ],
              }
          ]
  
              //App.jsx
              import React from 'react'
              import {NavLink,useRoutes} from 'react-router-dom'
              import routes from './routes'
  
              export default function App() {
                  //根据路由表生成对应的路由规则
                  const element = useRoutes(routes)
                  return (
                      <div>
                          {element} // 注册路由
                          // 转换后的效果为
                          <Routes>
                          	// 想要其直接子路由进行匹配须在该路由的组件中使用<Outlet/>组件放行，然后							  其子路由渲染的页面会被放到Outlet组件位置
                          		如 当访问 /activity 路由渲染 <Activity/> 组件后，如果想要其直接子路							    由能够匹配，因在其<Activity/>内使用<Outlet/>组件，此时表示可以匹配其							  直接子路由，当匹配后，该路由下的组件会渲染到直接父路由组件的<Outlet/>							   组件位置
                          	<Route path="/activity" element={  <Activity/>}>
                          		<Route path="basketball" element={ <ActivityBaskectball/>}>
                          		<Route path="football" element={ <ActivityFootball/>}>
                          			<Route path="item" element={ <ActivityBaskectballItem/>}>
                          		</Route>
                          	</Route>
                          </Routes>
                      </div>
                  )
              }
              
              
              
   	useSearchParams
   		作用
   			该钩子函数可以拿到前端路由携带的查询字符串参数(query参数)
   		使用
   			// 路由跳转
   			navigate("/about?id=1001")
   			// 获得 query参数
   			let [params] = useSearchParams() 
   			let id = params.get("id"); // 获得查询字符串的id属性的值
   			
   	useParams
   		作用
   			获得路由携带的params参数
                   如<Route path="/:id" element={<Ro/>}>，此路由匹配成功后会将params参数以					  {id:xx} 方式挂载到Ro路由组件身上，想获取需要使用 useParams()钩子函数
           使用
               //前端地址跳转
                navigate("about/1111");
                //获得params参数
                let params = useParams() // 获取的是一个对象
                let id = param.id
       
      useNavigate 
       	注意
       		navigate在页面跳转时，默认执行的是push操作的，如果想执行replace则可以进行配置
          前言
          	此钩子函数继承了v5中的history对象的特点，并进行了封装
          作用
          	返回一个函数用来实现编程式导航。
          语法
          	 let navigate = useNavigate();
          	navigate(url,{配置项});
          常见用法
          	navigate(url); // 跳转至指定url
          	navigate(1); // 前进一步(相当于点击1次前进按钮)
          	navigate(-1); // 后退一步(相当于点击1次后退按钮)
          	navgate(url,{replace:true}) // 访问页面时执行replace覆盖
          使用
          	render() {
          		// 函数当作组件渲染时，react底层会进行一次调用，且携带的行内参数会以对象的形式存					  储，放到该函数的第一个形参上
                     function Redirect({to}){
                          let navigate = useNavigate();
                          useEffect(()=>{
                              navigate(to);
                          });
                          return null;
                      }
                      return (
                          <div className="router-part">
                              <ul>
                                  <li><Link to="/target">target</Link></li>
                                  <li><Link to="/move" }>move</Link></li>
                                  <li><Link to="/move/by" >MoveBy</Link></li>
                                  <li><NavLink to="/move" className={"demo"}>move</NavLink></li>
                              </ul>
                              <Routes>
                                  <Route path={"/move/by"} element={<MoveBy/>}/>
                                  <Route path={"/target"} element={<Target/>}/>
                                  <Route path={"/move"} element={<Move/>}/>
                                  
                                  // 重定向到指定路由
                                  <Route path={"/"} element={<Redirect to={"/move"}/>}/>
                              </Routes>
                          </div>
                      );
                  }	
       
       useLocation
       	作用
       		获取当前 location 信息，对标5.x中的路由组件的location属性
       	使用
       		import {useLocation} from 'react-router-dom'
   
              export default function Detail() {
                  const x = useLocation() // 获取location对象
                // x就是location对象:
                      {
                    hash: "",
                    key: "ah9nv6sz",
                    pathname: "/login",
                    search: "?name=zs&age=18",
                    state: {a: 1, b: 2}
                  }
                  return (
                      <ul>
                          <li>消息编号：{id}</li>
                          <li>消息标题：{title}</li>
                          <li>消息内容：{content}</li>
                      </ul>
                  )
              }
              
       useMatch
       	作用
       		返回当前匹配信息，对标5.x中的路由组件的match属性。
       		每次打开一个新路由时，useMatch方法就会调用一次，如果能匹配成功就以对象形式手机该路由信息，否则为null
       	使用
  		   import {useMatch} from"react-router-dom";
              export default function Login() {
                const match = useMatch('/login/:x/:y')
                console.log(match) //输出match对象
                //入股哦match对象匹配成功，则显示内容如下：
                  {
                    params: {x: '1', y: '10'}
                    pathname: "/LoGin/1/10"  
                    pathnameBase: "/LoGin/1/10"
                    pattern: {
                      path: '/login/:x/:y', 
                      caseSensitive: false, 
                      end: false
                    }
                  }
                return (
                  <div>
                    <h1>Login</h1>
                  </div>
                )
  }
  
       useNavigationType
          作用
          	返回当前的导航类型（用户是如何来到当前页面的）。
          返回值
          	POP、PUSH、REPLACE。
          备注
          	POP是指在浏览器中直接打开了这个路由组件（刷新页面）。
       
        useOutlet
  		作用
  			用来呈现当前组件中渲染的嵌套路由。
  		使用
  		   const result = useOutlet()
              console.log(result)
              // 如果嵌套路由没有挂载,则result为null
              // 如果嵌套路由已经挂载,则展示嵌套的路由对象
              
  	 useResolvedPath
  	 	作用
  	 		给定一个 URL值，解析其中的：path、search、hash值。
  		使用
  			console.log('useResolvedPath',useResolvedPath('/user?id=001&name=tom#qws'))
   			//输出：{pathname: "/user", search: "?id=001&name=tom", hash: "#qws"}
  
     
      
          
  */
  ```
  
  



## antd解析

**此笔记只做特别的，极难查找的，极其重要的内容，配置项去官网看**

+ 踩得坑

  ```java
  /*
  1. 密码输入框 需要设置 autoComplete="on" 否则可能出警告
    <Input.Password autoComplete="on" /> 
   2. 使用npm 下载 @craco/craco 时，可能报错，尝试使用yarn下载
   3. 如果自定义主题颜色无法成功修改，检查依赖包是否都下载了(查看package.json)
   4. menu组件只能被渲染一次，因此需要注意antd配合react组件编写问题(逻辑需编写严谨，不能渲染多个相同的组件，否则会报错(很难定位的错误))
   5. antd有自己的一套默认样式，如果使用了antd导致布局于实际位置有冲突，可以自己重新给掉样式以达到自己的预期
   6. 当使用Input组件设置默认值无法满足预期时，可以外层包裹一个Form组件，此时通过该组件的setFieldsValue()和resetFields()来达到设置期待初始值
   7. antd update组件实现手动上传需要后端配合？(未解决,需要后端配合，具体使用查看官方文档).customRequest可以堆action进行一次封装
   8、 form的initValue为不受控值(再状态改变导致重绘时，不会重新赋予一次初始值，只有再首次被加载时才会赋予)，如果想让表单的初始值可控，应该使用 form.setFieldsValue
  */
  ```


+ 使用原则

  ```java
  /*
   1. 在其需要的组件中进行修改即可
  */
  ```

  

+ 集合

  ```java
  /*
   1. antd 可以快速搭建一个中后端服务器
   4. 再拿到antd设置的样式后，我们可以对其样式进一步更改，以满足自己需求(组件也可以设置样式)
   5. antd把每一个盒子位于外部包含块的位置分成了 24个span,如果全占满说明铺满整个父盒,
   6. offset 可以向右移动x个span的距离，如 offset ={4} 向右移动了4个span位置
   7. 再查阅属性的时候 使用 ctrl+f 进行快速查找
   8. antd设置的组件类大多具有语义化的，Form组件就是一个form表单,Input组件就是一个Input框
  */
  ```
  
+ antd安装及高级设置(按需引入)

  ```java
  /*
   参考，https://ant.design/docs/react/use-with-create-react-app-cn
   注意
   	import '~antd/dist/antd.less'; 需要在App.js 或Index.js进行引入
  */
  ```

+ Button组件

  ```java
  /*
  配置项(即传入的props属性，该组件会接收并解析，渲染一个满足要求的一个组件实例对象)
  type属性
  	primary 设置为主题颜色
  	warning 黄色
  	error 红色
  	
  
   注意
   	该组件得type属性可以设置按钮的类型(即antd约定好的样式)
  */
  
  ```
  



+ 栅格系统

  ```java
  /*
   介绍
   	antd设置了自己的一套栅格系统，它们把屏幕分成了24个span位
   	
   使用
   	<Row justify="start"> // 设置行
        <Col span={4}>col-4</Col> // 设置该行每一列占用的 span长度
        <Col span={4}>col-4</Col>
        <Col span={4}>col-4</Col>
        <Col span={4}>col-4</Col>
      </Row>
      
   Row 常用属性
   	wrap
   		作用
   			是否自动换行
   		使用
   			wrap = {false}
   	justify
   		作用
   			标签水平排列方式
   		可选值
   			start | end | center | space-around | space-between | space-evenly
   	align
   		作用
   			垂直对齐方式
   		可选值
   			top | middle | bottom
   	gutter
   		作用
   			栅格间隔(元素间距)，可以写成像素值或支持响应式的对象写法来设置水平间隔 { xs: 8, sm: 16, 			  md: 24}。或者使用数组形式同时设置 [水平间距, 垂直间距]
   		可选值
   			number | object | array   默认为0
   Col 常用属性
   	order 
   		作用
   			改变该组件的显示位置
   		使用
   			order={4}
   	flex
   		作用
   			指定Col组件再该行占用的位置
   	offset
   		作用
   			向左间隔 x个span位置
   	pull
   		作用
   			向左移动 x 个span位置
   	push
   		作用
   			向右移动 x 个span位置
   	span	
   		作用
   			占用的栅格数
  */
  ```

  

+ Form组件

  ```java
  /*
  配置项(即传入的props属性，该组件会接收并解析，渲染一个满足要求的一个组件实例对象)
  	name 
  		指定当前标签id，Form表单下的组件标签的name会依次进行匹配拼接
  			如 Form name="basic" ,  Input name = "input"
  				则它们id分别为 form id="basic" ,  input id="basic_input"
  	initialValue
  		指定表单初始值，如果Form指定就以Form为准，其没有指定就以自己的为准
  	labelCol
  		设置当前label标签的位置
  		使用
  			labelCol={{
                         span: 7, // 长度
                    }}
        rules 
        	用来给每个表单设置输入校验提示
        	自定义配置规则
        		// 每次输入依次内容就会调用依次 validator所指向的函数
        		 const verifyConfig = (_, value) => {
        		 if(value === undefined) return Promise.reject(new Error("表单不能为空"));
                  let reg = /^[a-zA-Z\d_]+$/;
                  // 想正确的条件很简单，想错误的条件很难想全，因此我们只用列出满足条件的判断，然后取反就是				不满足条件
                  if(!(value.length >=6 && value.length <=15)) {
                      return Promise.reject(new Error("长度需大于5位小于16位"));
                  }
                  if(! reg.exec(value)) return Promise.reject(new Error("只能输入字母数字或下划线"));
                  return Promise.resolve();
              }
        		 rules={[
                      {
                          validator:verifyConfig // 该函数由底层调用(调用时传入两个参数)
                      },
                  ]}>
   
  
   注意
   	form组件实际渲染的是一个form表单
   	Input 组件的label属性为 表单显示名，name 属性为表单 id
  */
  
   //使用 form实例达到强制更新Input框的初始值
  	// form实例常用方法
  		/*
  			 form.setFieldsValue({category:extra.name}); 该方法需要传入一个形参为一个对象，其属性为一个指定Item的name属性值，value为该Item下Input的初始值，当指定后，即可给对应Item下的input框指定初始值
  			  form.resetFields(); // 该方法会重置所有Input框的内容
  		*/
  // form = {form} 为指定form实例获取方式
  const [form] = Form.useForm(); // 创建form控制实例
   form.setFieldsValue({category:extra.name}); // 给Item的name为cateogry的Input组件指定初始值
     form.resetFields(); // 清空所有表单内容
  <Form initialValues={{remember:false}}  form={form} >
  	<Form.Item  rules={[{ required: true ,message:"分类不能为空",pattern:/^\w*/}]} 				label={"分类："}name={"category"}>
            <Input  placeholder={"请输入信息"} ref={inputRef}
                onPressEnter={caller === "update" ? updateCategory : addCategory}/>
      </Form.Item>
  </Form>
  ```
  



+ menu组件使用

  ```java
  /*
  介绍
   	menu 组件没一个主侧边导航栏
  */
  import React from "react";
  import {useNavigate} from "react-router-dom";
  import {Menu, MenuProps} from 'antd';
  import {
      HomeOutlined, MailOutlined, AppstoreAddOutlined,
      BarsOutlined, DatabaseOutlined, UserOutlined, AreaChartOutlined
      , BarChartOutlined, LineChartOutlined, PieChartOutlined
  } from '@ant-design/icons';
  import {ROUTERS} from "../../config";
  
  const {
      INDEX, MERCHANDISE, CATEGORY_MANAGE, MERCHANDISE_MANAGE, USER_MANAGE,
      ROLE_MANAGE, ICON_MANAGE, BAR_GRAPH, LINE_GRAPH, PIE_GRAPH
  } = ROUTERS;
  
  
  type MenuItem = Required<MenuProps>['items'][number];
  
  /**
    @description: 工厂函数，用于生成导航栏的规则
  */
  function getItem(
      label: React.ReactNode,
      key: React.Key,
      icon?: React.ReactNode,
      children?: MenuItem[],
      type?: 'group',
  ): MenuItem {
      return {
          key,
          icon,
          children,
          label,
          type,
      } as MenuItem;
  }
  
  /**
    @description: 配置导航栏规则
  */
  const items: MenuProps['items'] = [
      getItem('首页', INDEX, <HomeOutlined/>),
  
      getItem('商品', MERCHANDISE, <AppstoreAddOutlined/>, [
          getItem('分类管理',CATEGORY_MANAGE, <BarsOutlined/>),
          getItem('商品管理', MERCHANDISE_MANAGE, <DatabaseOutlined/>)
      ]),
      getItem('用户管理', USER_MANAGE, <MailOutlined/>),
      getItem('角色管理', ROLE_MANAGE, <UserOutlined/>),
      getItem('图标管理', ICON_MANAGE, <AreaChartOutlined/>, [
          getItem('柱状图', BAR_GRAPH, <BarChartOutlined/>),
          getItem('折线图', LINE_GRAPH, <LineChartOutlined/>),
          getItem('饼状图', PIE_GRAPH, <PieChartOutlined/>)
      ]),
  ];
  
  let curPage = INDEX;
  
  /**
    @description: 每次选中第一个导航Sider函数会重新调用依次
  */
  const Sider = () => {
  
      const navigate = useNavigate();
      /**
        @description: 用于路由跳转
      */
      const onClick: MenuProps['onClick'] = e => {
  
          if(curPage === e.key) return; // 减少页面无效重绘
          curPage = e.key;
          // 做一个排我，即自己点击自己无响应
          switch (e.key) {
              case INDEX:
                   navigate(INDEX);
                   return;
              case CATEGORY_MANAGE:
                  navigate(CATEGORY_MANAGE);
                  return;
              case MERCHANDISE_MANAGE:
                  navigate(MERCHANDISE_MANAGE);
                  return;
              case USER_MANAGE:
                  navigate(USER_MANAGE);
                  return;
              case ROLE_MANAGE:
                  navigate(ROLE_MANAGE);
                  return;
              case BAR_GRAPH:
                  navigate(BAR_GRAPH);
                  return;
              case LINE_GRAPH:
                  navigate(LINE_GRAPH);
                  return;
              case PIE_GRAPH:
                  navigate(PIE_GRAPH);
                  return;
  
          }
  
      };
      return (
          <Menu
              onClick={onClick}
              style={{width: "100%", height: "100%"}}
              defaultSelectedKeys={[INDEX]}
              mode="inline"
              items={items}
          />
      );
  };
  
  export default () => <Sider/>;
  ```

+ form组件

  ```java
  /*
   注意
   	每个标签的name值需不一致，如果一致则它们之前的数据共享
  */
  /**
   @description: Form配置项介绍
   @property: Item  ,为form中的一行数据，antd为其设置了对应的外边距
   @config:
          name = " 为该 From组件再渲染为真实dom时的id属性，其下关联的所有组件再渲染时会带上此id前缀"
          labelCol = "是一个对象，对象中有个属性span{span:number}，设置Item组件内为Input的组件的Item整体显示偏移量(相当于offset)"
          wrapperCol = "是一个对象，对象中有个属性span{span:number}，设置Item组件内为Input的组件的Input框宽度"
          onFinish = "是一个函数，当点击提交后，通过前端验证规则调用此函数"
          onFinishFailed= "是一个函数，当点击提交后，未通过前端验证规则调用此函数"
  
  */
  
  /**
   @description: Form组件下 Item组件的配置项
   @config:
          label = "是一个字符串，设置显示内容"
          name = " 为该 Item组件再渲染为真实dom时的id属性，其下关联的所有组件再渲染时会带上此id前缀"
          rules = "此Item组件下Input组件的验证规则"
              max input表单中输入的最大长度   number
              min input表单输入的最小长度   number
              message 不满足限制条件时，出现的内容 string
              pattern 正则表达式匹配 RegExp
              require 必填项  ，前面会出现 * 提示   true | false
              validator 自定义匹配规则 ，接收的是一个函数，第一个参数为匹配规则(几乎不用使用 _占位接收)，第二个参数为当前input中的信息
              注意：每一次修改input框时就会触发验证
          wrapperCol = "是一个对象，该对象下常用属性offset:4 ,可以设置当前Item组件的偏移量"
  
  
  */
  ```

+ Input

  ```java
  /**
   @description: Input 配置项介绍
   @config
          placeholder = "再表单没有内容时的虚拟占位"
          size = "表单的大小  large | small | 默认
          prefix = "icon组件图标 ，可以添加该图标组件到input表单首部"
  */
  ```

  

+ table组件

  ```java
  c
  /**
   @description: columns是配置每一个列头的数组，数组中的每一个索引为一个对象，该对象内制定每一列的规则
   @config: 常用配置项{
      title : 为该列的名字
      dataIndex: 为其列下的可以展示的对象属性，如 dataIndex：name,表示该列只展示name属性
      render(text,record,index): 是一个函数，起到对原数据的装饰作用(如添加标签样式等)，装饰的标签最终会返回到该列下
                                  第一个形参为当前列匹配到的属性值(如果没有匹配到任何信息则为当前行)
                                  第二个形参为当前行对象 ， 第三个形参为当前的行(对象)位于数组中的索引位置
                                  当此行不需要匹配内容时，可以调用render并返回一个虚拟dom来生产一个我们需要的内容
                                  render为最终该列展示的内容，return什么就展示什么
      filters: 表示过滤掉信息(过滤掉不包含选中信息的行)，此时会产生一个过滤图标，当点击此图标即可开始过滤
          是一个数组，每个数组的索引存储一个对象，该对象下有text(展示内容),value(过滤的内容),
          children(进行二次过滤,是一个数组，然后继续适配 text,value)属性
      onFilter: 当点击过滤后底层会遍历每一行然后调用此函数，再调用时把每一个过滤的条件和被比较目标依次传入比较，因此我们只用做
          简单相等判断 ， 如 record.name.indexOf(value) === 0;(底层会根据此返回值来判断此数据是否被过滤掉，false为过滤，true不过滤)
      defaultSortOrder: 数据初始状态时，默认的排序规则 ，为一个string<"descend"|"ascend">
      sorter: 需要传入一个函数(相当于sort函数，我们可以配置排序规则)，当点击列名时调用
      sortDirections: 可以设置排列方向，是一个数组 [ ?<"ascend"> | ? <"descend">]
      filterMode: 可以选择过滤框展示的类型， 可选值有 <"tree">
      filterSearch : 可以选择是否能使用搜索方式过滤 ，可选值 true(可以搜索过滤) | false(不可以搜索过滤)
      align： 选择当前列即列内容的对齐方式，可选值 <"left"|"center"|"right">
      width: 可以设置当前列的显示宽度
      fixed: 当前列数较多，且给当前table设置了scroll属性后，我们可以使用此属性对当前重要元素进行固定显示(相当于position:fixed)
              可选值 <"left" | "right">
    }
   */
  
  /**
   @description: table常用配置项
   @config:
   columns = "为一个数组，数组每一个索引为一个对象，该对象为当前列的信息和可接收的属性"
   dataSource = "为一个数组，数组每一个索引为一个对象，每个属性对应列中的dataIndex进行匹配(key除外)"
   rowSelection = "用来给每行数据的行头指定一个button类型 为一个对象，该对象的type属性为该button的type,
   另外可以传入一系列方法来展示和改变button的状态"
   pagination ="为一个对象，可以设置当前表单显示的数据列数如 {{pageSize:5}}"
   scroll = "为一个对象，可以设置当前表单的滑动宽高"
   title = "为一个函数，返回一个虚拟dom(组件)到表单头部"
   footer = "为一个函数，返回一个虚拟dom(组件)到表单尾部"
   rowKey = "指定每一行数据的key值"
   */
      
   /*
    注意
    	如果后台一次返回所有数据(前端分页)，我们可以使用Table的pagination直接处理每页展示个数，其组件会自动适配页数；如果后台分页返回数据(后端分页)，其会返回总数据数，并根据传入的分页数据返回对应数据，我们进行pagination额外配置即可处理
    	columns和dataSource的值必须是一个数组
   */
   // 解决 前端分页
      <Table
              dataSource={dataSource} // 指定匹配数据源
              columns={columns}  // 指定了表格的列头
              pagination={{
                pageSize: 5, // 每页显示 5个数据
              }}
              rowKey="_id" // 指定每行数据的key为 _id属性
            />;
   // 解决 后端分页
  	<Table
              dataSource={dataSource}
              columns={columns}
              pagination={{
                pageSize:4, // 每页显示 4个数据
                total: this.state.totalPage, // 后端返回的数据总数，我们做一个预展示页
                current: this.state.currentPage, // 当前的页数 
                onChange: this.onChangeFun // 当点击时,获取对应页的页数,并向后端发送请求,获取该页的数据
              }}
              rowKey="_id"
            />;
  ```
  



+ modal组件

  ```java
  /**
   @description: Modal配置项介绍
   @config:
          title = "为该Modal框的标题"
          visible = "该modal选项框是否显示，可选值 true | false，配合button完成启停"
          onOk = ”当点击ok(确认)时，调用的函数“
          onCancel = ”当点击取消时，调用的函数“
          okText = "设置 成功按钮的文本"
          cancelText = "设置 失败按钮的文本"
          centered = "居中展示 modal对话框 ， 可选值 true | false"
          closable = "是否显示可关闭按钮 ， 可选值 true | false"
          width = "可设置 对话框宽度"
          包裹的为显示的内容
   @tips:
          Modal对象下有 info,success,error,warning 四个方法，该方法接收一个对象，其对象配置和Modal配置差不多，可以显示对应效果
      的提示框
  
  */
  ```

+ Upload组件

  ```java
  /**
    @description: fleList 为图片资源集，收集则图片的所有基础信息，是一个数组，数组中每个索引是一个对象(图片基础信息)
      name : 为该图片的名字  string
      status: 为当前图片的状态，不同状态对应的样式不同 	error | success | done | uploading | removed
      url: 为该资源的基础路径
      percent: 为当前图片上传的百分比
      uid: 为当前图片的key ，不设置antd会自动生成
  */
  /**
    @description: Upload 资源上组件
      action ： 为上传的后端服务器地址
      listType:  上传列表的内建样式（突变的展示方式），支持三种基本样式 text, picture 和 picture-card
      method： 发送请求的方式 。 默认 post 请求
      withCredentials： 上传是否携带 cookie 默认为 false
      onPreview : 点击预览时调用的函数
      onChange: 点击删除，上传中、完成、失败，调用的函数
      onRemove： 点击删除调用的函数
      beforeBupload : 把图片上传到服务器前调用的函数，如果没true表示上传，为false表示不上传
   */
  
  /*
   注意
   	Upload组件在传入图片时，默认上传，因此我们需要用到组件的一个钩子函数 beforeUpload ，其接收一个布尔值，当为true时表示上传，为false时表示不上传(我们自己控制上传逻辑)
  */
  
  /*
   // 点击上传后所有图片才会交给后端，而非上传一个图片就发给后端一次
  import React, { useState } from 'react'
  import { Modal, Upload,Button } from 'antd';
  import { RollbackOutlined, CameraOutlined, PictureOutlined, PlusOutlined } from '@ant-design/icons';
  import axios from 'axios';
  
  export default function Identify() {
      const [isModalVisible, setIsModalVisible] = useState(false);
      const [fileList, setFileList] = useState([])
      let history = useHistory();
  
      const handleOk = () => {
          const formData = new FormData();//创建formData对象，用于序列化文件
          fileList.forEach(item => {
          //将fileList中每个元素的file添加到formdata对象中
          //formdata对Key值相同的，会自动封装成一个数组
              formData.append('file', item.originFileObj);
          });
          axios({
              method: 'post',
              url: '',
              data: formData
          }).then(res => {
              console.log(res.data.msg)
              if (res.data.msg === 'success') {
                  //上传成功后执行的函数
              }
          })
          //清空fileList
          fileList.length = 0;
          setIsModalVisible(false)
      };
  
  	//会话框的方法，可以忽略
      const handleChange = (info) => {
          setFileList(info.fileList)
      }
      const handleCancel = () => {
          setIsModalVisible(false);
      };
      
      const showModal = () => {
          setIsModalVisible(true);
      };
      const uploadButton = (
          <div>
              <PlusOutlined />
              <div style={{ marginTop: 8 }}>Upload</div>
          </div>
      );
      return (
          <div className='identify'>
              <Modal title="上传图片" visible={isModalVisible} onOk={handleOk} onCancel={handleCancel}>
                  <Upload
                      multiple={true}
                      //陈列样式，现在是卡片式
                      listType="picture-card"
                      beforeUpload={() => {
                         //阻止上传
                          return false;
                      }}
                      onChange={(info) => { handleChange(info) }}
                      action=''
                  >
                      {fileList.length >= 8 ? null : uploadButton}
                  </Upload>
              </Modal>
             
                  <Button onClick={() => showModal()} >
                  上传图片
                  </Button>
              </div>
          </div>
      )
  }
  
  
  */
  ```
  



+ Select组件使用

  ```java
  /*
    <Select
         showSearch // 可搜索所有的选项
         placeholder="搜索或下拉选择分类"  // 提示符
         mode="multiple"  // 可以多选
         optionFilterProp="children"   
         defaultValue="" // 默认显示的值
         onChange={(e) => {   // 在改变Select中的值时调用 ,e为当前Option的 value值
             console.log(e);
         }}
     >
         {
             Object.keys(categoryObj).map((key: any) =>
                 <Option value={key} key={key}>{categoryObj[key]}</Option>)
         }
     </Select>
  */
  ```




+ tree组件

  ```java
  /**
    @description: Tree 组件常用配置项
      @config:
          checkable : tree结构中是否有复选框
          defaultExpandAll： 初始化时展开所有的节点
          onExpand : 为一个函数，当树结构被折叠或展开时调用，函数的第一个形参为此次操作受影响的节点key值
          onSelect : 为一个函数，当某个树节点的文字本选中时调用，函数的第一个形参为此次被选中的节点信息
          onCheck: 为一个函数，当某个树节点的复选框被选中时调用，函数的第一个形参为此次被选中的节点的key值
          selectedKeys: 为一个数组，表示此时树中选中的节点(key值)
          checkedKeys: 为一个数组，表示此时树中选中的节点复选框(key值)
          expandedKeys: 为一个数组，表示此时书中展开的节点(key值)
          autoExpandParent: 为一个 布尔值 true|false , 初始化时，是否展开所有树节点
          treeData: 为树结构信息，为一个数组，数组里为一个个对象
              treeData 对象配置
                  title: 为此节点的展示信息
                  key: 为该节点的Key值
                  children: 为该树分支的子树节点(其仍可以包含 title,key,children属性)
   */
  /*
   注意
   	tree的控制需要配合状态使用，才能完成预期效果，否则页面不会重绘
  */
  ```

  

## redux解析

+ 集合

  ```java
  /*
   1. redux适合用在复杂项目中的集中状态管理上（如100个组件，其中20各组件需要用到同一个状态）。小型项目可以使用pubsub或context
   2. redux在初始化和更新时底层会进行一次调用
   3. 在reducer中 不要去改变原有地址的数据，而是应该重新创建一个地址然后再进行修改
   4. 只引入redux管理状态时，再更新redux中的状态时不会调用react组件中的render方法重绘，因此需要使用 其对象下的subscribe()方法进行刷新以达到重新渲染的目的（subscribe方法再每次redux的状态改变后会被调用一次），或用redux-react(react推出的关联redux的中间件)
   5. 使用redux后，状态统一由redux进行集中管理，所有再首次更新redux时，需要给它们赋予初始状态值
   6. 再使用redux时，因为是使用type来做状态操作区分，而type属性又是一个string类型，因此可以创建一个专门用于保存type的配置文件，这样可以避免失误
   7. 引入了redux管理的状态后，当其的状态进行更新后，由 react-redux 完成重绘即局部更新
   8. store对象中保存了redux的所有状态，以对象形式
   9. react-redux 的connect方法是一个经典的闭包，当我们传入两个函数后（第二个参数可以是一个对象，最终会由其底层做一个静态代理），其内部进行调用，并对此函数返回的对象进行解构进行保存，此connect会返回一个函数，并且这个函数接收的需要是一个组件类，当传入后，第一个函数保存的对象会被以props参数传递到该组件类身上进行包装，并返回一个包装类到该行，此时使用 export default 暴露即可(暴露的是被传入props全局状态的组件类)
   	大致实现
   		function connect(fn1,fn2) {
            let obj1 = fn1(store.state);
            let obj2 = null;
            if (fn2 instanceof Function) {
              obj2 = fn2(store.dispatch);
            } else {
              for(let i  in fn2){
                obj[i] = function () {
                  store.dispatch(fn2[i](...args));
                }
              }
            }
            return function (Component) {
              Component.props = { ...obj1，...obj2  };
              return Component;
            }
          }
  */
  ```
  
  
  
+ 常见面试题

  ```java
  /*
   redux 结构图
   redux 的基本编码
   对redux的理解
   区别ui组件与容器组件
   何为高阶组件
  */
  ```

+ 介绍

  ```java
  /*
   redux是一个独立专门用于做状态管理的js库(并非react插件库).它可以用在react,angular,vue等项目中，主要功能为集中式的状态管理工具
   
   actionCreators
   	用于包装dispatch的缓冲层，可以在该层做判断等限制语句
   store
   	用于集中管理所有redux状态.再调用此对象的dispatch方法时，该store会去分发给对应的reducer进行状态改变（reducer在进行返回）
   reducers
   	用于操作状态并返回给store
   react Component
   	最终会去状态的组件类
   	
  */
  ```
  
  ![image-20220422165152266](D:\typora_import_images\typora-user-images\image-20220422165152266.png)

+ redux中常用方法

  ```java
  /*
   dispatch
   	作用
   		用来发布改变状态的格式，会去调用对应的reducer处理该对象
   	使用
   		store.dispatch({type:"xx",data:"xx"}) // type为一个字符串，data为状态值，可以是任何类型
   
   getState
   	作用
   		获取当前store管理的状态
   	使用
   		store.getState()
   
   subscribe
   	作用
   		当redux管理的状态更新后，该方法会自动被调用（底层调用）
   	使用
   		store.subscribe(()=>{})
  */
  ```

  

+ react-redux

  ```java
  /*
   介绍
   	react-redux是一个react插件库，用于简化在react中 使用redux
   
   react-redux 组件分类
   	react-redux 将组件分成两类 UI组件和容器组件
   		UI组件
   			只负责UI呈现，不带任何业务逻辑
   			通过其容器盒子props传递参数
   			不适用任何Redux的api
   			一般保存在components目录下(在react-redux角度中，路由组件也应该放到该目录)
   		
  		容器组件
          	负责管理数据和业务逻辑，不负责UI呈现
          	使用redux的api
          	一般在containers目录下
   
   工作流程
   	使用 import {connect} from "react-redux"; 后，我们可以传入两个函数来获取store管理的状态和追加操作store状态的方法，第一个参数为一个函数，该函数由react-redux底层进行调用，并把store管理的所有状态(store管理的状态是一个对象，我们可以对其进行解构获取需要的全局状态)当作第一个参数传入，第二个参数为一个函数或者对象，当为函数时，会由底层调用并传入 dispatch 当作该函数的第一个参数，当传入对象时，其对象的value必须是一个action函数，然后react-redux会对该函数进行简单代理模式的包装，当此代理函数被调用后，帮助调用dispatch方法改变store里的状态 。（ 以上两个形参如果使用到了函数类型则需要返回一个对象，最终它们会被当作props参数挂载到需要包装的组件类上），此connect返回的是一个函数，我们必须在进行一次函数调用并把需要操作全局状态的组件当作第一个参数传入，最终前一个函数中返回的对象会被解构并挂载到该组件类中进行包装(会被放到props参数下)
   
   底层大致调用写法
   	方式1 传入时
   	方法名：function(){  // 方式一传入时，react-redux底层会做一个静态代理模式的一个包装，不向外部暴露dispatch
   		dispatch(actions(...args)); // 可以使用 thunk中间件，当actions(...args)返回的是一个函数时，该thunk中间件就会去调用它，并把dispatch当作参数传递(向外暴露dispatch)，此时我们就可以完成异步封装
   	}
   	方式2 传入时
   	方法名(){ // 方式二传入时，react-redux会把dispatch暴露给我们，此时可以完成异步操作
   		dispatch(actions(...args));
   	}
   
   使用
   	// 包装UI组件的两种方式
   	import {connect} from "react-redux";
      import IncrementIfOdd from "../component/IncrementIfOdd";
      import {handlerIncrement, handlerSubtract} from "../action";
  
  	// 方式1 ，使用对象传入时，则对象中属性的值必须是一个函数，最终会由底层完成调用然后拿到对象，并由其完成 dispatch({type:"xx",data:"xx"}); 操作（底层使用了静态代理模式，在外层套了一套代理函数，当我们调用这个方法时，底层完成了该操作    如function(){dispatch(handlerIncrement())} ）
      export default connect(number => ({number}), {
       	// 把increment和subtract当作方法挂载到指定组件的props下，当被调用时，底层会调用此方法并包装成dispatch(handlerIncrement()) 去操作store对象
              increment: handlerIncrement, 
              subtract: handlerSubtract
          }
      )
      (IncrementIfOdd)
  
  	// 方式2 ，使用函数传入时，底层调用本函数并传入了dispatch，因此由我们来完成 						dispatch({type:"xx",data:"xx"}); 操作
      export default connect(number => ({number}), dispatch => ({
              increment() {
                  dispatch(handlerIncrement())
              },
              subtract() {
                  dispatch(handlerSubtract())
              }
          })
      )
      (IncrementIfOdd) // 该函数调用完毕后，返回该包装后的组件类，只有该组件类身上才维护了store全局属性
  注意
   	react-redux把组件分为了容器组件和UI组件，由容器组件去拿到store里的数据并包装到对应UI组件上(必须向外暴露此包装后的组件，在使用其时props上才有操作redux的状态和方法(原UI组件不经过包装没了此redux的状态))，在开发中通常此包装过程和ui路由写在一起，然后暴露包装后的组件类，然后此组件被称为容器组件
   	react-redux 提供了一个 Provider 组件，该组件是所有组件的最顶层，把store传递给该组件即可，该组件底层会完成redux的渲染监听(当redux管理的状态改变后，会去重新渲染引用了该状态的组件)
   	react-redux 加载的 redux中的状态，最终 会以 props参数挂载到 指定的组件上，且调用该方法传入参数后，会由其底层传递到acitons函数的形参中，并由其完成dispatch(actions(..args))状态改变(如果采用方式一则底层dispatch，如果使用的是方式二则我们自己封装了dispatch的过程)
   	只有经过react-redux包装过并向外暴露的组件类，其props属性中才有react-redux挂载的redux全局状态和操作方法 
   	
  */
  ```

  

+ redux工作机制

  ```java
  /*
  原理
   	在使用 createStore(reducer); 创建了一个store对象并指定了reducer函数后(该函数第一个形参为State,第二个形参为action)，一个redux集中状态管理器就诞生了(其在初始化和状态更新时，会调用该reducer来维护一次store的状态),当其他组件类引入了这个store，并使用其方法dispatch({type:"x",data:"x"})后,该store会去把该对象交给对应的reducer处理(根据type属性的值，调用对应的reducer)，该reducer处理完毕后将改变后的状态又返回给store对象统一管理(此时sotre下的subscribe方法会被底层调用一次)，一次store的状态管理就结束了
   进阶
   	可以创建一个action.js文件对在提交状态更改前做一个缓冲层处理,然后将对应的对象返回
   注意
   	可以使用 store.getState()获取其管理的状态
   	reducer(state,aciton) 中第一个形参的state第一次由自己手动设置初始化状态(redux初始化会调用),在之后的调用中，store仓库会传入对应的state和action
   实现
   =========================
   //store
   import {createStore} from "redux";
   import reducer from "./reducer";
   export default createStore(reducer);
   
   =========================
   //reducer
   import {SUBTRACT, INCREMENT} from "./typeConfig"
   export default function reducer(state = 0, action) {
      let {type, data} = action;
      let newState = null;
      switch (type) {
          case INCREMENT:
              newState = state + data;
              return newState;
          case SUBTRACT:
              newState = state - data;
              return newState;
          default:
              return state;
      }
   }
   =========================
   //action
   import {INCREMENT, SUBTRACT} from "./typeConfig";
   export const handlerIncrement = () => ({type: INCREMENT, data: 1});
   export const handlerSubtract = () => ({type: SUBTRACT, data: 1});
   
    =========================
   //computer
  import React, {Component} from "react";
  import Increment from "./Increment";
  import IncrementIfOdd from "./IncrementIfOdd";
  import Subtract from "./Subtract";
  import store from "../store";
  
  export default class Computer extends Component {
     
      render() {
  
          return (
              <div>当前的数是:{store.getState()}
                  <Increment store = {store}/>
                  <IncrementIfOdd store = {store}/>
                  <Subtract store = {store}/>
              </div>);
      }
  }
  =========================
   //Increment
  import React, {Component} from "react";
  import {handlerIncrement} from "../action";
  
  export default class Increment extends Component {
      handlerClick = () => {
          let {store} = this.props;
          store.dispatch(handlerIncrement());
      }
  
      render() {
          return (<button onClick={this.handlerClick}>+</button>);
      }
  }
  =========================
   //IncrementIfOdd
  import React, {Component} from "react";
  import {handlerIncrement} from "../action";
  export default class IncrementIfOdd extends Component {
      handlerClick = () => {
  
          console.log(this);
          let {store} = this.props;
          console.log(handlerIncrement());
          if (Math.abs(store.getState() % 2) === 1) {
              store.dispatch({type: "increment", data: 1});
          }
      }
  
      render() {
          return (<button onClick={this.handlerClick}>increment if odd</button>);
      }
  }
  
  =========================
   //Subtract
  import React, {Component} from "react";
  import {handlerSubtract} from "../action";
  
  export default class Subtract extends Component {
  
      handlerClick = () => {
          console.log(this);
          let {store} = this.props;
          console.log(store);
          store.dispatch(handlerSubtract());
      }
  
      render() {
  
          return (<button onClick={this.handlerClick}>-</button>);
      }
  
  }
  */
  ```




+ redux异步编程

  ```java
  /*
   注意
   	react-redux 使用方式一不能完成异步编程，需要使用中间件来进行调停(redux-thunk),因为完成异步编程主要是用到了dispatch，而方式一挂载到props上的redux状态，我们获取不到dispatch所以需要中间件(方式二可以获取)
   	此方式适用于方式一引入完成redux状态得props挂载，当react-redux代理函数调用此函数得到一个函数返回值后，引入的中间件就监听到了此函数，调用它并传入了 dispatch 来帮助完成异步调用
   安装
   	npm i redux-thunk
   作用
   	当 dispatch()接收的是一个函数时，此时该中间件会去调用该函数，并把dispatch作为第一个形参传递，此时就可以利用dispatch来完成异步编程等操作
   	在使用方式一传入状态时。我们无法获取和操作dispatch所以无法完成异步操作，而此中间件可以把react-redux内部封装的dispatch暴露出来，当作参数去调用我们的函数，来帮助我们完成异步操作
  
  工作流程
  	当调用了一个操作store全局状态的方法后，react-redux底层调用我们的actions函数(如果是一个对象则react-redux底层会直接dispatch({type:"xx",data:xx})进行调用)，并对其返回值做判断，如果是一个对象则其会继续进行store状态的更改操作(如 dispatch(actions()))，如果是一个函数，且使用了redux-thunk中间件，则此时该函数会被调用并把dispatch当作第一个参数传入,此时我们自定义的函数就完成了调用，我们可以按照需求完成异步操作
   	
    使用(thunk中间件完成异步编程)
    	// store
    	import {createStore,applyMiddleware} from "redux";
      import reducer from "./reducer";
      import thunk from "redux-thunk"; // 引入异步编程中间件
      export default createStore(reducer,applyMiddleware(thunk)); // 使用该中间件，此时就可以在action中返回一个函数，该函数会被此中间件调用并把 dispatch传入，完成异步操作(此方式适合方式一传参，符合设计原则(单一职责原则))
     
     // action
     export const asyncIncrement = () => ((dispatch) => {
      // 此方式适用于方式1引入，当代理函数调用此函数后，得到一个函数类型的返回值，此时会被 thunk中间件监听，调用此函数并传入了 dispatch，来帮助完成异步操作(如果返回的是一个对象则直接由react-redux完成操作)
      setTimeout(() => (dispatch({type: INCREMENT, data: 1})), 1000)
  	});
  
   
    使用 挂载方式2完成异步编程 (此方法不需要用到thunk中间件)
    import {connect} from "react-redux";
    import AsyncSubtract from "../component/AsyncSubtract";
    import {SUBTRACT} from "../typeConfig";
    
    export default connect(number => ({number}), dispatch => ({
        asynSubtract() {
        		// 方式2，我们可以拿到dispatch 因此无需用到thunk也能完成异步操作，不过不符合设计原则(单一职责原则)
                setTimeout(() => {
                    dispatch({type: SUBTRACT, data: 1})
                }, 1000);
        }
    }))(AsyncSubtract);
  
  */
  ```

  

+ 浏览器激活redux开发工具

  ```java
  /*
   前言
   	redux开发工具可以灵活的查看redux管理的状态，下载插件 Redux DevTool 和进行相关代码配置即可激活
   	
   下载包
   	npm i redux-devtools-extension
   
   使用
   	import {createStore,applyMiddleware} from "redux";
      import reducer from "./reducer";
      import thunk from "redux-thunk"; // 引入异步编程中间件
      import {composeWithDevTools} from "redux-devtools-extension"; // 引入开发工具启动装置
      // 进行使用composeWithDevTools()为开发工具调用，如果想使用中间件则在里面传入形参
      export default createStore(reducer,composeWithDevTools(applyMiddleware(thunk)));
  
  */
  ```

  

+ 使用redux管理多个reducer(管理多个状态)

  ```java
  /*
   作用
   	总和所有reducer的状态到store对象中
  
   使用
  =============================================
  	// index.js  用于汇总所有的reducer
   	import count from "./count";
      import personData from "./personData";
      import {combineReducers} from "redux"; // 用于总和多个reducer的状态
      export default combineReducers({ // 把reducer的所有状态暴露出去，最终由store引入
          count, personData
      });
  =============================================
  	// count.js  count状态的reducer,用于操作此状态的更改
  	
      import {SUBTRACT, INCREMENT} from "../typeConfig"
      export default function count(state = 0, action) {
          let {type, data} = action;
          let newState = null;
          switch (type) {
              case INCREMENT:
                  newState = state + data;
                  return newState;
              case SUBTRACT:
                  newState = state - data;
                  return newState;
              default:
                  return state;
          }
      }
  
  
  =============================================
  	// personData.js  personData状态的reducer,用于操作此状态的更改
      export default function personData(state = [], action) {
         return state;
      }
      
  =============================================
  	// store.js  用于管理所有的全局状态
      import {createStore,applyMiddleware} from "redux";
      import thunk from "redux-thunk"; // 引入异步编程中间件
      import {composeWithDevTools} from "redux-devtools-extension";
      import reducers from "./reducers";
      export default createStore(reducers,composeWithDevTools(applyMiddleware(thunk)));
  */
  ```

  





+ redux-toolkit使用

  ```java
  /*
   介绍
   	redux-toolkit 是一个redux的一个强大的辅助工具，帮助集成了redux繁琐的写法，是对redux的一个上层封装
   安装
   	npm i redux-toolkit
   注意
   	slice在编写为reducers后，其底层会创建对应的actions，因此我们可以直接导出actions和reducers,在我们使用 dispatch(actions)调用这些actions后，其会去调用对应的reducer
   	在reducer中，我们无法获取原state,而是获取的一个代理(该代理存储着原状态的映射)
   	当想对state的状态进行修改时，如果是改变该状态的指向就必须返回state(如 原状态为简单数据类型，当改变其后需要返回新的状态，如原状态指向一个引用数据类型时，如果想改变引用则必须返回新状态),否则不需要返回(往对象或数组里添加数据，因为此时是地址引用，所以不需要返回数据)
   	只有使用普通redux才需要区分 容器组件和普通组件(因为使用搞了react-redux 报下的connect方法进行组件包装了)，使用redux-toolkit不需要进行组件区分，因为其使用的是钩子函数获得或修改状态(按照路由规范区分组件)
   	
   常用api	
   	createSlice
   		用来指定一个切边，该切片包含，name,initialState,reducers字段，并总和了actions和reducers
   	configureStore
   		像从Redux中创建原始的createStore一样创建一个Redux store实例，但接受一个命名的选项对象并自动设	  置Redux DevTools扩展
   		
   基本使用
   	// store
   	import {configureStore} from "@reduxjs/toolkit";
      import counterSlice from "./counterSlice";
      export default configureStore({
          reducer:{
              counter:counterSlice,
          }
      });
      
   =====================================
      // slice  
  
      import {createSlice, createAsyncThunk} from "@reduxjs/toolkit";
  
      export const counter = createSlice({
          name: "counter",
          initialState: {value: 0},
          reducers: {
              increment(state) {
                  state.value += 1;
              },
              decrement(state) {
                  state.value -= 1;
              },
              asyncAdd(state, {payload}) {
                  state.value += payload;
              }
          },
  
  
      });
  
      export default counter.reducer;
      export const {increment, decrement, asyncAdd} = counter.actions;
      export const asyncIncrement = (payload: any):any => (
          (dispatch: any) => {
              console.log("async被调用");
              setTimeout(() => {
                  dispatch(asyncAdd(payload));
              }, 2000);
          }
      )
     ===================================
     // 使用状态
     // useDispatch钩子函数可以得到该store里的dsipatch方法，然后将slice中抽离出来的actions进行传入即可调用对应的reducer的方法改变状态
     // useSelector钩子函数可以得到当前store管理的全部状态
      import {useDispatch, useSelector} from "react-redux";
      import {increment, decrement,asyncIncrement} from "./counterSlice";
  
      export default () => {
          let count = useSelector((state: any) => {
              console.log(state);
              return state.counter.value;
          });
          let dispatch = useDispatch();
          return (
              <>
                  <h2>当前为：{count}</h2>
                  <button onClick={(): void => {
                      dispatch(increment())
                  }}>+1
                  </button>
                  <button onClick={(): void => {
                      dispatch(decrement())
                  }}>-1
                  </button>
                  <button onClick={(): void => {
                      console.log("异步+");
                      dispatch(asyncIncrement(Math.random()*5))
  
                  }}>异步+1
                  </button>
              </>
          );
      }
     
  */
  ```



+ react-toolkit 异步action

  ```java
  /*
  介绍
  	异步action使用 createAsyncThunk 函数创建，它的返回的是一个promise对象，在其调用后，根据返回值Promise状态不同调用对应extraReducers的Promise状态
  	
  注意
  	当外部使用 dispatch(loadMoviesAPI())时，就是调用的extraReducers里loadMoviesAPI对应的Promise状态
  	使用react-toolkit发送请求后，可以直接将发送请求部分写在 Slice文件里
  	异步action多用于封装含有promise的回调，如果你的返回值是一个promise就交给createAsyncThunk处理吧
  	
  使用
   	import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
      // createAsyncThunk创建一个异步的action，这个方法被触发的时候会有三个状态
      // pending(进行中) fulfilled(成功) rejected(失败)
  
      import { increment } from './counterSlice';
  
      // 发起网络请求获取数据
      const loadMoviesAPI = () =>
       fetch(
       'https://pcw-api.iqiyi.com/search/recommend/list?channel_id=1&data_type=1&mode=11&page_id=2&ret_num=48'
        ).then((res) => res.json());
  
      // 这个action是可以直接调用的，用来处理异步操作获取数据
      export const loadData = createAsyncThunk('movie/loadData', async () => {
       const res = await loadMoviesAPI();
       return res; // 此处的返回结果会在 .fulfilled中作为payload的值
      });
  
      export const movieSLice = createSlice({
       name: 'movie',
       initialState: {
       list: [],
       totals: 0,
        },
       reducers: {
       loadDataEnd(state, { payload }) {
       state.list = payload;
       state.totals = payload.length;
          },
        },
       // 可以额外的触发其他slice中的数据关联改变
       extraReducers: {
          [loadData.fulfilled](state, { payload }) {
           console.log(payload);
           state.list = payload.data.list;
          },
          [loadData.rejected](state, err) {
           console.log(err);
          },
          [loadData.pending](state) {
           console.log('进行中');
          },
        },
      });
      export const { loadDataEnd } = movieSLice.actions;
      export default movieSLice.reducer;
  
  */
  ```


+ 使用fetch异步action发请求（redux-toolkit）

  ```java
  
  // sendRes.tsx
  import {useSelector, useDispatch} from "react-redux";
  import {clear, fetchSend, SendReqInitState, ResponseLimit} from "../redux/sendReqSlice";
  import {Button, Card, Input, Row, Col, Spin} from "antd";
  import {LoadingOutlined} from '@ant-design/icons';
  
  export default () => {
  
      let resMsg: SendReqInitState = useSelector(({resMsg}: any) => resMsg);
      const dispatch = useDispatch();
      
      /**
       @description: 获取数据时响应
       */
      const getRes = () => {
          console.log("我来展示数据");
          console.log(resMsg.resArr);
          return (
              <Row justify={"start"}>
                  {
                      resMsg.resArr.map((data: ResponseLimit): any => (
                              <Col key={data.id} >
                                  <a href={data.url}>
                                      <img src={data.avatar}/>
                                      {data.name}
                                  </a>
                              </Col>
                          )
                      )
                  }
              </Row>
          );
      }
  
      /**
       @description: 加载时 响应
       */
      const lodingRes = () => {
          console.log(resMsg);
          const antIcon = <LoadingOutlined style={{fontSize: 24}} spin/>;
          return (
              <Spin indicator={antIcon}>加载中</Spin>
          );
      }
      
      const {isLoad, awaitSearch} = resMsg;
  
      /**
       @description: 获取搜索数据，并渲染
       */
      const sendSearchReq = (value: string) => {
          initSearch();
          dispatch(fetchSend(value));
      }
      /**
       @description: 初始化数据
       */
      const initSearch = () => {
          dispatch(clear());
      }
  
      return (
          <>
              <Card title={
                  <Input placeholder={"请输入搜索内容"} onPressEnter={(e: any) => {
                      sendSearchReq(e.target.value);
                  }}/>
              } extra={<Button type={"primary"}>查询</Button>}>
                  {awaitSearch ? <h2>请输入查询内容</h2> : (isLoad ? lodingRes() : getRes())}
              </Card>
          </>
      );
  }
  
  // ===========================
  // store
  import {configureStore} from "@reduxjs/toolkit";
  import todoReducer from "./todolistSlice";
  import asyncReducer from "./asyncModeSlice";
  import sendReqReducer from "./sendReqSlice";
  export default configureStore({
      reducer: {
          todo: todoReducer,
          async: asyncReducer,
          resMsg: sendReqReducer
      }
  })
  // ===========================
  // sendResSlice
  import {createSlice,createAsyncThunk} from "@reduxjs/toolkit";
  export interface ResponseLimit {
      name:string,
      url:string,
      avatar:string,
      id:number
  }
  export interface SendReqInitState {
      isLoad:boolean,
      awaitSearch:boolean,
      resArr:ResponseLimit[]
  }
  const initialState:SendReqInitState = {
      isLoad :false,
      resArr:[],
      awaitSearch:true
  };
  
  /**
   @description: 创建一个异步action,并向外暴露，当调用此异步函数后（Dispatch(fetchSend())），extraReducers中的对应promise状态就会调用
   */
  export const  fetchSend:any = createAsyncThunk("sendReq/fetchSend",async(payload:any)=>{
      const s: string = "stars";
      const url: string = `https://api.github.com/search/repositories?q=${payload}&sort=${s}`;
      return await fetch(url).then(res => res.json());
  })
  export const sendReq = createSlice({
      name:"sendReq",
      initialState,
      reducers:{
          clear(state){
              state.isLoad = true;
              state.awaitSearch = false;
              state.resArr = [];
          }
      },
      extraReducers:{
          [fetchSend.fulfilled](state:any,action:any){
              const {items} = action.payload;
              items.forEach((obj:any)=>{
                  let tmpArr:ResponseLimit = {
                      name:obj.name,
                      url: obj.html_url,
                      avatar: obj.owner.avatar_url,
                      id:obj.id
                  };
                  state.resArr.push(tmpArr);
              });
              state.isLoad= false;
          },
          [fetchSend.rejected](state:any,action:any){
              console.log("失败了");
          }
      }
  });
  export default sendReq.reducer;
  export const {clear} = sendReq.actions;
  ```



+ todolist(使用 redux-toolkit)

  ```java
  // store
  import {configureStore} from "@reduxjs/toolkit";
  import todoReducer from "./todolistSlice";
  import asyncReducer from "./asyncModeSlice";
  import sendReqReducer from "./sendReqSlice";
  export default configureStore({
      reducer: {
          todo: todoReducer,
          async: asyncReducer,
          resMsg: sendReqReducer
      }
  })
  
  // todolistSlice
  import {createSlice} from "@reduxjs/toolkit";
  
  export interface todoInitState {
      id: string,
      data: string,
      btnShow: boolean
  }
  
  const initialState: todoInitState[] = [];
  const todoSlice = createSlice({
      name: "todo",
      initialState,
      reducers: {
          removeTodo(state: todoInitState[], action: any) {
              let {payload} = action;
              state.forEach((todo: any, index: number) => {
                  console.log(todo);
                  if (todo.id === payload) {
                      state.splice(index, 1);
                  }
                  console.log(todo);
              })
              console.log(state);
          },
          addTodo(state: any, action: any) {
              let {payload} = action;
              state.push(payload);
          },
          btnDisplay(state, action) {
              let {payload} = action;
              state.forEach(todo => {
                  if (todo.id === payload) {
                      todo.btnShow = true;
                  }
              })
          },
          btnHidden(state, action) {
              let {payload} = action;
              state.forEach(todo => {
                  if (todo.id === payload) {
                      todo.btnShow = false;
                  }
              })
          }
      }
  });
  export const asyncAdd: any = (payload: any) => (dispatch: any) => {
      setTimeout(() => {
          dispatch(addTodo(payload));
      }, 1000);
  }
  export const {removeTodo, addTodo, btnDisplay, btnHidden} = todoSlice.actions;
  export default todoSlice.reducer;
  
  
  
  // asyncModeSlice
  import {createSlice} from "@reduxjs/toolkit";
  // 当state里的指向发生改变后，需要返回新的地址，在原有地址上添加数据则不会
  const initialState = false;
   const asyncSlice = createSlice({
      name: "isAsync",
      initialState,
      reducers: {
          changeStateToTrue(state) {
              console.log(state);
              state = true;
              return state;
          },
          changeStateToFalse(state){
              state = false;
              return state;
          }
      }
  })
  export const {changeStateToFalse,changeStateToTrue} = asyncSlice.actions;
  export default asyncSlice.reducer;
  
  // todolist
  import TodoShow from "./TodoShow";
  import TodoInput from "./TodoInput";
  import {Card, Switch} from "antd";
  import {useDispatch} from "react-redux";
  import {changeStateToFalse,changeStateToTrue} from "../../redux/asyncModeSlice";
  export default () => {
      let dispatch = useDispatch();
      const handlerSwitch = (flag:boolean)=>{
          if(flag){
              dispatch(changeStateToTrue());
          }else{
              dispatch(changeStateToFalse());
          }
      }
  
      return (
          <>
              <Card title={<TodoInput/>} bordered style={{width: 500, margin: "100px auto"}}
                    extra={
                        <Switch checkedChildren={"异步"} unCheckedChildren={"同步"}
                                onChange={handlerSwitch}/>
                    }>
                  <TodoShow/>
              </Card>
          </>
      );
  }
  
  // todoInput
  import { Input} from "antd";
  import {useDispatch,useSelector} from "react-redux";
  import {addTodo,todoInitState,asyncAdd} from "../../redux/todolistSlice";
  
  
  export default  ()=> {
          const dispatch = useDispatch();
          let isAsync = useSelector((state:any)=>state.async)
          console.log("async",isAsync);
  
          /**
            @description: 验证是否添加todo
          */
          const additionTodo = (e:any):void=>{
              let value = e.target.value;
              if(value.trim() === "") return ;
              let todo:todoInitState = {
                  id: Date.now()+"",
                  data:value,
                  btnShow:false
              };
              console.log("async",isAsync);
              if(isAsync){
                  dispatch(asyncAdd(todo));
              }else{
                  dispatch(addTodo(todo));
              }
              e.target.value = "";
          };
          return (
              <>
                  <Input onPressEnter={additionTodo} placeholder={"输入你要做的事情"}/>
              </>
          );
  }
  
  // todoShow
  import {List,Button} from "antd";
  import {useSelector, useDispatch} from "react-redux";
  import {todoInitState,removeTodo,btnDisplay,btnHidden} from "../../redux/todolistSlice";
  export default () => {
  
      let todoData:todoInitState[] = useSelector((state: any) => {
  
          return state.todo;
      })
      let dispatch = useDispatch();
  
      const {Item} = List;
      return (
          <>
              <List
                  bordered
                  dataSource={todoData}
                  renderItem={({id,data,btnShow}) => (
                      <Item key={id} onMouseOut={()=>{dispatch(btnHidden(id))}} onMouseOver={()=>{dispatch(btnDisplay(id))}}>
                          {data}
                          <Button type={"primary"} danger
                                  style={btnShow ? {float:"right", display:"block"}:{float:"right",display:"none"}}
                                  size={"small"} onClick={()=>{dispatch(removeTodo(id))}}>
  
                              删除</Button>
                      </Item>
                  )
                  }
              />
          </>
      );
  }
  ```





## 新特性

+ Fragment

  ```java
  /*
   介绍
   	在写jsx时，在虚拟dom的最外侧必须要使用一个标签进行包裹(以前为div，这样会导致无意义层级的包裹，因此可以使用Fragment或一个<></>进行包裹,此时就符合了jsx语法规范，且不会生成多余的包裹标签)
   注意
   	<></> 和 <Fragment></Fragment> 虽作用相似，但 <Fragment></Fragment> 可以接收一个key值，在遍历时如果需要使用包裹层级可以使用 Fragment包裹(效率高)
   语法
   	<Fragment>
   		<div></div>
   	</Fragment>
   	或
   	<>
   		<div></div>
   	</>
   使用
  import React, {Fragment} from "react";
  
  export default () => {
      
      return (
          <>
             <div>我是div</div>
          </>
      );
  }
  
  */
  ```

+ Context

  ```java
  /*
   介绍
   	Context和redux功能相似，可以集中管理状态，但Context通常适用于小项目的状态管理(如果状态躲起来了则必须创建更多的Context)
   使用场景
   	Context主要应用场景在于很多不同层级的组件需要访问同样一些的数据(或操作同一个状态)
   	在开发中一般不用context,通常使用context封装react插件
   参数分析
   	Context
   		为Provder，Consumer的对象，类组件可以通过指定Context的方式将共享信息挂载到该类的context属性下
   	Provider
   		用于包裹引入的组件，并传递需要共享的信息(通过属性value)
   	Consumer(类和函数组件都可以通过该方式获取数据)
   		作用与被包裹的组件，该组件会接收一个函数，该函数最终会被调用并且传入共享的信息value，其函数的返回值会被渲染到Consumer组件所在的位置
   		
   注意
   	每个Context只能管理一种状态
   	context主要是做一个数据共享，将父组件的数据传递给其包裹的组件
   	当使用函数组件时，因为其的状态的返回值是状态和操作状态的函数，因此该值也可以通过context传递，当其包裹组件使用了该状态后，当调用了操作该状态的方法，引用了该状态的组件都会进行页面重写(组件更新调用)
   
   使用Context数据的两种方式
   	声明该Provider提供的数据来自哪个Context对象(只适用于类组件)
   	在Provider包裹的组件中适用 Comsumer 使用传递的状态(类组件和函数组件都可使用)
   		如 
   			<Consumer>
                          {
                              value => { // 该函数的由Provider调用，并把管理的数据传入
                              
                              }
                          }
                </Consumer>
   
   使用(使用 声明context来自的Context对象获取数据)
   	// context.jsx ,创建context状态管理器
   	import {createContext} from "react"; 
      const MyContext = createContext({});
      export const {Provider, Consumer} = MyContext;
      export default MyContext;
  	
  	// A,jsx
  	import React, {Component} from "react";
      import MyContext,{Provider} from "./Context";
      import B from "./B";
      export default class A extends Component {
          public static contextType = MyContext;
          constructor(props:any) {
              super(props);
              this.state = {name :1};
          }
          render() {
              console.log(this);
              return (
                  <>
                      <h1>我是A组件</h1>
                      <Provider value={this.state}> 
                          <B/>
                      </Provider>
                  </>
              );
          }
      }
      
      // b.tsx
      import React, {Component} from "react";
      import MyContext from "./Context";
      import C from "./C";
  
      export default class B extends Component {
  
          public static contextType: any = MyContext;
  
          render() {
              console.log(this);
              const {name}:any = this.context;
              return (
                  <>
                      <h1>我是b组件，
                          {name}</h1>
                      <C/>
                  </>
              );
          }
      }
      
      // c.tsx
      import React, {Component} from "react";
      import MyContext from "./Context";
  
      export default class C extends Component {
          public static contextType = MyContext;
  
          render() {
              const {name}: any = this.context;
              console.log(this);
              return (
                  <>
                      <h1>
  
                          我是c,{name}
                      </h1>
                  </>
              );
          }
      }
   
   使用(使用 Consumer完成Provider提供的数据使用)
   // context.tsx
   import {createContext} from "react";
  
  const MyContext = createContext({});
  export const {Provider, Consumer} = MyContext;
  export default MyContext;
  
   // Fa.tsx
   import React,{useState} from "react";
  import {Provider} from "../Context";
  import Fb from "./Fb";
  export default () => {
      const [count,setCount] = useState(0);
      return (
          <>
              <h1>我是 Fa </h1>
              <Provider value={{count,setCount}}>
                  <Fb/>
              </Provider>
              <h1>我是a页面，当前计数为{count}</h1>
          </>
      );
  }
  
  // fb.tsx
  import React from "react";
  import Fc from "./Fc";
  import {Consumer} from "../Context";
  export default () => {
      return (
          <>
              <h1>Fb页面</h1>
              <Consumer>
                  {
                      (value:any) =>{
                          console.log(value);
                          return <h1>我是b页面，当前计数为 {value.count}</h1>
                      }
                  }
              </Consumer>
              <Fc/>
          </>
      );
  }
  
  // fc.tsx
  import React from "react";
  import {Consumer} from "../Context";
  
  export default () => {
      return (
          <>
              <h1>Fc页面</h1>
              <Consumer>
                  {
                      ({count, setCount}: any) => {
  
                          return (
                              <>
                                  <h1>{count}</h1>
                                  <button onClick={() => {
                                      setCount(count + 1)
                                  }}> 我是c的按钮我来操作a的状态
                                  </button>
                              </>
                          )
                      }
                  }
              </Consumer>
          </>
      );
  }
  */
  ```




+ 路由懒加载

  ```java
  /*
   介绍
   	可以提高用户体验，实现路由按需加载，避免用户依次加载过多没有看到的内容，导致页面显示缓慢
   遇到的问题
   	在使用antd Menu组件+react 懒加载时，导致的每次加载Menu组件会初始化展开
   语法
   	import {lazy,Suspense} from "react"; // 引入懒加载需要的组件类和函数
   	const NotFound = lazy(()=>import("./NotFound")); // 此时使用NotFound路由时，就会触发懒加载，即只有跳转到该路由时，这个路由才会加载
   	 	  <Suspense  fallback={}> // 在懒加载引入文件时，fallback的内容就会调用，充当过渡效果
                  {element}
              </Suspense>
   使用
   	// 路由配置
   	import {lazy} from "react";
      import {Navigate} from "react-router-dom";
      import projectRouters from "./projectRouters";
      import testRouters from "./testRouters";
  
      // 懒加载引入 ，随用随更新，增加用户体验
      const NotFound = lazy(()=>import("./NotFound"));
      export default [
          ... projectRouters,
          ...testRouters,
          {
              path: "/",
              element: <Navigate to={"/login"}/>
          },
          {
              path: "*",
              element: <NotFound/>
          }
      ]
      
      //App.tsx
      import React,{Suspense} from 'react';
      import {useRoutes} from "react-router-dom";
      import router from "./router";
      import "antd/dist/antd.less";
      function App() {
          let element = useRoutes(router);
          return (
              <div className="App"> 
              	// 懒加载路由组件时，Suspense组件的fallback内容就会加载充当过渡效果
                  <Suspense fallback={<h1>加载中...</h1>}>
                      {element}
                  </Suspense>
              </div>
          );
      }
  
      export default App;
  
  
  
   	
   注意
   	懒加载可能导致屏闪现象，需要处理
   	基本上只有路由需要懒加载
  */
  ```

  



+ renderProps属性

  ```java
  /*
   前言
   	一个组件的props属性可以接收任何数据，包括组件实例对象，因为这个特性，我们可以将一个组件或数据以props传递，然后另一个组件使用props下的key即可获取想要的属性(包括组件)
   介绍
   	我们可以通过props参数传递方式传递组件，通常该组件被函数包裹，此时另一个组件可以调用这个函数传入需要挂载到该组件实例对象上的数据，即可完成数据挂载操作
   注意
   	此方式通常使用函数进行包裹，方便传递props参数
   使用
   	export default class A extends PureComponent {
          constructor(props: any) {
              super(props);
              this.state = {
                  count: 1
              }
          }
  
          render(): React.ReactNode {
              return (
                  <>
                      <h1>我是A组件</h1>
                      <B count={12} render={(name:any)=> (<C name={name} />)}/>
                  </>
              );
          }
      }
      interface Props {
          count ?:number;
          render ?:any;
      }
      interface State {
  
      }
      class B extends PureComponent<Props,State> {
          render(): React.ReactNode {
              console.log(this.props);
              return (
                 <>
                  <h1>我是B组件</h1>
                     {this.props.render("张三")}
                 </>
              );
          }
      }
  
      interface CState {
  
      }
      interface CProps {
          name ?:string;
      }
      class C extends PureComponent<CProps,CState> {
          render(): React.ReactNode {
              return (
                  <>
                      <h1>我是C组件</h1>
                      {<h2>my name is {this.props.name}</h2>}
                  </>
              );
          }
      }
  */
  ```
  
  

### 类组件

+ setState()

  ```java
  /*
   介绍
   	setState是类组件改变当前组件状态的方法(修改后类组件重掉更新生命周期函数)
   语法
   	setState({}[,()=>{}]); // 对象+回调函数写法(此掉函数在setState改变完状态后被调用)
   	setState{(state,props)=>{}} // 函数写法，底层在调用时，会传入两个形参，第一个为state,第二个为props 此时来简化我们的操作
   	
   使用
   	import React, {Component} from "react";
  
      export default class CurNewFeature extends Component {
          constructor(props: any) {
              super(props);
              this.state = {
                  count: 0
              };
          }
  
          updateNumber = () => {
              // setState 是异步任务，会进入微任务队列执行
              let {count}: any = this.state;
              // 对象式写法
              this.setState({
                  count: count + 1
              }, () => {
                  console.log("我在setState改变状态后执行");
              })
              // 函数式写法
              this.setState((state: any, props: any) => {
                  console.log(state, props);
  
                  return {count: state.count + 1};
              });
          }
  
          render() {
              let {count}: any = this.state;
              return (
                  <>
                      <h1>当前数字为{count}</h1>
                      <button onClick={this.updateNumber}>+1</button>
                  </>
              );
          }
      }
   
   注意
   	对象写法的state是函数式state的简写方式
   	setState属于微任务，在执行时，只有当前作用域的所有简单任务执行完毕后才会开始执行
  */
  ```




+ 组件优化(PureComponent)

  ```java
  /*
   引入
   	Component类组件在使用setState改变状态后，会掉用一次更新声明周期函数，且render引用的其他组件实例对象即使没发生修改也会跟着更新一次，这是不合理的，因此需要进行优化(即只要我不改变组件渲染的内容时，就不需要重新调用其他组件)
   解决方法
   	（一旦结构复杂我们重写就非常麻烦）因为底层的shouldComponentUpdate总是为true,因此会产生此问题(每次调用setState是无限制放行，导致组件无意义更新)，我们可以重写shouldComponentUpdate来完成是否更新的判断，来达到优化目的(解决页面无更新时恶意更新状态导致效率降低)
   	（常用）使用 PureComponent组件，其重写了shouldComponentUpdate的判断逻辑，帮助我们解决了setState更新状态时的shouldComponentUpdate声明周期函数的控制(只有前后状态或属性不一致时，才进行更新放行，否则不更新)
   	
  
  
  使用(PureComponent)
  	export class AVC extends PureComponent {
          constructor(props: any) {
              super(props);
              this.state = {
                  name: "张三",
                  feature: {
                      height: 1.78,
                      weight: 45
                  },
                  like: "play"
              }
          }
  
          public handlerClick = () => {
              let {name,like}: any = this.state;
              name = "李四";
              like = 12;
              this.setState({name,like});
          }
  
          render() {
              console.log("A is rendered");
              let {name, feature: {height, weight},like}: any = this.state;
              return (
                  <>
                      <h1>当前状态的信息是 ：姓名:{name} , 身高{height},体重{weight},爱好：{like}</h1>
                      <button onClick={this.handlerClick}>更新</button>
                      <B/>
                  </>
              );
          }
      }
  
      class B extends PureComponent {
          render(): React.ReactNode {
              console.log("B is rendered");
              return (
                  <>
                      <h2>我是b组件</h2>
                  </>
              );
          }
      }
  
  
   注意
   	当父组件更新后，其包裹的组件(无论多少层)也会更新，因此我们需要重写shouldComponentUpdate以监听更新的状态变化，只有状态或数据发生改变时我们才更新
  	PureComponent 重写的shouldComponentUpdate() 会先比较传入的地址与该组件状态的地址是否相同(如果相同认为是同一个状态，不予放行)，之后传入的新状态会和该组件的原状态去做一个状态值的判断(如果相同认为是同一个状态，不予放行)，因此在更新状态时，需传入一个独立的对象空间且该对象的属性值需和组件的状态值不相同
  	在使用PureComponent时，因此其重写的shouldComponentUpdate() 特性，我们想改变一个复杂数据类型时，需要进行深拷贝然后再对值进行修改(PureComponent内部的shouldComponentUpdate()只有满足地址不同且状态不同时，才会进行放行，进而产生状态改变，页面重绘)
  	深拷贝方式:如果知道数据的结构可以使用 ... 展开每一层复杂数据类型的结构然后放到一个新地址中，完成深拷贝，如果不知道其结构可以使用JSON.parse(JSON.stringify()) 完成深拷贝  
  	js中使用 [] 就相当于 new Array() 创建了一个新数组，使用 {} 就相当于 new Object() 创建了一个新对象(开辟了一个独立的空间)
  */
  ```

  

+ 错误边界

  ```java
  /*
   介绍
   	错误边界可以将错误限制再一个组件范围内，这样不会因为此组件的失误而导致页面全部无法显示情况(再没有使用错误边界前，只要一个组件错误就导致全部无法加载)
   语法
   	再可能出错的组件的父组件中使用下列写法，即可捕获其子组件的错误，将其维护到该组件的状态中即可捕获该错误，并做错误处理(类似于try-catch)
   		static getDerivedStateFromError(error){ } // 和getDerivedStateFromProps一样需要返回一个状态到state中
   		componentDidCatch(){} // 此声明周期函数可以查看子组件抛出的错误，多用于错误计算，并转发给后端
  
  使用
  	export default class A extends PureComponent {
          state = {
              isError: ""
          }
  
          // 用于捕获错误并维护到状态中
          static getDerivedStateFromError(error: any) {
  
              return {isError: error};
          }
  
          // 当发生错误时调用的此生命周期函数
          componentDidCatch(error: Error, errorInfo: React.ErrorInfo): void {
              console.log(error);
              console.log(errorInfo);
          }
  
          constructor(props: any) {
              super(props);
          }
  
          render(): React.ReactNode {
  
              return (
                  <>
                      <h1>我是A组件</h1>
                      {this.state.isError ? <h1>网络不稳定，稍后再试</h1> :
  
                          <B count={12} render={(name: any) => (<C name={name}/>)}/>}
  
                  </>
              );
          }
      }
  
      interface Props {
          count?: number;
          render?: any;
      }
  
      interface State {
  
      }
  
      class B extends PureComponent<Props, State> {
          render(): React.ReactNode {
              console.log(this.props);
              return (
                  <>
                      <h1>我是B组件</h1>
                      {this.props.render("张三")}
                  </>
              );
          }
      }
  
      interface CState {
  
      }
  
      interface CProps {
          name?: string;
      }
  
      class C extends PureComponent<CProps, CState> {
          constructor(props:any) {
              super(props);
              this.state = {a: 1};
          }
          render(): React.ReactNode {
              // @ts-ignore
              let {a} = this.state;
              return (
                  <>
                      <h1>我是C组件</h1>
                      {a.map((item:any)=>{
                          console.log(item);
                      })}
                      {<h2>my name is {this.props.name}</h2>}
                  </>
              );
          }
      }
  
  注意
   	错误边界再生产环境中可以完全使用(即组件的错误不会影响全局的错误)，而再开发环境中只能短暂生效(因为要告诉程序员错误所在点)
   	错误边界多用于处理用户界面显示问题，当错误达到一定次数上线时，即可告知程序员解决bug(说明确实有错误，而不是网络波动导致的加载异常)
   	错误边界现阶段只适用于类组件，且只能捕获其子组件产生的错误
  */
  ```
  
  

### 钩子函数

+ 特别注意

  ```java
  /*
  1.useXxx()开头的都是钩子函数，只有自定义函数可以使用
  2. 使用 useState() 钩子函数后，每次setXxx()后该函数组件就会重新渲染一次(内部全部执行一遍)
  */
  ```

  

+ 介绍

  ```java
  /*
   Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性
   
  */
  ```

+ useState

  ```java
  /*
    介绍
    	  useState是16.8版本新增的一个钩子函数,它可以再函数组件中操作状态来触发函数的重新调用(重新渲染页面)
    语法
    	 let [state,setState] = useState("状态值") // 该钩子函数返回的是一个数组，其第一个位置上的是当前的组件的状态，第二个位置上的是改变状态的函数，当此函数被调用后 此组件(一般是函数)会被重新调用 
    	setState(state+1); // 更新状态的第一种写法
    	setState((state)=>{}) // 更新状态的第二种写法(函数写法)，底层会调用该函数并且把该setState对于的状态传入
    	
    使用
    	import React, {useState} from "react";
      import {Card, Input, Form} from "antd";
  
      export default () => {
      	// useState({count:1}) 传入的是一个状态值，该函数返回的是一个数组
          let [todo, setTodo] = useState({count:1}) ;
          const handlerClick = ()=>{
              console.log(this);
               todo = JSON.parse(JSON.stringify(todo)); // 如果状态是复杂数据类型则需要深拷贝
              todo.count++;
              setTodo(todo);
          }
          return (
              <div>
                  <Card title="todolist">
                      <div onClick={handlerClick}>
                          今天天气很 {todo.count}
                      </div>
                  </Card>
              </div>
          );
      }
    注意
    	使用useState创建的状态如果是复杂数据类型则需要进行深拷贝才能进行页面局部刷新(底层认为地址没有改变则没有变化)
    	setState可以传入任何参数，但一般传入的是原状态改变后的值，如果传入其他类型的值，可能导致引用错误(如原始状态是一个对象，且做了获取属性 xx.a的操作，但改变为了一个数组后，重新调用该函数，因无法从数组上获得属性而报错)
    	useState钩子函数多用于函数组件，此时其可以通过时间调用来调用来更改state，以达到重绘页面
    	此钩子函数的渲染只有在事件触发时才会执行(无生命周期函数驱动)
    	在useState("x") 接收了状态后，会做一个判断，如果当前useState钩子函数没有管理状态则会返回一个该状态的初始值及操作状态的方法，当管理了状态则直接返回管理的数据，每次改变状态重绘时(重新调用)，因此每次都会做这个判断(使用了享元模式)
  */
  ```

+ useEffect

  ```java
  /*
   介绍
   	useEffect钩子让函数拥有了 类似 componentDidMount,componentDidUpdate,componentUnmount 生命周期函数的功能 ，在这些阶段该钩子会被调用
   
   useEffect钩子函数调用时机
   	在函数组件首次渲染时
   	在指定数据更新时(第二个参数里的内容，如果没有指定，默认为监听此函数的更新，指定了就按指定的来)
   	在此函数组件被卸载时(调用第一个参数的返回值)
   	
   语法
   	useEffect(()=>{ "需要执行的代码"}{,[]}); // {,[]}为监听的目标，只有这些改变了才会继续执行此副作用
  
   常用搭配
   
   	// 此副作用函数只在组件初次加载和现在时执行
   	useEffect(
   		// 此函数在组件初次加载和指定组件更新时执行(可以在第二个参数传入一个空数组，表示不监听任何变化，此时只会在初次加载执行相当于 componentDidMount();如果没有传入第二个参数表示监听此函数组件的更新，只要此函数的状态一更新就会执行；如果传入指定目标到数组中，只要这些值一更新此函数就会执行)
   		()=>{    
              console.log("componentDidMount");
  
              // 在return的函数在函数组件卸载时执行
              return ()=>{
                  console.log("componentWillUnmount"); 
              }
   	},[]) // 监听列表为空，也就是说监听更新为空，此 effect钩子函数只在 初次加载和即将卸载时执行
  
   注意
   	useEffect在第二个参数没有传入参数时，在此函数组件被初次挂在和组件更新时调用
   	useEffect在第二个参数传入指定参数后(是一个数组，在这个数组内可添加监听目标，只有这些目标更新了，effect函数就会执行)，即 在此函数组件初次被宰和指定参数更新后调用
   	useEffect在第二个参数传入一个空数组后，表示不监听任何更改目标，仅在函数组件首次加载后调用一次
   	一个函数组件可以使用多个 useEffect(副作用)钩子函数
   	useEffect函数中的第一个参数可以指定一个函数返回值，当这个函数组件被卸载时，该函数的回调就会执行(相当于componentWillUnmount)
   	一个 useEffect可以同时拥有类似 componentDidMount,componentDidUpdate,componentUnmount 的状态，我们可以对其进行配置，来完成我们需要的状态
   	
   使用
   	import {useEffect,useState} from "react";
      export default () => {
          let [count,setCount] = useState(0);
          
          // 默认在 函数组件挂载时，函数组件更新时，函数组件卸载时执行，如果传入第二个参数，且是一个数组是只在 函数组件挂载时，函数组件卸载时 执行
          useEffect(()=>{  // 当此组件初次更新时会调用和指定监听目标更新时会调用(如果没有指定则组件更新就会调用)
              console.log("我来了");
              return ()=>{ // 当此组件被卸载会调用此回调
              	console.log("此组件被卸载了");
              }
          },[])
  
          return (
              <>
                  <h2>hookTest</h2>
                  <h3 onClick={()=>{setCount(count++)}}>{count}</h3>
              </>
          );
      }
  */
  ```



+ useRef

  ```java
  /*
   介绍
   	和类中的ref作用基本一致，收集 虚拟dom元素到一个容器中
   语法
      let nodeRef = useRef(null);
      <input type="text" ref={inputRef}/> // 此时这个input虚拟dom标签就被收集到了nodeRef容器的current属性下
    注意
    	此写法，和 React.createRef() 作用相同，一次只能存储一个虚拟dom标签
  */
  ```

+ useState,useEffect,useRef综合使用

  ```java
  /*
   import React, {Component, useState, useEffect, useRef} from "react";
  
  export default () => {
  	// 维护了一个状态，一个函数组件中可以有多个状态，但setState只能操作对于的state状态，当state状态更新时，该组件会重新渲染并进行diff对比
      let [opeObj, setObj] = useState({name: "张三", age: 18});
      let [count, setCount] = useState(0); // 维护了一个状态
  
      let inputRef = useRef(null); // 创建了一个ref容器
      const changeUser = () => {
          console.log(inputRef);
  
          // @ts-ignore
          const value: string = inputRef.current && inputRef.current.value;
          const celarSpaceValue = value.trim();
          const newName = celarSpaceValue.substring(0, celarSpaceValue.indexOf(" "));
          const newAge = celarSpaceValue.substring(celarSpaceValue.lastIndexOf(" "), celarSpaceValue.length);
          console.log(newName, newAge);
          // @ts-ignore
          setObj((state: any) => ({
              age: newAge,
              name: newName
          }));
      }
      // 使用副作用钩子函数，此钩子函数在该组件初次加载和卸载时执行
      useEffect(() => {
          console.log("此函数被调用了");
          return () => {
              console.log("此函数被卸载了");
          }
      }, [])
  
      const incremenrtCount = () => {
          setCount(count + 1);
      }
      return (
          <>
              <h1>
                  当前的用户是{opeObj.name},
              </h1>
              <h1>
                  他的年龄是{opeObj.age}
              </h1>
              <div>
                  <input type="text"
                         placeholder={"此处添加更改的内容，格式如 张三 18"}
                         ref={inputRef}
                  />
              </div>
  
              <p>
                  <span>当前数量为{count}</span>
                  <button onClick={incremenrtCount}>点击增加数字</button>
  
                  <button onClick={changeUser}>改变姓名和年龄(随机)</button>
              </p>
          </>
      );
  }
  */
  ```

  




## 常用库

+ Nprogress

  ```java
  /*
   作用
   	可以以一种伪进度调的形式过渡浏览器请求效果
   安装
   	npm i nprogress
   使用
   	import axios from "axios";
      import Qs from "qs";
      import NProgress from "nprogress"; // 引入进度条包
      import "nprogress/nprogress.css"; // 引入进度条样式  ,两个包需同时引入
  
      // 请求拦截器
      axios.interceptors.request.use((config)=>{
          let {method,data} = config;
          NProgress.start(); // 开启伪进度条
          if(method?.toLocaleUpperCase() === "POST"){
              config.data = Qs.stringify(data);
          }
          return config;
      })
      // 响应拦截器
      axios.interceptors.response.use((req) => {
          NProgress.done();
          return req;
      }, (err) => {
          // 后序再来改变state状态表示网络错误
          NProgress.done();
          console.log(err);
          alert("网络错误");
      })
      export default axios;
  */
  ```


+ **[ crypto-js](https://github.com/brix/crypto-js)**

  ```java
  /*
   作用
   	用于数据加密，还有 md5,sha1等
   使用
   	var CryptoJS = require("crypto-js"); // 引入包
  
      // 加密
      var ciphertext = CryptoJS.AES.encrypt('my message', 'secret key 123').toString();
  
      // 解密
      var bytes  = CryptoJS.AES.decrypt(ciphertext, 'secret key 123');
      var originalText = bytes.toString(CryptoJS.enc.Utf8);
  
      console.log(originalText); // 'my message'
  */
  ```


+ dayJS

  ```java
  /*
   介绍
   	可以获取当前时间
   下载
   	npm i dayjs
   使用
   	dayjs().format('YYYY MM-DD HH:mm:ss ')// 获取当前年月日 时间
   注意
   	YYYY获取的为当前年份
   	MM 获取的是当前月
   	DD 获取的是当前日期
   	HH 获取的是当前小时
   	mm 获取的是当前分钟
   	ss 获取的是当前秒钟
  */
  ```

  

+ echarts

  ```java
  /*
   介绍
   	前端的一个画图库。可以快速搭建一个柔美的图表
   下载
   	npm i echarts 
   	npm i echarts-for-react // react中使用echarts
   具体使用可查阅文档
  */
  ```

  

## 前言

IDE 集成开发环境 WebStorm，将所有需要的环境都配置好了

Visual Studio Code: 渐进式开发工具，只支持一个壳，额外功能需安装插件

React文件引入原则，含有js,jsx后缀的可只写文件名，引入Index文件可只引入文件夹，react内部会查找index进行引入

在return里只能写语句不能做赋值操作，所以赋值操作要在render里return外进行，然后再把值写在return里

+ vscode使用

  ```js
  /*
   ctrl+shift+p打开控制面板
   键盘快捷方式可以设置快捷键
   用户代码片段可以自定义代码($1标识让光标停留在此处)
   alt+左回退到上一步 alt+右回退到下一步
   ctrl+p 快速打开文件
   ctrl+g 快速切换行
  */
  ```
  
  

声明式：不亲自操作DOM,只声明数据回来去哪里

命令式：亲自操作DOM

模块话： js

组件化：页面上一个功能点所有代码资源集合

+ React特点

  Declarative(声明式编程)

  Component-based(组件化编码)

  Learn Once,write Anywhere(支持客户端与服务端渲染)

  **高效（DOMdiff算法，编码人员不需操作DOM），React会在内存中虚拟开辟一个DOM当操作全部完成后，一次性返回结果，极大减少了页面重绘重排**

  单向数据流

  

  

## 1.0 react 入门

+ React基本使用

  ```js
  /*
  react 资源 
  <script src="https://cdn.bootcdn.net/ajax/libs/react/18.1.0-next-aa05e7315-20220331/umd/react.development.js"></script>
  
  再开始编写React代码时需要引入三个库 react源生库，reactDOM库，babel翻译库
  	<script type="text/babel"> // 使用react写下的是jax语法，必须用babel进行翻译，type类型不能丢
      let vDOM = <h1>Hello World</h1> //创建虚拟dom,写下的内容不能用引号包裹，否则按字符串处理
      ReactDOM.render(vDOM,document.querySelector('#test')) //借助react把dom渲染到页面上
      </script>
      
  */
  ```

+ 使用js创建虚拟dom

  ```js
  /*
  注意
  	<script type="text/javascript"></script> 比 <script type="text/babel"></script>查询优先级高，且<script type="text/javascript"></script> 先执行
  
   1.使用js语法创建虚拟DOM
     let myID = 'atzhangsan'
      let myData = 'ewZHANGSAN'
      //创建一个虚拟dom标签，该api接收三个参数，1 为标签名，2 为 该标签的属性 ， 3 为 内容
      let vDOM = React.createElement('h2',{class:myID.toLowerCase()},myData.toLowerCase())
      // 渲染虚拟DOM ，render 接收两个形参 ， 1 为插入的内容 ， 2 为插入的dom标签
      ReactDOM.render(vDOM,document.querySelector('#test1')) // 元素创建太繁琐，若有多层嵌套代码冗杂
   
   
   2.使用Jsx语法创建虚拟DOM(创建的元素只能有一个根包裹标签)
    //使用React创建的虚拟DOM更加直观，javascript内部引用参数需用{}括起来，整体可以用()包起来，提示为一个语句。再引用class需用className方式(因为js已经有class类概念)
     <script type="text/babel">
      let vDOM1 = (
        <div>
          <h3 id={myID.toUpperCase()}>
            <span className="red">
              {myData.toUpperCase()}
            </span>
          </h3>
          <h1>哈喽</h1>
        </div>
      )
      ReactDOM.render(vDOM1, document.querySelector('#test2')) 
    </script> 
    
   3.虚拟DOM本质
    是js种的一般Object对象，虚拟DOM比较小和真实DOM相比少了很多API
   */
  ```

+ jsx语法

  ```js
  /*
  1.常规概念
   全称 javascriptXML, react定义的一种类似于XML的JS扩展语法：XML+JS,本质是React.createElement(component,props,...childrens)方法的语法糖，用来简化创建react虚拟DOM元素对象。
   标签名任意：HTML标签或其他标签
   标签属性任意：HTML标签属性或其他
   
  2. 基本语法规则
   （1）遇到<开头的代码，以标签的语法解析：html同名标签转换为html同名元素，其他标签需特别解析
   （2）遇到{开头的代码，以js语法解析：标签中的js表达式必须用{}包含
   （3）babel.js作用，浏览器不能直接解析jsx代码，需要借助babel转义为纯js代码才能执行，只要用了jsx，都要加上type="text/babel",声明需要babel处理
   （4）js语句和js表达式
   	表达式：一个表达式会产生一个值，它可以放任何需要一个值的地方，如3+b,arr.forEach()都有返回值，即所有可以用变量接收的语句都可以算表达式
    (5)只有(<>)里包裹的元素才需要加{}(把改元素包装称react对象)再分解
  */
  ```



+ 模块与组件

  ```js
  /*
   模块
   	向外提供特定功能的js程序，一般是js文件，作用：复用js,简化js编写
   组件
   	用来实现特定界面功能效果的代码和资源的集合(html/css/js/image),作用复用编码，简化项目编码，提高运行效率
   模块话
   	当应用js都以模块编码写的，这个应用就是模块化
   组件化
   	当应用以多组件的方式实现，这个应用就是一个组件化应用
  */
  ```

  

+ 组件使用

  ```js
  /*
  1. 组件创建有 工厂函数，类创建两种方法，都需要用<Conponent/>方式引入，它们的命名都需要首字母大写。在类中主要是使用类里面的render方法去得到渲染返回结果
  2.组件嵌套返回，直接再后面接其他组件的<Conponent/>即可，需要注意的是要添加一层外包裹元素
  3. 在</>中想写number和js表达式都需要用{}包裹 
  4. 简单组件无状态，复杂组件有状态
  5. 工厂函数的this指向undefined。类组件中，Render方法里的this指向组件类
  6. 表单元素也要以<input type="text"/> 结尾
  7. 以</>方式引用的类组件本质是在操作React.Component里的全局类属性，所以this指向一致，指向同一块类组件空间，使用自己的类组件的construcotr创建(new 对象方式)的不互通，指向new出来的新空间，且rop,ref,state属性不互通。
  8. 在类组件中，以<Con/>方式引用的类，自己后来额外添加的函数this指向为undefined(不包括继承函数重写),继承的内部函数和自己的constructor&&render函数this指向同一块内存，以new Con新增的类this指向自己独有的内存
  */
  
   <script type="text/babel">
      // 使用工厂函数定义组件，工厂函数必须大写开头
      //  jsx标签必须闭合(即再表便末尾谢上 /)
      // 嵌套其他工厂函数直接 < MyHa /> 引入即可
      function MyComponent() {
        return (
          <div>
            <h1>hello</h1>
            <MyHa />
          </div>
        )
      }
      function MyHa() {
        return <h1>哈哈</h1>
      }
      /*
       如果渲染标签时，首字母是小写的，系统会尝试着寻找html标签池中与之对应的标签渲染，未找到报错
       如果渲染标签时，首字母是大写的，尝试着寻找组件是否定义，定义则渲染
       */
      /*</>做了两件事,寻找MyComponent函数是否定义，若定义了,判断其是否为工厂函数，若是，则返回并渲染*/
      ReactDOM.render(<MyComponent />, document.querySelector('#example1'))
      console.log(new MyComponent())
      // 使用es6类组件
      class MyComponent2 extends React.Component {
        render() {
          return <h1>我是es6类组件(复杂的)</h1>
        }
      }
      console.log(new MyComponent2('zs'))
      // 使用类创建组件主要是借助该类里的render方法，需写上继承对象 React.Component
      ReactDOM.render(<MyComponent2 />, document.querySelector('#example2'))
    </script>
  ```

+ 组件三大属性

  ```js
  /*
   1.state
   (1)state是组件实例对象(只有class有权限才做state)最重要的属性，值是对象（可以包含多个key-value）
   (2)组件被称为 状态机，通过更新组件的state来更新对应页面显示
   (3)React支持再函数行内添加事件(和外部添加事件效果一致)
   (4)再React定义的类中，只有constructor，全局定义类变量和render里的this指向实例对象，其余的this指向undefnied， 想让其他函数this指向实例对象使用bind()函数
  (5) state不能直接修改和更新，需要使用this.setState()来设置状态
  （6）当状态改变时，才会重新渲染页面，不能再render里直接操作状态
  
   class Weather extends React.Component {
        constructor(a) {
          super(a)
          this.state = { isHot: false }
          this.demo = this.demo.bind(this) //改变this.dome函数this的指向
        }
        // demo函数必须写在class里，这样才能实时更改状态
        demo() {
          let { isHot } = this.state
          console.log(this)
          this.setState({ isHot: !isHot }) // 设置或更新状态
        }
        run() {
          console.log(this)
        }
        render() {
          return <h1 id="e" onClick={this.demo}>今天天气太{this.state.isHot ? '热' : '冷'}了</h1>
        }
      }
      ReactDOM.render(<Weather />, document.querySelector('#test1'))
   只要状态发生更改，render就会渲染1+n
    
  
  2.prop
  prop可以动态保存传入的参数(num,str,function,obj...)到一个该类的实例对象属性里
  	传参方式必须为 ReactDOM.render(<Person name="z3" age={18} sex="male" />, document.querySelector('#test1'))，其中 ame="z3" age={18} sex="male"是传递给prop类的参数，以键值对的形式存储。也可以在外部定义一个对象，把这些属性以键值对形式写入然后以 {...obj}方式进行写入到prop <Person {...obj} />，{}在react中是js语法的展开符，其内部配合babel帮我们进行了对象展开(仅在</>中有效)，在常规js语法中要使用{...obj}的方式展开对象，是es8新语法可以对一个对象进行展开
  	
  设置必须传入参数及默认传入参数
   // 限制传参以及设置默认值 15.5开始已弃用
   15.5前需在PropTypes前加React, React.PropTypes.string.isRequired,
   15.5后只需写 PropTypes.string.isRequired
  
    // 使用react想页面插入数据
      class PropsTest extends React.Component {
  
          constructor() {
              super();
              console.log(this);
              this.state = {isHot: false};
          }
  
          changeState = () => {
              console.log("我被调用了");
              this.setState({isHot: !this.state.isHot});
          }
  
          render() {
              return (
                  <div>
                      <div onClick={this.changeState} className="weather">今天天气非常{this.state.isHot ? "热" : "冷"}</div>
                      <div className="list">
                          <ul>
                              {
                                  Object.keys(this.props).map((item, index) => {
                                  return <li key={index}>{item + " -> " + this.props[item]}</li>
                              })}
                          </ul>
                      </div>
                  </div>
              )
          }
      }
  
      // 限制prop接收参数
      PropsTest.propTypes = {
          name:PropTypes.string.isRequired, 
          age:PropTypes.number,
          sex:PropTypes.string
     }
  
      // 在不传入参数时,prop的默认类型
      PropsTest.defaultProps = {
          age :13,
          sex: "未知"
      }
        
   3.refs
   refs为类组件里的一个属性，里面存储的是键值对信息，可以用三种方法定义
    （1）可能弃用refs写法 
    在refs属性里添加一个键值对对象 使用 this.refs.input2获取input表单元素
    <input type="text" ref="input2" onBlur={this.blurEnd} />
    获取值：this.refs.input2.value
    （2）回调refs写法
     在组件类属性里添加一个input属性，value为input,使用this.input获取表单元素
     <input type="text" ref={input => this.input = input} /> 
     获取值：this.input.value
    （3）createRef写法
    调用React.createRef即创建了一个ref容器，该容器专人专用(只保存一个ref),再次重写会把之前的覆盖(所以用此方法写的容器需要声明n个React.createReft)
    myRef = React.createRef()
    <input type="text" ref={this.myRef} />
    获取值：this.myRef.current.value
  */
  ```

  

+ 组件化编程流程

  ```js
  /*
  React原则，状态在那个类里，就在哪个类里改变
   1.拆分组件：拆分界面，抽取组件
   2.实现静态组件：使用组件实现静态页面效果
   3.实现动态组件
   	动态显示初始化数据
   		数据类型
   		数据名称
   		保存那个组件
   		交互
   4.onSubmit在表单提交的时候触发(有些时候需要住阻止默认行为)，onchange在内容发生改变时触发
  */
  ```

+ 受控组件，非受控组件

  ```js
  /*
   数据是往state里存的标识受控组件
   数据不往state里存的标识非受控组件
  */
  ```

+ 生命周期函数(旧)

  ```js
  /*
   1. 在jax里写style标签需要用两个{{}}，第一个标识js表达式，第二个标识为一个对象，样式和样式之间用逗号间隔
   jax中 style={{ opacity: this.state.opacity, color: "#ace" }}
   js中 style="opacity: .8;width:500px;height:300px"
   
   2. 生命周期函数也叫生命周期钩子
   有三个过程：挂载，更新，卸载组件
   工作过程：初始化render,组件将要挂载，render完成渲染，挂载完成，组件将要更新，组件更新完毕
   
   3.生命周期函数(旧)
   组件挂载(在render第一时间渲染完毕时自动触发)
   componentDidMount() { // 一般在此函数发送异步ajax请求，定时器等
          setInterval(() => {
            if (this.currentNum <= 0) {
              this.currentNum = .9;
            }
            this.currentNum -= this.changeNum
            console.log(this.currentNum)
            this.setState({
              opacity: this.currentNum
            })
  
          }, 200)
        }
    
  组件更新（在render更新时，自动触发此函数）
  componentDidUpdate(){
  
  }
  
  组件卸载（在组件将被销毁时自动触发）
   componentWillUnmount() { // 组件将被卸载时自动触发，一般做一些收尾工作，如关闭定时器等
          console.log(5)
        }
  death = () => { // 执行此函数标识删除组件，继而触发componnetWillUnmount
          ReactDOM.unmountComponentAtNode(document.querySelector('#test'))
        }
  
  
  其他生命周期函数
  constructor(){} // 在每次引用或生成实例对象时自动执行
  componentWillReceiveProps(){  // 此函数在第二次(包含第二次)以后获得的参数时，自动触发
  
  }
  componentWillMount() {} // 组件将要挂载完成时更新(少用)
  shouldComponentUpdate(){} // 每次更新组件时执行，询问是否更新，返回true&false
  componentWillUpdate(){} // 组件将要更新时触发
  this.forceUpdate() // 强制更新
  4.总结
   【初始化】
    触发条件： ReactDOM.render(<Component/>)
    constructor()
    componentWillMount()
    render()
    componentDidMount()
    【更新】
    触发条件： this.setState({})
    componentWillUpdate()
    render()
    componentDIdUpdate()
    【卸载】
    触发条件： ReactDOM.unmountComponentAtNode()
    componentWillUnmount() // 收尾工作 ，如关闭定时器，只执行一次
  */
  ```
  
  ![image-20220406195601900](D:\typora_import_images\typora-user-images\image-20220406195601900.png)旧

+ 声明周期函数(新)

  ```js
  /*
   1. 即将废弃的钩子函数
   componentWillMount()
   componentWillReceiveProps()
   componentWillUpdate()
   
   2. 新增钩子函数
   （1）static(在类中需要加) getDerivedStateFromProps(props,state){} props,state都是解构后的对象 ,取代了componentWillMount和componentWillUpdate,getDerivedFromProps，返回值必须是一个对象或null,返回的对象会挂载在状态中，如果状态有重名则覆盖之前的，没有重名就添加一组新的键值对
   
   （2）getSnapshotBeforeUpdate(props,state){},需要返回参数或者null，这个返回参数会传递给componentDidUpdate(props,state,data) 中的data参数，可以把getSnapshotBeforeUpdate(props,state){}和axio.interceptors.response.use()类似，都是做一个中间处理
  */
  ```

  ![image-20210809100451985](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210809100451985.png)新





+ 虚拟DOM Diff算法

  ```js
  /*
   初始化显示界面
   	创建虚拟DOM数(是js一般对象，用到文档碎片优化)，真实DOM树，绘制界面显示，setState()更新状态，重新创建虚拟DOM树，新/旧树比较差异，更新差异对应真实DOM，局部重绘
  */
  ```

  ![image-20210809111721560](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210809111721560.png)

+ key标识

  ```js
  /*
   作用：key是虚拟DOM的标识符，当状态数据发生变化时，React会生成新的虚拟DOM节点，然后React会把新的DOM节点和旧的DOM节点作Diff比较
   若旧的虚拟DOM中如果找到了与新的虚拟DOM相同的key，则会对比它们的差异，若没变化，则直接使用原来的真实DOM,不刷新页面的真实DOM，若有变化，则先更新虚拟DOM，随后刷新页面的真实DOM
   若旧的虚拟DOM中没找到虚拟DOM，则渲染到真实DOM页面
   若把key的值设置为一个可能发生改变的值(如下标)，那会产生没必要的真实DOM更新，因为key的值一直在和原来的值想改变，会一直触发局部重绘重排，再者，一些标签相同，可标签里的输入内容不同的标签不会被刷新，因为Diff对比差异是对比标签本身和字符串内容的差异，并非查找到输入的内容，所以会产生错位(如input:text　标签)
   
   总结: key的值一定要设置为一个唯一身份标识符,避免信息错位和无意义重绘重排
  */
  ```

  

## 1.1.1 React脚手架

refs属性可以给标签和组件添加，添加后可以调用该标签或组件的属性和方法
为了避免setState冲突，一个函数最好只设置一个this.setState（如果只能设置多个最多设置两个，如果报错则改为一个）

+ 下载react脚手架库

  ```js
  /*
   创建项目并启动
   安装脚手架:npm i create-react-app -g
   创建项目: create-react-app hello-react(创建脚手架的名字)
   启动: 切换到下载项目目录 使用 npm start开启项目
  */
  ```

+ React结构&标签讲解

  ```js
  /*
  public 文件夹下的内容
  	index.html 下的内容
           <link rel="manifect" href="xx.json"> // 用于给网站加壳,使它能称为app
           <meta name="theme-color" content="#000"> // 更改安卓手机地址栏颜色
           <meta name="description" content="Web site created using create-react-app"/> // 项目上线		时的网站描述
           <noscript>you should agree this React.js runing in the app</noscript> //不能执行script代	   码时会执行这个标签
           <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" /> // 设置在IOS系统中,被收藏	    的应用图标
           
      manifest.json , 此文件为页面打包成app时的配置，写的不全，使用HBuilder打包即可
   	robots.txt ，此文件规定了爬虫允许获取的内容
   
   src 文件夹下的内容
       App.test.js ,用于项目测试使用
       reportWebVitals.js — 页面性能分析文件（需要web-vitals库的支持）
       setupTests.js — 组件单元测试的文件（需要jest-dom库的支持）
       
   注意
  	在idnex.js页面渲染时,可以直接获取public下的index.html里的标签(React内部完成链接,文件夹不能更改)
  	如果文件夹或文件改过名可能导致编译出错,这时需要重启react服务器
  
  在组件类命名时最好与模块名引入模块值一致，符合开发规范，虽然组件中的类名值可有可无，最终还是以引入为准，但一致时会有代码提示
  
  组件的js文件一般写为jax，主js文件一般以App.js命名，入口文件以index.js命名（.jax和.js可以省略，不建议省）
  
  在初级阶段可以先搭建静态页面，然后把内容粘贴到React App.js中，注意<input/>等单回路标签要闭合 。 React不希望a标签有网址(后期解决) 可以先用#1锚点占位
  
  所有成型的第三方库编写的代码可以放到public文件夹,引入给index.html即可(如 bootstrap)。当使用bootstrap字体时，会调用对应的字体文件，如果没找到才会报错。如果按css模块引入会立刻扫描这个文件，看到字体路径不合理会立即报错
  
  书写的静态页面复制到React文件中，需要把class写的类该为 className,input标签要闭合
  <input type="text"/>
  
  写项目时，要多写注释，在css样式时需要些注释表明 ...开始 ...结束
  
  在划分模块时要把这一部分的html和css都带过去，利于模块话拆分和开发
  
  key是内部参数 不属于 props
  
  
  */
  ```
  



+ uuid

  ```js
  /*
   可以创建一个完全无冲突的唯一id
   安装： npm i uuid
   引入： let uuid = require('uuid')  import引入可能报错
   使用
   1、uuid.v1(); -->基于时间戳生成  （time-based）
  
  2、uuid.v4(); -->随机生成  (random)
  */
  ```

+ state一些细节&&常用细节

  ```js
  /*
  0. 把看到的交互视作一种状态，并单独开一个对象控制
  
  1.state 不能直接更新修改和更新数据(在相同地址中)
   state在更新状态的时候才会重新渲染页面，所有在使用结构赋值这种方法获得状态时，可能react不承认更换了状态导致不会重新刷新页面，因为使用结构赋值更新的状态实际上还是指向同一块地址，在这个地址上进行修改，react有时候不认可这种写法，
   如 let {test} = this.sate   这种的地址值一致(复杂数据类型地址传递，简单数据类型值传递，因此简单数据类型可以使用此方法改变状态)。所以复杂数据类型在获得state状态值时要使用...运算符展开数据在进行传题(值传递) 如 let test = [...this.state.comments]
   
   总结：react的状态改变是根据地址值和原有值都改变的情况下才渲染的，只有对象和数据可以使用扩展运算符(使用扩展运算符返回的是一个新的地址)
   
   2.再触发一个事件又想传递参数时
   再触发一个事件(所有事件都有环境变量)又想传递参数时，可给事件指定一个匿名函数，在匿名函数中调用其他函数并传入参数即可 如 onClick=()=>{this.delete(a)}
   
   3.window.confirm在React中不能省略window
  */
  ```



## 1.1.2 react ajax

React本身只关注界面，并不包含发送ajax请求，前端应用需要通过ajax请求和后台交互需要用到ajax库

+ 常用ajax请求库

  jQuery:比较重，如果需要例外引入不建议使用

  axios:轻量级，建议用

  fetch：原生函数，老版本浏览器不支持

  原生ajax请求

  fly



+ 常见问题

  ```js
  /*
   1. 如果使用 axios 页面请求失败时，使用err.message拿到错误信息(字符串类型)状态，直接获取err会导致程序异常(err是一个Object对象)
   
   2. stackoverflow 常见代码错误解决平台
   
   3. 在component文件夹中区分各个模块后，可以把各个模块都用index.jsx命名，这样就只用指定到index的上级目录就可以访问index.jsx文件(React会自动引入index.jsx文件)
   比如 component/show/index.jsx 引入时可以简写import Show from './component/show',React会自动去查找index.jsx文件
  */
  ```

  

+ fetch

  ```js
  /*
   0. 关注者分离，把每个模块做的事情划分开，比如fetch就采用了这个原则，第一个.then判断是否链接成功，第二个.then获取返回数据(依附于第一个.then return JSon数据)，若失败则传递给Catch打印错误信息
   
  1.在使用fetch时不能直接返回json格式数据，因为其只会判断域名是否符合(匹配则返回json文件执行第二个.then)，而不会判断路由是否匹配。,所以需要自己做判断，不能盲目返回xx.json().
   
   2. 在fetch判断第一个.then时可以通过状态码等来判断
   （1）域名路由都正确，状态码为200,ok为true，
   （2）域名正确路由不正确，状态码404,ok为false,
   （3）域名不正确，直接执行catch语句
  */
  ```

  

+ 组件间通讯

  ```js
  /*
   1.消息订阅与发布
   要使用PubSubJS工具库，npm安装 npm i pubsub-js 
   安装后配合componentDidMount()钩子函数设置交流密文完成持续监听，当有其他模块传递正确密文时，立刻把信息传入
   接收模块（默认pubsub模块引入）：
    componentDidmount(){
    	PubSub.subscribe('list',(meg,data)=>console.log(meg,data))
    }
   发布模块(默认pubsub模块引入):
    PubSub.publish('list',{a:1,b:2}) // 接收模块会接收{a:1,b:2},并存储data里
    
    2. 所有通讯方式总结
     （1）通过props传递
     	   Ⅰ 共用的数据放在父组件上，特有的数据放在自己上
     	   Ⅱ 通过props可传递一般属性和函数属性，只能一层一层传递
     	   Ⅲ 一般属性->父组件传递数据给子组件->子组件读取数据
     	   Ⅳ 函数属性->子组件传递数据给父组件->子组件调用函数
     （2）使用消息订阅与发布机制
         Ⅰ传递消息便捷，可跨等级传递数据，适用于任意组件通讯
         Ⅱ多用可能导致报文出现雷同产生冲突，
         Ⅲ 不能集中式管理
         
     （3）
  */
  ```

  

## 1.1.3 react  router

+ 前端路由

  ```js
  /*
   安装包 npm i react-router-dom ,即可安装web路由
   react-router-dom时react的一个插件库，专门用来实现一个SPA应用，基于react项目基本都会用到此库
   1.SPA
   	(1) 单页Web应用
   	(2) 中各应用只有一个完整页面
   	(3) 点击页面的链接不会刷新页面，本身不会想服务器发起请求
   	(4) 当点击路由链接时，只会做页面的局部更新
   	(5) 数据需要通过ajax请求获取，并在前端异步展现
   	
   2. 使用react-router-dom包
    (1) 引入 import { Link, BrowserRouter } from 'react-router-dom'
    
    (2) 包裹路由<BrowserRouter><Link to='./about.html'>xxx</Link></BrowserRouter>可同时包裹多个Link路由 to写明相对路径路由，当点击到此Link标签时，自动切换到to引入的路由下,同时地址栏会变为/xxx(to指定的路由)
    
    (3) <Link>跳转的路由不能增加高亮效果，需使用<NavLink>才能添加高亮，需要写activeClassName才能添加样式，否则默认添加active样式。样式可能添加不上此时需要!important来提高权重，只有当前路由的样式会发生改变
    
    (4) <Route path="/xxx" component={Xxx}/> 用来协助<NavLink>完成对应页面的渲染，当url路径为/a时，只会匹配<Route path='/a' conponent={A}> 路径，其余路径不匹配，必须用<Browser>包裹，和if判断相似，Route可以看作为多组if判断
    
    (5) 可直接在index.js主引入模块中写下包裹<Browser><App/></Browser>，避免使用多组Browser导致代码冗杂
    
    (6) <Switch></Switch>可以帮助route加快效率，在第一时间匹配到路由后，不会继续往下查找。不引用Switch则路由会一直挨个匹配，导致效率降低,一般最频繁访问的路由就要越往前放
    
    (7) 自己定义且以<Com/>方式引入的组件为一般组件，如果组件依靠路由渲染为路由组件。通常一般组件放到Component里，路由组件放在pages文件夹里
    
    (8) 当路由写为<Route path="/" component={Com}>,标识匹配所有路由，在前端路由中 / 采用模糊匹配方法即，只要我写的path内容能完美匹配上你url上的一部分就算匹配成功(后端路由为精准匹配) 如  path为 /he  我的url路径为 /he/hello 也会被/he 匹配上，同理，当 path 为 / 时，标识所有路由等能匹配 因为url中一定包含 / ,为了解决这个问题需要把模糊匹配转换为精准匹配 在Route中添加属性 exact <Route path="/" component={Com} exact>此路由就会精准匹配
    
    (9) 在引入Route匹配二级路由时，会丢失public文件夹里引入的文件(引入路径发生改变)，因为写./是按照当前文件夹来的，直接变为 /xxx/则是按服务器根路径路径来的
     解决方法1 复制它引入其他link时的神秘代码 %PUBLIC_URL% 用他替换掉 ./xxx中的.
     解决方法2 直接删除./xxx中的. 变为 /xxx/ 则可以完美匹配
     解决方法3 使用HashRouter，不使用BrowserRouter
     
   (10) <Redirect/>重定向，写在所有路由最后面，当所有标签都没匹配上时，它会进行匹配，快跳转到你指定的路由点
   
   (11) 一般react精准匹配用的较少 大多把多级路由(难匹配)写在前方，低级路由(易匹配)写在后方
   
   (12) 当在切换路由时，当前路由显示，前一个路由会被卸载
   
   (13) HashRouter 会以/#/ 作为开头标识是前端路由，是h5提出的概念
   
   (14) 前端路由切换会留下历史记录(符合堆结构，先进后出，后进先出)，每次点击一个前端路由都会留下Push操作(历史记录)，使用replace可以一直替换当前堆底的前端路由(只会留下一次历史记录，点击回退是回退到上一个非前端路由控制页面)，解决push前端路由过多问题
   
   (15) 路由组件组件的this.props自带一些环境参数，如history,location,match等  ，    （1）history中常用的属性和方法有 goBack,goForward,push,replace
     （2）location中常用的有 pathname, match中常用的有 params
     （3）params可以获取路由的/xx/xx里的参数并以对象形式存入params
  如 <Link to={`/home/msg/detail/eq`}> < Route path="/home/msg/detail/:id" component={Detail} /> ，:id接收/eq里的数据以 {id:eq}保存
     （4） React环境变量history主要是靠history库实现
   
    (16) withRouter 属性，是一个高阶组件，可以把一个普通组件包装成ui组件，使用ui组件中的history,match等属性 
    	使用： Header = withRouter(Header)
  
   
    应用： 页面中出现选项卡切换效果时使用，
    （1）找到html中 选项卡 的位置 Link 带有高亮效果Navlink ,编写路由链接
    （2）找到html中 切换内容 的位置 把展现不同的内容，单独抽离成组件
    
    总结：前端路由采用了单页面多组件的风格，react-router-dom包中常用插件为 Link,BrowserRouter,NavLink,Route，Switch,HashRouter,exact,Redirect,activeClassName
  */
  ```

  

+ 组件间传参小细节

  ```js
  /*
   1.  自封装button组件按钮
   当给组件传参时可以以 <Com>点击</Com> 方式传递，默认会被接收到 this.props.children下 ，在另一个组件中可以使用  {...this.pros} 展开，children指向的值会自动输出到值上去 
   如 <Button>点击</Button>（默认Button被引入）  (Button组件写法) <button {...this.props}></button>
   
   2.  普通组件用 <He> 路由组件用{Hel} 普通图片和变量用{we}
  */
  ```



+ mock.js，用于模拟数据库信息



## 1.1.4 antd

antd组件本质为把所有的api排列组合形成对应效果

Input组件相同name值会一起控制

给一个函数设置多个 this.setState可能导致程序异常，最好只设置一个

antd组件的显示与隐藏只用操作其对应的关键状态即可

再状态里的数组要使用 [...this.state.xx]方式扩展才能使用数组的属性和方法，否则只能使用字符串的

+ 表单提交&antd按需引入

  ```js
  /*
   1. 安装antd: npm i antd , 再App.css引入 import 'antd/dist/antd.css'
   
   2. 按需引入(用到什么引入什么，要不然antd会全部引入)
   	(1) 下载 @craco/craco , npm i @craco/craco
   	(2) 配置 package.json
   		"scripts": {
             "start": "craco start",
             "build": "craco build",
             "test": "craco test",
          }
       (3) 再根目录创建 craco.config.js 修改默认antd配置
       (4) 自定义主题色
       	引入 less-loader 和 less(帮助编译less)一级craco-less                  安装 npm i less less-loader craco-less 。修改 craco.config.js
       	const CracoLessPlugin = require('craco-less');
          module.exports = {
            plugins: [
              {
                plugin: CracoLessPlugin,
                options: {
                  lessLoaderOptions: {
                    lessOptions: {
                      modifyVars: { '@primary-color': '#1DA57A' },
                      javascriptEnabled: true,
                    },
                  },
                },
              },
            ],
          };
   
   3. 表单验证规则
    	rule有两种适配方案，1按照已给API自定义限制，2按照以给API给定挟制
    	(1) 按照API给定限制
    	rules={[
            { required: true, message: 'Please input your username!',},
            {max:14,message:'输入的位数必须小于14位'},
            {max:4,message:'输入的位数必须大于4位'},
            {pattern:/^\w+$/,message:'输入的账号或密码必须由数字字母或下划线组成'}
          ]}
       
      (2) 按照API自定义限制
      	需要传三个参数，返回的为Promise对象，rule标识该表单配置，value标识表单值，str标识自己传入的标量
      verifyForm = (rule, value, str) => {
          if (value == null) return Promise.reject(new Error(str + '不能为空'))
          else if (value.length < 4) return Promise.reject(new Error(str + '长度要大于等于4位'))
          else if (value.length > 12) return Promise.reject(new Error(str + '长度要小于等于12位'))
          else if (!/^\w+$/.test(value)) return Promise.reject(new Error(str + '需由数字，字母或下划线组成'))
          else return Promise.resolve()
        }
        
  4. Form提交验证
  	(1) Form下有环境API,这时可以把他存到ref里，然后当按钮点击时，调用此ref下的Form下的validator方法,返回的是一个promise对象，可以用 async await拆包
  	 formRef = React.createRef()  // 默认被class包裹
        formCommit = async () => {
          try {
            const values = await this.formRef.current.validateFields();
            console.log('Success:', values);
            // message是antd模块下的一个组件，可以美观的在屏幕上弹出错误信息 message('错误信息'[,显示秒数])
            message.error('输入正确，即将进入..')
          } catch (errorInfo) {
            console.log('Failed:', errorInfo);
             message.error('输入格式不匹配，请重新输入')
          }
        }
        <Form ref={this.formRef}></Form>
        
  5. 清空form表单输入框(默认被class包裹)    
  ref={(form) => this.form = form}
  cancelModal = () => {
      console.log('取消了')
      this.setState({
        visible: false
      })
      this.form.resetFields()
    }
    
  
  6. Form组件使用注意点
  	(1) 被设置了name属性的Form.Item控件，表单空间会自动添加value,onChange,数据也会统一交给Form表单处理。这意味着不用使用onChange来监听数据变化，不能使用value和defaltValue设置初始值，此时不能使用setState而要使用form.setFieldValue来动态改变表单值
  */
  ```

  

+ 高阶函数&高阶组件

  ```js
  /*
   高阶函数：如果一个函数接收一个函数作为参数，则此函数为高阶函数
   高阶组件：如果一个组件接收一个组件，并把这个加工组件返回出去就为高阶组件
   组件的本质是函数
  */
  ```




+ antd layout使用

  ```js
  /*
   1.在引入antd布局时，可能出现元素不展开，在器外侧元素(Layout)上添加height:100%即可
  */
  ```

+ antd 使用menu导航菜单

  ```js
  /*
  1. 使用menu导航菜单
   menu导航菜单一般和路由使用再一起，再包裹时，路由应再<Item>组件内包裹
   如 <Item><Link to="/home">首页</Link></Item>
   
   2. icon使用map遍历问题，遍历不出来效果，须在config中把icon直接写为<Username/>(不能使用字符串，{<{}>}嵌套会报错)格式在遍历时即可
  */
  ```




+ table&&card

  ```js
  /*
   找到响应组件引入即可
   1.table 参数
   	columns={columns}  // 设置列头规则
      dataSource={data}  // 获取对应得列资源
      size="default"  // 设置尺寸
      bordered={true}  // 设置边框
      pagination={{ 
      pageSize: PAGE_SIZE， // 设置一个表格显示几行
      total: TOTAL, // 设置以共多少条数据
      current: NUMBER, // 设置当前默认选择第几列
      }} 
      rowKey="_id"    // 设置key值，会去找当前行列下得_id属性内部把它设置位改元素得key值,如果不使用rowKey参数，则需要在获取列表时用map挨个遍历给它们添加key关键字
          result.data.list.map(item => {
            return item.key = item._id
          })
   	loading = {}  // 设置是否加载，是一个布尔值 
   	(1) agination :可以改变一页显示多少个元素 如 pagination={{pageSize:5}}
   	
   	(2) rowKey: 可以代替react key值(接收唯一标识符，再后端传递得数据没有key选项得情况下)，可接收字符串，函数
   2. columns 参数 
   	title:标识列头id
   	dataIndex:标识所管元素
   	key: 唯一标识符
   	className:可以为当前列位设置类名
   	render:(item)=>{return<Button onClick={()=>{this.cl(item)}}/>} 此项用来生成对应行的元素，其中环境形参item(可以为任意值)可以拿到该列dataIndex的所有值,例如再请求后端拿到返回参数时，dataIndex可以写为 dataIndex:xxx格式(与后端返回key相匹配)render形参即可保存其数据，在必要时,配合事件可以完成对应逻辑和参数携带。若没有dataIndex属性，则该形参拿到是本行的所有数据
   3. columns和dataSource
   (1)columns用于设置该列样式及标识所管列，title属性设置列头名称，dataIndex用于收集dataSource中对应得资源，把它们整合到一列，如 dataIndex:'name',意为收集dataSource中所有得name都这一行
   (2)dataSource以数组嵌套对象格式展示出可选得属性
   如  {
      key: '1',
      name: 'John Brown',
    },
    {
      key: '2',
      name: 'Jim Green',
    },
    
   4. 如果想要给Item内的表单设置初始值需再Item下添加 initialValue=xxx属性
   如  <Item
                name="name"
                initialValue={this.state.itemNameInTable}
                rules={[
                  // rule和value都为环境变量，rule保存的是该标签的信息，value保存的是数值
                  { required: true, message: '信息不能为空' }
  
                ]}
              >
                <Input className="site-form-item-icon user-icon" placeholder="请输入信息" />
  
  5. 当一个Input被Form包裹时，想要给Input添加默认表单显示，只能给Form添加使用initialValue设置初始值，想要这个值动态变化需要使用form.setFieldsValue
    如 setTimeout(() => { 
            this.form.setFieldsValue({
              itemName: this.itemName // 指定一个键值对，键用来指向对应的数据，然后把这个键作为Item name值
            })
             // 需要给setFieldsValue设置一个定时器，把它放到任务队列里，避免因它先给值，而产生报错
          }, 0);
      <Item name="itemName"> .... </Item>
      
    例 1 
        setTimeout(() => {
            this.form.setFieldsValue({
              name: this.state.name, // Item name值：状态值
              desc: this.state.desc,
              price: this.state.price,
              categoryId: this.state.categoryId
            })
          }, 0);
     
     
  6. 当调用后端修改状态接口数据成功后，页面往往不会自动刷新(需更改状态，或重新获取列表)，当重新获取列表时，如数据量过大则不太适用，可使用改变状态方法，重新对比校对数据，再利用React Diff校验 达到完美加载效果
   修改状态代码
    productList = [...this.state.productList]
   if(status === 0) {
   	productList.map(item=>{
   		if(item.id === _id){
   			return item.status = status
   		}
   	})
   }
  */
  ```



+ 分页

  ```js
  /*
   及页面底端显示得当前页数，通常为 1 2 3 ... 最后一个，分，真分页 和假分页
   
   再antd周搜索分页即可查看分页组件
   
   分页分为前端分页，和后端分页
   前端分页： 一次性返回所有数据，由前端人员进行数据的切割，整理，划分页数，当数据量足够大时，会产生页面卡顿或浏览器崩溃
   后端分页： 返回的是一部分数据，需请求时指明每页显示多少条，再那一页，交由后端进行数据切割，后台需要明确，每页显示多少条，同时后台返回全部数据个数给前端
  */
  ```
  



+ 表格布局流程

  ```js
  /*
   一般会使用Card组件进行包裹，然后再里面使用Input和Button，Table等
  */
  ```




+ antd 插件 onchange事件细节

  ```js
  /*
   1.在 Select中 onChange的第一个形参就是表单里的值，也就是defaultValue值
   	如 <Select defaultValue="Description" onChange={v=>console.log(v)}/>  此形参v拿到的就是defaultValue的值
   2.在 Input中 onChange的第一个形参就是环境变量，使用event.target.value拿到值，该值为表单里实时输入的内容
  */
  ```
  



+ React解决htmlDOM数据

  ```js
  /*
   有时候我们向后端发起请求获取数据时，可能会返回一些html标签，这些标签不会被React解析，且原样输出，如果想让这些标签生效，需要加上dangerouslySetInnerHTML属性  
   如 <span dangerouslySetInnerHTML={{__html:this.state.desc}}></span>
  */
  ```




+ antd栅格系统

  ```js
  /*
   antd中把栅格系统分成了24份
  */
  ```



+ upload组件

  ```js
  /*
   用于上传图片等文件
   1. state中的参数
    state = {
      previewVisible: true, // 是否展示预览图
      previewImage: '', // 要预览图片的地址
      previewTitle: '', // 域栏图片的id
      fileList: [
        {
          uid: '-1', // 唯一标识符
          name: 'image.png', // 图片名
          status: 'error', // 当前图片的状态 uploading done remove
          url: 'https://zos.alipayobjects.com/rmsportal/jkjgkEfvpUPVyRjUImniVslZfWPnJuuZ.png', //图片地址
        }
   2. Upload携带参数
   <Upload
            // action 接收图片的服务器地址
            action={`${BASE_URL}/manage/img/upload`}
            method="post" // 请求方式，默认Post
            name="image" // 发给服务器的key
            listType="picture-card" // 展示图片方式
            fileList={fileList} // 图片列表，一个数组里面包含多个图片对象
            onPreview={this.handlePreview} // 点击预览按钮的回调
            onChange={this.handleChange} // 图片状态改变回调，里面包含了图片信息，和图片状态码，有 {file(里会返回是否上传成功，和图片地址等),fileList,event}
          >
            // 设置图片最大图片数量 
            {fileList.length >= 4 ? null : uploadButton}
          </Upload>
          
      // 上传，完成，删除都会触发onChange
      handleChange = ({ fileList, file, event }) => { 
  
          console.log(file)
          if (file.status === 'done') { // 图片上传完成
            console.log(fileList)
            fileList[fileList.length - 1].url = file.response.data.url // url该为服务器地址
            fileList[fileList.length - 1].name = file.response.data.name // name和服务器存储名关联
          }
          this.setState({ fileList })
        };
  3. 使用注意
  	在上传图片时需要把服务器返回状态的地址维护到本状态中，这样才能和服务器数据进行关联，否则前端做的操作，服务器不会发生修改
  */
  ```
  



+ 富文本编辑器

  ```js
  /*
   wysiwyg 库，专门为react定制
   1.安装包  npm i react-draft-wysiwyg draftjs-to-html html-to-draftjs draft-js
   2.打开网址 https://jpuri.github.io/react-draft-wysiwyg/#/demo ，复制自己需要的富文本
   3. 引入样式(系统不会自动引入)
    import 'react-draft-wysiwyg/dist/react-draft-wysiwyg.css'
    
   4. 将html语义化标签文本转换为正常文本(数据回写)
   	setRichText = (html) => {
      console.log(html)
      const contentBlock = htmlToDraft(html);
      if (contentBlock) {
        const contentState = ContentState.createFromBlockArray(contentBlock.contentBlocks);
        const editorState = EditorState.createWithContent(contentState);
        this.setState({ editorState })
      }
    }
  */
  ```




+ Model组件

  ```js
  /*
   可以设置点击出现对话框效果，通过设置visible={}状态方式
  */
  ```

+ tree组件

  ```js
  /*
   ztree和easyUi为jqeury树插件
   tree为antd组件
   
   1. 标签分析
    state = {
      expandedKeys: ['0-0-0', '0-0-1'], // 配置默认展开的节点
      autoExpandparent: true, // 设置是否自动展开父节点
      checkedKeys: ['0-0-0'], // 默认选中的元素
    };
    
     <Tree
              checkable  // 增加复选框
              onExpand={this.onExpand} // 展开时触发
              expandedKeys={this.state.expandedKeys} // 运行时，展开的树
              autoExpandParent={this.state.autoExpandParent} // 自动展开父节点
              onCheck={this.onCheck} // 当选择复选框后触发,自带环境形参，第一个为当前选中的元素
              checkedKeys={this.state.checkedKeys}
              onSelect={this.onSelect} // 当点击文本时触发，自带环境形参，第一个为当前点击的元素
              selectedKeys={this.state.selectedKeys} // 选中的key
              treeData={treeData}
              defaultExpandAll = {true} // 展开所有列表
            />
  */
  ```

  

## 1.1.5 react redux

redux用于收集一些公共操作的状态，私有状态内部修改即可

this.setState是异步任务，无法在同一个函数中设置和获取状态值(搭配定时器或设置全局属性可以获取想要的状态值)

+ redux集中式状态管理工具

  ```js
  /*
   1. redux是一个独立抓门用于做状态管理的JS库，它可以用再react,angular,vue等项目中，但基本配合react  . 作用：集中式管理(读/写) react应用中多个组件共享状态
   2. redux应用场景
   	总体原则：能不用则不用，如果不用比较吃力才考虑使用(出现在复杂交互中)
   	某个组件的状态，需要共享
   	某个组件需要再仍和地方都可以拿到
   	一个组件需要改变全局状态
   	一个组件需要改变立一个组件的状态
  */
  ```

  ![image-20210814150530032](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210814150530032.png)

+ redux三个核心概念

  ```js
  /*
   1. action
   	标识要执行行为的对象，包含2个方面的属性，type:xxx,值为字符串，唯一必要属性。xxx:xxx 数值属性，值类型任意，可选属性
  */
  ```

  

+ redux使用

  ```js
  /*
   1. 安装redux  npm i redux ,创建redux文件夹，在里面创建reducer.js,和store.js，store文件夹需引入 redux下的createStore方法，并和reducer创建关联
   2. 核心store文件与index对话书写为 <App store={Store}>
   3. 再reducer.js中需要做状态初始化，同时reducer是一个函数接收两个参数，第一个参数为状态值，第二个参数为操作行为(dispatch)，再此函数中做判断推荐使用switch
  */
  ```

+ redux使用注意事项

  ```js
  /*
    1. 再reducer.js中不能修改传递过来的参数
    2. 当store状态改变时，不会重新渲染页面，因为其没有渲染权限，需要利用他的方法store.subscribe(()=>{<App store={store}>,xxx})借助ReactDOM.render渲染
    3. store一般只和index.js对话
    4. createStore下的dispatch方法用于传递action
    5. createStore下的getState用于获得改变状态值(伪)
    6. createStore下的subscribe方法再每次dispatch后都会触发，再index.js页面中使用可以配合页面渲染，redux并没有渲染页面的权限
    7. reducer(proviousState = initState, action) ，proviousState和action都由store传入，proviousState标识传入状态(内部由getState方法获取)，action由我们调用dispatch传入再有store转接
    8. reducer必须给定一个初始状态，因为getState()一开始获取不到值，当他第一次执行reducer函数后既获取了状态值
    9. actionCreators 和params_include配置js文件夹出现是为了模块化管理状态，使结构变得清晰
    
   store运行流程： 首先redux.createStore创建的实例对象给index.js引入，然后把store对象传递给app.js,app.js使用其下的dispatch()变量传递值给store，store内部处理把此值保存,并当作reducer函数中的action形参的值传入，reducer函数中的proviousState是由其下的getState()推动，由于程序刚运行没有状态导入，所以要再其上设置一个初始状态，当一个流程完毕后，可以获取到状态值，其也失效，store本质上没有页面渲染能力，当他的状态改变时，需其下的subscribe方法和index.js中的ReactDOM.rende配合实现页面再次渲染
  一般为了app页面显示参数整洁明了，故使用ActionCreator.js加工{type:xx,data:xx}对象后返回，因其中可能涉及到过多的变量所以要专门给ActionCreator.js配置专门的js文件 
   */
  ```



+ react-redux

  ```js
  /*
   1.React-Redux将所有组件分为两类
   	(1)UI组件
   		只负责UI呈现，不带任何业务逻辑(if,setTimeout等)，通过props接收数据，不适用任何Redux的API,一般保存再components文件下
   	(2)容器组件
   		负责管理数据和业务逻辑，不负责UI呈现
   		
   2. react-redux使用
   	(1) react-redux的出现更加规范了组件的使用和组件的分别，使每个组件都可以改变Redux的状态，而不经过App Prop传递
   	(2) 组件分Ui和容器后，在容器中引入 import { connect } from 'react-redux' 以及 import Counter from '../components/counter'和之前封装好的actionCreator  import { createAdditionAction, createSubtractAction } from '../redux/actionCreators'
   	(3) 使用 解构出来的 connect()函数完成组件封装，connect((state函数状态),(操作状态的方法))(绑定的ui组件)  
   	export default connect(state => ({ count: state }), {
        addition: createAdditionAction,
        subtract: createSubtractAction
      })(Counter)
      其中createAdditionAction函数(我们已封装)会返回一个{type:xx,data:xx}对象，当这个属性在Counter ui组件中被调用时，会自动转换并调用dispatch函数传递给store,sotre在转交给reducer处理 ，进而改变状态
  */
  ```
  



+ react-redux使用注意点

  ```js
  /*
   1. index.js也可引入 react-redux 使用解构出来的 Provider把store对象传递给App , ReactDOM.render(<Provider store={store} ><App /></Provider>, document.querySelector('#root')) 此总方法可以将store传递给全局react,当其他组件需要connect创建时，第一时间完成渲染，完美代替了 subscribe方法
   
   2. 在app.js引入中只引入他对应的容器组件即可，因为容器组件包含了ui组件，这样可以达到继承渲染效果
   
   3.connect传递规则为 connect(函数，对象{函数})(ui组件)，这些函数下的属性绑定在ui组件下的props对象下，第一个函数专门用于获得当前状态，第二个函数用于改变函数状态，在使用中ui组件直接调用方法传递数据即可，此函数内部会自动dispatch给store，进行状态校对
   	export default connect(state => ({ count: state }), {
        addition: createAdditionAction,
        subtract: createSubtractAction
      })(Counter)
  
  工作流程：首先，在我们封装好store后，把他引入到index.js中，然后下载 react-redux,把它也引入到index.js中，解构出 Provider属性 包裹在 App上，并给Provider 传递一个props,store={store},Provider会把store进行分发，和实时监听状态变化，当状态改变时，他会立刻重新渲染页面。第二，我们进行ui组件和容器组件封装，把ui组件和react-redux引入到容器组件中，对react-redux进行解构，得到connect方法，在其中我们可以给改ui组件下的props添加state(受store管理)，和操作reducer的方法，在ui界面像操作状态时，直接对props下的方法进行调用即可
  
  4. 在没有react-redux之前，store只能和index对话，并把store传递给App，使App能操作dispatch,getState等，不能跨阶段和阶级传递，而react-redux则解决了此问题，谁需要操作store状态，直接使用connect方法连接即可
  
  5. action操作代码启停流程，reducer负责改变状态
  
  6. 为了避免文件过多，通常会把ui和容器写在一个文件里
  
  7. Provider是顶级组件
  */
  ```

  

+ redux异步编程

  ```js
  /*
   用于解决异步任务(如定时器)不返回对象问题
    1.安装：  npm i redux-thunk
    2.使用
  	(1)在store.js中 写入
  	import { applyMiddleware, createStore, Middleware } from 'redux'
      import reducer from './reducer' 
      import thunk from 'redux-thunk'
      export default createStore(reducer, applyMiddleware(thunk))
      (2)在改容器组件中写下对应的actionCreator中的函数
      (2)在actionCreator中给对象的调用语句写成以下形式即可解决异步问题
      export const createAsyncAddAction = (value, delay) => {
            return (dispatch) => {
              setTimeout(() => {
                dispatch(createAdditionAction(value))
              }, delay)
            }
          }
  */
  ```

  

+ redux开发者工具

  ```js
  /*
   1.谷歌商店下载 redux DevTools ，
   2.下载后需安装 npm i redux-devtools-extension 
   3. 在store.js引入 
    import { createStore } from 'redux'
  import reducers from './reducers'
  import { composeWithDevTools } from 'redux-devtools-extension'
  
  export default createStore(reducers, composeWithDevTools())
  */
  ```

  

+ 多Ui多容器组件开发

  ```js
  /*
   1. 一般一个项目一个store有n个reducers和actions，它们的文件夹都位于redux下，为了统一管理需要把所有的reducers收集到本文件夹的index.js里
   	import counterReducer from './counter_reducer'
   	import personReducer from './person_reducer'
   	import {combineReducers} from 'redux'
   export defualt	combineReducers({  // 把它们各自reducers状态收集到combineReducers对象里
   		count: counterReducer,
   		person: personReducer,
   	}) 
   	这样书写后，获取响应reducers的状态就要用 state.xxx 方式来获取 如state.count
  */
  ```

+ 多Ui多容器组件开发注意点

  ```js
  /*
   1. 多组件开发以当前文件夹的index为收集点，传递给store.js，做组件分发
   2. connect中，第一个函数的环境形参可以拿到当前全部组件的状态点，并以对象的形式存储，获取一个值时要以 state.xx方法 如 { count: state.count, person: state.person }
  */
  ```

  

+ 高阶函数和纯函数

  ```js
  /*
  1.纯函数
       一类特别的函数：只要是同样的输入(实参)，必定得到同样的输出(返回) 
       如function dome(a){console.log(a)} dome(a)调用三次
       纯函数必须遵守以下一些约定
       	(1) 不得改写参数数据
       	(2) 不会产生任何副作用，列入网络请求，输入和输出
       	(3) 不能调用Date.now()或Math.random()等不纯的方法
    	 redux的reducer函数必须是一个纯函数
   
   2.高阶函数
   	形参接收的是函数，返回的也是函数，常见的高阶函数有 定时器，Promise,forEach等
  */
  ```

  

+ postman使用

  ```js
  /*
   1. 发post请求时，参数需要由body携带，还需选择 x-www-form-urlencoded
   2. 身份识别主要是看其携带的token是否准确
   3. 当在测试接口时，如需要登录权限，可在登录后复制登录的token，然后在测试其他需要token的接口时，把刚刚复制的token放到HEader中 以 Authorization:xxx(token) 形式携带即可验证身份
  */
  ```



+ 跨域问题

  ```js
  /*
   1.跨域本质是服务器收到了请求且返回了数据，但被https拦截了
   2.常见解决跨域问题的方法为 jsonp ,cors,和创造一个代理,这个代理可以接收数据且不会被拦截，同时还可以把数据带给用户
   3. 代理搭建， 将 "proxy":"http://localhost:4000"添加到package.json所有配置，然后连接此代理即可，这个代理和我们是同一地址，所以没有跨域问题。代理服务器本质为转发请求
   如 axios.post('https://localhost:3000/login', values)
   
   4. 代理为React私有功能，只有React可以使用
   
   5. 当向服务器发起请求时，如果是对象的形式 如 {name: tom, pwd: 123}
  那么服务器需使用 app.use(express.json()) 解析 ， 如果是name=tom&pwd=123的形式，则可以按传统post请求处理 
  
   6. 如果后端不允许传递json数据，则可以使用querySting包(react已安装)，它可以把对象转换为字符串
   
   7. 项目在开发环境中时，可以不指定地址，默认为自己本机地址，但在生产环境中必须指明地址
   
   注意事项： 在传递参数时，先与后端协商，看后端的跨域是否写了Cors,或json等
  */
  ```
  
  

+ 使用axios拦截器返回promise成功回调利用async await接收

  ```js
  /*
  1. 在书写axios请求时，可以使用拦截器代替.then和.catch避免主文件代码过多，请求拦截器可以加工请求数据，而响应拦截器可以对数据进行判断，如果数据判断成功直接返回第一个函数的值，在主函数使用async await即可获取数据，如果失败则返回第二个函数的值，为了终端Promise链，通常返回Promise.rejct('xxx')抛出错误，而await 接收不到失败数据为undefined，从而施加判断条件
   
   主文件关键代码(async关键字写在包裹函数上)
  let result = await reqLogin(values)
        let { status } = result
        if (status === 0) message.success('连接成功')
        else if (status === 1) message.warning('账号或密码错误')
        else message.warning('网络错误') //await获取不到状态码执行此句
        
   拦截器关键代码 
  export const loginAxios = axios.create({
    timeout: 2000
  })
  loginAxios.interceptors.request.use((config) => {
    if (config.method.toUpperCase() === 'POST' && config.data instanceof Object) {
      config.data = qs.stringify(config.data)
    }
    return config
  })
  
  loginAxios.interceptors.response.use(
    // 借用了promise思想，成功(找到服务器) 执行第一个函数，失败(没找到)执行第二个函数
    response => response.data,
    error => error // 返回一个await能给解析的值(也就是resolve值，没有中断await链)
  */
  ```

  

+ NProgress

  ```js
  /*
   可以展示一个加载进度条(欺骗性)
   下载包  npm i nprogress 
   引入 import NProgress from 'nprogress'
  	 import 'nprogress/nprogress.css'
  	 NProgress.start() 开始展示进度条
  	 NProgress.doen() 关闭进度条
  */
  ```

  

+ 没登陆跳转登陆页面

  ```js
  /*
   1. render()必须写return ,最好和<Redirect/>搭配使用
  */
  ```

  

+ 装饰器语法

  ```js
  /*
   @dome class MyClass{}  // 把class作为实参传递给dome函数
   function dome(target){
    	target.a=1
   }
  */
  ```



+ 常见状态码

  ```js
  /*
   401 没有权限访问(在没登录的情况下查看页面)
   404 没找到对应页面
  */
  ```

  

+ token

  ```js
  /*
   1.身份的唯一标识符，在注册成功时会生成一个唯一的token，在每次登录时和查看其它页面时都需要携带此token
   2. token永远不会通过url参数携带，而是以公共请求头添加，公共给请求头需要一个自定义头和token才能完成认证，这种方式比较安全
  */
  ```

  ![image-20210818092316961](C:\Users\zbcz\AppData\Roaming\Typora\typora-user-images\image-20210818092316961.png)

+ 容器组件，普通组件，路由组件，ui组件

  ```js
  /*
   在只使用前端路由时，组件分普通组件(component文件夹下)和路由组件(page文件夹下)
   在使用前端路由基础上使用redux时，则只分为ui组件(component下)和容器组件(container下)
  */
  ```

  

+ 校验身份是否失效

  ```js
  /*
   if (error.response.status === 401) { // 状态码为401标识没权限
        message.error('身份验证失败请重新登录')
        // 清除token
        store.dispatch(deleteLoginInfoAction())
      }
  */
  ```

  

+ 全屏显示库

  ```js
  /*
   安装： npm i screenfull
   使用 screenfull.toggle()方法即可完成全屏切换
   
   关键代码
   import screenfull from 'screenfull'
   state = {
      isFullScreen: false
    }
    fullScreen = () => {
      screenfull.toggle()
  
    }
    componentDidMount() {
      screenfull.on('change', () => {
        let { isFullScreen } = this.state
        this.setState({
          isFullScreen: !isFullScreen
        })
      })
    }
   
   全屏显示会有bug,当用户第一次按下f11时，全屏功能会失效
  */
  ```

  

+ 优雅的交互框(antd提供)

  ```js
  /*
   可代替原生的confirm或alert,原生的太不友好，且样式无法修改
   1. 进入antd ->组件->反馈
  */
  ```

  

+ localstorage

  ```js
  /*
   localstorage存储复杂数据类型数据时，需要JSON.stringify()转换为字符串存储，在获取对应复杂数据类型数据时，需要使用JSON.parse()解析为识别类型，否则可能出现重大程序故障
  */
  ```

  

+ 获取当前时间库

  ```js
  /*
   npm i dayjs
   
   dayjs(value) 如果有value则按value时间来，如果没有value就按当前时间来
   
   获得当前时间 
   state = {
   	date: dayjs().format('YYYY年 MM月DD日 HH:mm:ss')
   }
  */
  ```

  

+ 获取实时天气接口

  ```js
  /*
   https://tianqiapi.com/api.php
  */
  ```

+ 访问其他服务器跨域问题

  ```js
  /*
   使用axios发起请求时，若后端没设置跨域选项，则会出现跨域问题，这时可以使用jsonp库解决
   安装 npm i jsonp
  */
  ```

  

+ 公共资源

  ```js
  /*
   把公共资源放到public目录下， 由于组件不能跨出src引入文件资源，所以再src下要创建一个static目录，src内的公共资源引入static,public同级的公共资源引入publicz
  */
  ```

  

+ 再一串数组对象中查找值得最好方法

  ```js
  /*
  1.利用全局类属性保存key值，这样笔避免了forEach递归传递错误值问题，forEach无法退出，会一直遍历完数组里得所有值
  以下代码默认被class包裹
    title = null
    getTitle = (arr, value) => {
      // 出现点击bug的原因，每次点击都是把全部的config拿去给forEach遍历
      //  title = this.getTitle(item.children, value) 可以拿到值，但只能拿到最后一个子导航的值，因为会一直遍历完,title值在变化
      //所以只能使用find查找
      arr.find(item => {
        console.log('forEach  ')
        if (item.children instanceof Array) {
          this.getTitle(item.children, value)
        } else {
          if (item.key === value) {
            this.title = item.title
          }
        }
      })
    }
  */
  ```

  

+ 当使用了connect连接状态时，第一个参数不能为空需要返回一个对象

  ```js
  /*
   export default connect(
    state => ({}), // state不能为空，可以返回一个空对象
    { saveProduct: saveProductMessage }
  )(Product)
  */
  ```

  

+ connect工作流程

  ```js
  /*
   connect用来给指定组件添加操作store公共状态的属性，第一个参数用来设置获取公共状态，为 state=> {return {xxx:state.xxx}} 或 state=> ({xxx:state.xxx})（括号表示一个整体，括号内是一个对象，箭头函数只有一条语句时可以省略{}且会返回这一个对象），第二个参数用来操作公共状态，一般为 {xxx: (actions)} actions为自己书写的一个js库，里面封装的返回值为 {type:xxx,data:xxx} 返回后connect自动会做dispatch操作，转交给store,store在分配给对象的reducer处理，在reducer中第一个参数为获取到的全局状态(store.getState()第一次需要自己设置初始值，因为没有设置全局状态)，第二个参数为接收到的action值，配合action里的type和data以及switch完成状态更新，处理完后暴露此reducer给store(reducer封装一个Index.js用于汇总)，自己保留一份状态，然后返回给store一个状态，在connect取值时，state存储的就是大的状态，state.xxx 就是获取对应reducer里的状态
  */
  ```



+ 在查找中find和filter的区别

  ```js
  /*
   find 返回的是对象
   filter 返回的是数组
  
  
  */
  ```

  

## 1.1.6 逻辑技巧

+ 权限技巧

  ```js
  /*
   在遍历菜单时做一个判断，看其是否拥有此权限如果有则为true，没有为false
  */
  ```

  

+ 数据可视化

  ```js
  /*
   可以快速高效的生成一个指定图表
   https://echarts.apache.org/examples/zh/index.html
  */
  ```

  

