**Github 中Tag的使用**
=======================
github的使用过程中tag也是不可或缺的一个命令，

Tag
-------

	git tag
	>>>>>>>>>>
::
1 // 查看tag，列出所有tag，列出的tag是按字母排序的，和创建时间没关系。
2 $ git tag
3 v0.1
4 v1.3
    
	git tag -l
	>>>>>>>>>>>>>

1 //查看指定版本的tag，git tag -l “v1.4.2.**”
2 $ git tag -l 'v1.4.2.*'
3 v1.4.2.1
4 v1.4.2.2
5 v1.4.2.3
6 v1.4.2.4

    git show
	>>>>>>>>>>>>>>
::
1 //显示制定tag的信息
2 $ git show v1.4
3 tag v1.4
4 Tagger: Scott Chacon <schacon@gee-mail.com>
5 Date:   Mon Feb 9 14:45:11 2009 -0800
6
7 my version 1.4
8
9 commit 15027957951b64cf874c3557a0f3547bd83b3ff6
10 Merge: 4a447f7... a6b4c97...
11 Author: Scott Chacon <schacon@gee-mail.com>
12 Date:   Sun Feb 8 19:02:46 2009 -0800
13
14    Merge branch 'experiment'

    git tag -a tagName -m “注释”
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1 -m 后面带的就是注释信息，一般写当前的版本作用，这种是普通tag,-a 取 annotated 的首字母也可以给commit版本添加如下:git tag -a tagName   ef0264   -m "注释"

    git push origin tagName
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1 我们在执行 git push 的时候，tag是不会上传到服务器的，比如现在的github，创建 tag 后 git push ，在github网页上是看不到tag 的，为了共享这些tag，你必须这样：git push origin v1.0或者git push origin --tags

    git push origin –tags
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1 将所有tag 一次全部push到github上。

    git tag -d tagName
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1 删除tag

    git push origin –detele tagName
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1 删除github远端的指定tag

    git checkout -b branchName tagName
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1    使用git checkout tag即可切换到指定tag，例如：git checkout v0.1.0
2 切换到tag历史记录会处在分离头指针状态，这个时候修改是很危险的，在切换回主线时如果没有合并，之前的修改提交基本都会丢失，如果需要修改可以尝试git checkout -b branch tag创建一个基于指定tag的分支，例如：git checkout -b tset v0.1.0  这个时候就会在分支上进行开发，之后可以切换到主线合并

这篇文章参考的是：`Quick reStructuredText`__。

.. __: https://blog.csdn.net/Kenway090704/article/details/77854624 


**Git tag 标签完全用法**
=============================


打标签
-------------

同大多数 VCS 一样，Git 也可以对某一时间点上的版本打上标签。人们在发布某个软件版本（比如 v1.0 等等）的时候，经常这么做。
本节我们一起来学习如何列出所有可用的标签，如何新建标签，以及各种不同类型标签之间的差别。
列显已有的标签

列出现有标签的命令非常简单，直接运行 git tag 即可：
::
1 $ git tag
2 v0.1
3 v1.3

显示的标签按字母顺序排列，所以标签的先后并不表示重要程度的轻重。

我们可以用特定的搜索模式列出符合条件的标签。在 Git 自身项目仓库中，有着超过 240 个标签，如果你只对 1.4.2 系列的版本感兴趣，可以运行下面的命令：
::
1 $ git tag -l 'v1.4.2.*'
2 v1.4.2.1
3 v1.4.2.2
4 v1.4.2.3
5 v1.4.2.4

新建标签
-------------

Git 使用的标签有两种类型：轻量级的（lightweight）和含附注的（annotated）。
lightweight ：轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。
annotated：含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用 GNU Privacy Guard (GPG) 来签署或验证。

一般我们都建议使用含附注型的标签，以便保留相关信息；当然，如果只是临时性加注标签，或者不需要旁注额外信息，用轻量级标签也没问题。

含附注的标签
----------------

创建一个含附注类型的标签非常简单，用 -a （译注：取 annotated 的首字母）指定标签名字即可：
::
1 $ git tag -a v1.4 -m 'my version 1.4'
2 $ git tag
3 v0.1
4 v1.3
5 v1.4

