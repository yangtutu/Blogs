---
title: Git基本设置及指令别名
date: 2020-06-06
---

## 基本设置

- 设置用户名

```bash
git config --global user.name  "{your username}"
```

- 设置邮箱

```bash
git config --global user.email "{your email}"
```

<!--more-->

## 指令别名

在.gitconfig文件中，添加以下内容：

```bash
[alias]
st = status
ci = commit
co = checkout
br = branch
df = diff
dft = difftool
dfs = diff --staged
dfts = difftool --staged
mr = merge
mrt = mergetool
last = log -1 HEAD
ls = log --oneline --graph --all --decorate
lg = log --oneline --graph --all --decorate --pretty=format:"%h%x20%Cgreen%d%x20%Cred%an%x20%C(yellow)%ad%x20%Creset%s" --full-history --date=short
rb = rebase -i
cp = cherry-pick
```


## Beyond Compare

使用Beyond Compare作为差异对比工具，在.gitconfig文件中，添加以下内容：

```bash
[diff]
tool = bc4
[difftool]
prompt = true
[difftool "bc4"]
cmd = \"D:/Program Files/Beyond Compare 4/bcomp.exe\" "$(cygpath -w $LOCAL)" "$REMOTE" # Beyond Compare安装路径
[merge]
tool = bc4
[mergetool]
prompt = true
[mergetool "bc4"]
cmd = \"D:/Program Files/Beyond Compare 4/bcomp.exe\" "$LOCAL" "$REMOTE" "$BASE" "$MERGED" # Beyond Compare安装路径
```
