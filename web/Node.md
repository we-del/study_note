# Node

## 细节

+ 集合

  ```java
  /*
   1. 在数据校验时，前端和后端都要进行校验，前端进行输入校验，后端进行逻辑校验
   2. package.json 里的script对象中 ，test 和 start 属性可以直接使用 npm test & npm start 运行
   3. 如果html页面想引入第三方包 可以引入该包提供的js或css，如果js文件想引入第三方包则直接使用引入语法即可
   4. 后端可以分为 接口路由(提供数据)，UI路由(用于界面显示，暴露静态资源), 逻辑路由(做数据库逻辑校验)
  */
  ```
  
  

浏览器运行调用方式

![image-20220319153518350](D:\typora_import_images\typora-user-images\image-20220319153518350.png)

+ javascript的运行机制

  在javascript中，分同步任务(基本赋值，函数调用)和异步任务(函数执行，定时器等)，同一阶段运行的程序会被放到任务队列中，同步任务会放到简单任务队列，异步任务会放到复杂任务队列，简单任务队列中的数据在进来时会立刻执行，复杂任务队列中的数据则需要等到全部简单任务全部执行完后才会执行，当该阶段的简单任务队列执行完后，会执行复杂任务队列(即函数体部分)，又可能新的同步任务和异步任务，此时按照以上顺序执行

  javascript代码从上往下执行

  为了解决同步异步导致想要的数据不能第一时间得到的问题，可以给需要延时执行的部分添加一个定时器这样就能规避此问题

# 代码边敲边总结

## 1.0 版本控制

当在做开发时都是很多人一起做页面,因此index主页会多次被更改,如果有人改错了想撤回代码,可记不清是什么时候更改的了,因此就需要用到版本控制工具来对代码做一个实时更新(记录),

v(version). .....   版本控制工具 --git , svn(subversion) svn为集中式(有队长接收)版本控制软件 ,svn弊端:队长失效,成员间通讯会出现异常.git为分布式(互相独立)工具软件

在工作中,会安排一个**bug平台**,测试bug的人员会把bug写下并发送给你并标记优先级,优先级越高越先处理

bug(交流)处理平台有 ,**禅道**



服务器: 配置极高 , 安装了装也的服务器操作系统(linux),有极佳的网络环境

个人PC电脑:配置相对满足日常需求,安装都是常规软件

+ IP地址

  分局域网IP 和 公网IP , 局域网IP只有自己站内访问，公网IP所有人都能访问

+ 网卡 -- mac -- 指纹(唯一的地址) 使用 ipconfig/all 查看

## 1.1 svn&&git

### svn

+ svn基本使用

  ```js
  /*
  1. svn基本使用
  创建 repositories 代码仓库 。生成成员类(给它们设置密码)，
  Users 页面可以看到所有的用户
  Groups可以对Users进行分组
  Jobs 用来代码备份
  
  2. 服务器端和客户端交互
    安装好服务端和客户端，且服务端有创建项目库和添加成员后，成员可以鼠标右键，选择 checkout 输入用户名密码后可获得改组的数据，更新数据后，再svn文件夹内右键鼠标选择 SVN commit 提交更改，再服务器端点击刷新即可获取最新更改数据
    当用户每次获得数据时文件夹可能会出现小图片标识此文件夹的状态(需关闭壁纸美化等软件)。？ 号表示新建的文件。+表示文件已被标识需要交给svn.绿勾 符号标识与服务器数据一致 。 红！ 符号 标识本地文件与服务器不一致，已修改。 黄！符号 标识文件之间有冲突。灰勾 符号标识当前文件以上锁，当前用户不能操作。锁 符号 标识当前文件已解锁。 灰杠 标识被忽略文件 
    
  3. 用户名清除
   每次改变项目库时，需要清除以往的默认用户名数据，否则系统会默认选择以前的用户名，导致登录失败。解决方法：再桌面选择 tortoise SVN --> setting -->saved Data 全部清空即可
   
  4. 追回之前版本
    再一个迭代多次的项目中点击鼠标反键，选择 tortoise SVN --> Update to revision.. ,点击 showlog 查看保存版本的时间点，点击该时间点可以复制该版本的内容，所以当一个大功能或者一个大模块写完时可以及时记录，当出现问题时可以及时退回
    
  5. 处理冲突
  当再合作开发时，两个及以上的人对一个文件进行更改，当它们提交时，可以选择两种解决方式 手动解决 webStorm 以及 工具解决, 手动解决方法，打开有 黄！号的文件会出现 <<<< r1 ||||..=== ... >>>r2 其中，<<<<r1是你写的代码，||...==之间的是相同的代码，...>>>r2是别人写的代码，把代码依次拼接好后，删除多余说明符和说明文件提交即可
  
  webStorm 解决，点击存在冲突的文件，反键 subversion ---> Resolve Code Conflict -->Merge ，公共部分为白色，不同部分为橙色，点击>>图标即可选择谁先移入
  
  工具解决，再有冲突的页面中，鼠标右键点击tortoise--> Edit conflicts 打开冲突面板，可以选择代码前后摆放顺序，修改完成后点击 as mark resolved 即可。此方法解决后需浏览一遍文本，可能功能代码区域没有自动换行
  
  在每次代码提交前先update更新一遍，有冲突解决冲突，如果直接点击提交可能造成之前代码丢失
  
  再代码提交时可以选择忽略一些配置环境变量文件，如果不忽略会对我们的选取造成影响，鼠标反键 TortoiseSVN -> setting ->General -> Global ignore pattern 添加需要忽略的格式 如果是单独的写 ide a .ei 则忽略的是文件夹，如果加上 *.jpg 等忽略的是文件
  
  文件上锁，鼠标反键 TortoiseSVN -> properties ->new (needLock)->lock... 即可上锁，上锁后需要重新提交文件 并说明原因
  
   添加 + 符号方法,鼠标反键 TortoiseSVN -> add 即可，标识在我提交时自动勾选上的代码
   
   文件对比差异，鼠标反键 TortoiseSVN ->Diff ... 对比差异
   
   文件代码撤回，鼠标反键 TortoiseSVN -> Revert... 撤销到上一操作
   
   清空队列，再程序代码量极大的情况下我们需要清除subversion的队列，第一层清理，clear,第二层清理 鼠标反键 TortoiseSVN -> clean up.. 清空队列，若不能解决，需要 将sqlite3.exe放到和.svn同级目录下，打开cmd，cd到仓库目录，执行： sqlite3 .svn/wc.db "select*from work_queue",这是会看到很多队列，执行：sqlite3 .svn/wc.db "delete from work_queue" ,清空完毕，重新执行 clean up即可
  */
  ```

+ webStorm操作svn

  ```js
  /*
   webStorm支持svn，再打开svn时可以对其进行更新和提交操作，也有常规的svn操作手段，作用基本相似，不过用词略有不同
   1.webStorm标识svn文件颜色标志 
   暗红色： 新增文件
   亮红色：冲突
   蓝色：修改了但没有提交的文件
   绿色：文件已经被标识
   
   2. webStorm提交 右键文件夹 选择 subversion-> commit Dictionary
   
   3. webStorm更新文件 右键文件  选择 subversion-> show History ,查看当前提交信息，点击jump..可查看此文件明细  点击get 可以获得此文件信息
   
   5.webStorm对比文件 右键文件  选择 subversion-> Compare...对比
  */
  ```



### git

+ 注意点

  ```java
  /*
   在使用 https对远程仓库建立连接时，建议使用https方式，此时需要网络通畅(可以使用网易UU加速器加速学)
  */
  ```

  

+ 分支的交互

  ```java
  /*
  1.当git有管理分支的情况下，创建一个新分支，该分支会复制上一个分支的内容，并隐式进行一次提交，当git没有管理分支的情况下，创建一个新分支，该分支会复制上一个分支的内容到本分支，然后删除掉上一个分支。
  2. 当分支切换时，会从本地仓库的该分支下拉取其上次提交的所有内容 
  3. 没有被提交的内容由所有分支(被git管理和没有被git管理)共享(即哪个分支提交这个内容，这个内容就归谁)
  
  注意
  	git branch 可以查看被git管理的所有分支
  */
  ```

  

+ git 工具
  git里每一个分支都是一个单独的树杈，每个树杈都可以生成，提交文件，每个文件的存储位置相同，存储信息不同，当你打开git文件夹时，只有切换都对应的分支，里面的文件才能展示对应的内容

  基本使用

  ```js
  /*
  再linux操作中，命令行提示，没有提示代表成功，有提示代表错误
   鼠标反键 点击 git bash 进入 linux 虚拟命令行
   交互第一步告知你的id和邮箱 $ git config --global user.name"zbcz"
    git config --global user.email "1103644163@qq.com"
   如果想要修改你的id或邮箱 在执行依次命令即可覆盖添加
   
   1.把文件夹变为git仓库(文件为红色)
    git init (把当前目录下的文件夹变为git仓库)
    没有任何提示标识成功
    若未初始化会提示 fatal:Not a git repository
    失败会提示 fatal: pathsper xx did not match any files
    可能会出现的警告： warning: LF will be replaced by CRLF in xxx ，原因：linax和window空格不一致 解决方法：git config -gobal core.autocrlf false
   2. git add xxx 把内容提到暂存区(文件为绿色，可以选择单个添加 git add a.txt 也可以全部添加 git add * 或 git add .)
     git status 可以查看暂存区内容
     
   3. git commit -m 'xxx' 把暂存区内容提交到版本区(文件不显示，标识提交成功，git commit 标识把暂存区去拿不内容提交到版本区,-m 标识给该信息添加描述)
   
   4. 文本差异对比
    git diff 比较暂存区与工作区，(不要使用 git add 添加，否则无法对比)
    对比信息 
    a/... b/... a标识之前的数据  b标识修改后的数据
    index dekjlsd .. sds jjij 哈希值唯一地址编号
    --- a/... 之前的数据用 -号标识
    +++ b/... 之后的数据用 +号标识
    @@ -1,3 +1,6 @@ 。 -1，3标识修改之前所占用的行数，+1,6标识修改后的占用行数，下放会列出差异，绿色标识修改后的数据 
    git diff -cashed 比较把版本区与暂存区
    git diff master 比较版本区与工作区
    
   5. 日志&&版本号
   git log 显示从最近到最远的所有提交日志
   git reflog 显示每次提交的(commit) 的commit id
   
   6. 版本回退&&版本穿梭&&版本撤销
    git reset --hard HEAD^ 版本回退(回退一次提交 ，一个^表示会退一次，二个^表示回退两次)
    git reset --hard 版本号 回退到指定版本号 的 commit id 版本
    git reset HEAD 用版本库中的文件区替换暂存区的全部文件
    git checkout -- x.txt 用暂存区指定文件去替换工作区的指定文件(危险)
    git checkout HEAD x.txt 用版本库中的文件替换暂存区和工作区的文件(危险)
    git rm --cached x.txt 从暂存区删除文件
  */
  ```

  linux命令

  ```js
  /*
   再哪个目录打开linux控制台，linux会自动跳转至该目录位置
   绿色文件都是快捷方式 ，紫色都是文件夹 白色都是文件
   1. mkdir xxx 新建文件夹
   2. vi x/txt 新建/编辑目录(无则新建，有则编辑 Visual editor) 
      输入： i 进入编辑模式  
      输入： ESC:wq 保存并退出
      输入： ESC:q! 不保存退出
   3. cd xxx 进入 xx目录
      cd .. 返回上一级目录
      ls 列出当前文件夹中所有文件
      pwd 显示当前目录
      cat x.txt 显示文件内容
      clear 清屏
      
   4. 删除文件 git rm x.txt(必须要经过git管理(需要放到版本区))
   5. 删除文件夹 git rm -r xxx (必须要经过git管理(需要放到版本区)，且必须要有内容)
  */
  ```

  git概念

  ```js
  /*
   git模型分 工作区 版本去 暂存区
   工作区 项目文件夹，进行：增，删，该的区域(git add xxx 将工作区文件添加到暂存区)
   暂存区 暂时保持，即交给git进行版本管理(git commit -m '提交信息' 将暂存区文件添加到版本区)
   版本区 已经被git版本控制
  */
  ```

  

+ git 分支

  ```js
  /*
   主分支：master----代码经过了开发人员单元测试，以及测试人员详细的一套测试之后，确实没问题的代码
   开发分支：dev ----------- 开发人员所有的代码提交到此分支
   测试分支：test -------- 测试人员专用
   里程碑分支 tags ------ v1.0.0,第一位大版本号 第二位，第三位：bug解决 1.12.4
   测试的种类：回归测试 压力测试 性能测试
   
   1. 泛指管理命令行
   git checkout -b dev 创建 dev分支，并切换到dev分支
   git branch 查看当前分支（列出所有被git本地仓库管理的分支，没有提交内容的分支不会被保存）
   git checkout master 切换分支
   git merge dev 合并dev分支到当前分支，dev源文件不受影响
   git branch -d dev 删除指定分支
   git diff branch1 branch2 显示出两个分支之间所有有差异的文件的详细差异
   git diff branch1 branch2 -stat 显示两个分支之间所有有差异的文件列表
   git diff branch1 branch2 xxx 显示指定文件的详细差异
   
   注意
   	进行分支切换时，首先会去当前分支的仓库里拉取之前提交的数据，如果之前没有提交东西就会去复制上一个分支的内容用于使用
  */
  ```

+ git冲突处理

  ```js
  /*
   挡在分支合并时，如果多个人操作同一个文件，则会产生冲突且会分支会以 master|MERGING 提示，冲突的解决方式 手动处理 ，webstorm处理
   手动处理
   打开文件删除 HEAD ===== >>>> 等冲突标志，重新拼接代码
  */
  ```

  

