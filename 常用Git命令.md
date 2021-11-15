# 常用Git命令

## 基础信息配置

### 用户信息

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```console
$ git config --global user.name 'John Doe'
$ git config --global user.email 'johndoe@example.com'
```

再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置

检查配置信息

如果想要检查你的配置，可以使用 `git config --list` 命令来列出所有 Git 当时能找到的配置。

```console
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：`/etc/gitconfig` 与 `~/.gitconfig`）。 这种情况下，Git 会使用它找到的每一个变量的最后一个配置。

你可以通过输入 `git config <key>`： 来检查 Git 的某一项配置

```console
$ git config user.name
John Doe
```

![image-20210922213250409](%E5%B8%B8%E7%94%A8Git%E5%91%BD%E4%BB%A4.assets/image-20210922213250409.png)

设置优先级:local > global > system





## Git仓库构建

通常有两种获取 Git 项目仓库的方式：

1. 将尚未进行版本控制的本地目录转换为 Git 仓库；
2. 从其它服务器 **克隆** 一个已存在的 Git 仓库。

两种方式都会在你的本地机器上得到一个工作就绪的 Git 仓库。



## 常用指令

```bash
$ git help <命令>

$ git init

$ git clone ssh  
git@github.com:Novmbrain/Reseau-de-Petri.git

$ git status

##查看过去提交记录，注意后面的参数可以组合使用
$ git log
$ git log --oneline ## 简介常看log
$ git log -n4 ## 只查看最近4次
$ git log --all ##打印所有分支
$ git log --all --graph ##打印所有分支并且图形化
$ git log <分支名>

## 对工作区中已经update过的文件，可以使用-u命令添加
$ git add -u
## git add和commit一起执行
$ git commit -am"" (不推荐使用)

## 新的分支创建
$ git branch <branchname>
$ git checkout <branchname>
$ git checkout -b <branchname> //这个指令可以等于上面两个指令

## 新的分支合并
首先回到合并到的主分支，然后
$ git merge <branch>

## 给文件重命名的简便方法
假设git管理的目录下有个readme文件被重命名成了readme.md
git add readme.md
git rm readme

可以用一条命令解决:
git rm readme readme.md


##删除新的分支
$ git branch -d <分支名>

用-d 报“error：The branch is not fully merged”，是指这个分支不曾合入到其他任何分支。在日常开发中，我们通常赋予有意义的分支名，Git判断本分支没和任何别的分支合并，意味这删除存在风险。它也提供我们-D的方式，如果确定无风险就用-D 。

## 使用图形界面
$ gitk
```



### 多人协作开发

可以通过邀请协作者到作为合作开发者进行开发

![image-20210921155914424](%E5%B8%B8%E7%94%A8Git%E5%91%BD%E4%BB%A4.assets/image-20210921155914424.png)

### 撤销之前的提交记录

## 使用心得记录



### git未commit就切换分支

![image-20210923100952727](%E5%B8%B8%E7%94%A8Git%E5%91%BD%E4%BB%A4.assets/image-20210923100952727.png)

git有工作区、缓存区的概念，工作区即为自身本地的相应文件夹，通过`git add`命令后，即可将文件放置缓存区；继而通过`git commit`即可将文件放置于相对应的git分支

问题出现的内容就是因为，我没有add ，而工，你作区和缓存区内容是公共的，不从属于任何一个分支，所以切换到A分支时，仍然将修改的东西带过去了；
当既想要在切换分支，又不想add时，可以使用`git stash`,当使用了`git stash`后，在其他分支仍然可以通过`git stash pop`找出来

### commit tree blob 的关系

![image-20210923112140065](%E5%B8%B8%E7%94%A8Git%E5%91%BD%E4%BB%A4.assets/image-20210923112140065.png)

执行一次commit操作时，一个commit包含一个tree。tree中的内容就是执行commit时项目中包含的所有文件夹和文件。即tree是一个大的文件夹，它包含了那个时刻整个项目的文件夹和文件。再者也可以说这个tree包含了整个项目的tree和blob，即文件夹和文件

### 分离头指针

通常，我们工作在某一个分支上，比如 master 分支。这个时候 master 指针和 HEAD 指针是一起前进的，每做一次提交，这两个指针就会一起向前挪一步。但是在某种情况下（例如 checkout 了某个具体的 commit），master 指针 和 HEAD 指针这种「绑定」的状态就被打破了，变成了分离头指针状态。

1.  如果我们当前位于分离头指针，不要在没有建立新分支的情况下进行commit操作，因为在分离头指针的commit不跟任何分支挂钩，当我们切换到具体的master分支或者develop分支的时候，这些提交都看不到了。git会自动清理这些没有和branch挂钩的commit
2. 通过`git branch branchName commitId`给这个提交创建一个临时的分支，这个分支是基于头指针分离下修改提交的commit id创建的。合并分支:`git merge temp`。最后删除临时分支：git branch -d temp

### 查看commit_id所属的对象类型

 commit其实是某次提交的文件快照，git是基于提交的，我们可以用`git cat-file -t commit_id`(Git为标识每次提交产生的40位经过SHA1加密过之后的HASH值)， 来查看commit_id所属的对象类型，使用`git cat-file -p commit_id`来查看每个对象的内容和简单的数据结构。

git主要有四种对象类型，Blog,Tree,Commit,Tag,他们都是用SHA1计算出来的HASH值进行命名。

```
$ git cat file 

