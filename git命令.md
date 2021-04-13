# git命令

### git本地上传代码步骤(use)

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