## 工作流简介

### 注册 Phabricator
> Phabricator 是 Facebook 研发的任务管理系统，用于多人协作下的任务指派、评审和提交

1. 打开 [http://phab.riuir.com](http://phab.riuir.com)，然后注册一个新用户
2. 完成邮箱认证并等待管理员通过注册申请
3. 进入网站后依次进行如下操作：
    ![phab1](https://cdn.riuir.com/owner/phab1.png)
4. 这样你就已经上传了自己的 SSH key，之后就可以对代码进行提交了

### 安装 Arcanist
> Arcanist 是要与 Phabricator 配合的命令行工具，用于代码的提交，任务的交接

1. 安装：[MacOS安装指南](https://secure.phabricator.com/book/phabricator/article/arcanist_quick_start/)，[window安装指南](https://secure.phabricator.com/book/phabricator/article/arcanist_windows/)
2. 安装完成之后你需要在 github-repo 所在的目录下运行：
``` sell
arc install-certificate
```
3. 然后要输入一个 Token，这个 Token 跟着下图的步骤可以获取：
    ![phab2](https://cdn.riuir.com/owner/phab2.png)
4. 至此，工作前的准备就完成了

### 开始
1. 首先，你会接受到一个 task，一般情况下我会在 IM 软件中与你沟通，发给你网址，这个网址类似于：[http://phab.riuir.com/D4](http://phab.riuir.com/D4)
2. 任务的要求会在页面上展示，根据要求 coding
3. 进入索要工作的 repo 目录下，执行：
```shell
arc tasks
```
你会看到类似这样的返回：
![phab3](https://cdn.riuir.com/owner/phab3.png)
4. 看到你的 task 列表后，然后执行：
```shell
arc feather T7
```
这个**`T7`**是任务的编号，然后你就会自动 new 一个叫 T7 的 branch，现在就可以开始 coding 了
> `注意：你在 arc feather 之前先 checkout 到 master 分支，因为你要在当前 branch 创建一个新分支`<br/>
> `注意：前端和后端的开发人员，除非是严重的 bug fix，不然不要直接 git push to master branch，因为 master 分支是自动部署的`
5. 当你 coding 完之后，先将代码 push 到你的 dev 分支，比如我这边就是 T7，然后你需要运行如下命令：
```shell
arc diff master --preview 
```
它会把你当前所在的 branch 和 master 分支 diff，然后提交 preview，如图：