而 -m 选项则指定了对应的标签说明，Git 会将此说明一同保存在标签对象中。如果没有给出该选项，Git 会启动文本编辑软件供你输入标签说明。（git 提交注释都是-m ‘XXxx’）
可以使用 git show 命令查看相应标签的版本信息，并连同显示打标签时的提交对象。
::
1 $ git show v1.4
2 tag v1.4
3 Tagger: Scott Chacon <schacon@gee-mail.com>
4 Date:   Mon Feb 9 14:45:11 2009 -0800
5 
6 my version 1.4
7
8 commit 15027957951b64cf874c3557a0f3547bd83b3ff6
9 Merge: 4a447f7... a6b4c97...
10 Author: Scott Chacon <schacon@gee-mail.com>
11 Date:   Sun Feb 8 19:02:46 2009 -0800
12 
13    Merge branch 'experiment'

我们可以看到在提交对象信息上面，列出了此标签的提交者和提交时间，以及相应的标签说明。

签署标签
-------------

如果你有自己的私钥，还可以用 GPG 来签署标签，只需要把之前的 -a 改为 -s （译注： 取 signed 的首字母）即可：
::
1 $ git tag -s v1.5 -m 'my signed 1.5 tag'
2 You need a passphrase to unlock the secret key for
3 user: "Scott Chacon <schacon@gee-mail.com>"
4 1024-bit DSA key, ID F721C45A, created 2009-02-09

现在再运行 git show 会看到对应的 GPG 签名也附在其内：
::
1 $ git show v1.5
2 tag v1.5
3 Tagger: Scott Chacon <schacon@gee-mail.com>
4 Date:   Mon Feb 9 15:22:20 2009 -0800
5
6 my signed 1.5 tag
7 -----BEGIN PGP SIGNATURE-----
8 Version: GnuPG v1.4.8 (Darwin)
9
10 iEYEABECAAYFAkmQurIACgkQON3DxfchxFr5cACeIMN+ZxLKggJQf0QYiQBwgySN
11 Ki0An2JeAVUCAiJ7Ox6ZEtK+NvZAj82/
12 =WryJ
13 -----END PGP SIGNATURE-----
14 commit 15027957951b64cf874c3557a0f3547bd83b3ff6
15 Merge: 4a447f7... a6b4c97...
16 Author: Scott Chacon <schacon@gee-mail.com>
17 Date:   Sun Feb 8 19:02:46 2009 -0800
18
19    Merge branch 'experiment'

轻量级标签
-------------

轻量级标签实际上就是一个保存着对应提交对象的校验和信息的文件。要创建这样的标签，一个 -a，-s 或 -m 选项都不用，直接给出标签名字即可：
::
1 $ git tag v1.4-lw
2 $ git tag
3 v0.1
4 v1.3
5 v1.4
6 v1.4-lw
7 v1.5

现在运行 git show 查看此标签信息，就只有相应的提交对象摘要：

1 $ git show v1.4-lw
2 commit 15027957951b64cf874c3557a0f3547bd83b3ff6
3 Merge: 4a447f7... a6b4c97...
4 Author: Scott Chacon <schacon@gee-mail.com>
5 Date:   Sun Feb 8 19:02:46 2009 -0800
6 
7    Merge branch 'experiment'

验证标签
-------------

可以使用 git tag -v [tag-name] （译注：取 verify 的首字母）的方式验证已经签署的标签。此命令会调用 GPG 来验证签名，所以你需要有签署者的公钥，存放在 keyring 中，才能验证：
::
1 $ git tag -v v1.4.2.1
2 object 883653babd8ee7ea23e6a5c392bb739348b1eb61
3 type commit
4 tag v1.4.2.1
5 tagger Junio C Hamano <junkio@cox.net> 1158138501 -0700
6 
7 GIT 1.4.2.1
8 
9 Minor fixes since 1.4.2, including git-mv and git-http with alternates.
10 gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
11 gpg: Good signature from "Junio C Hamano <junkio@cox.net>"
12 gpg:                 aka "[jpeg image of size 1513]"
13 Primary key fingerprint: 3565 2A26 2040 E066 C9A7  4A7D C0C6 D9A4 F311 9B9A

