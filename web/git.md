# git

## 1.git使用

+ git使用注意点

  ```js
  /*
   1. 在切换分支时，注意先提交当前的分支给git帮助管理，如果不提交则标识分支之间的文件共用，再哪提交的文件哪里可以用
   2. git checkout xxx 在切换分支时，实际上是回调到你上一次提交代码那个版本(commit)
   3. git checkout -b xxx 在创建分支时，此分支会复制上一分支的全部内容，在编写自己部分代码时，重新提交即可有自己的git管理仓库文件
  */
  ```

+ git常见问题

  ```js
  /*
   1.提交文档时可能出现密码不正确问题，此时是git的安全提示，使用二次输入账号密码，且第二次要使用github上的token作为密码
   2.更换库源时使用 git remote set-url 库名
   3.在提交文件时，注意好切换到对应分支，否则会出现错误
   4. 报错信息 443 Timeout标识github连接失败
   5. 如果只弹出一次账号密码表单框，需要删除Github凭据，再 控制面板->凭据管理器->windows凭据，删除github里对应的数据即可
   6. git checkout 切换分支时，会回溯到你上次提交文件的状态，当你创建一个分支时，该分支会复制上一个分支的所有文件，再次基础上添加自己新的代码，提交文件并构建出自己的状态，当一个分支新出的文件没有被提交时，则该分支新的文件会暴露在所有分支下，直到该文件被一个分支提交。如果分支不提交就切换，则会丢失此文件和此分支
   */
  ```

  

+ git常用命令总结

  ```js
  /*
   git init  初始化git文件夹
   git add *  提交所有代码到暂存区
   git commit -m 'xxx' 把暂存区代码提交到版本区，并记录版本
   
   git checkout -b dev 创建分支 ，此语句为常见dev分支
   git branch 查看全部分支
   git checkout master 切换到指定分支
   git merge 合并指定分支到当前分支，指定分支不受影响
   git branch -d dev 删除指定分支
   
   git remote -v 查看当前连接仓库
   git remote remove <name>
   git remote set-url xxx(地址) 即可更改连接仓库
   
   git reflog 显示每次提交的信息 id
   git log 显示最近到最远的素有提交日志
   
   git reset --hard HEAD^ 版本回退(每有一个^标识向前回退一个版本)
   git reset --hard 版本号 回退到指定版本
   
   git push orign 分支  提交本地分支文件到github仓库
   git pull origin master 获取github仓库的分支文件
   git fetch origin master:xx 把该代码抓取到新的一个分支，并传给github 
   
   git clone git://xxx 克隆仓库
   克隆分支默认克隆 master分支，但这个master分支实际上携带着该项目中的所有分支，只是不体现，当我们以这个克隆分支切换分支时， 应使用 git checkout -b dev orign/dev 意为 在本地仓库创建一个dev分支该分支继承 orign/dev 下的全部内容
  */
  ```

  

+ git上传仓库ssh(推荐使用)

  ```js
  /*
   1. 创建密钥
   	ssh-keygen -t rsa -C "邮箱"，创建中会提示输入路径和密码，连按3此空格即可，会自动创建在C:\Users\用户名下，并生成一个.ssh文件夹
   2. 密钥关联github仓库
   	打开setting-> SSH and GPG keys ->New SSH key , 进到ssh文件，打开.ssh后缀的文件(此文件内容则为ssh密钥)，复制此密钥，输入到SSH key的表单中，即可关联完毕
   3. 仓库连接
   	此后，可直接使用ssh连接，git remote add origin 'ssh地址' ， 可能会出现输入yes/no的情况，写yes即可
  */
  ```

  

+ git分支理解

  ```js
  /*
   在切换分支时，首先去该分支下的本地仓库拉取文件，如果没有则会复制上一个分支的全部内容到工作区，如果代码没有提交，则该分支会被仓库默认删除，当重新切换到该分支时，仍然会继续获取上一个分支的所有文件到工作区 。 如果代码提交，就会形成自己的版本区，这个分支也会被git本地仓库记录 ， 当下次切换到该分支时直接去本地仓库拉取内容
  */
  ```

  

