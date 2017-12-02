# Git 工作指南

## 前言

作为一个非常有历史的 _版本控制系统_ ，Git 理所当然地存在很多历史遗留问题。然而我们不想解决历史遗留问题——它们实在是太多了。
所以，你应该学会叫 Git 这个 git（饭桶）闭嘴；至少，不要叫那些历史遗留的 s@#t 来污染你的大脑。

如果你不能理解本文的内容，照着做就是了；如果你连照着做也做不到，就不要用 Git。

## 修改全局设置

在你能好好地使用 Git 管理你的代码之前，把这几个设置改一下：  
```sh
$ git config --global color.status=auto       # 使 git status -s 命令的输出带有颜色。
$ git config --global color.diff=auto         # 使 git diff -s 命令的输出带有颜色。
$ git config --global color.branch=auto       # 使 git branch -a 命令的输出带有颜色。
$ git config --global color.interactive=true  # 使 git add -i 命令的输出带有颜色。
$ git config --global core.editor=/bin/nano   # 请不要理会那个竟然敢自称编辑器的叫 vi 的傻逼。
$ git config --global core.autocrlf=input     # 别把 CR LF 提交到服务器上。
$ git config --global push.default=simple     # 仅 push 当前分支。
$ git config --global pull.ff=only            # 禁用非 --ff-only 的 pull 操作。
$ git config --global merge.ff=only           # 禁用非 --ff-only 的 merge 操作。
```

## 开发者手册

1. 《Git 工作流程》  
<http://www.ruanyifeng.com/blog/2015/12/git-workflow.html>  
仅参考其中的 Gitlab flow。
master 分支（debug/unstable）仅作开发，产品分支（release/stable）仅以 cherry-pick 更新。

2. 常用 Git 命令清单  
<http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html>

3. Git 使用规范流程  
<http://www.ruanyifeng.com/blog/2015/08/git-use-process.html>

## 禁止项

1. 禁止直接在 master 分支上开发。
2. 禁止直接向 master 和 release/stable 分支 push。
3. 禁止 merge， `--ff-only` 除外。
4. 禁止 pull， `--ff-only` 和 `--rebase` 除外。
5. 文本文件禁止使用宽字符编码，非手动编辑的文件除外。

## 一般开发流程

1. _获取_ 最新代码：  
```sh
$ git checkout master
$ git fetch
$ git reset --hard origin/master
```
2. _创建_ 用于开发的 _本地分支_ （此处假定用于开发的分支名为 working）：  
```sh
$ git checkout -b working
```
3. 在 working 分支上 _进行修改_ 。  
4. _提交修改_ 到 working 分支：  
```sh
$ git add 第一个文件
$ git add 第二个文件
$ git commit
```
5. 将本地分支 working _推送到_ 服务器上，创建一个新的 _远端分支_ origin/working：  
```
$ git push origin working
```
6. 在 GitLab 上查看推送的分支， _创建_ 到 master 分支的 _Merge Request_ 。  
7. 查看新建的 Merge Request，如果没有冲突，点击 _合并_ 。  
8. 再次 _获取_ 最新的代码：  
```sh
$ git checkout master
$ git pull --ff-only
```
9. _删除本地的开发分支_ ：  
```sh
$ git branch -d working
```
10. _删除服务器上的开发分支_ ：  
```sh
$ git push origin :working
```
