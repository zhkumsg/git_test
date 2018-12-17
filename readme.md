# git

> git 是一个开源的版本控制工具，类似于 svn 和 tfs

-   同类的工具有 svn 和微软的 Team Foundation Server（TFS，vs2010 那一套），用于本地保存不同版本的资源（保存在`.git`文件夹中，也就是仓库）。常与 github 网站搭配使用，git 把 github 当做线上仓库。

*   git 和 github 不是必然关系，github 是一个搭配了 git 工具的服务器，并通过免费向用户提供存储空间的网站（也叫`同性交友网`）;而 git 可以脱离 github 而存在，git 原本就是本机使用的，为了方便才推出了`远程仓库`的功能，而 github 就是最出名的一个远程仓库

下面将以初始化 git、查看状态、添加文件进暂存区、提交到缓存区、注册 github 账号、推送本地文件、拉取最新文件的顺序介绍，最后再介绍一下 git 的版本回退、分支以及 git 忽略规则。

---

## git 简单入门

#### window 下安装 git

直接去官网下载安装即可，你已经安装了，可以跳过这一步

#### 创建项目（创建文件夹）

git 是对文件夹下的全部资源做监听的，一般情况下我们是对一个项目做版本控制监听，所以用 node 创建一个项目（在新建文件夹`git-test`下）

```bash
$ npm init
```

一路默认就行了，这里是 npm 的知识，还没到 git。全部回车后会在 git-test 下新建一个 package.json

#### 初始化 git

git 需要在文件夹下初始化，表名对该文件夹下的资源进行监听（默认是全部监听，可以配置监听特定部分，后面介绍），这里使用的是`git init`命令。我们先在命令行工具下进入 git-test 目录，可以使用 cmd，也可以使用 win10 的 PowerShell，还可以使用 git 标配的`git bash`,三种的命令是一样的

```bash
$ git init
```

回车后提示如下

```
Initialized empty Git repository in F:/Team/git-test/.git/
```

而且会在当前文件夹下新建一个隐藏文件夹`.git`,这个文件夹很重要，该项目的版本控制都在里面，如果删除了这个项目就变回普通模式。

#### 新建文件

git 本地的最常见操作就三条命令，分别是添加文件就暂存区（内存中）、提交文件到本地仓库（.git 文件夹中）以及查看当前状态。为了方便演示，先在项目下新建一个`test.txt`文件（怎么新建都行，复制也行）。  
然后在该 txt 文件中随意写点东西，保存。

```txt
随便写任何东西........
```

只要文件已发生变化，git 就会监听到，这时候我们先用 git 来查看一下当前的状态，使用 git status

```bash
$ git status
```

回车后可以看到下面有红色字体标着的

```
package.json
test.txt
```

这表明当前工作区内有两个文件发生了变化。这时候 git 还没用做任何操作，只是查看了一下状态而已，接着我们要把这个文件添加进暂存区（英文提示也说了`use "git add <file>..." to include in what will be committed`）

#### 添加文件进缓存区

文件变动后，我们要把文件添加到暂存区，告诉 git 准备把文件`备份`一下。这是我们先使用`git add` 命令,它可以带一些参数，如文件路径列表或者通配符，比如要把 test.txt 和 package.json 放进暂存区，使用

```bash
$ git add ./test.txt ./package.json
```

这是可以精确控制具体文件添加到暂存区的，不过更多情况下是使用通配符`-A`，这代表把全部更新的文件都放进暂存区（放到内存中），如下

```bash
$ git add -A
```

要记得使用 status 来查看一下状态，及时了解当前情况，使用`git status`。查看状态是可以看到绿色的文字，留意的话可以看到里面有这么一句话

-   use "git rm --cached <file>..." to unstage  
    现在还不用理会这句话，它是拿来做文件回退的，也就是撤销更改，我们现在要做的是继续`提交进产库`

#### 提交进仓库

文件 add 进暂存区（内存）后其实并没有真实进到仓库（隐藏文件夹.git）中，接着我们需要填写提交信息并提交，命令如下

```bran
$ git commit -m "你要备注的信息..."
```

其中-m 表示 message，后面是你要备注的信息，回车后文件直接添加进.git 隐藏文件夹中（本地仓库），这就是文件修改和提交的完整过程。

这时候我们在通过 git status 来查看一下状态，会发现  
_nothing to commit, working tree clean_ 工作区是干净的，这表明你当前编辑的文件与本地仓库里的文件是一模一样的。

## 到这里都是本地操作，不过 git 和 github 配合才能发挥 git 的强大功能，我们要把项目放到线上，可以在不同电脑上修改同一份代码。

## github 简单入门

github 是最出名的一个代码管理平台，官网为https://github.com/，如果因为网络原因无法访问，可以使用码云，链接为https://gitee.com/。

#### 注册 github 账号