若是没有签署者的公钥，会报告类似下面这样的错误：
::
1 gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
2 gpg: Can't check signature: public key not found
3 error: could not verify the tag 'v1.4.2.1'

后期加注标签
-------------

你甚至可以在后期对早先的某次提交加注标签。比如在下面展示的提交历史中：
::
1 $ git log --pretty=oneline
2 15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
3 a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
4 0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
5 6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
6 0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
7 4682c3261057305bdd616e23b64b0857d832627b added a todo file
8 166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9 9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
10 964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
11 8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme

我们忘了在提交 “updated rakefile” 后为此项目打上版本号 v1.2，没关系，现在也能做。只要在打标签的时候跟上对应提交对象的校验和（或前几位字符）即可：

$ git tag -a v1.2 9fceb02
可以看到我们已经补上了标签：
::
1 $ git tag
2 v0.1
3 v1.2
4 v1.3
5 v1.4
6 v1.4-lw
7 v1.5
8
9 $ git show v1.2
10 tag v1.2
11 Tagger: Scott Chacon <schacon@gee-mail.com>
12 Date:   Mon Feb 9 15:32:16 2009 -0800
13
14 version 1.2
15 commit 9fceb02d0ae598e95dc970b74767f19372d61af8
16 Author: Magnus Chacon <mchacon@gee-mail.com>
17 Date:   Sun Apr 27 20:43:35 2008 -0700
18
19    updated rakefile
20 ...


分享标签
-------------

默认情况下，git push 并不会把标签传送到远端服务器上，只有通过显式命令才能分享标签到远端仓库。其命令格式如同推送分支，运行 git push origin [tagname] 即可：
::
1 $ git push origin v1.5
2 Counting objects: 50, done.
3 Compressing objects: 100% (38/38), done.
4 Writing objects: 100% (44/44), 4.56 KiB, done.
5 Total 44 (delta 18), reused 8 (delta 1)
6 To git@github.com:schacon/simplegit.git
7 * [new tag]         v1.5 -> v1.5

如果要一次推送所有本地新增的标签上去，可以使用 –tags 选项：
::
1 $ git push origin --tags
2 Counting objects: 50, done.
3 Compressing objects: 100% (38/38), done.
4 Writing objects: 100% (44/44), 4.56 KiB, done.
5 Total 44 (delta 18), reused 8 (delta 1)
6 To git@github.com:schacon/simplegit.git
7  * [new tag]         v0.1 -> v0.1
8  * [new tag]         v1.2 -> v1.2
9  * [new tag]         v1.4 -> v1.4
10 * [new tag]         v1.4-lw -> v1.4-lw
11 * [new tag]         v1.5 -> v1.5

现在，其他人克隆共享仓库或拉取数据同步后，也会看到这些标签。

拉取tag分支
-------------

先 git clone 整个仓库，然后 git checkout tag_name 就可以取得 tag 对应的代码了。
但是这时候 git 可能会提示你当前处于一个“detached HEAD” 状态，因为 tag 相当于是一个快照，是不能更改它的代码的，如果要在 tag 代码的基础上做修改，你需要一个分支：
git checkout -b branch_name tag_name
这样会从 tag 创建一个分支，然后就和普通的 git 操作一样了。

其实要取得不同的branch的tag，只需要在相应的分支上打tag就行了。这样的tag就唯一对应了不同的分支。例如，你在master上打了tag为v1，在某个branch上打了tag为v2，则你取出v2代码的时候，自然就是对应的branch分支了。

删除tag分支
-------------

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
::
1 $ git tag -d v0.9
2 Deleted tag 'v0.9' (was 6224937)

然后，从远程删除。删除命令也是push，但是格式如下：
::
1 $ git push origin :refs/tags/v0.9
2 To git@github.com:michaelliao/learngit.git
3  - [deleted]         v0.9

要看看是否真的从远程库删除了标签，可以登陆GitHub查看。

这篇文章参考的是：`Quick reStructuredText`__。

.. __:https://blog.csdn.net/philos3/article/details/72812120


