+ GitHub

  ```js
  /*
   GitHub 是一个Git项目托管网站
   GitHub 能够分享你的代码或者其他开发人员配合一起开发，是一个基于Git的代码托管平台，Git并不像SVN那样有一个中心服务器，目前使用Git命令都是再本地执行，想要完成协作书写同项目，需将数据放到一台其他开发人员能够连上的服务器上(GitHub)
   
   1. 关联本地库，上传文件到github
   首先创建一个文件夹，把这个文件夹初始化为git文件夹，再操作完毕后，再github上创建一个新仓库，再转到git上，填写 git remote add pigo(该值为仓库别名) https://github.com/we-del/demo_test.git
   git push -u(第一次上传需要写) xxx(仓库别名) master(上传的分支)，关联后，文件的更改后(执行完 add commit-m)直接上传即可
   	注意
   		push操作可以为 把当前分支的内容上传到指定分支(本地分支和远程分支要一致，否则无法提交)
   
   若推送依次后，仍然需要登录账号和密码，则输入 git config --global credential.helper store . 即可解决
   
   如果关联仓库时，关联有误可输入 git remote remove xxx(和第一次输入的名称一致) 即可解除关联
   
   git remote -v 查看当前绑定地地址 
   git remote remove 库别名(一般是origin)  ，即可删除之前绑定的连接
  
   如果 git已创建分支，要指明推送的是哪一个分支 ,如 git push -u origin master(指明分支，且要与当前所在分支保持一致)
   
   
   2. 获取github上的内容
     git pull xxx(仓库别名) master(上传的分支) 。即可获取github仓库里的全部内容,且内容会自动合并，可能会产生冲突
     git fetch xxx(仓库别名) master:xxx(为本地的分支)  把该代码抓取到新的一个分支，并传给github (此方式可以便于完整代码的分支转移)
     注意
     		pull 会拉取指定分支的全部文件到当前分支，并完成合并，此过程可能产生文件冲突，需要解决
     		petch 会拉到取指定的全部文件到指定的本地分支，因此特性 我们可以将可能出错的内容使用fetch单独拿去到一个独立分支，然后使用 git diff xx(当前分支) xx(存储着petch拉取的内容的临时分支) 。进行差异对比来处理文件冲突
     		git pull 可以拉去关联库的全部分支(包括新增分支（隐式拉取，本地不会显示，但可以切换到该分支）)
     
     
   3. clone 网站项目(下载)
   当新加入一个公司时，需要连接公司的内部github项目，这是获取https clone地址 输入 git clone http://....(刚刚克隆的地址) ，即可和这个项目产生连接，这样就可以更新或上传项目了
  
   输入 git clone http://...  可能会报错 把http 该为 git即可
  
   clone下来的git项目，无法使用 git branch 查看当前分支，但可以使用 git branch xxx 切换到当前分支，需再次使用 git pull 获取分支数
   
   
   
   4. git密码切换
   当你需要改变你的git之前输入过的账号和密码时，你需要先去 windows凭据管理器(控制面板里)，找到github栏 删除之间保存的账号和密码，这样就可以重新输入了
   
   
  注意
  	如果git账号密码正确但连接不上github，可尝试更新git解决此问题
  	最新版git连接github已经不支持 账号+密码登录连接到github，而是需要使用账号+token方式登录(token获取方式百度即可)，若还不成功选择 ssh连接github即可
  	再提交数据时，是把本地当前分支的内容提交到远程的指定仓库
  	
  */
  ```

  webstorm操作 git

  ```js
  /*
  基本操作
   打开一个被git管理的目录时，可以再webStorm右下角选择分支，即git基本操作
   再当前目录创建一个.gitignore 文件,可在其内编写提交代码时忽略的文件或文件夹(每一行即一个忽略文件或文件夹)
  
  提交文件(此提交是提交到本地仓库)
   可单独提交，也可全部提交
   	单独提交
   		选中需要提交的文件，右键git,选择 add, 和commit，即可 提交指定文件
  	全部提交
  		选中需要提交的文件夹，右键git,选择 add, 和commit 即可
  		
  将本地仓库的文件上传(push操作)到远程仓库(github)
   点击右下角选择分支，右键push提交该分支内容到远程仓库
  
  将远程仓库(github)文件更新到本地仓库(点击update,相当于pull操作)
  
  webstorm处理文件冲突，当文件存在冲突时，按webstorm引导即可解决冲突
  
  webstorm版本回退
  	选中指定文件，右键git选择show history即可查看文件的提交记录，选择具体的某次操作点击Reset Current..即可回退到该状态
  
  webstorm状态回滚
  	webstorm状态回滚可以回到上一次 update(pull)的一次状态 
      使用方法
      	选择文件夹，选中git,点击rollback 选中需要回滚的文件，即可回到初始状态(刚pull下来文件时的状态)
      	
  注意
  	当进行 push,pull操作时，首先要确保本地仓库的文件全部已经提交到版本区，这样才能进行合并等操作
  */
  ```

  

+ github说明

  ```js
  /*
   Overview 你关注的人的动态
   repository 代码仓库
   project 工程 
   package 包
   wiki 给你的代码库添加说明文件
   security 安全 
   
   1. 创建团队
   new organization 选择团队规格(free spend)
   fork 解决别人的代码
   issue 别人的评论
   pull requests 别人请求修改你的代码
  */
  ```

  搭建服务器

  ```js
  /*
   1. 域名
   2. 服务器(自塔7，8万)
   3. 域名绑定到服务器上
   4. 编写一个个人主页
   
   github 仓库(免费域名，然后自己买一个域名替换掉github域名)
   https://www.github.com
   点击 Setting GitHub pages source 选择master branch 即可把仓库转换为一个个人博客 ，等页面刷新完后，再次点击次页面会刷新出来一个地址
  */
  ```

+ 提交代码规范

  ```js
  /*
   每次提交前，先更新，再提交
   敏感时间点，一定及时更新文件(功能完成，下班，上班，测试时间点)
   多提交，避免“只关注写代码，不关注提交”的现象
   每次提交必须书写清晰的提交说明
   不要提交不能通过编译的代码
   不要提交自己不明白的代码
   慎用锁定功能(尽量避免使用锁，不轻易解锁上锁的文件)
   不要提交本第自动生成的文件，文件夹
  */
  ```

![image-20220318160139219](D:\typora_import_images\typora-user-images\image-20220318160139219.png)

![image-20220318160428632](D:\typora_import_images\typora-user-images\image-20220318160428632.png)


## 1.2 阻塞和性能优化

+ 性能优化(前端)

  ```js
  /*
   主流的，面向Chrome
   浏览器内核 
   Safari---> Webkit
   IE ---> trident
   chrome ---> webkit分支引擎 -->Blink
   Opera Chrome用什么Opera用什么
   
   算法有： 先来先服务，优先级，时间片轮转
   
   进程有：
   Browser进程：浏览器的主进程，负责浏览器界面显示和管理
   renderer进程： 负责页面渲染
   
   一个渲染引擎主要包括：HTML解析器，CSS解析器，javascript引擎，布局layout模块，绘图模块。。。
   
   大致渲染过程
   遇到HTML标记html进行解析 ,遇到style/link标记css进行解析，遇到script，调用javascript引擎进行解析，最后将DOM树和CSS树合并成一个渲染树，最后呈现到屏幕上
   
   style标签中的样式由html解析器进行解析，页面style标签(html解析)写的内部样式是异步解析的(容易产生闪屏现象，即样式实时变化感)，浏览器加载资源是异步的，link(css解析)加载是同步的有良好的用户体验，不会呈现多种颜色
   
  
  */
  ```

  ![image-20220318161158322](D:\typora_import_images\typora-user-images\image-20220318161158322.png)

  ![image-20220318161413880](D:\typora_import_images\typora-user-images\image-20220318161413880.png)

  ![image-20220318161439801](D:\typora_import_images\typora-user-images\image-20220318161439801.png)

  阻塞

  ```js
  /*
    1.阻塞
   关于css阻塞
   link引入的外部css由css解析器解析会阻塞浏览器渲染及js代码。不阻塞DOM解析
   style标签中的样式不会阻塞页面渲染和dom解析，但会出现闪屏现象
   
   关于js阻塞
   阻塞后续DOM解析，js操作可能对html标签产生影响，所以会阻塞DOM解析，避免浪费内存，阻塞页面渲染，阻塞后续js执行
   
   css解析和js执行是互斥的,因为它们都可以对同一目标进行操作，所以会按顺序执行。任何阻塞都不会影响外部资源获取。webKit和firefox都进行了“预解析”优化，如果其他js没有操作dom元素或者style会提前被预解析，当执行到此js时会立刻完成操作，而不会消耗大量时间去读取
   
   js再页面执行完立刻执行的三种写法 window.onload , DOMcontentLoaded (前两个都为时间), 或再script标签后写上 defer属性
   
   以上是一个完整的选软过程，但再现代网页里这些都是动态的，意味着再渲染完成之后，优于网页的动画或用户的交互，这一过程会不断重复执行渲染(重绘，重排)
  */
  ```

  ![image-20220318162231654](D:\typora_import_images\typora-user-images\image-20220318162231654.png)

  图层与重绘重排

  ```js
  /*
   1. 图层
    浏览器再选软一个页面时。会将页面分为很多个图层，图层有大有小，每个图层上有一个或多个节点
    在渲染DOM时，浏览器实际在做：
    （1）获取DOM后分割为多个图层
    （2）对每个图层的节点计算样式结果
    （3）为每个节点生成图形和位置
    （4）将每个节点绘制填充到图层位中
    （5）图层作为纹理上传至GPU
    （6）组合多个图层到页面上生成最终屏幕图像
    
    创建图层的条件
    （1）拥有 3D变化的CSS属性
    （2）使用<video>标签
    （3）使用 <canvas></canvas>标签
    （4）CSS3动画的节点
    （5）拥有CSS加速属性的元素(will-change)
    
    2. 重绘
    重绘是一个元素外观的改变所触发的浏览器行为，例如改变outline等，浏览器会根据元素的心如行重新绘制，使元素呈现新的外观，重绘不会带来重新布局，所以不一定伴随重排。
    重绘重拍是以图层为单位，如果图层中某个元素需要重绘，那整个图层都需要重绘，因此应让这些 变化的元素 单独拥有一个自己的图层(以免造成原本图层重绘重拍)，不过浏览器会为变化的元素自动创建图层
    
    3. 重排(回流)
    渲染对象再创建完成并添加到渲染树后，并不包含位置和大小信息，计算这些值的过程称为 布局或重排
    重绘 不一定需要 重排 (颜色改变，只改变重绘)，重排 大多情况会导致 重绘(重新排列了需要重新绘制图形)
    
    常见触发重排的操作
    重排消耗的资源比重绘多得多，特别是在基于H5开发的网页小游戏上，那里运用了非常多的重排导致页面算率过大，导致耗电增加，手机发热等
    以下操作会产生重绘(repaint)重排(reflow)
    （1）增删改DOM节点时
    （2）移动DOM位置时
    （3）修改CSS样式时
    （4）Resize窗口时
    （5）修改网页默认字体时
    （6）获取属性时(width,height...),(浏览器会自动刷新图层，获取该值然后返回值)
    （7）display:none 会触发重排，visibility:hidden会触发重绘
    
    如果再浏览器上看到 layout标识重排  paint标识重绘
    
    优化方案(重绘重排)
    1. 元素位置移动变化时，尽量使用CSS的transform来代替 top left等操作
    2. 使用opacity代替visibility,opacity单独使用会触发重绘和重排，但配合能够创建图层的元素时，不会触发重绘和重排 如 transform和opacity搭配使用
    变换(transform)和透明度(opacity)的改变仅仅影响图层的组合
    3. 不使用table布局
    4. 将 多次改变样式属性的操作合并成一次 操作，不要一条一条地修改DOM样式，预先定义好class，然后修改DOM时操作className
    5. 将DOM离线后在修改 ， 由于display 属性为none地元素不在渲染树中，对隐藏地元素操作不会引发其他元素地重排，如果要对一个元素进行复杂地操作时，可以先隐藏改元素，等到操作完成后再显示，这样只在显示和隐藏时触发2次重排
    6. 利用文档碎片 ，vue使用了该方式提升性能
    7. 不要把获取某些DOM节点地属性值放再一个循环里当成循环地变量
    8. 动画实现中，启用GPU硬件加速 transform: translateZ(0)
    9. 为动画元素新建图层，提高动画元素地z-index
    10. 编写动画时，使用let id= requestAnimationFrame(function),此函数会在页面下一次重绘页面之前执行，此函数地移动帧率可以与屏幕分辨率保持一致 ，cancelAnimationFrame(id(上文开启地动画))可以停止动画
    例如： demo(){console.log(1)}  requestAnimationFrame(dome)，可以配合定时器使用(构成完美帧)，需再定时器里执行 requestAnimationFrame(dome)，因为此方法只会执行一次
    
    */
  ```

+ CDN

  ```js
  /*
   CDN是一个服务器集群，它可以管理，分配访问地址到他离该用户最近地服务器集群，当一个地区地服务器集群故障时，CDN会把本该访问这一地点地请求，转给另外一个离它最近地地区服务器集群，也可以阻止网络攻击
  */
  ```

+ 优化核心

  ```js
  /*
   1. 使用CDN节点进行外部资源加速
   2. 对css进行压缩(利用打包工具，webpack,gulp)
   3. 减少http请求数，将多个css文件合并
   4. 优化样式表的代码
  */
  ```



+ 函数节流&&函数防抖

  ```js
  /*
  前言
  	防抖和节流是使用定时器和if语句的巧妙配合来完成页面控制
  	防抖和节流的出现就是为了避免用户在一次连续操作中对服务器或前端产生过多的行为而出现的，比如 防抖就是为了让一段连续的时间内，减少向服务器发送请求的次数，从而减少服务器压力 ； 节流的出现是为了限制用户在一段时间内只能进行一次操作，有效解决了页面的怪异行为
  
   1. 函数防抖
    延迟要执行地动作，若再延时地时间内，再次触发，则取消之前开启的动作，重新计时(清空队列)
    举例： 电脑再1分钟无操作情况下进入休眠，挡在此计时期间有移动则从新计时
    应用： 搜索框用户输入字符串 (一般用setTimeOut，延时200||300)
    
      let inp = document.querySelector('input');
      let id = null;
      inp.addEventListener('keyup', () => {
          if (id!=null) clearTimeout(id);
          id = setTimeout(() => { getAjax(inp.value)}, 300);
      })
  
      function getAjax(data) {
          console.log(`您搜索的是${data}`)
      }
    
   2. 节流
   设定一个特定的事件，让函数再特定时间内只执行一次，不会频繁执行
   
    let canLog = true
      document.documentElement.onscroll = function(){
          if(canLog){
              console.log(1)
              canLog =false
              setTimeout(()=>canLog =true,2000)
          }
      }
  */
  ```

  浏览器存储

  ```js
  /*
   Cookie,SessionStorage,LocalStorage都可以存储本地数据
   sessionStorage 和session不是一个概念
   sessionStorage 是浏览器用于存储数据的容器，前端人员简称session
   sessioin是会话存储，服务端一种存储数据的方式，后端人员简称session
   
   SessionStorage 和LocalStorage 简称为 Web Storage,存储大小一般为5-10MB
   查看Cookie等方法： F12 -->Application
   
   1. sessionStorage API ,sessionStorage会再浏览器关闭时自动释放
    sessionStorage.setItem('key','string') // 必须是字符串，可以使用 JSON.stringify(xxx)把数据转换为字符串数据进行存储
    JSON.parse(sessionStorage.getItem('key')) // 获取key的value
    sessionStorage.removeItem('key') // 移除key 键值对F
    sessionStorage.clear() // 清空全部sessionStorage
    
   2.LocalStorage API,关闭浏览器不会自动释放，直到用户清除LocalStorage数据
     localStorage.setItem('key','string') // 必须是字符串，可以使用 JSON.stringify(xxx)把数据转换为字符串数据进行存储
    JSON.parse(localStorage.getItem('key')) // 获取key的value
    localStorage.removeItem('key') // 移除key 键值对
    localStorage.clear() // 清空全部sessionStorage
    
   3. store事件
    store对象发生变化时触发，在同一个页面内发生的改变不会起作用，在相同域名下的其他页面发生的改变才会起作用
    event对象下的常用属性：
    key : 修改或删除的key值，如果调用clear()为null
    newValue : 新设置的值
    oldValue : 上一次设置的值
    url : 出发该脚本变化的文档url
    storageArea: 当前的storage对象
    
    使用方法：
    window.addEventListener('storage',()=>{})
    
   注意
   	session存储按域名来隔离，即相同域名(端口或地址)g用一个session存储
   	localStorage存储实际上是存储的一
  */
  ```

  

