## 将已忽略的文件重新添加到监听中。

`git add -f [filepath]`

## 分支来回切换

`git checkout -`

## 合并指定分支

`git cherry-pick [commit]`

想充分享受这个命令带来的好处的话，就不要再用 `git add .` 一股脑的把所有文件变更放在一个 commit 里面了。

例如用 git 作为博客系统，不同分支分别代表草稿箱，校验箱，已发布，通过这个命令可以充分隔离不同的分支。

## 清除 Untracked files 的文件

`git clean -f`

一些项目新添加的文件，未 add 之前如果想删掉怎么半？`git checkout` 是撤销修改，不适合删除文件，使用 `git rm` 又提示未找到文件，这时 `git clean -f` 就派上用场了，可以把所有提示 Untracked files 的文件清除掉。

## 设置某个 commit 为 HEAD

使用 `git log` 可以列出所有 commit 历史,最顶端的 commit 称为 HEAD。使用 `git reset [commit]` 可以将某个 commit 设为 HEAD。不过只是 index 区域会变，在命令添加 `--hard` 则 index 和 workspace 都会变 `git reset --hard [commit]`。使用 --hard 要千万注意，commit 后面的 commit 会丢失。
