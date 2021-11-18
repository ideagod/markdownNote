# git命令

## 1.git本地上传代码步骤(use)

1. 登陆github后，进入Github首页，点击New repository新建一个项目

2. 

   填写相应信息后点击create repository即可 

   Repository name: 仓库名称（输入名字，最好不要使用中文）

   Description(可选): 仓库描述介绍

   Public, Private : 仓库权限（公开共享，私有或指定合作者）

   Initialize this repository with a README: 添加一个README.md

   gitignore: 不需要进行版本管理的仓库类型，对应生成文件.gitignore

   license: 证书类型，对应生成文件LICENSE

3. 接下来就到本地操作了，首先右键你的项目，Git Bash Here

5. 接下来输入如下代码（关键步骤），把github上面的仓库克隆到本地

   git clone（https://github.com/e1152366636/myown1.git替换成你之前复制的地址）

6. 将你本地的项目复制到myown1目录下

7. 接着继续输入命令 cd myown1，进入myown1文件夹

8. 接下来依次输入以下代码即可完成其他剩余操作：

   git add * .  （注：此操作是把myown1文件夹下面的文件都添加进来）

   git commit  -m  "提交信息"  （注：“提交信息”里面换成你需要，如“first commit”）

   git push -u origin main（注：此操作目的是把本地仓库push到github上面，此步骤需要你输入帐号和密码，这里的账号和密码是你的github的账号和密码，你第一次上传的话可能会需要你输密码）.

9. 更新同理,就把更新后的文件夹重新传，他会自动覆盖的.（7



### **git上传代码步骤**

- 建立git仓库，在要上传的文件夹里面执行:git init
- 将项目的所有文件添加到仓库中：git add .(注意最后的点“.”不能省略)
- 将add的文件commit到仓库:git commit -m "填写需要的注释"
- 在github上面创建自己的repository（点击个人头像旁边的加号 ），点击New repository，填好所有信息后点击create repository
- 将本地的仓库关联到github上 :git remote add origin http:// git.coding.net/用户名/项目名.git自己的仓库url地址
- 上传：git push -u origin master（第一次上传有可能会遇到push失败的情况，那是因为跟SVN一样，github上有一个README.md 文件没有下载下来 。我们得先：git pull --rebase origin master  ，然后执行git push -u origin maste）



假如没有配置，则：

- git config --global user.email 123456@qq.com(邮箱)
- git config --global user.name zhangsan(名字)

ee16418117331dd9940b298df920d8c4f7e1ae8f



/*--------------------------------------------------------oldNote----------------------------------------------------------------

2.git更新本地代码

若代码需要合并（本地有修改，需先

```
git add .
git commit "" 暂存
```

commit 暂存，否则会覆盖掉。

**可视化git管理工具：sourceTree**



--------------------------------------------------------oldNote----------------------------------------------------------------*/



### 2.1git 更新本地代码

1. 本地代码未修改，只有master分支，直接更新

```
git pull
```

但前提必须是本地的代码没更改过。比如，你提交了代码到 github 后，随后别人也提交代码到 github，然后你需要更新别人提交的代码到你本地，

就可以直接使用该命令。假如你提交代码后再修改过你本地的代码，就会产生冲突，直接使用该命令会失败的。

2. 本地代码有修改，多分支。

```git
//切换到master分支
git chekout master

//更新master分支
git pull

//切换到自己的分支isso
git checkout isso

//把master分支合并到自己分支
git merger master
```

3. 本地代码有修改，只有master分支，直接覆盖本地代码

```git
//重置索引和工作目录
git reset --hard

//更新代码
git pull
```

4.本地代码有修改，直接覆盖远程仓库代码

```git
// 覆盖isso分支
git push --force origin isso

// 覆盖主分支
git push --force origin master
```

换服务器地址了要重新git clone一遍，后面用git pull就可以

## 3.从公司下克隆项目

```git-bash
git init
git clone +ssh地址
```

！ssh地址的localhost要改成192.168.1.30

●此方法应用在同一个局域网里面，不同的域网需要使用http来拉。

2021.4.29

2.使用http拉，不同局域网要改局域网地址

```
http://改成链接上的域网地址/root/activitycenter.git
```

拉下来后倒入hbuilderX，然后

```
npm install
npm install @vue/cli
npm install vue -g
```



/*-------------------------------------------------------oldNote----------------------------------------------------------------

4.uni-app运行微信小程序项目

```
运行-小程序-微信
```

如果工具的服务端口关闭，则去微信-设置-安全设置打开

![image-20210407121229528](C:\Users\breo\AppData\Roaming\Typora\typora-user-images\image-20210407121229528.png)

运行后uni-app的项目会自动转为wxss，贼牛

2.1但是uni-app不能用到微信小程序的样式，得自己调





如何将基础项目与本地存储挂钩

git clone下去，然后用sourceTree挂钩既可 

----------------------------------------------------------------------------------------------------------------------------------*/



# 本地分支合并到远端仓库

2021/09/27

1. 需求：本地迁出分支

   1. ```
      git checkout -b lxe0927
      ```

2. 查看

   1.现在所在分支

   ```
   git branch -a
   ```

   2.分支状态

   ```
    git status
   ```

   3.从a分支跳到主分支

   ```
   git checkout master
   ```

   4.拉主分支下来解决冲突

   ```
   git pull origin master
   ```

   5.提交a分支，融入现有的主分支上

   ```
   git add .
   git commit -m "merge a"
   git merge a
   ```

   6.合并，解决冲突

   ```
   git add .
   git commit -m "merge fix"
   git push origin master
   ```

   7.创建新的分支 newBranch

   ```
   git checkout -b newBranch
   ```

3. 切换分支

   ```
   git checkout newBranch
   ```

放弃本地更改

```
git checkout .
```

git 获取远端，创建本地分支

```cmd
git fetch //融合远端和本地分支
git branch -a //查看全部分支
git checkout -b 本地分支名 origin/远程分支名 //远程分支切到本地分支
```