+ 缓存

  ```js
  /*
   浏览器再本地磁盘上将用户之前请求的数据存储起来，当访问者再次需要改数据的时候无需再次发送请求直接从浏览器本地获取数据
   缓存的好处： 减少请求个数，节省带宽，减轻服务器压力，提高浏览器加载速度
   
   1. 强缓存
   不会向服务器发送请求，直接从本地缓存中获取数据
   
   2. 协商缓存
   向服务器发送请求，服务器会根据请求头的资源判断是否命中协商缓存，如果命中，则返回304状态码通知浏览器从缓存中读取资源，否则从服务器获取资源
   
   3. 缓存中的header参数
   	请求： 请求头 + 请求数据
   	响应： 响应头 + 响应数据
   	
   （1）强缓存的header参数
   	expires: 为http 1.0的规范，它规定了一个GTM格式的绝对时间字符串，如果发送请求的事件再expires规定事件之前那么本地缓存始终有效，反之失效，会重新向服务器获取资源
   	cache-control" max-age=number ,为http 1.1时退出的header信息，利用该字段的max-age值进行判断，是一个相对值(秒数),资源第一次请求时间和Cache-Control设定的有效期，计算出一个资源过期时间，再拿这个过期时间跟当前的请求时间比较，如果请求时间再过期时间之前，则命中缓冲，反之则重新向服务端获取数据 。 再项目中，如果服务端有数据更新则直接把cache-control设置为0即可，一般情况下 cache-control时间和expires时间一致
  
  注意： 当cache-control与Expires共存的时候 cache-control优先级高
  
   （2）协商缓存的header参数
   	 协商缓存都是由服务器来确定缓存资源是否可用，所以客户端与服务端要通过某种标识来进行通信，从而让服务器判断请求资源是否可以缓存访问
   	 浏览器在第一次跟服务器请求资源时，服务器再返回资源的同时会返回一个包含Last-Modified的header头，这个Last-Modified包含的时这个资源在服务器上最后修改的时间，当浏览器第二次跟服务器请求资源时，会带上Last-Modified，传到服务器上的请求变为 If-Modified-Since的header头，服务器会对比这个数据，如果和上次访问本服务器资源的最后修改时间一致，则返回304 Not Modified ，标识命中缓冲，直接从本地获得数据，反之重新向服务器端获取数据，并返回最新的资源最后修改时间
   	 
     	 Etage(请求时发送)/If-None-Match(相应时发送)
       两个值由服务器生成的每个资源的唯一标识字符串，只有资源变化时这个值才改变，其判断过程与Last-Modified/if-Modified-Since
       HTTP1.1中Etag的出现主要是为了解决几个Last-Modified比较难解决的问题:
      ① 一些文件也许会周期性更改，但他的内容并没改变(仅仅改变修改时间)，这个时候我们并不希望哭护短认为这个文件被修改，而重新GET获取
      ② 某些文件修改非常频繁，比如再瞄一下的时间内进行修改，比如再1s内修改了n次，if-Modified-Since能检查到s级，但无法判断s以下的时间
      ③ 某些服务器不能精确的得到文件的最后修改时间。
      
   综上所述，只要发起请求时 携带过去的id或时间一致就能命中缓存
  */
  ```

  

## 1.3 Node

**模块化:将一个复杂的程序一句一定规则拆分成几个模块，并进行组合，块内部数据只向外部暴露一些接口与外部通信**

+ 没有模块化之前函数的调用

  ```java
  /*
   1. 使用引入方式调用，会造成命名污染，不符合迪米特法则
   2. 使用命名空间包裹(即对外暴露一个对象，对象里封存了一系列属性)，解决了命名污染，但不符合迪米特法则，外部可以修改内部的数据
   3. 使用IIFE模式(匿名函数自调用),结局了命名污染，但模块关联不方便
   	// IIFE使用
   	((w)=>{
   		let data = "123";
   		let test = ()=> console.log(data);
   		let module1 = {test};
   		w.module1 = module1; // 此时该对象就向外暴露了
   	})(window)
  */
  ```

  

Node是一个js运行环境，他把每个函数功能都封装成了一个个模块，外部只能使用模块里的方法，而不能操作其内部的属性，这样有效解决了命名污染等问题

模块抛出的东西一般都用{}方式抛出 避免命名冲突

Node中每个包模块下的函数对应一个功能，再次功能基础上进行额外的操作

+ npm

  ```js
  /*
   一个Node包管理器，是一个强大的模块下载工具
   
   1. 基本包管理命令
   创建package包， npm init ,包名不能有中文，大写字母，和npm上已经存在的包名
   npm install xxx 安装一个包的命令，在安装好后，npm会自动再当前文件夹生成一个 package-lock.json文件，用于缓存下载地址
   npm install 安装package.json里所有用到的依赖（在上传github时，只用上传这个json文件即可，利用npm下载的文件不需要上传，让使用者直接npm install 下载package里的依赖即可）
   
   npm config set registry http://registry.npm.taobao.org/ 切换npm下载源
  */
  ```




### 1.3.1 模块引入

+ 模块化规范（CommonJS）

  ```js
  /*
   1. commonJS 双端模块化规范  Node和brower下都能运行
      （1）基本语法
       暴露模块  module.exports = value || exports.xxx = value ，暴露的本质指暴露module.exports 或 exports所指向的那个对象 。module.exports和exports.xxx不能混写，如果混写以 module.exports为主
       
       引入模块 require('./modules/we') 自己的代码需要以相对路径或绝对路径查找 || require('uniq') npm下载的模块可以直接引入文件夹名，默认去他自己下载的文件夹里找
       
       
    src: 源代码，没有经过压缩检查等操作的代码
    dist: 代码经过压缩，扩展前缀
    
    注意
    	commonJs是双端模块化规范，即web端和服务器端都可以使用
    	一个js文件就是一个模块,(每个模块下默认会向外部暴露一个空的对象，我们可以往改对象下挂载属性)，我们在引入一个模块时实际上就是引入别人暴露出来的对象(或数组等)
    	模块在引入时是同步(顺序)加载的，每个模块只会被加载一次(即有两次或两次以上引入相同的模块也只会引入一次)
    	在引入模块时，如果引入自定义模块需要指名路径(如 ./a.js) ,如果没有指明路径则会按第三方模块方式引入(如 /aa/xx 此时会去node_module文件查找该模块)
    	在node环境中没有window对象
  */
  ```

+ browserify

  ```js
  /*
   Node服务端和网页端交互命令大相径庭，命令之间有的不互通，而我们又想要其中的一些命令可以互通，如 require属性，此时就用到browserify插件
   1. 安装
   npm install browserify -g(global全局)
   2. browserify ./index.js(node模块) -o js/app.js(dom模块) 。此时require引入的模块dom就能识别了 ， 每次node模块下的js重写了一个模块，就要执行一次browserify 操作，把最近的node模块代码关联到dom模块。
   
   使用node时注意node打开的当前目录是否为当前工作目录
  */
  ```

  

+ ES6暴露模块&&引入模块

  ```js
  /*
    1. export
     三种暴露方式 export 隔行暴露
     
   
     有三种暴露方式
     (1) 单个暴露只能逐条暴露 export let data = [231]
     export function demo(){console.log('我是dome')}
     
     (2) 统一暴露 let arr=[1,2,3,4]
      function demo2(){ console.log('我是dome2')}
      function demo3(){console.log('我是dome3')}
      export{arr,demo2,demo3}
      
     （3）默认暴露 只能暴露对象(只能暴露一次，多次暴露会报错)，且不能有变量和等号
         export default {
          name: '张三',
          age: 15,
          speak(){
              console.log('我${this.name}能说话')
          }
      }
  
     （1）和（2）暴露方式只能用 import {xxx,yyy} from '/module1'，且引入的内容和暴露接收的变量需一致，功能和模块里的一致
     （3）只能用 import module3 from './module3' ，获取值用 module3.xx方式获取，相当于调用对象里的属性和方法
     
    2. import
     有两种引入方式
     常规引用：
     import {xxx,yyy} from '/module1'
     import module3 from './module3'
     对象引用：
     import * as gather(别名) from './module1' (把module1里的所有引出属性和方法封装到 gather对象下，通过对象操作gather.xxx方式获得值)
     import module3 from './module3' 也支持 as 别名写法但没有必要这样做，因为 export default 只能引出一个对象
     当统一引入时也可以通过再引出文件时给变量起别名，其他js文件引入此文件时，可以以这个别名来获取该变量(少用)
     
     混合引用(也可以用 import * as gather(别名) from 'str'方式引用)
     import m4,{arr,bar,str} from './module4' (此模块为混合写入，及单个引入，统一引入，默认暴露出现在同一模块中，m4接收默认暴露参数(必须放到最最前面)，{arr,bar,str}接收统一和单个引入参数)
    
    注意
    	export 本质上和module.exports 向外暴露对昂本质上是一致得 ,但es6模块引入比reuiqre引入更有优势，es6模块引入可以做到按需引入，大大节省了空间
    	
     
     
    3. 安装babel-cil 
    因ES6语法中的export和import浏览器并不认识，所以就需要npm下载babel
    babel 插件可以将 es6转换为 es5  ,将 jsx 转换为 js
    (1)全局安装：npm install babel-cli browserify -g (如browserify安装了则本次不用安装)
    (2)然后局部安装： npm install babel-preset-es2015 . 
    (3)在当前文件夹下创建.babelrc(rc == run code)文档 输入 {
     "presets":["es2015"]
    }
    (4)babel js/src -d js/dist (把js下的src文件夹下的js文件编译后放到了dist文件夹)
    (5)browserify js/dist/app.js -o js/dist/index.js 。即可转换成Dom能够识别的js代码
    (6)每次node模块下的js改动时，需要重新翻译到指定文件(键盘↑↓可以转到指令)
  */
  ```

  

+ AMD模块开发

  ```js
  /*
   定义没有依赖的模块
   define(function(){
      let data = 'abvc'
      function getDataL(){
      return data.toLowerCase()
      }
      function getDataU(){
      return data.toUpperCase()
      }
   	return {getDataL,getDataU}
   })
   
   定义有依赖的模块
   // 再AMD模块中，主js文件(入口文件) 需要写一个特殊的配置对象
     requirejs.config({
     baseUrl: './js'(公共路径), // 公共路径以Index主页面位置开始
       paths:{
       	module1(别名):'./module', //  别名可以自更改，但再引入模块函数时需对应相同别名，引入的模块不能有后缀
       	jquery: './lib/jquery-1.10.1' // jquery再引入时，必须小写
       }
     });
     requirejs(['module2'],function(m2){
     	m2();
     })
   // 引入模块和函数的形参一一对应
   define(['module1'],function(m1){
   	m1();
   })
   
    再html文件也要引入require配置文件,这样才能解析并执行
    <script data-main="./app.js(配置文件路径，再配置文件中引用其他模块)" src="./lib/require.js"></script>
  */
  ```
  



+ CMD模块开发

  ```js
  /*
   定义没有依赖的模块
   define function(require,exports,module){
   	exports.xxx =value
   	module.exports = value
   }
   
   定义有依赖的模块
   define(function(require,exports,module){
   	// 同步
   	let module2 = require('./module2')
   	// 异步
   	require,async('./module3',function(m3){
   	
   	})
   	exports.xxx= value
   	
    })
   入口js配置
   define(function(require){
   let m1=require('./module1')
   let m4 = require('./module4')
   m1.getData()
   })
   // 页面引入模块
   <script src="js/libs/sea.js"></script>
   <script> seajs.use('./js/main')(入口js)</script>
  
  */
  ```



+ Node.js是一个基于Chrome V8 引擎 的 javaScript 运行环境

  ```java
  /*
   优点：异步非阻塞I/O(I/O线程池) ,特别使用与I/O密集型应用，事件循环机制，单线程，跨平台
  
    I/O线程池，再一个命令被唤醒时，它会触发然后进入线程池，进入一个待命状态，下次再出发此命令，直接去线程池里查找此命令，而不需要重写加载
  
    I/O密集型，只要频繁的操作I/O就是I/O密集型
  
    CPU 密集型 频繁的发起高时延请求
  
    跨平台(PC端) 能再多种浏览器上运行
    	
  
  java服务器处理线程池，专线专用(一条请求对应一条线路)，服务器不够推服务器，用专线专用解决高迸发（同时解决I/O密集型和CPU密集型，缺点，贵）
  
  node服务器处理线程池，专线多用(多条请求对应一条线路(足够强大))，用回调解决高迸发。如果一条请求占用时间过长，则全部请求响应速度会明显变慢，java服务器则不会出现这种问题(可以解决I/O密集型，难以解决CPU密集型)
  
  
  
  缺点
  	回调函数数据嵌套太多，太深(使用Promise解耦)
  	单线程，处理不好CPU 密集型任务
  */
  ```

  ![image-20220319152947603](D:\typora_import_images\typora-user-images\image-20220319152947603.png)

  





+ Node文件函数

  ```js
  /*
   Node文件本质也是被一层系统函数给包裹着的(documents.callee.toString())可以查看,支持模块化和隐藏内部实现
   包含函数形如：function (modules,exports,require,__filename,__dirname){...js内容..}，也就是我们在node内能使用modules,exports等得原因(在该文件执行时，会被node调用)
   
   注意
   	__filename 可以拿到到当前文件得绝对路径(string形式)
   	__dirname 可以拿到当前目录得绝对路径(string形式)
  */
  ```