在 github 官网导航栏上，点击`sign up`按钮注册，填完信息并邮件验证账号后将完成注册

#### 配置本地 git 身份（全局）

为了与 github 或者 gitee 等代码管理平台通讯，本地 git 先配置好自己的身份（name 和 email），我们通过

```bash
$ git config user.name
$ git config user.email
```

可以分别查询到自己的 name 和 email，当时默认情况下是空，因为还没有配置过，下面我们假设名字为`test`、邮箱为`test@qq.com`

```bash
$ git config user.name "test" -global
$ git config user.email "test@qq.com" -global
```

回车后将配置了全局的 git 用户信息

#### 本地生成连接密钥（全局）

git 要与 github 建立 ssh 连接，除了本地配置好自己的身份信息外，还需要在 github 上添加自己身份的白名单，这时候我们需要创建 ssh 密钥（使用 git bash），执行下面命令

```bash
$ ssh-keygen -t rsa -C "test@qq.com"
```

一路回车执行就行

-   ssh-keygen 在 cmd 是无法找到，要使用 git bash 或者 powershell
-   -C "test@qq.com"对应前面配置的 git 身份信息

运行完成后，在用户主目录下可以看到多了一个.ssh 文件夹（用户目录是当前 window 用户目录，也就是桌面的 Administrator 目录下，如果没找到，可以去 C:\Users\下找对应的文件夹）

用户目录下有很多文件，都是一些系统配置文件，如.config 是 npm 全局模块相关的、.android 是安卓环境相关的、.vscode 是编辑器相关的，其中我们现在要进去的是`.ssh`文件夹

.ssh 下在执行完前面的命令后，会有两个文件，分别是 id_rsa 和 id_rsa.pub，代表 ssh 私钥和 ssh 公钥。
我们要把 id_rsa.pub 公钥的内容添加到 github 的白名单上（用 vscode 打开公钥文件，复制里面的内容）

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC7sNGIEIpCS1bnHcmdRfIbYvogch4GFi6Vq3EX+raYj1HqoVda0B....................97wMNn1 moshenggui@ijunhai.com
```

#### github 配置密钥白名单

复制好公钥后，在 github 用户中心的设置页面中的`SSH and GPG Keys`https://github.com/settings/keys 选项卡中添加一个 ssh key，title 随便填，key 就是刚刚复制的 key

新建 ssh key 完成后，当前电脑就可以连接 github 仓库了。如果没有配置，github 将会拒绝连接。

#### github 创建项目

点击 github 官网的“+”号，选择“new repository”创建一个开源项目（私有项目要收费，这也就是我选择码云的原因，因为码云免费）。仓库名 Repository name 可随便填，不过建议以本地项目同名，也就是 git_test。描述选填，“Initialize this repository with a README”是在这个项目中初始化一个 readme.md 文件，方便阅读。

我们现在都不勾选，填好仓库名 Repository name 后点击创建按钮就可以了

#### 本地克隆项目

创建完 github 空白项目后，官网给了两种连接建议，一个是创建一个空白项目再配置仓库地址最后推送，第二种是在原有项目上配置仓库地址最后推送。这两种方法都是一样的，不过因为我们已经有了项目，我们采用第二种就行。

当然，如果希望直接用线上项目，在合适位置打开 cmd，执行下面代码就可以克隆（可理解成复制）项目到本地了（带文件夹的）

```bash
$ git clone https://github.com/zhkumsg/git_test.git
```

克隆下来后，默认已经配置好远程仓库信息，不用再去配置一次（跳过下面一步）

#### 配置远程仓库

cmd 打开本地项目，查看一下当前项目的仓库信息

```bash
git remote
```

发现没有信息，这是因为我们还没有配置任何的仓库信息。我们现在复制并 github 官网的信息

```bash
git remote add origin https://github.com/zhkumsg/git_test.git
```

这代表为当前项目添加一个名为 origin 的远程仓库。（origin 为默认的远程仓库名称，现在不建议修改，后面介绍）

回车执行后就添加成功了（也是没有信息输出的，不过已经成功了）

这时候我们再输入 git remote 查看一下仓库配置信息就可以看到刚刚的配置了

#### 提交到仓库

配置好仓库后，我们可以把文件推送到远程仓库
``` bash
$ git push
```

但是，有可能会抛这样的异常出来：“git push --set-upstream origin master”，这就是手动配置仓库而不是clone的原因的，此时git不知道你要推送到哪一个仓库上（是可以配置多可仓库，而且还有分支），所以这时候我们需要指定仓库和分支（默认仓库origin、默认主分支master）,另外需要注意的是，如果线上仓库是空白的，还需要在命令中加`-u`新建主分支，如果已经不是空白的了，-u可以忽略
``` bash
$ git push -u origin master
```
进度条走完后，刷新一下github，会发现你的项目已经推送上去了。  




#### 拉去最新到本地