-p
Pretty-print the contents of <object> based on its type.
-t
Instead of the content, show the object type identified by <object>.
```

### HEAD

### 如何修改的commit的message

```bash
## 修改最新的commit的message
git commit --amend

## 修改老旧的commit的message
在团队合作不建议使用，只能在没有分享分支前使用
$ git rebase -i  <commit号> ## 通过参数i进入交互模式，commit号选择需要进行message修改的前一个commit
在交互窗口里将要修改的commit 前的pick改成reword，保存退出
进入修改窗口，进行进行修改，然后保存退出
```

### 把连续多个commit整理成一个

```bash
$ git rebase -i <commit号>
选择commit合并的起始commit的父亲commit号，进入该交互式

```

![image-20210924110511176](%E5%B8%B8%E7%94%A8Git%E5%91%BD%E4%BB%A4.assets/image-20210924110511176.png)

补充合成commit的原因，作为合成后commit的commentarie

![image-20210924110709193](%E5%B8%B8%E7%94%A8Git%E5%91%BD%E4%BB%A4.assets/image-20210924110709193.png)

### 文件比较与恢复

``` bash
## 比较暂存区和HEAD所含文件的差异
$ git diff --cached
## 工作区和暂存区比较
$ git diff
$ git diff -- 文件名

## 将暂存区全部恢复成HEAD
git reset HEAD

## 将工作区部分文件恢复成和暂存区一样
git checkout -- <文件名>
-- 表示后面添加文件

总结：工作区checkout，暂存区reset


## 取消暂存区部分文件的更改恢复成 commit区一样
git reset HEAD -- <文件名>

## 消除最近几次的提交
git reset --hard <commit号>
这条命令会将commit号之后的所有暂存区和工作区的内容到恢复到commit号，并删除之后的所有commit

## 比较两个commit之间的差异
git diff <commit号> <commit号>
git diff <分支名> <分支名>
还可以在后面加上 -- 文件名来指定具体的文件
```

Various ways to check your working tree

```
$ git diff            (1)
$ git diff --cached   (2)
$ git diff HEAD       (3)
```

1. Changes in the working tree not yet staged for the next commit.
2. Changes between the index and your last commit; what you would be committing if you run "git commit" without "-a" option.
3. Changes in the working tree since your last commit; what you would be committing if you run "git commit -a"

### 正确的文件删除方法

```
两步走：
1.首先在工作区删除文件 
rm readme
2. 然后再暂存区删除文件
git rm readme

直接合并成一条指令
gi


```

### 开发中临时加塞了紧急任务如何处理

当开发中加塞了紧急的临时任务，同时当前手头的任务还没有到可以commit的地步。

首先将手头的任务add到暂存区，然后隐藏到栈中

```
##查看当前stash 的list
git stash list
git stash
##然后处理手头的紧急任务，并提交commit

## 将stash中的保存弹出栈
git stash pop （弹出stash中的记录，并且恢复现场）
git stash apply （恢复现场，并且不会弹出stash中的记录）
```

### 指定不需要Git管理的文件

可以通过定义.gitignore文件来指定不需要Git管理的文件

## 远端仓库与合作

```
## 查看当前本地仓库所对应的远端仓库
git remote -v
## 添加远端仓库
git remote add <远端仓库别名> <远端仓库地址>