+ 浏览器端&&Node端组成

  ```js
  /*
    浏览器端
    	主要由BOM(BOM指widnow对象) ,DOM(指document对象),ES规范等组成
    
    Node端
    	主要由ES规范，global全局对象等组成
    	Node中禁止函数的this指向gloabl，而是指向一个空对象 
    	
    gloabl全局对象下常用得属性
    	clearImmediate  清空立即执行函数
    	clearInterval   清除循环定时器
    	clearTimeout    清除延迟定时器
    	setImmediate    设置立即执行函数
    	setInterval     设置循环定时器
    	setTimeout      设置延时定时器
  */
  ```



+ 事件循环模型

  ```js
  /*
   第一阶段：timers(定时器阶段 包括setTimeout,seInterval)
      开始计时，执行定时器的回调
   第二阶段：pending callbacks（系统阶段）
   第三阶段：idel,prepare（准备阶段）
   第四阶段：poll （轮询阶段，核心）
      如果回调队列里有待执行的回调函数，从回调队列中去除回调函数，同步执行，直到回调队列为空或达到系统极限值
      如果回调队列为空，则再此阶段停留，等待回调函数插入队列
      如果有设置过setImmediate进入下一个check阶段。
      若定时器到了指定时间，则它也进入下个check阶段，为了走第五阶段，随后第六阶段，然后回到第一阶段
   第五阶段: check（专门用于执行setImmediate所设置的回调）
   第六阶段：close callbacks(关闭回调阶段)
   process.nextTick() 用于设置立即执行函数，能在任意阶段执行，异步轮询里所有人都必须让他先执行然后先出队列
   
   当主线程有代码时，轮询顺序为 主->nextTick->setTimeOut->setImmediate
   当主线程没有代码时，轮询顺序为 nextTick->(setTimeOut->setImmediate,两种定时器先后可能发生变化)
  */
  ```


### 1.3.2 包管理器

+ 包与包管理器

  ```js
  /*
   Node.js包遵循CommonJS规范，包由包结构和包描述结构组成
   在初始化npm时，初始化名不要和文件夹名以及即将安装的包名一致，会产生冲突
   
   1.包结构
    包实际上是一个压缩文件，解压后为目录，符合CommonJS规范目录应该包括文件夹：package.json , bin 可执行二进制文件 ，lib js代码，doc文档,test单元测试
    
    2.包描述文件
    当输入 npm init 后(名字不能有中文，大写字母，不已数字开头)，即可把该文件夹转换为 npm包目录，需要添加描述包括name,version,descript,keyword,git repository等
    
    3. npm 常见命令
    安装可使用 npm install xxx --save 或 npm i xxx -S 或 npm i xxx
    局部安装的第三方包再 node_module文件夹里，安装完毕后会产生一个Package-lock.json,用来缓存你的用到的包的地址，在下次下载时会直接到该地址下载，有效提高下载速度，同时包名会自动写入package.json（生产依赖dependencies）里。生产依赖(部署到服务器上的代码)
    
    使用npm install xxx --save-dev 或 npmi xxx -D 安装包将写入
    开发依赖(devDependencies)里(正在编写的代码)
    
    在开发时把语法检查等和项目内容无关的包需要用npm i xxx -D下载，项目内容中用到的包使用npm i xxx 下载，当在项目即将上线时，打开package.json删除devPackage{}模块，这样可以避免项目上时因无用代码过多而导致的资源加载过慢问题
    
   全局安装：npm i xxx -g 。一般带有指定集的包需要全局安装，如browserify,如果该包不带有指定则无需全局安装，查看全局安装的位置 npm root -g
   
   指定版本安装： npm i xxx@1.10.8 (@符跟版本号)
   查看包的版本： npm view xxx(jquery) versions
   删除包： npm remove jquery
   查看包版本： npm ls xxx
   
   关于版本号说明(x默认为最新版)
   ^3.x.x 锁定大版本，以后安装包的时候，保证包是3.x.x版本
   ~3.1.x 锁定大中版本，以后安装包时，保证包是3.1.x版本
    3.1.1 锁定完整版本，以后安装包的时候，保证是此版本
  */
  ```
  
  
  
+ cnpm

  ```js
  /*
   cnpm是淘宝团队开发的一个稳定版的npm包，命令相同，网速快，可以再安装和删除包时会出现一些问题，不推荐式想，通过给npm切换淘宝源，来解决下载网速问题 http://registry.npm.taobao.org/
   config get registry
  */
  ```

+ yarn

  ```js
  /*
   yarn下载  npm i yarn -g
   在使用yarn时可能出现报错问题，                                           解决方法：打开PowerShell输入 set-ExecutionPolicy RemoteSigned，全选是(A),
   查看执行策略：get-ExecutionPolicy ,是RemoteSigned即可关闭命令窗口
  
   yarn 相关命令
    初始化项目：yarn init  
    下载package里的依赖： yarn
    下载指定的运行时依赖： yarn add xxx@1.10
    下载制定的开发依赖： yarn add xxx@1.1 -D
    全局下载指定包：yarn global add xxx
    删除依赖包：  yarn  remove xxx || yarn global remove xxx
    查看包信息：yarn info xxx
    
    yarn安装的包，解决了文件夹名和包名相同的问题
  */
  ```

### 1.3.3 文件

+ Buffer 缓冲器

  ```js
  /*
  Buffer特点
   Buffer是一个数组类似的对象，Buffer专门用来保存二进制数据
   Buffer的效率很高，存储和读取很快，直接对计算机的内存进行操作
   Buffer的大小确定了，不可修改
   每个元素占用内存的大小为1字节
   Buffer是Node种非常核心的内置模块
   
   
  开辟Buffer缓冲器内存
    (1) new Buffer(10) // 可以开辟一个崭新Buffer但效率过低
    (2) Buffer.alloc(10) // 可以开辟一个独有空间的Buffer效率比(1)好
    (3) Buffer.allocUnsafe(10) // 随意开辟一个Buffer空间，可能夹杂他人数据
    
  将数据存入一个Buffer实例
  let buf = Buffer.from('hello world') 
  console.log(buf) // buf结果是一系列16进制编码，需要用专门的手段转换
  */
  ```




