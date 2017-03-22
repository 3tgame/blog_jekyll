---
published: true
layout: post
title: npm dependencies 使用Git仓库
date: 2017-03-22T17:10:18.000Z
categories:
  - npm git
---
Git仓库，可以是自己建的，可以是Github仓库，本文以Coding.net仓库为例。

因为 package.json 里的 dependencise，是使用npm install 执行安装的，可参考 [install | npm Documentation](https://docs.npmjs.com/cli/install)的说明，了解 可使用哪些方式配置 dependencise。


# 使用Coding.net私有仓库
因为私有仓库，涉及授权，故如下各种方法都要配置授权信息，才能使用。

## 指定http url
也支持直接使用 http 指定压缩包url，从项目代码页面右上方可看到“下载”按钮，可从中获得代码压缩包url，可配置如下：

	"jkpt_mobile": "https://coding.net/t/eyevision/p/jkpt_mobile/git/archive/develop"

但在Coding.net没找到该方法的授权方式，故该方式不支持使用私有项目。

## 使用git+ssh
因为git协议不包含授权机制，需结合ssh协议才能达到授权目的。

     "jkpt_mobile": "git+ssh://git@git.coding.net:eyevision/jkpt_mobile.git"

该方式需要生成SSH密钥，参考[配置SSH公钥](https://coding.net/help/doc/git/ssh-key.html#section)的方法生产公私钥。生成的公私钥id_rsa、id_rsa.pub默认是在用户目录的 .ssh 中，如Windows则是（C:\Users\xxx\.ssh，其中xxx为用户名），Linux则为~/.ssh。

不要修改生成后的文件名，如修改为coding_rsa，coding_rsa.pub，则需要在~/.ssh/config文件中配置如下：
     Host git.coding.net
     #User xxxx@email.com
     PreferredAuthentications publickey
     # 生成的非默认地址的公钥存放点
     IdentityFile ~/.ssh/coding_rsa

然后在Coding.net的项目或个人账户里添加公钥，添加方法参考[配置SSH公钥](https://coding.net/help/doc/git/ssh-key.html#section) 。

首次建立连接会要求信任主机，因此在 Git Bash（在Windows安装Git后，在任意目录右键菜单项中选择“Git Bash Here”，即可打开Git Bash） 中，执行 ssh -T git@git.coding.net，输入yes，信任主机后，才执行 npm install

## 使用git+https
因为git协议不包含授权机制，需结合https协议才能达到授权目的。

     "jkpt_mobile": "git+https://git.coding.net/eyevision/jkpt_mobile.git#develop"

#后面可指定分支名，或tag，或commit id

### Windows下的授权（本文以Window 10 为例）
执行 npm install（或执行 git clone https://git.coding.net/eyevision/jkpt_mobile.git#develop），如没有授权信息，则会弹出一下窗口，提示输入你在coding.net的用户名、密码。

![enter_credential.png]({{site.baseurl}}/_images/enter_credential.png)

添加后，可在 Window 10 中的 cortana 中输入“Credential Manager”，打开“凭证管理器”，可看到已加入git.coding.net的凭证。

![credential_manager.png]({{site.baseurl}}/_images/credential_manager.png)

如没有，可在这里按上图所示的信息，手动添加一个普通凭证。

### Linux下的授权
在 ~/.git-credentials （没有就创建）内添加

      https://username:passwd@git.coding.net

其中 username、passwd，根据实际值编辑，然后执行配置 Git 命令存储认证命令：

      $ git config --global credential.helper store

执行后在 ~/.gitconfig 文件会多出下面配置项: credential.helper = store

参见 [怎样在每次 Push 时不用重复输入密码？](https://coding.net/help/faq/git/git.html#push-)

# 使用Coding.net公有仓库
如果使用公有仓库，则除可使用以上私有仓库介绍的方式（不需配置授权信息）外，还可使用git://协议。

在 package.json 中配置

     "dependencies": {
          "jkpt_mobile": "git://git.coding.net:eyevision/jkpt_mobile.git#develop"
     }