git push -u <远端仓库别名> master
如果远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：
$ git push origin master
```



# VIM



- `i` → *Insert* mode. Type `ESC` to return to Normal mode.
- `x` → Delete the char under the cursor
- `:wq` → Save and Quit (`:w` save, `:q` quit)
- `dd` → Delete (and copy) the current line
- `p` → Paste

Recommended:

- `hjkl` (highly recommended but not mandatory) → basic cursor move (←↓↑→). Hint: `j` looks like a down arrow.
- `:help <command>` → Show help about `<command>`. You can use `:help` without a `<command>` to get general help.

# 命令行常用命令

```bash
## 展示当前目录下文件
$ ls
$ ls -al 

$ pwd : Print Working Directory 

```

# 如何给开源项目做贡献



## 提起 pr 的过程描述

以 macaca-chromedriver 项目举例：

1. 先确认此项目还有人维护，不然浪费自己精力
2. 检查已经存在的 issues 和 pull requests，确保不要做别人已经在做的事情
3. 找到项目 macaca-chromedriver 点击 Fork，在你的 Github 主页也会存在此项目的拷贝，一定要先 Fork！我忽略了这一步，然后在坑里蹲了一会...
4. 将 macaca-chromedriver Fork 的项目本地 Clone; 我的是`git clone https://github.com/Super-Ps/macaca-chromedriver.git`
5. 进入刚才 Clone 的 Macaca-chromedriver 项目，建立本地仓库跟原始项目的的链接:`git remote add upstream https://github.com/macacajs/macaca-chromedriver.git`这样可以随时从原始项目 pull 最新的内容
6. 查看远程仓库列表此时应该有 2 个仓库，一个你 Frok 的项目，一个原始的项目,我的是这样的：

```
$ git remote -v`
`origin https://github.com/Super-Ps/macaca-chromedriver.git (fetch)`
`origin https://github.com/Super-Ps/macaca-chromedriver.git (push)`
`upstream https://github.com/macacajs/macaca-chromedriver.git (fetch)`
`upstream https://github.com/macacajs/macaca-chromedriver.git (push)
```

1. 想好要修改文件之前，先创建一个分支，且切换在该分支上 git checkout -b BRANCH_NAME
2. 现在可以修改项目了，要清楚现在是在你自己 Fork 的项目上操作的，跟原始的项目没暂时没关系，所以自己 push 到 Fork 项目之前，要从原始项目拉 1 次最新代码，比如我这里的远程 upstream 仓库 pull 最新的修改。
3. 修改并提交完成之后，推送 git push origin BRANCH_NAME，这个分支名 是你 Fork 项目时创建的分支，现在是往 Forx 上 push，不是原始的仓库。
4. 现在可以 pull request 了，打开你的 github Fork 的 macaca-chromedriver 项目 ，点击 new pull request ，可以查看原始项目和你 Fork 项目的不同分支，查看对比你修改的部分，如果你确定你的提交是没有问题的，点击确定，等待作者的反馈
5. 收尾工作，如果贡献代码被采用合并了，或者被拒绝了，就可以删除本地分支了: git branch -D BRANCH_NAME ，删除 GitHub 上的分支: git push origin --delete BRANCH_NAME ,如果需要同步源项目代码到 Fork 项目 ，先把代码从源项目更新到最新 `git pull upstream master` 合并`git merge upstream/master` 再 push 到 Fork 项目上 `git push origin master`





```
将远程分支和本地分支同步
git fetch origin –prune

```

```
package metarettaf;

import java.io.IOException;

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import org.json.JSONObject;

public class Avwx {
	public static JSONObject getJson() throws IOException {
		CloseableHttpClient httpclient = HttpClients.createDefault();
		HttpGet httpGet = new HttpGet("https://avwx.rest/api/taf/LFRB");
		CloseableHttpResponse response1 = httpclient.execute(httpGet);
		
		HttpEntity entity = response1.getEntity();
		String result = EntityUtils.toString(entity);
		EntityUtils.consume(entity);
		
		JSONObject jsonObject = new JSONObject(result);
		
		System.out.println(jsonObject.toString());
		return jsonObject;
	}

}
```

