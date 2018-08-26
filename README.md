# gitflow 工作流指南

## git-flow 中分支介绍

- master 中央仓库
- hotfix issue 或 bug 修复分支
- release 预发布分支
- develop 功能集成分支
- feature 单个功能分支

### master 中央仓库

> master 分支存储了正式发布的历史，

### hotfix issue 或 bug 修复分支

> hotfix 分支是基于 master 分支，功能对外发布后，在对线上功能进行全面测试时，可能会发现一些 bug 和 issue，这种情况下需要添加一个 hotfix 分支。

### release 预发布分支

> release 预发布分支是基于 develop 分支，作为功能开发 develop 分支和对外发布 master 分支间的缓冲

### develop

> develop 分支是所有已完成功能的集成分支，并等待被整合到 master 分支中

### feature

> feature 具体的功能仓库，功能添加完成之后合并到 develop 分支，并删除当前 feature 分支

## git-flow 初始化

```
$ git flow init
Initialized empty Git repository in /Users/tobi/acme-website/.git/
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
```

## git-flow 添加 feature 分支

> git-flow 会创建一个名为 “feature/user-login” 的分支（这个 “feature/” 前缀 是一个可配置的选项设置）

```
$ git flow feature start user-login
```

## git-flow 完成功能

> feature 分支将合并到 develop 分支
> git-flow 也会进行清理操作。它会删除这个当下已经完成的功能分支，并且换到 “develop” 分支

```
$ git flow finish user-login
Switched to branch 'develop'
Updating 6bcf266..41748ad
Fast-forward
    user-login.html | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 feed.xml
Deleted branch feature/user-login (was 41748ad).
```

## git-flow 管理 release

### 创建 release

> git-flow 推荐 release 分支使用版本号命名
> release 分支是基于 develop 分支创建
> 完成了 release 后，git-flow 会适当地自动去标记那些 release 提交

```
$ git flow release start 1.0.0
Switched to a new branch 'release/1.0.0'
```

### 完成 release

> 1. git-flow 会拉取远程仓库，以确保目前是最新的版本。
> 2. release 的内容会被合并到 “master” 和 “develop” 两个分支中去，这样不仅产品代码为最新的版本，而且新的功能分支也将基于最新代码。
> 3. 为便于识别和做历史参考，release 提交会被标记上这个 release 的名字
> 4. 删除 release 分支，并切回到 develop 分支

```
git flow release finish 1.0.0
```

## hotfix 修复 bug 和 issue

### 创建 hotfix

> 推荐 hotfix 分支名使用 issue 编号进行命名
> hotfix 分支基于 master 分支创建
> 和 release 一样，修复这个错误也会直接影响到项目的版本号

```
$ git flow hotfix start #1039
```

### 完成 hotfix

> 完成的改动会被合并到 “master” 中，同样也会合并到 “develop” 分支中，这样就可以确保这个错误不会再次出现在下一个 release 中
> 这个 hotfix 程序将被标记起来以便于参考
> 这个 hotfix 分支将被删除，然后切换到 “develop” 分支上去

```
$ git flow hotfix finish #1039
```
