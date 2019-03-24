## git reset --hard

### 前言

+ 有这样一天，我的项目是社交项目，我写了4大模块的基础代码，并且我把他们都 add 交给Git管理了，**注意是只add了最后一次**但是我并没有提交，因为我想把4大模块内容基本完成在进行commit，但是此刻的我并没有很熟悉Git的命令，`$ git reset --hard`在不了解的情况下是很危险的。然后然后文件就全部丢失了。



### 命令解读

******

+ `$ git reset --hard`不加参数的情况下，是将当前暂存区的保存内容清除，这句话并不权威，需要栗子来举证。
+ 我的工作目录下本来有 **readme.md**文件是已经提交过的，下面我新增一个**readme2.md**文件，并且将**readme.md**修改一下下，然后add给Git，那么这时候Git status将会是绿色的一个new file，一个modify file，这个时候我reset --hard,就会将**readme2.md**删除掉，并且**readme.md**修改的部分也撤销掉！
+ 另外一种情况，我现在git status是干净的（也就是都提交了），我新增一个 wave.txt文件，add给Git，那么这时候git status是new file，然后我并未提交，将wave.txt 修改一行，再add给Git，这时候，status便是 new file wave.txt和modify wave.txt两个，这个时候reset，便会删掉wave.txt文件。



### 手欠操作了怎么办？

***下面内容来自Stack Overflow***

```java
If you didn't already commit your local changes (or at least stage them via `git add`, they're gone. `git reset --hard` is a destructive operation for uncommitted changes.

If you did happen to stage them, but didn't commit them, try `git fsck --lost-found` and then search through the contents of .git/lost-found - it will contain all of the objects that aren't referenced by a known commit, and may include versions of files that were staged.

You can recover anything you git added, with git fsck --lost-found and poke around in .git/lost-found.  find .git/objects -type f | xargs ls -lt | sed 60q will give you the last 60 things to get added to the repo, that'll help.

Anything you didn't git add is gone as surely as if you'd deleted it yourself.

```

翻译一下：

如果你没有commit你的本地修改（甚至于你都没有通过git add追踪过这些文件，当他们被删除，git reset --hard对于这些没有被commit过也没有git add过的修改来说就是具有毁灭性的，destructive！！）

but，如果你幸运的是曾经通过git add命令追踪过这些文件，只是没有commit它们而已！那么试试`git fsck --lost-found`这个命令吧！然后你就可以在本地项目文件中路径为.git/lost-found/other（楼主亲自试验就是这个路径）中找到它们！！并且呢，这里面包含了所有的没有被commit（指定到某次commit）的文件，甚至可能还包括你每次git add的版本（version一词实在不知道在这里怎么翻译，姑且就认为是版本吧）！

使用git fsck --lost-found这个命令，通过.git/lost-found/other这个路径，你可以恢复任何你git add过的文件！再通过`find .git/objects -type f | xargs ls -lt | sed 60q`这个命令，你就可以找到最近被你add到本地仓库的60个文件，综上所述，希望对你有所帮助！

- 具体操作步骤，**注意是建立在你已经add的情况下**
  - 执行`git fsck --lost-found`命令，会发现在`.git\lost-found\other`里面多了文件，没错就是你删掉的文件
  - 可以将这些个文件在`notepad++`打开，会发现里面都是些什么内容，然后就需要你改名了~~
  - 这个命令真是个好东西。

***所以呀，一定要保持一个良好的add commit习惯，又不多花钱***