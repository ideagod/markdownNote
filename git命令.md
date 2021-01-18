# git命令

#### **git上传代码步骤**

- 建立git仓库，在要上传的文件夹里面执行:git init
- 将项目的所有文件添加到仓库中：git add .(注意最后的点“.”不能省略)
- 将add的文件commit到仓库:git commit -m "填写需要的注释"
- 在github上面创建自己的repository（点击个人头像旁边的加号 ），点击New repository，填好所有信息后点击create repository
- 将本地的仓库关联到github上 :git remote add origin http:// git.coding.net/用户名/项目名.git自己的仓库url地址
- 上传：git push -u origin master（第一次上传有可能会遇到push失败的情况，那是因为跟SVN一样，github上有一个README.md 文件没有下载下来 。我们得先：git pull --rebase origin master  ，然后执行git push -u origin maste）





假如没有配置，则：

- git config --global user.email 123456@qq.com(邮箱)
- git config --global user.name zhangsan(名字)

