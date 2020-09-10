# git生存手册

## git基础

### 学习git之前

- `diff`：用于源码比较
- `patch`：打补丁

用法示例：

文件1：`test01.txt`

```
hello world
```

文件2：`test01.txt`

```
Hello World 
```

比较两个文件：

```shell
diff -u test01.txt test02.txt >diff.txt
```

如果将`test01.txt`删除，以下命令，可以恢复`test01.txt`：

```shell
cp test02.txt test01.txt
patch -R test01.txt < diff.txt
```



测试恢复`test01.txt`：

```shell
$ cat test01.txt
hello world
$ cat test02.txt
Hello World
$ rm test01.txt
$ ll
total 2
-rw-r--r-- 1 hasee 197609 140 3月  21 19:53 diff.txt
-rw-r--r-- 1 hasee 197609  12 3月  21 19:53 test02.txt

##以下命令可以恢复test01.txt
$ cp test02.txt test01.txt
$ patch -R test01.txt < diff.txt
patching file test01.txt

##查看test01.txt,可以看到已经恢复
$ cat test01.txt
hello world
```

### 版本控制的历史

- RCS：一个针对单独文件的版本管理工具
- CVS：客户端/服务器架构的版本控制系统，CVS版本库中任意一个目录拿出来都可以成为另一个版本库。
  - 确定了版本控制系统的标准，如：提交说明（`commit log`）、检入（`checkin`）、检出（`checkout`）、里程碑（`tag`）、分支（`branch`）等概念。
  - CVS在工作区的每一个子目录下都创建CVS目录。
- SVN（Subversion）：集中式版本控制系统
  - 经历了重BDB（简单的关系型数据库）到FSFS（文件数据库）的转变
  - SVN的每一次提交，都会在服务器端的`db/revs`和`db/revprops`目录下各创建一个以顺序数字编号的文件。
    - `db/revs`目录下的文件（即变更集文件）记录了与上一个提交之间的差异。（字母A表示新增，M表示修改，D表示删除）
    - `db/revprops`目录下的同名文件，保存着提交日志、作者、提交事件等信息。
  - SVN拥有全局版本号，每提交一次，SVN的版本号就会自动加一。
  - SVN在创建版本库时需遵守一个约定：先创建三个顶级目录`/trunk`，`/tags`和`/branches`
  - SVN在工作区的每一个子目录下都创建`.svn`目录
- Git：分布式管理控制工具，诞生于2005年
  - 在工作区的顶级目录下创建名为`.git`的目录（版本肯呢个在目录），可以将其移动到工作区外（移动后需要使用`--git-dir`或环境变量`GIT_DIR`为工作区指定版本库目录）

### git相关概念

- 工作区
- 暂存区
- 

### git的安装

#### linux上安装

##### 二进制安装

	git软件包在有的liunx上发行版中叫`git-core`，因为GNU交互工具（GNU Interactive Tools）在一些Linux中占用了git这个名称。不过部分Linux中`GUN Interactive Tools`名为`gnunit`，`git-core`改为了`git`。

- `git`：必装的软件包，包含了大部分Git命令
- `git-svn`
- `git-eamil`
- `git-gui`
- `gitk`
- `git-doc`：选择安装，包含了Git的HTML格式文档，安装后可以通过`git help -w <sub-command>`命令通过浏览器查看相关子命令`sub-command`的HTML帮助。

测试：Centos 安装Git

```shell
yum install git
yum install git-svn git-email git-gui gitk
```

##### 源码安装

从git官网<https://git-scm.com/>下载Git源码包，如：[git-2.9.5.tar.gz](https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz)

1. 解压：` tar -zxvf git-2.9.5.tar.gz`

2. 进入相关目录：` cd git-2.9.5`

3. 编译：`make prefix=/usr/local all`

   注意：如果出现` cc: command not found`，可以通过`yum insatll gcc`安装gcc；

   	如果出现`fatal error: openssl/ssl.h: No such file or directory`，可以通过` yum install openssl openssl-devel`解决。

4. 安装：`make prefix=/usr/local install`

安装完后，可以在`/usr/local/bin`下找到git命令。

##### 从git版本库安装

1、克隆Git项目的版本库到本地

```shell
git clone https://github.com/git/git.git
cd git
```

2、更新获取最新版本的Git：`git fetch`

3、清理上一次编译的文件，丢弃本地对Git代码的修改

```shell
git clean -fdx
git reset --hard
```

4、查看Git的里程碑，选择适当的版本安装

```shell
git tag
```

5、检出该版本的代码

```shell
git checkout v1.7.4.1
```

6、执行安装，如：安装到`/usr/local`目录

