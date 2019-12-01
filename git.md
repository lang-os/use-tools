## git版本控制系统

### 常用的版本控制系统

- svn/cvs: 集中式版本控制系统

  - 必须有一个中央总控的服务器(用来存储历史版本和代码信息)
  - 中央服务器
    - 代码仓库
      - 储存最新代码
      - 提交既生成历史版本(1,2,3)
      - 获取代码,直接在中央服务器上拉取就行

- git: 分布式版本控制系统

  - 每个开发者本地就是一个代码管理仓库

    - ```she
      $git init
      ```

    - 工作区>暂存区>历史区(版本1,2,3)

  - 中央服务器 git-hup

  - 优点

    - 无需联网也能记录和查看历史版本信息
    - 无需依靠中央仓库,每个人本地也有全部的信息
    - 传输以文件流形式  更快

## git工作原理

- 工作区 
  - 我们能看到的,并且用来写代码的区域
- 暂存区
  - 临时存储用的
- 历史区
  - 生成历史版本

## git的全局配置

- 第一次安装完成git后,我们在全局环境下配置基本信息:

  ```shell
  $ git config - l 查看配置信息
  $ git config --global -l  查看全局配置信息
  配置全局信息:用户名和邮箱
  $ git config --global user.name 'xxx'
  $ git config --global user.email 'xxx#xx.xx'
  ```

## 创建仓库完成版本控制

- 创建本地git仓库

- ```shell
  git init
  会生成一个隐藏文件夹'.git' (这个文件夹千万不要删,因为暂存区和历史区还有一些其他的信息都在这儿,删了就不是一个完整的仓库了)
  ```

- 在本地编写完成代码后(在工作区),把一些文件提交到暂存区

  - ```shell
    $ git add xxx		#提交一个文件
    $ git add . / -a  #提交所有文件
    $ git status #查看当前文件状态(红色代表在工作区,绿色代表在缓存区,看不见代表所有的修改的信息都已经提交到历史区中)
    ```

- 把暂存区的内容提交到历史区

  - ```shell
    $ git commit -m'描述信息:本次提交内容的一个描述'
    
    #查看历史记录
    $ git log
    $ git reflog  包含回滚信息
    ```

## 上传git-hup

- ```shell
  建立本地仓库和远程仓库的连接
  $ git remote -v  查看连接
  $ git remote add origin [git仓库地址]
  取消关联
  $ git remote rm origin
  
  ```

- ```shell
  提交之前最好先下载
  $ git pull origin master
  把代码提交到远程仓库 (需要输入用户名密码)
  $ git push origin master
  
  ```

- ```shell
  用于真实项目开发克隆git项目到本地
  $ git clone [远程仓库地址] [别名:可以不设置,默认是仓库名]
  ```

- 

## 创建分支

- ```shell	
  #创建
  git checkout -b login
  #查看
  git branch
  ```

- ```shell
  #在云端创建分支user
  git push -u origin user
  #将分支的代码合并到主分支
  git merge user
  #提交
  git push
  ```

- 