+ 文件

  ```js
  /*
   再NodeJS种有一个文件系统，可以堆计算机中的文件进行操作，同时也提供了一个内置模块,fs模块，专门用于操作文件，fs模块是Node核心模块，
   
   1.异步文件写入(简单文件写入)
   	fs.writeFile(file,data[,options],callback)
   	file 要写入的文件
   	data 要写入的数据
   	options 可选项 ，包括 encoding(编码格式) 默认为 utf8 及其他编码格式(utf8最全)。mode(设置文件的操作权限) 默认值为 0o666，还有0o111文件可以被执行的权限，0o222文件可以被写入的权限，0o444文件可以被读取的权限，0o666为0o222+0o444标识此文件有读写权限，所以文本，视频，图片等使用0o666权限,.exe格式文件用0o777权限。flag打开文件的要执行的操作，默认是w ,a标识追加，w标识写
   	callback 回调函数 其中有一个环境变量err,err会接收此次写入是否成功的返回值，有错误打印错误信息，没错误值为undefined，标识写入成功
   	Node中有一个 错误优先 原则
   	写入数据和写入文件的编码格式要符合规范，比如写入图片就要使用图片的编码格式
   	
   	fs.WriteFile(__dirname+'/demo.txt','天气好',function(err){
  	if(err) console.log(err)
  	else console.log('文件写入成功')
  	})
   	
   2. 流式文件写入(流式写入需要自己手动关 ws.end()&&ws.close())
   	createWriteStream(path[,option])
   	path 要写入文件的路径 + 文件名 + 文件后缀
   	options 配置对象 包括，flags打开文件的要执行的操作，默认是w ,a标识追加，w标识写。encoding 默认为 utf8 及其他编码格式(utf8最全)。fd 文件统一标识符。mode(设置文件的操作权限) 默认值为 0o666，还有0o111文件可以被执行的权限，0o222文件可以被写入的权限，0o444文件可以被读取的权限，0o666为0o222+0o444标识此文件有读写权限，所以文本，视频，图片等使用0o666权限,.exe格式文件用0o777权限。autoClose 自动关闭文件，默认值true。emitClose关闭文件(当出现问题时自动触发)，默认false 。start读取文件的起始位置
      
   	// 创建流
   	let fs = require('fs')
   	let ws = fs.createWriteStream(__dirname+'/dome.txt')
   	
   	// 只要用到了流，必须检测流的状态
   	ws.on('open',function(){  // 打开事件，开始写入触发
   		console.log('可写流打开了')
   	})
   	ws.on('close',function(){  // 关闭事件，需要需要手动触发
   		console.log('可写流关闭了')
   	})
   	ws.write('我来了')
   	ws.close() // 关闭流
   	再node8版本可以使用ws.close()关闭流，以下使用ws.end()关闭流
   	
   3. 简单文件读取	
  	fs.readFile(path[,option],callback)
  	path 文件路径+文件名+后缀
  	option
  	callback 两个环境变量err,data
  	fs.readFile(__dirname+'/demo.txt',function(err,data){
  	if(err) console.log(err)
  	else console.log(data) // data为buffer数据，需要专门手段转换
  	})
  	__dirname可以锁定到当前路径
  	每个后缀都有其特殊的编码规则，单纯改变后缀并不能改变其内部本质
  	
  4. 流式读取(流式写入需要自己手动关 ws.end()&&ws.close())
  	createReadStream(path[,options])
  	path 文件路径+文件名+后缀
  	options 包括 flag,encoding ,fd,mode,autoClose,emitClose,start(起使偏移量),end(结束偏移量),highWaterMark(每次读取数据的大小。默认是64*1024(64kb),根据自己内存增加读取长度)
  	
  	let {createReadStream} = require('fs');
  	let rs = createReadStream(__dirname+'/test.mp4')
  	rs.on('open',()=>console.log('流打开了')) // 开始读取触发
  	rs.on('close',()=>console.log('流关闭了'))// 读完触发
  	rs.on('data',(data)=>{
  	console.log(data.length) // 一次读取数据的大小,默认一次读 64kb
  	})
  	
  使用
  	// 拷贝一个吻技安
  	let ws = fs.createWriteStream('./txtFile/streamws_copy.java', { encoding: 'utf-8', flags: 'a' })
  
      let rs = fs.createReadStream('./txtFile/stream.java', { flags: 'r' })
  
      rs.on('open', () => {
        console.log('开始读取文件了')
      })
      rs.on('data', (chunk) => { // 每次读取指定文件大小，直到读取完毕才会结束
        console.log('我正在已一个字节的速度流式读取文件')
        ws.write(chunk, (err) => { // 将文件写入
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

### 1.3.4 数据库

+ 数据库

  ```js
  /*
   按照数据结果来组织，存储和管理数据的仓库
   
   1.关系型数据库(RDBS) 有 MySQL(免费，轻量),Oracle（庞大，收费），特点，关系紧密(增加一条选项，全部数据都要增加)，SQL通用，高级查询。缺点，读写性能差，有固定的表结构，高并发读写需求
   
   2.非关系型数据库(NoSQL)，有MongoDB,Redis,特点，关系不紧密，有文档，有键值对，格式灵活，速度快，安装配置简单。缺点，不支持sql,不支持事务(原子性，不可分割性)，
   
   
  3. MongoDB
  	(1)安装MongoDB,配置环境变量(mongoDB的bin目录)，再c根目录下创建data文件夹，再文件夹里创建db文件夹，cmd输出mongod启动服务端
  	(2) 打开一个新的cmd串口输入mongo启动数据库的客户端
  	
  4. 端口号
   端口号 1-65535.不要使用1-2000端口(系统预留)
   MongoDB默认端口号为 27017，可以通过--port 端口号，来指定端口，MongoDB数据库默认会存放再C盘根目录下的data/db/文件夹，可以通过 --dbpath来指定数据库目录
  结合 mongod --dbpath C:\Users|web|DEsktop|database --port 7777
  
  5. 关系型数据命名&&非关系型数据命名
  	数据库 -------  数据库
  	表     ------  集合
  	字段   ------- 字段
  	一条数据------  一条数据
  	
   
   7. mongoDB3T(辅助开发软件)
  	(1)基本连接，点击file->connection->new connection,输入项目名，然后锁定服务器(Server)及端口号(port)，点击Test Connection，如果成功点解Save即可连接此数据库，如果不成功，检查自己服务器和端口地址是否写错
  	(2)点击IntelliShell进入输出指令页面
  */
  ```

  

+ mongoDB增删改查

  ```js
  /*
   mongoDB基本命令
   db 查看当前正在操作的数据库
   show dbs 查看数据库列表(一共有几个数据库，如果数据库为空，不出现在列表中)
   use test 切换到test数据库，如果不存在，则创建一个test集合
   db.students.insert() 向当前数据库的students集合中插入一个文档
   show collections 展示当前数据库中所有的集合
   db.studetns.find({});
   
   1. 增
  	db.集合名.insert({}) // 只能添加一个文档对象
  	db.集合名.insertOne({}) // 只能添加一个文档对象
  	db.集合名.insertMany([{}，{}，...]) // 可同时添加多种文档对象数据
   
   2. 查	
  	db.集合名.find(查询条件,[投影]),查询全部符合数据
  	db.students.find({age:18},{stu_id:0,name:0}) //基本文档对象属性查询0 1不能混用(因为有显示取反，及0的不显示其余的显示.或者只写1的显示其余选项不显示),_id是一个例外，它代表数据唯一标识符，默认显示，所以可以用0隐藏
  	db.students.findOne(查询条件,[投影])，查询一条符合条件数据
  	
  	常用逻辑操作符
  	$lt(<),$lte(<=),$gt(>),$gte(>=),$ne(!=)，如db.集合名.find({age:[$gte:20]})年龄大于等于20的
  	$in,$or(逻辑或)
  	db.students.find({age:{$in:[18,20]}})
  	db.students.find({age:{$or:[{age:18},{age:20}]}})
  	$nin(逻辑非)
  	正则匹配
  	如 db.students.find({name:/^j/)
  	$where能写函数
  	db.students.find($where:function(){
  		return this.name ==='zhangsan'
  	})
  	
  3.更新
  	db.集合.update(查询条件,更新内容[,配置对象])
  	db.students.update([name:'zhangsan'],{$set:{age:18}})，改变一条名字为zhangsan的age信息.如果$set没写，则其他的信息全部置为空，只有_id和age属性(极为危险)
  	db.students.update([name:'zhangsan'],{$set:{age:19}},{multi:true}) ,可以改变名为zhangsan的多条age信息，把它们的age该为19
  	
  	db.集合.updateOne(查询条件，更新内容[,配置对象])
  	db.集合名.updateMany(查询条件,更新内容[,配置对象]) ,此操作已经默认为多次更新所以不需要加{multi:true}选项
  	
  4.集合
  	db.集合名.remove(查询条件)
  	db.students.remove({age:{$lte:18}}) 删除所有年龄小于等于18的数据
  */
  ```

  mongoDB 数据库名

  mongod 启动服务端命令

  mongo 启动移动端命令

  mongoose node下的包

+ mongoose

  ```js
  /*
   node下的一个包，可以更简单，快捷，高效，安全的操作mongoDB
   
  
  1. 数据库模块创建及初始化配置
  let mongoose = require('mongoose')
  
  // 解决数据写入时抛出的警告
  mongoose.set('useCreateIndex',true)
  
  // 连接数据库
  mongoose.connect('mongodb://localhost:27017/demo',{ // 选择端口下的一个数据库
      useNewUrlParser:true,
      useUnifiedTopology: true
  })
  
  
  // 写入数据库
  mongoose.connection.on('open',(err)=>{
      if(err){
          console.log(err)
      }else{
          console.log('连接成功')
          console.log('开始写入数据库')
  
          // 引入mongoose下的Schema构造函数，开始指定规则
          let Schema = mongoose.Schema
  
          // 写入集限制规则
          let studentsRule = new Schema({
              stu_id: {
                  type:String, // 限制为字符串
                  required:true, // 必填项
                  unique:true //限制学号是唯一的
              },
              name:{
                  type:String, // 限制为字符串
                  required:true, // 必填项
              },
              age:{
                  type:Number, // 限制为数字
                  required:true, // 必填项
              }
              ,
              sex:{
                  type:String, // 限制为字符串
                  required:true, // 必填项
              },
              hobby:[String],
              info:Schema.Types.Mixed, //接收所有类型
              date:{
                  type: Date,
                  default: Date.now() // 设置默认选项
              },
              enable_flag:{
                  type:String,
                  default:'Y'  // y为yes  n为no
              }
          })
  
          // 创建规则模型对象
          let stuModel = mongoose.model('students',studentsRule)
  }})
  
  2. mongoose增删改查
  	（1）增：   模型对象.create(文档对象，回调函数)
  	
  	（2）查：    模型对象.find(查询条件[,投影],回调函数)不管有没有数据，都返回一个数组
     模型对象.findOne(查询条件[,投影],回调函数)找到了返回一个对象，没找到返回null
      （3）改：   模型对象.updateOne(查询条件,要更新的内容[,配置对象],回调函数)
    模型对象.updateMany(查询条件,要更新的内容[,配置对象],回调函数)
    备注：存在update方法，但是即将废弃，查询条件匹配到多个时，依然只修改一个，强烈建议用updateOne或updateMany
    	（4）删：    模型对象.deleteOne(查询条件,回调函数)
     模型对象.deleteMany(查询条件,回调函数)
     备注：没有delete方法，会报错！
  */
  ```
  



+ 连接mysql

  ```java
  /*
    下载 mysql 包
    	npm i mysql
    引入包,建立数据库连接
    	const mysql = require('mysql')
    	const db = mysql.createConnection({ // 指向数据库，完成配置
            host: 'localhost',
            user: 'root', // 用户
            password: 'lyx', // 密码
            port: '3306',
            database: '数据库名'
          })
      db.connect() // 建立连接
    引入后即可开始书写sql语句
    	let sql = 'SHOW TABLES'
      db.query(sql, (err, result) => {
        if (err) {
          console.log(err)
          return
        }
  
        // 所有表名
        for (const i of result) {
          console.log(i.Tables_in_jxgl0701)
        }
      })
      
   注意
   	根据数据库创建的基本操作，我们可以结合promise和函数让其操作变的更灵活
  */
  ```

  

### 1.3.5 http

一次请求有且只有一次响应，响应包括(给浏览器一段文字，一个图片，一个视频，一个下载文件等)，多个响应以response.send()为主

```js
/*
// 获得http模块
let http = require('http')

// /?name=zhangsan&age=18...urlEncoded编码形式，其中?name=zhangsan&age=18...叫做查询字符串参数
// urlCode格式，可以用querystring内置模块直接解析为对象键值对
let qs = require('querystring')
let server = http.createServer((request, response) => {
	// request形参有所有请求得信息，response用于设置所有响应信息
    let parameters = request.url.split('?')[1];

    // 使用qs.parse()方法把数组键值对转换为对象键值对
    let {username,password,sex} = qs.parse(parameters)
    response.setHeader('content-type', 'text/html;charset=utf8')
    response.end(`<h1>我是${username},我的密码时${password},我的性别是${sex}</h1>`) // 只允许写一次响应尾
})
// 服务器在4000端口等待监听
server.listen(4000, (err) => { // 向服务端发起监听 请求
    if (err) console.log(err)
    else console.log('127.0.0.1:4000 端口监听中...')
})
 
*/
```



+ querystring库

  ```java
  /*
   querystring是一个第三方库，其可以将字符串按照要求转换为对象
   	其他方法
   		parse(str); // 此方法可以按
  */
  ```

  

+ get&&post请求

  ```js
  /*
   http定义了八种发送请求方式，这八种方法之间没有本质上的区别，只是让请求，更加有语义化特征，常用的有get,post请求
   
   1.get
   	含义：从指定的资源获取数据(索取)，用于单纯获取数据或请求非敏感数据
   	常见的get请求，浏览器地址栏输入网址时，请求外部资源html标签时，发送Ajax请求没有指明方式时，form表单提交没有指明方式时，发的都是get请求
   2.post
   	向指定的资源提交要被处理的数据，用于传递相对敏感的数据，请求的结果有持续性的副作用，例如，传递的数据要写入数据库时，使用post不代表绝对安全
   	常见post请求，使用ajax时指出使用post方式，form表单指出使用post方式
   	
   3.get和post请求区别
   	(1) 网页刷新时 get仍然保留地址栏数据，post不会保存地址栏数据
      (2) 设置书签时 get可以正确收藏自己想要的页面,post不可能正确首词自己想要的页面
      (3) 历史记录 get地址栏数据会保存再历史记录中，而post相反
      (4) 数据长度限制 get地址栏添加数据受长度限制(2048),post则不受限制
      (5) 安全性  get比post安全性差，在发送密码等敏感信息不能用get，post比get安全强
      (6) 可见性 get数据会显示在url中，post数据不会显示在url中
  */
  ```

![image-20220319181049392](D:\typora_import_images\typora-user-images\image-20220319181049392.png)

![image-20220319181509814](D:\typora_import_images\typora-user-images\image-20220319181509814.png)

+ express包

  ```js
  /*
   1. express为三方包，需要用npm或yarn指令下载( npm i express)
    const express = require('express')
    const app = express() // 创建并开启服务
    app.get('/',(request,response)=>{}) // 设置主页路由，并指明请求方法，node会自动调用对应的请求路由，对比传统http请求需要自己判断用户使用的是什么请求(过于繁琐,代码冗杂)
    app.listen(4000,(err)=>{ // 监听服务器
    	if(err) console.log(err)
    	else console.log('服务器开启成功')
    })
    
    2. 路由
    url是整个完整的导航栏目录，uri是域名栏/后的部分
    路由分根路由，一级路由，二级路由，三记路由... ,根路由用/标识，一级路由用/xxx标识，二级路由用/xxx/xxx标识，三级路由用/xxxx/xxx/xxx标识... 。如果定义了相同的路由则最前面的路由先执行(代码从上往下执行，所有路由底层会被放到一个数组里，让后会从前往后找匹配的路由，找到立刻执行，并退出查找)
    
  */
  ```
  
  
  
+ http协议

  ```js
  /*
   http协议规定了服务端和客户端的网络传输的报文(请求报文，响应报文)
   
   http 1.0 不支持长连接
   http 1.1 支持长连接，缺点，同时发送资源的数量过小
   http 2.0(最新) 同时发送紫云阿德数量稍有提升
   
   Fiddler 工具可以查看http相应细节
   
   为了避免http显示敏感信息，可以使用 express().disable('x-powered-by')(app.disable('x-powered-by'))。禁止传输网络搭建者 
   
   1. http状态码
   	1xx: 服务器已收到本次请求，还需要进一步处理才能进入
   	2xx: 服务器已经受到本次请求，切已经分析，处理，最终处理完毕
   	3xx: 服务已经收到了请求，还需要求其他的资源，或者重定向到其他位置，或交给其他服务器处理
   	4xx: 请求的参数或者地址有错误，出现了服务器无法理解的请求(前端除了问题)
   	5xx: 服务器内部错误，无法响应用户请求(后端出了问题)
   	
   	常见状态码
   	200 成功
   	301 重定向，被请求的旧资源被永久移出(旧资源不存在)，将会跳转到一个新资源，搜索引擎在抓取新内容的同时也将旧网址替换为重定向页面的网址
   	302 重定向，被请求旧资源仍然存在，会临时跳转到一个新资源，搜索引擎会抓取新内容，同时保存旧资源
   	304 请求资源重定向到缓存中(命中了协商缓存)
   	404 资源未找到，一般是客户端请求了不存在的资源
   	500 服务器收到了请求，但服务器内部产生了错误
   	502 连接服务器失败(服务器在处理请求需和其他服务器配合，但连接不上其他服务器)
  */
  ```
  
  

+ 从输入url到得到页面响应过程

  ```js
  /*
   1.DNS解析(域名转换为ip地址)，找浏览器DNS缓存解析域名，找本机DNS缓存，找路由器DNS缓存，找运营商缓存，递归查询(查询全球13台DNS服务器)
   2. 进行TCP(协议)连接，三次握手，四次挥手
   	第一次握手，浏览器向服务端发起连接请求
   	第二次握手，服务端向浏览器响应请求
   	第三次握手，浏览器向服务器连接数据
   	第一次挥手，浏览器向服务端发起，标识数据接收完毕
   	第二次挥手，服务端向浏览器发起，检查数据完整性
   	第三次挥手，服务器向浏览器发起，接收完毕，准备断开连接
   	第四次挥手，浏览器向服务器发起，正式断开连接
   3. 发送请求(请求报文)
   4. 得到响应(响应报文)
   5. 浏览器开始解析html等前端资源
   6. 断开TCP协议
  */
  ```

### 1.3.6 路由用法

**所有的中间件都是函数**

**ui路由一般都是get请求，业务路由一般都是post请求(post携带数据)**

```js
/*
  路由的request,response环境变量
  1.request的API
  	(1) request.query 查询字符串参数，拿到的是一个对象
  	(2) request.params 获取get请求参数路由(uri)的参数，拿到的是一个对象,路由写法('/m/:xx',()=>{})，其中:xx标识获取当前/m/的下一个路由节点并封装成一个 xx:xxx的键值对,只有二级路由可以添加params参数. 如 localhost:4000/get/we  ,路由匹配为 app.get('get/:id', (req, res) => {})params参数为:{id:'we'}
  	(3) request.body 获取post请求体，拿到的是一个对象(要结助一个中间件)
  	(4) request.get(xx) 获取请求头中指定key对应的value
  	
  2.response的API
  	response.send() 给浏览器做出一个响应(自动加响应头)
  	response.end() 给浏览器做出一个响应(不会追加响应头)
  	response.download()告诉浏览器下载一个文件
  	response.sendFile()给浏览器发送一个文件
  	response.redirect(url)重定向到一个新的地址
  	response.set(header,value) 自定义响应头内容
  	response.get() 获取响应头指定key对应的value,拿不到Date,可以拿到大多值和自己设置的响应头
  	re.status(code) 设置响应状态码，很少操作
  	
*/
```

![image-20220319201145329](D:\typora_import_images\typora-user-images\image-20220319201145329.png)

![image-20220319201221199](D:\typora_import_images\typora-user-images\image-20220319201221199.png)



+ 中间件

  ```js
  /*
   本质上是一个函数，包含三个参数,request,response,next
   作用 
   	执行任何代码，修改请求和响应对象，终结请求相应循环(让一次请求得到响应)，调用堆栈中下一个中间件或路由
   
   分类 
   	应用(全局)级中间件，第三方中间件，内置中间件,路由器中间件
   
   1.使用应用级(全局)中间件
   	第一种写法 app.use((req,res,next)=>{})
   	第二种写法 使用函数定义，可以在任何需要的地方使用 app.get('/'[,...函数中间件(回调)],(err)=>{}) . 函数中间件会被自调用，且设置有(request,response,next)环境变量,此方式主要为单个路由设置设置一个缓存层进行安全校验，只有通过校验的路由才能看见本页面
   	
    全局中间件可以匹配所有url地址响应，会根据位置顺序决定响应先后，如果写在所有路由最前面可以起到拦截非法请求等作用，next()形参可以起放行作用，不写next会一直停留在此中间件
    在堆区中，只有1个请求对象和1个响应对象，因此多个路由操作的request,response都是操作的同一个请求对象或响应对象，所以它们的属性和方法是相通的，当在全局中间件上添加一个request.xx的属性时，所有访问请求对象的路由都可以拿到这个request.xx属性
    
  2. 使用第三方中间件
  	body-parser 
  		作用
  			该中间件用于解析前端发送post携带的数据 
  		安装
  			npm i -d body-parser 
   app.use(bodyParser.urlencoded({extended:true}))|| app.use(express.urlencoded({extended:true})) 即可接收并保存post请求头，一对象的形式存储呈现
   app.use(express.static('/public')) // 即可获得本地的静态资源(里面有一大推html文件)，需要写在路由最前面，这样用户输入 login.index或login都可以打开相应文件
   
  */
  ```

  ![image-20220319211205806](D:\typora_import_images\typora-user-images\image-20220319211205806.png)

  解析post请求中间件的使用

  ```java
  /*
  const express = require("express"); // 引入 express
  const bodyParser = require("body-parser"); // 引入 post请求解析器
  let app = express(); // 开启服务器
  app.listen(9000,()=> console.log("9000端口 监听中")); // 监听端口
  
  // 使用post携带参数中间件
  // 此中间件可以解析post请求体里携带的参数并以对象形式挂载到req上
  //app.use(bodyParser.urlencoded({extended: true})); 
  
  // express下也挂载了一个和bodyParser相同功能的中间件可以对post请求进行解析
  app.use(express.urlencoded({extended:true}));
  app.get("/", (req, res) => {  // 当发送get请求访问该服务端的 / 路径时匹配这个路由 
      let {name, age} = req.body;
      res.send(name + "_" + age);
  });
  */
  ```

  

  暴露静态资源中间件的使用

  ```java
  /*
   作用
      使用 app.use(express.static(__dirname+"/public"))  可以暴露我们设定好的静态资源，当前端在进行访问时，会优先从我们设置的静态资源路径里进行匹配，如果成功则使用该资源，否则会继续匹配路由，如果路由匹配则走该路由，否则则找不到该路径，此时需要做特殊处理
      
   注意
   	暴露静态资源应在所有路由之前，这样才可以先进行静态资源的匹配
  */
  ```

  

  

  

  路由器

  ```js
  /*
   路由器的思想：把所有业务页面，和逻辑页面分成两个路由，然后再暴露出去，这样好管理路由的添加和删除
   
   创建路由器
       const {Router} = require('express')
       let router = new Router()  
       router.get('/login',(request,response)=>{})
       module.exports = function(){
       	return router;
       }
       
      ======================================
      // 服务器.js使用路由器中间件（使用use()方法使用即可）
      app.use(UIRouter());
   
   
   
   
   
   
   注意
   	router对象和express服务器对象的功能类型,但router仅不能构造服务器(app对象和router对象都可以开启路由)
   	所有的中间件都是函数，所以router路由器开启的路由，需要以函数的形式暴露出去
   	在路由分页的时候可能会出现路径错误，这是会用到Node下的一个模块path，path下有一个resolve方法,path需要两个参数，第一个是 '从那出发'，第二个是'查找规则'
   如path.resolve(__dirname,'../public/login.html')
   
  */
  ```


+ 使用(拦截器)

  ```java
  /*
   const express = require("express");
  let app = express(); // 开启服务器
  app.listen(7000,()=>console.log("localhost:7000 监听中"));
  
  // 我是一个简单拦截中间件，用于开始导航前做信息校验限制(身份是否合法等)
  app.use((req,res,next)=>{
      next();
  });
  
  app.use('/user',(req,res)=>{
      res.send("我是user页面");
  });
  // app.use('/',(req,res)=>{
  //     res.send("我是主页面");
  // })
  // 我是一个后端拦截中间件，主要做页面重定向
  app.use((req,res,next)=>{
      console.log("开始重定向");
      res.redirect("/user");  // 重定向是重定向路由
  });
  */
  ```

  

### 1.3.7 模板引擎

```js
/*
 如果再express中基于Node搭建的服务器，使用ejs无需引进
 const express = require('express')
 const app = express()
 app.set('view engine','ejs') // 设置ejs模板引擎
 app.set('views','./views') // 设置模板所在的目录
 app.get('/',(req,res)=>{
 	res.render('person(ejs模板名)'[,{data:hello world}](传递的数据))
 })
 
 1.ejs语法
  再<% %>内可以使用服务端传输的数据
	（1）<% %>再两个%里可以写任何js代码，且不会向浏览器输出任何东西
	（2）<%- %> 可以将内容渲染到页面（会识别html字符串渲染到页面上）
	（3）<$= $> 可以将内容渲染到页面（不会识别html字符串渲染到页面上）
*/
```

### 1.3.8 cookie

在做登录页面存放cookie数据时，可将用户的_id唯一标识符暴露在浏览器上，通过内部代码实现name的查找

+ ```js
  /*
   1. cookie 本质上是一个字符串，包含了浏览器和服务器沟通信息，存储形式是以 [key-value]形式存储，浏览器会自动携带该网络的cookie,只要是该网站下的cookie全部携带
   2. cookie分类
    (1)会话cookie,关闭浏览器后，cookie会自动消失(cookie运行再浏览器内存上)
    (2)持久化cookie,看过期时间，一旦到期自动销毁，如果没到过期时间，用户清理了缓存也可销毁
    
   3. 工作原理
     当浏览器第一次请求服务器时，服务器可能返回一个或多个cookie给浏览器，此后客户端在访问该网站时，会自动携带cookie(无法干预),服务器会拿到之前 种 的cookie,分析内容，校验cookie合法性，根据cookie实现内部逻辑。
     
     cookie解决了http无状态问题，不同语言的cookie结构不一样但用法相同，cookie也可前端生成，只不过意义不明显
   注意
   	cookie和LocalSession原理相同，各域名之间互相隔离，相同地址(域名)互相联通(即相同域名下cookie共享)
   	cookie(包括会话和持久化)存储的数据key-value都需要时字符串，且value会被默认加密处理
   	持久化cookie单位ms,如res.cookie("msg","13",{maxAge:1000*10}); //种了10s的cookie
   	express只提供了种cookie没有提供读cookie,因此需要借助cookie-parser库进行解析(npm i cookie-parser)
     
     
     // 创建一个会话cookie(ts编写)
     class Express_ {
      public express = require("express");
      public app = this.express();
  
  
      public constructor() {
          this.app.listen(9999,():void=>{console.log("9999端口监听中";});
          this.setRoute(); // 开启路由监听
      }
  
      public setRoute(): void {
          this.app.get("/", (req: any, res: any) => {
              // 给客户端种下一个会话cookie,该cookie在浏览器关闭时会消失
              // cookie必须在服务端有反馈前在服务端种下
              let obj:object = {a:1,b:2};
              res.cookie("msg",JSON.stringify(obj));
              res.send("欢迎来到主页面");
          });
      }
  }
  
  new Express_();
  
     
     
     
  // 创建一个持久化cookie(原生js)
     let express = require('express')
     let app = express()
     
     // 读cookie数据需要一个中间件 cookie-parser
     let cookieParser = require('cookie-parser')
     app.use(cookieParser()) // 使用cookieParser 中间件,这样才可以读取cookie数据
     
     app.get('/',function(req,res){
     // res.cookie('str(key)','str(value)'[,time(销毁时间，单位为ms)]) 种持久cookie
     // res.cookie('str','value') 种会话cookie
     res.cookie('name',JSON.stringify({a:1,b:2}),{maxAge:1000*60}) 
     console.log(req.cookies) // 读取cookie数据，需要借助cookie-parser中间件
     
     //删除cookie,让其生命周期变为0
     res.cookie('指定cookie','',{maxAge:0})
     res.clearCookie('指定cookie')
     res.send('ok')
     })
     
     app.listen(4000,(err)=>{ if(err) console.log(err); else console.log('连接成功')})
  */
  ```
  
+ 只用cookie存储可能会造成用户信息泄漏，因此需要将cookie和session共同使用，保护用户隐私

+ cookie&&session

  持久cookie存硬盘，会话cookie存浏览器内存，session存在服务器内存里

  ```js
  /*
   当浏览器给服务器第一次发起请求时，session会话存储会给该浏览器开辟一个存储空间(里面存储的是该用户的各种信息)，该地址加密之后地址返回给cookie，cookie再带给浏览器，再下次访问时，cookie直接带着这个地址访问对应的存储空间
   
   session本质上是存在服务器内存里的，在服务关机时，数据会丢失，所以需要把session持久化(操作)，即把session数据存储再数据库中
   
   1. 前端通过浏览器去查看cookie时，会发现有些Cookie过期时间是session,意味着该cookie是会话cookie,后端人员常常把 session会话存储，简称为 session存储，或session
   
   2. 默认session存储再服务器内存中，当一个新客户端发来请求时，服务器都会开辟一个空间，供session存储数据
   
   3. 工作流程
     （1）第一次浏览器请求服务器时，服务器会开辟一块内存空间，供session会话存储使用
     （2）返回相应的时候，会自动返回一个cookie(有时会会返回多个，为了安全)，cookie里包含着，上一步产生会话存储“容器”的编号(id)
     （3）以后请求的时候，会自动携带这个cookie，交给服务器
     （4）服务器从该cookie中拿到对应得session得id,去匹配服务器中匹配
     （5）服务器会根据匹配信息，决定下一步逻辑，一般来说cookie一定配合session使用
     
     
  4. cookie&session配合
   为了方便操作session需要安装2个包 npm i express-session(操作包) && npm i connect-mongo(持久化)
   let express = require('express')
   let app = express()
   let session = require('express-session')
   let MongoStore = require('connect-mongo')(session) // 使session数据持久化
   app.use(session({
   	name:'userid', // 返回cookie得名字
   	secret:'zhangsan', // 返回cookie得加密字符串
   	saveUninitialized: false, // 是否在存储内容前创建session会话
   	resave: true, // 是否在每次请求时强制重新保存session,即使他们没有变化
   	store: new MongoStore({ // 设置数据库
   	url: 'mongodb://localhost:27017/session-repository',
   	touchAfter: 24*3600 // 设置修改频率 单位ms
   	})
   	cookie:{
   		httpOnly: true, // 开启后前端无法通过JS操作cookie
   		maxAge: 1000*60 // 返回Cookie得生命周期
   	}
   })) 
   app.get('/',(req,res)=>{
   	req.session._id = 3 // 往session写入数据，分配内存，返回session地址
   }) 
   app.get('/user_center',(req,res)=>{
   	// 获取客户端通过cookie携带过来得session编号
   	// 根据session编号匹配session容器
   	// 若匹配上，拿到session容器里得数据，未匹配上则驳回请求
   	const {_id} = req.session   // req,session内部已经帮忙处理了上3步，所以如果匹配成功则返回数据(给_id)，匹配失败则返回undefined
   })
  */
  ```

+ 数据加密

  ```js
  /*
   常见加密有 sha1,md5包，加密只可正向加密，不能反向解密
   1.md5
   let md5 = require('md5')
   md5('str') // 进行加密
   
   2.sha1
   let sha1 = require('sha1')
   sha1('1')
  */
  ```

  

### 1.3.9 Ajax

+ 集合(常见问题)

  ```java
  /*
   请求发起的方式要和接收方可接收的请求类型一致
   浏览器响应头的设置必须在open方法后
  */
  ```

  

+ 介绍

```js
/*
 Ajax(用到页面无刷新技术) 就是异步得JS和XML，最大优势为 无刷新获取数据，缺点为 没有浏览历史不可回退，存在跨域问题，SEO不友好(对搜索引擎不友好)
 XML 是可扩展标记语言，用来传输和存储数据
*/
```



+ 原生js发Ajax请求

  ```js
  /*
  前言
  	直接发送 ajax请求 会被同源策略给限制，导致无法正常发送，此时有两种方式可以解决此方法，使用jsonp方式发送(利用script标签不受同源限制去向后端发请求，请求来的数据会被当作js语句处理)，或cors解决(cors只能对保证安全的地址开放接口，如果 * 表示对所有地址开放接口 ，此时 可能遭受攻击)
  	
   使用原生js搭建ajax请求
   xhr.abort() 方法可以阻止接收服务端返回得数据
   let xhr = new XMLHttpRequest() // 生成HHR实例
              xhr.onreadystatechange = ()=>{ // 当状态改变时触发此事件
              // readyState有 0 1 2 3 4 种状态
              // 0 xhr对象实例出来得那一刻，就已经是0状态，代表xhr初始化
              // 1 send 方法还没有被调用，即 请求没有发出去，此时依然可以修改请求头
              // 2 send方法被调用了，即 请求已经发出去了，此时已经不可以在修改请求头
              // 3 已经回来了部分数据，如果是比较小的数据，会在此阶段一次性接收完毕
              // 4 数据接收完成
                  if(xhr.readyState == 4&&xhr.status == 200) {
                      console.log(xhr.response)
                      let div= document.querySelector('div')
                      div.innerHTML = xhr.response
                  }
              }
              // 当使用Ajax发出post请求时需要设置一段请求头,不设置则获取不到参数
              xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
              xhr.open('post','xxx(网站)') // 想之地那个网站发送post请求
              
              xhr.open('get','xxx(网站)')  // 选择发送get请求和网站
              
              xhr.send('name=kobe&age=18') // 发送请求数据
              
  1.get请求数据
  参数放在请求地址里
  以查询字符串方式携带
  2.post请求
  参数放在请求体里
  以请求体参数携带
  参数用urlencoded
  
  当form表单发送post请求时内部自动加了urlencoded解析，当ajax发送post请求时，需要自己写urlencoded还要加一个 特殊得请求头 ，或者使用 jason格式发送
  
  iE对get请求有严重响应缺陷(请求过一边地址后，会把该地址得数据缓存到硬盘，下次请求直接走强缓存)，为了解决这个问题可以给地址栏添加Date.now()属性，强迫ie实时刷新
  
  注意	
  	xhr对象下有一个abort()方法可以取消本次请求
  */
  ```
  
  
  
+ jquery封装ajax

  ```js
  /*
  // jquery发送ajax请求完整方式
   $.ajax('http://localhost:3000/ajax_get',{ // 第一个参数也可以省略，可以转移到对象里配置url地址参数
   	method: 'get', // 请求方式
   	data: {name:'keb',age:20}, // 请z'x求携带数据
   	success(result){ // 成功回调
   		console.log(result)
   	},
   	error(err){ // 失败回调
   		if(err) console.log(err)
   	}
   })
   
   // jquery实现get请求
   $.get('xxx(连接端口)'[,携带参数(对象形式)],(data,statusTest)=>{}(回调函数))
    $.get('http://localhost:3000/ajax_get',{name:'keb',age:20},(data,statusText,xhr(一般只用到data环境变量))=>{
    	console.log(data)
    })
   注意
   	此方式发送的请求，第一个参数为端口，第二...n个参数携带的时参数，最后一个参数是回调函数，该回调函数中第一个形参为该地址返回的数据，第二个形参为该请求的状态文字，最后一个参数是原生的xhr实例对象
  */
  ```

+ 跨域问题

  ```js
  /*
   ajax跨域，客户端不能获取数据，但服务器可以受到请求
   浏览器为了安全采用了同源策略（由网景公司提出得重要安全策略）
   同源同域指协议，域名，端口都一致
   没有同源同域时，可能会引起重大安全问题(可以使用iframe非常轻松得截取其他网页)
   非同源收到的限制：cookie不能读取(只能获取本站)，dom无法读读取，ajax请求不能获取数据
   form 跨域 不拦截，form请求走浏览器模块，不遵守同源策略
   ajax 跨域 拦截，ajax请求走得是ajax引擎，它遵守同源策略
   
   注意
   	不受同源策略影响的标签有 img,link,<script src="">，而ajax请求受同源策略限制，他们在允许时调用的模块是不同的
  */
  ```

+ 解决跨域问题(JSONP)

  ```js
  /*
   前言
   	script标签不受同源策略限制，且请求的内容默认当js代码处理，可以使用这一特性来解决前后端通讯问题，可该方式需要前后端配合才能使用
   	此方式解决跨域的原理，直接由后端调用前端方法，然后将后端参数以函数形参方式传递给前端
  	JSONP解决跨域不是一种技术，而是智慧的结晶(利用了script标签不受同源策略限制)
  	jsonp只能接收 get请求
  
    jsonp解决跨域
   利用了script标签发请求不受同源策略得限制，所以不会产生跨域问题。动态构建script节点，利用节点的src属性发出get请求，从而绕开ajax引擎，可是只能解决get跨域问题且要在后端配合前端得基础上
   script标签会自动解析符合js格式得字符串代码
   
  
  ===========================================================
  
  后端
  let express = require("express");
  let app = express();
  app.listen(9000, () => {
      console.log("9000 端口监听中");
  });
  app.get("/JSONP", (req, res) => {
      let {callback} = req.query; // 解析前端提供的接口
      res.send(`${callback}(${ JSON.stringify({ //返回数据给前端,因前端使用script标签引入,默认当js处理
          name: '涨三',
          age:15
      })})`);
  });
  ===========================================================
  前端(原生解决jsonp)
  <script>
      let btn = document.querySelector("button");
      btn.addEventListener("click", (e)=>{
          let scriptNode = document.createElement("script"); // 创建一个script标签对象
          scriptNode.src ="http://localhost:9000/JSONP?callback=getMsg"; // 设置属性，将前端api传递给后端，供其调用
          document.body.appendChild(scriptNode); // 将其添加到body尾部(一旦被添加就会发起请求)
          document.body.removeChild(scriptNode); // 将其删除
      });
      function getMsg(a) {
          console.log(typeof a);
          console.log(a);
      }
  </script>
  ===========================================================
  
  注意
  	json后端调用前端接口时，会出现数据不匹配问题，如 后端传递给前端一个数组和对象，前端无法正常接收，此时就要对数据进行处理。假设此时后端是node，则可以把目标对象使用JSON.stringify(x)转换为一个和JSON对象，此时前端就可以正常接收了
  	
  ===========================================================
  jquery封装的jsonp
  	// 完整写法
   	$('div').click(()=>{
   		$.ajax({
   			url:'http://localhost:3000/test',
   			method: 'get',
   			dataType: 'jsonp', 
   			data:{name:'zhangsan',age:18},
   			success(result){ // 请求成功调用函数
   				console.log(result)
   			},
   			error(err){  // 请求失败调用函数
   				console.log(err)
   			}
   		})
   	})
   	
   	// 精简写法
   	$('div').click(()=>{
   		// 此方式发送的jsonp必须指定callback我们可以指定一个为?此时会由jquery来完成调用
   		$.getJSON("http://localhost:3000/test?callback=?",{name:'zhangsan',age:18},(data)=>{
   			console.log(data); // 如果请求成功该函数的data就是返回的数据
   		})
   	})
   注意
   	jqeury封装的jsonp发送过程全程由jquery封装完整，返回的数据会放到成功的回调函数中
  */
  ```

+ 解决跨域(cors)

  ```js
  /*
  	服务器端设置一个响应头 即可解决跨域(可能有安全问题)
  	res.setHeader('Access-Control-Allow-Origin','http://localhost:63348') // 表示该端口允许 http://localhost:63348访问
  	
   使用
   	// 可以使用全局拦截器设置该服务器得响应信息
   	 this.app.use((req,res,next)=>{ // 设置所有路由向所有地址开放接口
              res.header('Access-Control-Allow-Origin', '*');
              next();
          });
  	
  注意
  	为了网站不被恶意引用，因只对信任地址或端口开放跨域，不推荐直接写 * ,因为其允许所有地址都可以访问此接口
  */
  ```



## 1.4 promise

+ 基本概念

  ```java
  /*
   promise使用了状态来保存一次promise的调用，状态包括resolved,pending,rejected,三种状态中只会出现一种(使用if做判断控制)，resolved表示成功，pending表示待定，rejected表示失败;成功的回调一般称为value,失败的回调一般称为reason(形参)
   
   let p = new Promise((resolve,reject)=>{
   	resolve("success"); // 一次只会保存一种状态，虽然是两次函数调用，不过在调用时函数的状态就确定了(if过滤)
   	reject("reject");
   }).then(
   	(value)=>{ console.log(value)},
   	reason=>{console.log(reason)}
   )
   注意
   	promise类在生成实例对象时需要传递一个函数，然后其函数的两个形参也是函数
   	promise传递的函数由Promise类底层完成调用，实际上两个函数都会调用，但由于使用了状态来控制，因此在函数被调用后进行了if判断(判断其是不是pending状态，如果是则继续改函数调用，否则退出函数)，此时该promise的返回值就只能是一种状态
   	js中函数可以当作参数传递，函数也有地址
  */
  ```

![image-20220324161845833](D:\typora_import_images\typora-user-images\image-20220324161845833.png)

+ error报错信息

  ```js
  /*
  1.error常见错误
   ReferenceError: 引用变量不存在
   TypeError: 数据类型不正确
   RangeError: 数据值不在其所允许得范围内
   SyntaxError: 语法错误 
   
   2.错误处理
   （1）try{}catch(err){} 捕获错误,把可能出错得代码放到try里，当出现错误立刻执行catch里得内容，err下有两个属性message(错误信息),stack(具体位置)
   try{
    const e={}
    e()
   }catch(err){
    console.log(err.message)
    console.log(err.stack)
   }
   （2）throw 可以抛出错误，并且可以自定义错误信息
    throw new Error('xxx')
    
   3. then catch  finally
  	then方法可以接收一个对象，里面至多包含两个函数至少包含一个函数，第一个函数接收resolve()函数的形参，第二个接收reject()函数的形参
  	catchf可以接收 reject()函数的形参
  	finally 是无论如何都会执行的一个函数
  */
  ```



+ promise

  ```js
  /*
   promise是es6规范出来得新的技术,可以更直观得异步编程，从语法上说，promise是一个构造函数(类)，从功能上说，promise对昂用来封装一个异步操作并可以获得成功/失败得结果值
   promise对函数的调用进行了解耦，大大提高了程序的可读性和可维护性
   
   1.重要语法
   Promise(execute) 构造函数
  const promise = new Promise((fun:resolve,fun: reject) => { // 同步执行函数
        setTimeout(() => { // 在内部封装异步任务
          const time = Date.now()
          if (time % 2 == 1) { 
            resolve('成功了' + time) // 如果成功调用then函数里的一个函数,resolve的实参传给value
          } else {
            reject('失败了' + time) // 如果失败调用then函数里的二个函数，reject的实参传给reason
          }
        }, 1000)
      })
  
      promise.then(
        value => console.log('成功', value),
        reason => console.log('失败', reason)
      )
      
   Promise.prototype.then 方法
   
   
   Promise封装Ajax
   function promiseAjax(url) {
        return new Promise((resolve, reject) => {
          const xhr = new XMLHttpRequest()
          xhr.open('get', url)
          xhr.send()
          xhr.onreadystatechange = () => {
            if (xhr.readyState != 4) return // 过滤到123
            if ((xhr.status >= 200 && xhr.status <= 299)) {
              console.log(xhr.response)
              resolve(JSON.parse(xhr.response))
            } else {
              reject('request error: status 404')
            }
          }
        })
      }
      promiseAjax('https://api.apiopen.top/getJoke?page=1&count=2&type=video')
        .then(
          data => console.log('请求成功', data),
          error => console.log(error)
        )
  */
  ```
  
+ promiseAPI

  ```js
  /*
  
   1.Promise构造函数：Promise (executor){}
   executor函数: 同步执行(resolve,reject)=>{}
   resolve函数：内部定义成功时调用value =>{}
   reject函数：内部定义失败是调用 reason=>{}
   
   2.Promise/prototype.then方法(原型对象的方法给实例对象用，也就是为什么要直接new Promise)：onResolved函数： 成功调用 (value)=>{}(then函数后的第一个函数，调用函数和函数位置有关，和函数名无关)，且接收resolve()的形参给value
   onRejected:  失败调用(reason)=>{}(then函数后的第二个函数，调用函数和函数位置有关，和函数名无关)，且接收reject()的形参给reason。 reason函数也可以省略不写，转由.catch()方法来完成失败回调函数
   
   3.Promise.prototype.catch方法： （onRejected） =>{},失败的函数可以不写在.then函数中，而可以写在 .catch(reason=>{}) 抛出错误
   onRejected函数：失败的回调函数（reason）=>{}
   
   
   4. Promise.resolve(3) . Promise.reject(4),可以快速创建成功以及失败的回调，但由于是只有一个成功或失败的回调，所以成功回调只能用then,失败回调只能用catch
   
   5.Promise.all([p1,p2,p3]),返回一个新的promise，只有数组里的所有Promise成功才返回成功得数组，有一个失败则返回失败和失败的值（应用场景：当需要同时获得n个请求时可以使用该方法，只有全部成功才获取，否则不获取）
   Promise.all和Promise.face无法识别链式写法的then和catch所以尽量少写
   使用
   	let p1:Promise<any>= new Promise((resolve, reject) => {
      resolve(1);
      reject(1);
      });
      let p2:Promise<any>= new Promise((resolve, reject) => {
          resolve(2);
          reject(2);
      });
      let p3:Promise<any>= new Promise((resolve, reject) => {
          resolve(3);
          reject(3);
      });
      let all =Promise.all([p1,p2,p3]);
      console.log(all.then(value=> console.log(value))); // [1,2,3]
  
   注意
   	Promise.all([p1,p2,p3])返回数据的索引和传入的索引一致
   
   6.Promise.race([p1,p2,p3]) 返回数组中所有promise中的第一个完成promise调用的结果值
  
  */
  ```



+ promise常见问题

  ```js
  /*
   注意
   	1.当promise抛出一个错误时(return new Error("error"); 等)，会切换到rejcted状态
   	2.当状态确认时，调用then方法传入两个函数，此时promise底层会自动识别并调用其中的函数(传入当前状态值，   如果为fulfill状态则调用第一个函数，如果为rejected状态则调用第二个函数)
   	3.then方法调用中也可以改变当前promise对象的状态值
   		如 return 4 则当前状态的值就变4,return Promise.reject(3)则会把当前Promise的状态更新为失败状	
   	4.当then方法 执行完毕后，会返回一个和当前状态相同的一个promise对象(因此可以链式调用)，其值如果没有被 该方法处理则会原样返回，如果被处理，则其值依赖于该then的返回值，如果没指定默认为undefined
   	5.由于then()方法默认会返回一个和当前状态相同的一个promise对象，但有时我们又不希望这样，因此我们可以手动返回一个空的promise对象，此时该promise对象返回的就是一个pending状态的promise，即无用的promise
   	6. promise的rejected状态会默认抛出一个异常，需要使用 try-catch处理
  
  具体演示
   1.Promise.prototype.then的三种状态，在没有resolve,reject传参是，为pending,当有resolve传参时为fulfill,由reject传参时为 rejected,当为 throw xxx时为rejected
   
   2.一个promise可以指定多个成功/失败的回调函数，并且都会执行
   
   3. 关于promise函数执行顺序
   promise构造函数生成实例后，是同步执行的，里面的函数调用则是异步执行的
   new Promise(((resolve, reject) => { //同步
        console.log('executor()')
        console.log('resolve()')
        setTimeout(() => { // 异步
          resolve(1) // 异步
          console.log('resolve()') // 异步里的同步
        }, 1000)
      })).then(value => console.log('onResolve()', value))
      console.log('html')//同步
      
  4.当then链式写法then时，后一个then其实绑定的为前一个then,只有前一个then给return后一个then才可以判断状态，否则一直为undefined(且执行value回调)
  new Promise((resolve, reject) => {
        resolve(1)
      }).then(
        value => {
          console.log('onResolve', value)
          return Promise.resolve(4)  // 当返回Promise.resolve执行value函数值为4
          当返回Promise.reject时执行reason函数，当返回throw 3时执行reason函数，当返回单数字时执行value函数
        },
        reason => console.log('onReject', reason)
      ).then(
        value => console.log('onResolve', value),
        reason => console.log('onRejct', reason)
      )
      
  5. promise错误穿透(抛出的promise对象then方法无法处理，并返回到该行)
  	new Promise((resolve, reject) => {
        reject(3)
      }).then(value => console.log(1)
      ).then(value => console.log(2)
      ).then(value => console.log(3)
      ).catch(reason => console.log(reason))
      在then回调中，没有写入第二个函数(reason函数)，默认会隐式往下抛出错误(相当于throw reason),直到抛给catch才正常输出
      
  6. 中断 then链 (返回一个pending状态的promise到该行，使以下then方法调用无法成立(只会处理resolve和reject状态的promise))
   向下返回pending状态的promise，让下一个then无法做判断
   new Promise((resolve, reject) => {
        reject(3)
      }).then(value =>{
       console.log(1)
      return new Promise(()=>{}) // 返回了一个为pending状态的promise，以下方法无法处理
      }
      ).then(value => console.log(2)
      ).then(value => console.log(3)
      ).catch(reason => console.log(reason))
  */
  ```




+ promise重写

  ```javascript
  // 简单 Promise重新写
  class CustomPromise {
      private  value: any;
      private reason: any;
      private curState: string = "pending";
  
      public constructor(fn: any) {
          this.value = null;
          this.reason = null;
          let _this = this;
          /*
          *  因为js特有 箭头函数this会指向外层的包裹对象，
          * 因此如果在创建实例对象时，使用箭头函数则需要保存一下this指向(因为其this会指向外层)
          * 在完成创建实例对象后，则不需要保存this,因为方法调用的this指向的是调用者
          * */
  
          // resolve状态
          function resolve(value: any): void {
              if (_this.curState == "pending") {
                  _this.curState = "fulfill";
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
  
      /**
       *  @description 此函数用来保证多个promise保证同一状态
       *      ，如果全为fulfill则返回全部promise对象，有一个rejected状态的promise则返回该promise对象
       *
       * @param arr 用来存储传入的所有数据
       * */
      public static all(arr:CustomPromise[]):CustomPromise{
  
          let success:any[] = new Array(arr.length);
          for (const ele of arr) { // 遍历每个promise对象，保存每个promise的状态
              if(ele.curState == "rejected"){ // 遍历到一个rejected状态的promise立刻返回
                  return new CustomPromise((resolve,reject)=>{
                      reject(ele.reason);
                  });
              }
              success.push(ele.value);
          }
          return new CustomPromise((resolve,reject)=>{
              resolve(success);
          });
      }
      /*
      *  then 和 catch方法都需要对当前的状态进行判断，才能调用对于的函数
      *
      * */
  
      public then(fn1: any = null, fn2: any = null): CustomPromise {
  
          /**
           *  当一次then完毕后 需要对返回值做判断，如果为 null 说明可以传递一个相同状态的为null的promise对象
           *  如果又返回值需要对其类型做判断，如果是异常则需要调用Reject,。。。
           *
           * */
          if (this.curState == "fulfill" && fn1 != null) {
  
              // 没有返回值 则抛出一个相同状态但值为null的Promise对象
              let successValue = null;
              try {
                  successValue = fn1(this.value);  // 没写返回值默认返回null
              } catch (e) {
                  // 出现的错误 要么是 Error类型的 有么就是直接throw 的
                  // Error类型需要使用 e.message 拿到错误信息
                  // throw 抛出的错误则可以直接打印
                  if(e instanceof Error){
                      return CustomPromise.reject(e.message);
                  }else{
                      return  CustomPromise.reject(e);
                  }
              }
  
              if (successValue == null) { // 如果没有返回值
                  return CustomPromise.resolve(undefined);
              } else if (successValue instanceof CustomPromise) { // 如果抛出的是一个Promise
                  return successValue;
              } else {
                  return CustomPromise.resolve(successValue);
              }
          } else if (this.curState == "rejected" && fn2 != null) {
              // 如果当前是rejected状态则执行第二个函数
              let failValue = null;
              try {
                  failValue = fn2(this.value);  // 没写返回值默认返回null
              } catch (e) {
                  if(e instanceof Error){
                      return CustomPromise.reject(e.message);
                  }else{
                      return CustomPromise.reject(e);
                  }
              }
              if (failValue == null) { // 如果没有返回值
                  return CustomPromise.reject(undefined);
              } else if (failValue instanceof CustomPromise) {
                  return failValue;
              } else {
                  return CustomPromise.reject(failValue);
              }
          }
  
          // 如果这个值没有被处理则照常返回 (把当前this抛出即可)
          if(this instanceof CustomPromise){
              return this;
          }
      }
  
      public catch(fn1: any = null): CustomPromise {
          if (this.curState == "rejected" && fn1 != null) {
              let returnValue = null;
              try {
                  returnValue = fn1(this.reason);
              } catch (e) {
                  if(e instanceof Error){
                      return CustomPromise.reject(e.message);
                  }else{
                      return CustomPromise.reject(e);
                  }
              }
              if (returnValue == null) { // 如果没有返回值
                  return CustomPromise.reject(undefined);
              } else if (returnValue instanceof CustomPromise) {
                  return returnValue;
              } else {
                  return CustomPromise.reject(returnValue);
              }
          }
          if(this instanceof CustomPromise){
              return this;
          }
      }
  }
  ```

  



+ async&await

```js
/*
注意
	await 必须写在async函数中 ，但async函数可以不写await
	async 函数返回的是一个promise实例对象(默认为成功的回调，如果想抛出一个失败的回调使用 throw或者 Error),当返回除了 throw或Error时默认都会包装成一个成功的回调；我们也可以返回一个promise实例对象，此时该async函数的返回值就是该promise

 1. async 函数
 函数的返回值为promise对象，promise对象结果由async函数执行的返回值决定
 简化了promise对象的使用，不用再通过then指定回调函数结果数据，有效解决了 "回调地狱问题"，是终极解决方案
 被 async 关键字修饰的函数会立刻执行不会进入分队列执行
 async function e() {
      console.log(3)
    }
    e()
    console.log(4)
 // 结果 3 4
 
 2.await 表达式
 await右侧表达式一般为promise对象，但也可以为其他值，如果表达式是promise对象，await返回的是promise成功(.then)的值，有效解决了代码冗杂问题，如果表达式是其他值，直接将此值作为await返回值
 3. await必须写在async函数里，但async函数中可以没有await,如果await的promise值为失败，就会抛出异常(await只能获取.then成功的返回值)，需要通过try...catch来捕获处理
 
 function fn3() {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(5)
        }, 1000);
      })
    }
    // async函数会立刻返回一个pending状态的promise对象(无论是否有定时器)
 async function fn2() {
      console.log(fn3())
      // await后如果是promise对象会自动执行.then的成功方法，并返回其及结果
      // 如果是reject对象则需要配合try...catch抛出错误(见过获得promise失败的值)
      try {
        const result = await fn3();
        console.log(result)
      } catch (err) {
        console.log(err)
      }
    }
    fn2()
*/
```





+ 宏队列&微队列

  ```js
  /*
   DOM，ajax,定时器都是宏队列里的，宏队列里的叫宏任务
   promise,mutation都是微队列里的，微队列里的叫微任务
   再执行一个宏任务前，系统会自动扫描微任务是否已经执行完毕，如果执行完毕就执行当前宏任务，在下次宏任务准备执行前也会再微队列进行扫描查看是否执行完毕再执行当前宏任务(往复)
   当js模块执行完主线程里的数据后，就会执行微队列里的数据，再执行宏队列里的数据
   js引擎执行过程：程序从上往下执行，执行到同步代码立刻执行，执行到异步代码放到分线程执行，当异步代码执行完后会被钩子函数勾到宏队列或微队列等待执行，待主线程代码全执行完毕，在执行微队列在执行宏队列
   
   注意
   	宏队列里 存储 dom事件回调，ajax回调，定时器
   	微队列里 存储 promise回调，mutation回调
  */
  ```
  
  

![image-20220326204041603](D:\typora_import_images\typora-user-images\image-20220326204041603.png)



## 1.5 axios

### 研究axios源码

+ api分类

  ```js
  /*
   1.REST API
   发送请求进行CRUD(增删改查)哪个操作由请求方式来决定，同意请求路径可以进行多个操作，请求方式会用到GET/POST/PUT/DELETE
   2.非REST API  restless
   请求方式不决定请求的CRUD操作，一个请求路径只对应一个操作，一般只有GET/POST  
  */
  ```

+ json-server服务器包

  ```js
  /*
   npm i -g json-server 全局安装此包
  */
  ```


![image-20220328182815396](D:\typora_import_images\typora-user-images\image-20220328182815396.png)



+ XHR

  ```js
  /*
   使用XMLHttpRequest(XHR)对象可以与服务器交互，也就是发送ajax请求，前端可以获得到数据而无需刷新页面
   ajax请求是一种特别的http请求，浏览器端发出的请求只有XHR和fetch发出的是ajax请求，其他的都不是ajax请求
   
   git从远程仓库克隆分支到本地
   git checkout -b dev origin/dev(指定远程的分支)
  */
  ```

  ![image-20220328204735113](D:\typora_import_images\typora-user-images\image-20220328204735113.png)

  ![image-20220328204915292](D:\typora_import_images\typora-user-images\image-20220328204915292.png)

  ![image-20220330153050813](D:\typora_import_images\typora-user-images\image-20220330153050813.png)

  ![image-20220330154140271](D:\typora_import_images\typora-user-images\image-20220330154140271.png)

  

  

  

+ axios

  ```js
  /*
  前言
  	axios是对ajax的异步promise封装
  	请求数据包括请求行(在url中，对应params),请求头(query)，请求体(body)
  
   函数的返回值是与一个promise对象，成功的结果是response，异常结果为error,能处理多种类型请求(GET/POST/PUT/DELETE),函数的参数为一个配置对象
   
   axios是前端最流行的ajax请求库，react/vue都推荐使用
   
   全局axios默认值设置方法
   axios.defaults.baseURL = 'xxx'  // 设置初始页面网址，如果不写绝对地址默认使用该地址可以再此基础上访问其他路由
   
   axios特点
   （1）基于promise的异步ajax请求库，浏览器端node端都支持使用，
   （2）支持请求/响应拦截器，支持请求取消
   （3）支持请求/响应数据转换，批量发送请求
   
   axios 常用语法
   	注意
   		axios中url必须写，其他参数为可选项，如没写传递方式(method)，默认使用get方式 ，
  
   	使用对象形式发送axios请求(请求发出后返回得是一个promise对象)
       axios({
          method: 'GET',
          url:'/post'
          params:{ // 指定的是query参数，并不是url中的params参数(即get携带的参数)
              id:3
          },
          data:{ // 指定请求体参数(即post请求通过该配置传递参数)
              name:22,
              age:1
          }
       }).then(response=>{},err=>{}) // 响应接收数据
       
       使用对象方法发送指定得axios请求
           //get方法发送请求, get发送请求时，请求参数由 params携带
            格式为 axios.get('url',{"配置对象"})
           axios.get('/index',{params:{id:3}}).then(response=>{},err=>{})
          	
          //post方法发送请求, post发送请求时，参数有第二个形参携带
           格式为 axios.post('url',{"携带参数"},{"配置对象"})
           axios.post('/post',{title:'e',author:'f'}).then(response=>{},err=>{})
   
  
  	
  
  */
  ```

+ 创建axios实例

  ```java
  /*
   前言
   	每个axios实例有自己的地址，发起请求时会遵守自己地址中得一套默认参数配置(该实例发起得请求都会依附于它得默认配置)，当需要额外一套参数配置时，此时就需要创建一个axios实例 然后对其进行配置即可。
   	一个axios地址可以发送n次ajax请求，开启一个实例axios(地址)，是为了配置一个独有配置信息，此时该axios实例发起的请求就会遵循其配置
  
   注意
   	在不创建axios其他实例的情况下，默认只有axios一个实例，axios实例发出的请求依附于其的配置
   	当一个axios实例指定了其baseURL后，其发出的请求只需携带路由即可(也可以携带完整路径，axios底层会自动做判断，如果是完整的url路径则使用指定的，如果是路由则其会进行拼接)，axios会自动进行 baseURL+路由的拼接
   	创建axios实例一般用于二次封装，也可以用于区别请求
   	每次生成一个axios实例 就相当于是开辟了一块空间，该空间可以使用axios提供的属性和方法来发出ajax请求，可以对其属性进行更改来达到控制该axios实例状态目的
   	
   创建实例axios
    语法：axios.create({}) 。 会返回一个指定配置的axios实例
    const host3000 = axios.create({  // 创建了一个实例，指定实例默认url为 http://localhost:3000 ，当已该实例发出请求时，url可以只写路由，默认以 baseURL+路由 拼接发送请求
        baseURL: 'http://localhost:3000',
      })
      const host4000 = axios.create({ // 创建了一个实例，指定实例默认url为 http://localhost:4000 
        baseURL: 'http://localhost:4000'
      })
      host3000({
        url: '/',  // 再直接路由时，默认会和当前axios实例的baseURL进行p
        method: 'GET'
      }).then(response => console.log(response, err => console.log(err)))
      host4000({
        url: '/',
        method: 'GET'
      }).then(response => console.log(response, err => console.log(err)))
      
  */
  ```

+ 拦截器

  ```java
  /* 
  注意
  	请求拦截器实质上是一个then方法，在发请求/响应成功时,调用的是第一个函数,如果失败了则调用的是第二个函数
  	拦截器工作过程  发出请求 --> 被请求与拦截器拦截 --> 服务器接收数据 --> 服务器返回数据 --> 被响应拦截器拦截 --> 客户端 ， 其中请求拦截器和响应拦截器拦截的数据一定抛出，否则数据会丢失
  
   axios拦截器
   
  	添加请求拦截器,请求拦截器在请求之前执行，可以对请求进行处理及检查(选择是否发出请求)
  	axios.interceptors.request.use(config=> {
  		.... // 逻辑代码
  		return config  // 必须返回config因为里面包含了环境信息，如果不返回则无请求发出
  	},err =>{... return Promise.reject(err) 发出请求可省略第二个函数}).
  	
  	
  	添加响应拦截器，响应拦截器在响应之前执行，可以对响应进行处理及检查(选择是否发出请求)
  	axios.interceptors.response.use(data=> {
  		....
  		return data  // 必须返回data因为里面包含了响应数据
  	},err =>{
  	... 
  	throw err // 如果失败则抛出Promise对象错误，并执行主axios的err
  	}). 
  */
  ```

+ 终止请求

  ```java
  /*
  前言
  	当我们想对发出的请求进行中断时，就可以使用此方法
  	axios 内部封装了一个中断该请求的方法
  	终止请求的方法也可以在拦截器中加载
  
  注意
  	axios实例下有一个isCancel()方法可以判断此请求的中断方式，如果是axios中断则为true,否则为false
  	
    axios.CancelToken 为构造函数，是axios的一个配置选项，可以取消(让其变为reject undefined)
   // 外部变量接收停止函数
      let cancel = null;
      host3000({
        url: '/',
        method: 'GET',
        cancelToken: new axios.CancelToken((c) => { // 配置强制取消对象
          cancel = c  // 获得取消方法,保存到全局
        })
      }).then(response => console.log(response), err => {
        //在添加了cancelToken配置项后需判断两种错误，即是服务器错误，还是强制取消
        // 是否因为axios.cancel中止请求
        if (axios.isCancel(err)) { // 是 axios.cancel强制取消
          console.log(err, 'cancel启动')
        } else {
          console.log('服务器错误') // 不是强制取消
        }
      })
      let btn = document.querySelector('input')
      btn.onclick = () => { // 当按钮被点击时调用中断方法来进行请求中断
       // 点击后强制取消本次响应，并执行reject分支
        cancel() // cancel函数内可以形参在执行err时以message形式存储
      }
  */
  ```

  

+ 源码分析

  ![image-20220330210234112](D:\typora_import_images\typora-user-images\image-20220330210234112.png)

  ![image-20220330210245119](D:\typora_import_images\typora-user-images\image-20220330210245119.png)

  

  





+ 简单axios实现

  ```js
      class Axios {
          constructor() {
          }
  
          sendReq(obj) {
              return new Promise((resolve, reject) => {
                  let xml = new XMLHttpRequest();
                  console.log(obj);
                  let {url, params, method} = obj;
                  xml.onreadystatechange = () => {
                      if (xml.status === 200 && xml.readyState === 4) {
                          resolve(xml.response);
                      }
                  }
                  xml.onerror = (e) => {
                      reject(e);
                  }
                  xml.open(method, url);
                  xml.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
                  let data = "";
                  for (const k in params) {
                      data += k + "=" + params[k] + "&";
                  }
                  data = data.substr(0, data.length - 1);
                  console.log(data);
                  xml.send(data);
              });
  
          }
      }
  
      new Axios().sendReq({
          url: "http://127.0.0.1:9999/login",
          method: "post",
          params: {
              user: "zs",
              pwd: 123456
          }
      }).then(value => console.log(value), reason => console.log(reason));
  ```

  