```shell
make prefix=/usr/local all doc info
make prefix=/usr/local install  \
install-doc install-html install-info
```

注：批量安装不同版本git的脚本

```shell
#!/bin/sh

for ver in v1.5.0 v1.7.3.5 v1.7.4.1 ;
do
	echo "Begin install Git $ver.";
	git reset --hard
	git clean -fdx
	git checkout $ver ||{ echo "Checkout git $ver failed.";exit 1 }
	make prefix=/opt/git/$ver all 	&& \
    make prefix=/opt/git/$ver install ||{
        echo "Install git $git failed."; exit 1
    }
    echo "Installed Git $ver"
done
```

##### 配置Git命令自动补全

如果是从源码编译安装的Git，需要手动添加命令补全，命令如下：

1、复制源码包中的命令补全脚本到`/etc/bash_completion.d/`

```shell
cp contrib/completion/git-completion.bash  /etc/bash_completion.d/
```

2、重新加入自动补全脚本，使之在当前的shell中生效

```shell
. /ect/bash.completion
```

3、设置在终端开启自动加载`bash_completion`脚本

```shell
#/etc/profile 和本地配置文件~/.bashrc

if [ -f /etc/bash_completion ];then
	.  /etc/bash_completion
fi
```

##### 配置git中文支持

- 如果文件名中的中文不能正常显示，而是显示为八进制的字符编码，通过将Git配置变量`core.quotepath`设置为flase，就可以解决，如下：

  ```shell
  git config --global core.quotepath false
  ```

- 将显示提交说明使用的字符集设置为`gbk`

  ```shell
  git config --global i118.logOutputEncoding gbk
  ```

- 设置录入提价说明的字符集是为`gbk`

  ```shell
  git config --global i18n.commitEncoding gbk
  ```




### 常用git命令

- `git help <sub-command>`命令帮助
  - `git <sub-command> --help`

- `git init`版本库初始化
- `git add`：添加文件到暂存区
  - `git add -A`将本地删除和新增的文件都加入暂存区
  - `git add -u`将所有修改过的文件加入暂存区
  - `git add -i`添加创建的新文件
  - `git add -p`可以对一个文件内的修改进行有选择的添加

- `git commit` 将暂存区中的内容提交
  - `git commit -m "描述信息"`
  - `git commit -a`修改文件并提交
  - `git commit --amend`修改提交说明
- `git rm --cached 文件名`从暂存区中删除文件

  - `git commit --amend`
- `git diff`比较文件 
  - `git diff --word-diff`逐字比较
  - `git diff --cached`查看暂存区中文件的修改
- `git tag`创建里程碑

  - `git tag v1`创建里程碑v1
- `git format-patch v1..HEAD`将从v1开始的历次修改逐一导出为补丁文件

  - 补丁文件都包含一个数字前缀，并 提取提交日志信息作为文件名
- `git send-email *.patch`通过邮件将补丁文件发出
- `git push`
- `git pull mirror master`
- `git push home`
- `git pull home master`
- `git pull`
- `git stash`保存工作进度

  - `git stash pop`恢复之前保存的进度
- `git checkout <new_branch>`切换分支

  - 注意切换分支前，先`git stash`，然后切换分支，切换回来后，执行`git stash pop`
- `git grep`工作区执行查找时，目录`.git`不会对搜索造成影响。
- `git rebase `变基操作

  - `git rebase -i <commit-id>^`修改`<commit-id>`所标识的提交

- `git svn`Git来操作SVN版本控制服务器
  - `git svn clone <svn_repos_url>`克隆svn版本库为一个本地的git库
  - `git svn fetch`获取SVN服务器上最新的提交
  - `git svn rebase`svn变基操作
  - `git svn dcommit`将本地提交推送到SVN服务器
  - 
- `less -FRSX`分页器,git命令自带一个分页器
  - `q`退出分页器
  - `h`显示分页器帮助
  - `空格`下翻一页
  - `b`上翻一页
  - `d`下翻半页
  - `u`上翻半页
  - `j`上翻一行
  - `k`下翻一行
  - `左箭头`和`右箭头`行左右滚动
  - `/pattern`向下寻找匹配的内容
  - `?pattern`向上寻找匹配的内容
  - `n`或`N`向前或向后寻找
  - `g`跳到第一行
    - `数字g`跳到对应的行
  - `G`跳到最后一行
  - `!<command>`执行shell命令
  - `export LESS=FRX`通过设置LESS的环境变量，可以设置自动换行
  - `git config --global core.pager 'less -+$LESS -FRX'`通过Git设置分页器自动换行


























































































































