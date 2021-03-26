---
title: git学习笔记
date: 2021-03-25 17:06:17
tags:
---

## Git

> [维基百科 - Git](https://zh.wikipedia.org/wiki/Git)

### 学习资源介绍

- [Git教程 - 廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)
- [Pro Git](http://git.oschina.net/progit/)
- [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
- [猴子都能懂的GIT入门](http://backlogtool.com/git-guide/cn/)

### Git 使用交互流程

{% asset_img g1.png git交互模型 %}

<!-- more -->

### 安装和配置 Git 环境

- 下载地址：https://git-scm.com/

#### git-bash 常用命令

在git-bash中可以执行 `linx` 命令语句，而在普通的 `cmd` 命令窗口中则不能。常见命令如下：

- `pwd`    打印当前工作目录路径信息
- `ls`   打印当前目录下所有文件
  + `ls -a`  打印当前目录下所有文件，包括隐藏的文件(例如.git文件夹)
- `cd  目录名`   切换到某个目录
- `mkdir 目录名`  在当前工作目录下创建一个新的目录（文件夹）
- `touch 文件名`    创建文件
- `cat 文件名`     查看文件
- `less  文件名`   翻页查看大文本文件
  - 有时候使用`git log `打印日志如果记录过多就会进入 less 查看模式，在该模式下按q键可以退出，上下键可以翻页。
- `vi 文件名`  visual interface
  + Esc 退出到命令模式
  + i 进入插入模式
  + :q 退出vi
  + :w 保存编辑
  + :wq 保存并退出
  + :q! 强制退出不保存修改
  + vi 的所有操作基本全部是命令，这里掌握基本使用基于可以了
  + 有时候使用 git commit 进行提交的时候希望能多写几行提交日志，这时候可以省略 -m 参数进入 vi 编辑模式
- `clear`   清除git-bash命令窗口
- `rmdir`   只能删除空目录
- `rm`
  + `rm 文件名`
  + `rm -rf 目录名`
    * 注：很强大，可以删除非空目录，以及一些比较顽固的文件或者目录

#### 初始化配置

```bash
# 设置用户名
git config --global user.name "你的名字"
# 配置用户邮箱
git config --global user.email "你的常用邮箱"
# 设置 gitk 图形查看工具中文显示默认编码（防止乱码）
git config --global gui.encoding utf-8
# 查看配置列表项
git config --list
```

### 基本使用

- `git init`
  + 初始化一个 Git 仓库
  + `git init 文件夹名`   自动创建一个文件夹并初始化仓库
- `git status`
  + 查看当前工作区、暂存区、本地仓库的状态
- `git add`
  - 将工作区文件添加到暂存区
  - 可以通过 `git add .` 将所有文件都添加
- `git commit`
  + 示例：`git commit -m "日志说明" --author="操作者姓名 <邮箱>"`
  + 执行 `git commit` 的时候，Git 会要求具有用户名和邮箱的参数选项
  + 可以通过 `git config` 命令配置一下用户名和邮箱
- `git log`
  - 查看提交日志
- `gitk`
  - 在特定窗口中呈现日志详细信息

总结：操作 Git 的基本工作流程就是先修改文件，然后执行 `git add` 命令。
`git add` 命令会把文件加入到暂存区，接着就可以执行 `git commit` 命令，将文件存入文档库，
从而形成一次历史记录。

- 问题1：关于 Git-bash 中文问题
- [Git for Windows Unicode Support](https://github.com/msysgit/msysgit/wiki/Git-for-Windows-Unicode-Support)
- 问题2：执行 commit 的时候一大堆的信息
- 问题3：配置 user.name 和 user.email 问题

### 工作区、暂存区、本地仓库

### 添加/删除文件

```bash
# 添加指定文件到暂存区
git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
git add [dir]

# 添加当前目录的所有文件到暂存区
git add .

# 删除工作区文件，并且将这次删除放入暂存区
git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
git mv [file-original] [file-renamed]
```

### 代码提交

```bash
# 提交暂存区到仓库区
git commit -m [message]

# 提交暂存区的指定文件到仓库区
git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
git commit -a

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
git commit --amend [file1] [file2] ...
```

### 版本回退

```bash
# git rm --cached <file>
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

### 分支管理

+ 默认在 git 的仓库中，会有个分支的原点：master

+ 如果在某个分支修改了某个文件但没有`commit`，此时若切换到其他分支，则其他分支的工作区文件也会显示被修改了。只有进行`commit` 操作后所有分支的工作区才会回到之前的版本。
+ 当合并发生冲突时，只需将冲突的文件进行手动修改，然后再次`commit`  就能解决。

```bash
# 列出所有本地分支
git branch

# 基于当前分支新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 基于当前分支新建一个分支，并切换到该分支
git checkout -b [branch]

# 切换到指定分支，并更新工作区
git checkout [branch-name]

# 切换到上一个分支，交替和上一个分支进行切换
git checkout -

# 合并指定分支到当前分支
git merge [branch]

# 删除分支
git branch -d [branch-name]
```

### 远程操作

```bash
# 下载一个远程仓库
$ git clone [url]

# 显示所有远程仓库
git remote -v

# 显示某个远程仓库的信息
git remote show [remote]

# 增加一个新的远程仓库，并命名
git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
git pull [remote] [branch]

# 上传本地指定分支到远程仓库
git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force
```

### Git 工作流程：分支策略

+ [Git 工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

#### Git Flow

+ [Git分支管理策略 git flow](http://www.ruanyifeng.com/blog/2012/07/git.html)

{% asset_img Git-Flow工作流.png Git-Flow工作流 %}

#### Github Collabrators 

这种方式公司团队项目使用居多

#### Github Flow

这种方式开源项目使用居多

- fork
- clone 到你的本地
- 在clone下来的项目中拉出一个新的分支
  + 修改的时候最好是基于 master 拉出一个修改的分支，例如这个分支是用来添加某个功能的
- 在新分支上开发或者修改完成之后，提交到本地仓库，然后 push 推到自己的账户中 fork 过来的仓库
- 最后，在 Github 上你 fork 过来的仓库界面中找到 New Pull Request 发起提交请求
- 对方就会在仓库的 Pull Requests 中收到你发起的提交请求
  + 然后双方就可以使用社会化交流方式进行沟通协作
  + 例如 Code Review 代码审查
- 最后对方审查通过没有问题之后，选择 Merge Request
- 到此，一个完整的 Github 工作流结束
- 这种方式开源项目更多一些（大家都不认识）

#### Gitlab Flow

[gitlab flow 工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

### 在线仓库托管服务

> 一个不知道 github、stackoverflow 的程序员想想都是可悲的

- github
- 码云
- coding
- gitlab   是一个开源的类似于 Github 的一个系统，开源免费部署到自己的公司内容。

---

## Github

> Github 就是程序员的新浪微博
> 它可以让你使用社交化的方式进行编程协作、
>     - 点赞
>     - 评论
>     - 转发
>     - etc.
> 主要作用：可以免费在线托管你的仓库
> 可以实现多人协作
> 提供了一个可视化界面（Web Page）让你能直观清晰的了解你的项目源代码

### 基本使用

- 注册
- 登陆
- 创建远程仓库
- 通过 `git clone  远程仓库地址` 命令下载远程仓库到本地
  + git clone 会自动帮你把远程仓库下载到本地，不需要再去 git init 了
  + 通过 clone 下来的仓库，git 有一个远程仓库地址列表，git 默认会把你 clone 的地址起一个别名：origin
  + 然后你执行 push 的时候实际上就是将本地的版本提交到 origin 上
- 在本地进行操作，通过 `git commit` 形成历史记录
- 通过 `git push` 将本地仓库中的历史记录提交到远程仓库

### 本地已有仓库，需要提交到线上

如果是 `git init` 出来的仓库，进行 `push` 提交的时候就不知道要往哪里 push。

所以，这里通过 `remote` 相关命令进行设置：

```bash
# 查看所有的远程仓库信息
git remote show
# 根据别名查看指定的远程仓库信息
git remote show 远程仓库地址别名
# 添加远程仓库信息
git remote add 别名 远程仓库地址
```

通过上面的 `git remote add` 添加完远程仓库地址信息之后，还不能直接 `git push`，必须在每一次
`push` 的时候加上 `git push 仓库地址别名 master` 就可以提交了。

如果想要省略 `git push` 后面需要指定的 `仓库地址别名 master` 可以通过下面的命令修改：

```
git push --set-upstream heima（别名） master
```

这样就可以直接使用 `git push` 进行提交而不需要指定 `heima master` 了

## Github Pages

Github Pages 是Github提供了一个免费在线托管静态资源的服务

使用方法如下：

1. 在个人的 Github 账户中创建一个仓库
2. 仓库名称必须为 `个人账户名称.github.io`
3. 往该仓库根目录中提交一个 `index.html` 文件
4. 然后就可以在地址栏输入 `个人账户名称.github.io` 地址，就可以看到 `index.html` 网页内容了

注意：上面创建的仓库名称必须是 `个人账户名称.github.io` ，否则无法访问

## Hexo

Hexo 是基于 Node.js 开发的一个静态博客生成器，提供本地实时预览及部署功能。

### Hexo基本使用

```bash
# 安装全局 hexo 包文件
npm install -g hexo-cli

# 初始化一个blog（博客）
hexo init blog

#切到blog文件夹
cd blog

# 启动本地预览服务，默认是 127.0.0.1:4000
hexo server

# 新建文章
hexo new 文章标题
```

也可以参考 Hexo 官方文档：https://hexo.io/zh-cn/ , 里面有具体的使用方式。

### 自动发布 Hexo 搭建的静态博客

第一：先修改 `_config.yml` 配置文件，下面是一个示例：

```yml
deploy:
  type: git
  repo: https://heima04:heima123456@github.com/heima04/heima04.github.io.git
```

​	上面的配置选项中，一定要注意在 repo 中按照对应的格式加入 Github 用户名和密码。或者直接填仓库地址，每次重新部署时手动输入账号密码。

+ 若上面的 repo 不想暴露git的用户名和密码可以采用ssh验证的方式，步骤如下：

  1 、在创建的 blog 文件夹中右键点击`Git Bash Here` ,输入`cd ~/.ssh`  ,然后再输入`ls`,如下图所示说明已经存在 id_rsa.pub 或 id_dsa.pub 文件，则可以跳过步骤2

  ```bash
  PC@WJPC MINGW64 ~/Desktop/blog (master)
  $ cd ~/.ssh
  
  PC@WJPC MINGW64 ~/.ssh
  $ ls
  id_rsa  id_rsa.pub  known_hosts
  ```

  2、 创建一个 SSH key，输入`ssh-keygen -t rsa -C "你的邮箱"`,然后连续回车

  ```bash
  PC@WJPC MINGW64 ~/.ssh
  $ ssh-keygen -t rsa -C "244227697@qq.com"
  Generating public/private rsa key pair.
  Enter file in which to save the key (/c/Users/PC/.ssh/id_rsa):
  /c/Users/PC/.ssh/id_rsa already exists.
  Overwrite (y/n)?
  ```

  3、 输入`eval "$(ssh-agent -s)"`，添加密钥到ssh-agent,然后输入`ssh-add ~/.ssh/id_rsa`

  ```bash
  PC@WJPC MINGW64 ~/.ssh
  $ eval "$(ssh-agent -s)"
  Agent pid 1881
  
  PC@WJPC MINGW64 ~/.ssh
  $ ssh-add ~/.ssh/id_rsa
  Identity added: /c/Users/PC/.ssh/id_rsa (/c/Users/PC/.ssh/id_rsa)
  ```

  4、 打开github个人中心的`Settings-->SSH and GPG keys`,创建SSH key，title 随便取名，key 的内容为～/.ssh/id_rsa.pub文件的内容，复制进去即可。

  {% asset_img c1.png create ssh %}

  {% asset_img c2.png finished %}

  5、通过 `ssh -T git@github.com` 命令验证是否添加成功，成功后返回信息如下

  ```bash
  PC@WJPC MINGW64 ~/.ssh
  $ ssh -T git@github.com
  Hi mcwj007! You've successfully authenticated, but GitHub does not provide shell access.
  ```

  6、修改 `_config.yml` 配置文件，其中repo为git仓库的 ssh 地址

  ```yml
  deploy:
    type: git
    repo: git@github.com:mcwj007/mcwj007.github.io.git
    branch: master
  ```

第二：安装自动发布的插件：

```bash
npm install hexo-deployer-git --save
```

第三：使用命令一键进行发布：

```bash
hexo generate --deploy
# 或者
hexo deploy --generate
```

上面两条命令都可以，发布可能有延时，稍微等待即可。

### 主题修改

step1：在github或者hexo官网上搜索想替换的主题，例如：https://github.com/litten/hexo-theme-yilia

step2：使用`git clone 主题git地址`  将文件克隆到博客文件夹的themes文件夹中

+ 使用 clone 时，可以在后面加上 depth = 1 表示只下载最新的版本，不下载历史版本，`git clone 主题git地址 --depth=1`

step3: 修改 `_config.yml`配置文件

```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: 主题名（下载的主题文件夹名称）
```

### 文章图片插入

step1：安装插件 hexo-asset-image。

+ `npm install https://github.com/CodeFalling/hexo-asset-image --save`

step2: 打开/node_modules/hexo-asset-image/index.js，将内容更换为下面的代码。

```javascript
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
    	var link = data.permalink;
	if(version.length > 0 && Number(version[0]) == 3)
	   var beginPos = getPosition(link, '/', 1) + 1;
	else
	   var beginPos = getPosition(link, '/', 3) + 1;
	// In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
	var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
		if ($(this).attr('src')){
			// For windows style path, we replace '\' to '/'.
			var src = $(this).attr('src').replace('\\', '/');
			if(!/http[s]*.*|\/\/.*/.test(src) &&
			   !/^\s*\//.test(src)) {
			  // For "about" page, the first part of "src" can't be removed.
			  // In addition, to support multi-level local directory.
			  var linkArray = link.split('/').filter(function(elem){
				return elem != '';
			  });
			  var srcArray = src.split('/').filter(function(elem){
				return elem != '' && elem != '.';
			  });
			  if(srcArray.length > 1)
				srcArray.shift();
			  src = srcArray.join('/');
			  $(this).attr('src', config.root + link + src);
			  console.info&&console.info("update link as:-->"+config.root + link + src);
			}
		}else{
			console.info&&console.info("no src attr, skipped...");
			console.info&&console.info($(this));
		}
      });
      data[key] = $.html();
    }
  }
});

```

step3：打开_config.yml文件，修改下述内容。

```yml
post_asset_folder: true
```

step4：打开_config.yml修改下述内容。

```yml
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://mcwj007.github.io
```

step5：将引用图片放入`_posts`文件夹中，与文章同名的文件夹内，然后用以下格式插入,下面就是将图片example.jpg 插入到了文章中。

```md
{% asset_img example.jpg This is an example image %}
```

