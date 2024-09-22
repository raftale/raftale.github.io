
# 如何同步上游仓库的最新提交到fork后的仓库

```bash
# 即使是fork的项目，也要添加upstream，否则git remote -v也显示没有
git remote add upstream xxx.git
git remote -v
git fetch upstream
git stash
# 合并到本地分支, 例如dev
git merge upstream/dev
# 不喜欢merge，还可以使用rebase
# 如何出现冲突了呢？这里目前还没搞清楚，现在解决方案就是删除fork项目再重新fork
git rebase upstream/dev

git push origin dev
# 如果出现冲突，那就需要新的rebase or merge 再push
```
