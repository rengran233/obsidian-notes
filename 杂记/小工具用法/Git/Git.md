`git branch`
- 查看本地分支
`git branch -r`
`git branch -a`
- 查看所有分支（包括远程）
`git branch -f <>`
`git status`
- 查看当前在哪个分支。
- 有没有修改了但还没 add 的文件。
- 有没有 add 了但还没 commit 的文件。
`git switch <分支名> / git checkout <分支名>`
- 切换到一个已经存在的分支
`git switch -c <分支名> / git checkout -b <分支名>`
- 创建一个新分支并切换过去
`git checkout <哈希值>`
- 查看哈希值对应的版本（不能修改）
`git commit --amend`
- 打开一个编辑器，让你修改 commit message。如果你之前 add 了新文件，它也会被包含进这次修正后的提交里。**注意：这会改变上一次提交的 hash。**
`git reset --soft`
- 撤销但不丢弃版本
`git reset --hard`
- 撤销且丢弃版本（危险！）
`git revert`
- 创建一个与想要撤销的版本
`git log`
`git log --graph --oneline --decorate --all`

`git branch -m <新名字>`

`git config --global core.autocrlf input`

`git push -u origin <branch name>` 
- 将本地分支推送到远程
- `-u`代表以后默认推送到该分支（即可直接使用`git push`）

`git restore .`: 将工作区里未`add`进暂存区的本地修改撤销

`git rm -r --cached .`: 删除本地缓存，用于更新`.gitignore`文件

`git rebase -i --root`: 