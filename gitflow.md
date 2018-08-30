# git-flow 工作流指南

> 这是一套基于`git-flow` 工作流 的团队协同开发规范，为了避免开发人员协同开发的混乱，请大家严格按照此规则执行；
> 此规则并非铁律，在开发过程中有任何疑问，可及时提出，我们将一起协商改进，特殊问题也可以特殊处理。

## git-flow 的 5 个分支

- master 中央仓库
- hotfix bug 修复分支
- release 预发布分支
- develop 功能集成分支
- feature 新功能分支

![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/gitflow_2.png)

> **长期分支：**`git-flow` 模式会预设两个主分支在仓库中：`master` 和 `develop`，它们会存活在项目的整个生命周期中，它们都包含对应的远程分支`origin/master` 和 `origin/develop`；
> **短期分支：**其他的分支，针对功能的分支 `feature`，针对发行的分支`release`，针对 bug 修复的分支`hotfix`，仅仅只是临时存在的。它们是根据需要来创建的，当它们完成了自己的任务之后就会被删除掉了；

![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/gitflow_init_1.png)

### master

> `master` 分支存储了正式发布的历史。
> 任何情况下 开发人员均不能再 `master` 分支上进行开发
> `master` 分支只有管理员才能执行`git push`操作，管理员：`毛陆军`

#### origin/master 分支

> `origin/master` 分支指向生产环境，管理员本地`master`分支操作`git push` 命令后，`origin/master` 分支将通过`Jenkins`自动构建到生产环境，过程如下：

```
master => oringin/master => Jenkins => 生产环境
```

### hotfix

> `hotfix` 分支是用来修复生产坏境出现的紧急 bug 的
> 推荐`hotfix`命名使用 bug 编号命名，如`hotfix/bug#001`
> 所以`hotfix`分支永远都是基于 `master`分支创建的
> bug 修复完成之后，要同时合并到`master`分支和`develop`分支，并打标签`git tag`添加一个版本号，最后删除 `hotfix`分支

### release

> `release`分支为新功能上线前和必要的修复的版本预发布
> `release`分支是基于`develop`分支创建的
> `release`分支命名使用版本号命名，如`release/1.0.0`
> `release`分支只有管理员才能操作，管理员：`毛陆军`
> 版本发布完成后，将合并到`master`分支，`git-flow`自动以发布版本号打标签，最后删除 `release`分支

### develop

> `develop` 分支是所有已完成功能的集成分支，并等待被整合到`master`分支中
> `develop` 分支每次更新，最终都会合并到`master` 分支，`master`分支每次更新必须立即合并到`develop`分支
> 多数情况下 开发人员均不能再 `develop` 分支上进行开发

#### origin/develop 分支

> `origin/develop` 分支指向测试环境，管理员本地`develop`分支操作`git push` 命令后，`origin/develop` 分支将通过`Jenkins`自动构建到测试环境，过程如下：

```
develop => oringin/develop=> Jenkins => 测试环境
```

### feature

> `feature`分支为创建新功能需求的分支
> `feature`分支是基于`develop`分支创建的
> `feature`分支使用以功能名称，如`feature/admin-login`
> `feature`分支功能完成后，合并到`develop`分支

注意：当新功能需求过大时，可在当前`feature`分支上拆分为多个子分支，如`feature/admin-login-front`、`feature/admin-login-end`, 子分支完成后合并到`feature`分支，待功能全部完成后，最终在合并到`develop`分支上

## git-flow 的常用命令

### 安装 git-flow

> 在 Windows 中 `git` 版本 ≥ `2.6.4`的，均集成有 `git flow`命令，如果你还未安装或版本过低 [请点击这里下载](https://cdn.op110.com.cn/soft/Git-2.18.0-64-bit.exe)

### git flow init 初始化

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

> 执行`git flow init` 命令时，它将在你的分支上配置了一些命名规则，我们直接使用默认的命名，一步一步地确定下去。

`git flow init` 命令会自动创建 `master`分支和`develop`分支
![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/gitflow_init_2.png)

### 创建 feature

我们开始开发一个新的功能`admin-login`, 先将当前分支切换至`develop`分支
`git-flow` 命令：

```
$ git flow feature start admin-login
```

集成的`git` 命令：

```
$ git checkout -b feature/admin-login develop   基于develop分支创建功能分支，并切换至新建的功能分支
```

![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/create_feature.png)

### 完成 feature

`git-flow` 命令：

```
$ git flow feature finish admin-login
```

集成的`git` 命令：

```
$ git checkout develop                  切换至develop分支
$ git pull origin develop               更新本地develop到最新
$ git merge -no-ff feature/admin-login  把指定的本地分支合并到本地develop
$ git branch -d feature/admin-login     删除本地的指定分支
```

![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/finish_feature.png)

`feature`分支从创建到完成的完整图：
![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/feature_1.png)

### 创建 releases

`git-flow` 命令：

```
$ git flow release start 1.0.0
```

集成的`git` 命令：

```
$ git checkout -b release/1.0.0 develop
```

![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/release_feature.png)

### 完成 release

`git-flow` 命令：

```
$ git flow release finish 1.0.0
```

集成的`git` 命令：

```
$ git checkout master                        切换至master分支
$ git pull origin master                     更新本地master到最新
$ git merge –no-ff release/1.0.0             合并release到本地master
$ git tag -a release/1.0.0                   添加版本号标签
$ git push origin master                     更新远程master
$ git checkout develop                       切换至develop分支
$ git pull origin develop                    更新本地develop到最新
$ git merge –no-ff release/1.0.0             合并release到本地develop
$ git push origin develop                    更新远程develop
$ git branch –d release/1.0.0                删除本地release
```

![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/finish_release.png)

### 创建 hotfix

档生产环境上出现 bug#001，我们需要修复`bug#001`, 首先将当前分支切换至`master`分支
`git-flow` 命令：

```
$ git flow hotfix start bug#001
```

集成的`git` 命令：

```
$ git checkout -b hotfix/bug#001 master
```

### 完成 hotfix

`git-flow` 命令：

```
$ git flow hotfix finish bug#001
```

集成的`git` 命令：

```
$ git checkout master                        切换至master分支
$ git pull origin master                     更新本地master到最新
$ git merge –no-ff hotfix/bug#001            合并hotfix到本地master
$ git push origin master                     更新远程master
$ git checkout develop                       切换至develop分支
$ git pull origin develop                    更新本地develop到最新
$ git merge –no-ff hotfix/bug#001            合并hotfix到本地develop
$ git push origin develop                    更新远程develop
$ git branch –d hotfix/bug#001               删除本地hotfix
```

![Alt text](https://cdn.op110.com.cn/lib/imgs/gitflow/hotfix.png)

## release 发布版本规则

> 版本号通常称为 `x.y.z`
>
> | 序号 | 格式要求 | 说明                                                          |
> | :--: | :------: | :------------------------------------------------------------ |
> |  x   | 非负整数 | 主版本号(major)，进行不向下兼容的修改时，递增主版本号         |
> |  y   | 非负整数 | 次版本号(minor)，保持向下兼容,新增特性时，递增次版本号        |
> |  z   | 非负整数 | 修订号(patch),保持向下兼容,修复问题但不影响特性时，递增修订号 |

`0.y.z` 表示开发阶段，一切可能随时改变，非稳定版。
`1.0.0` 界定此版本为初始稳定版，后面的一切更新都基于此版本进行修改。

注意：每个发布版本号都对应一份功能更新升级的记录文档

## git commit message 命名规则

> `git commit` 时，提交信息遵循以下格式
> :emoji1: :emoji2: 主题
> 提交信息主题

```
git commit -m":bug: 修复用户无法登录的问题"
```

| emoji                    | emoji 代码         | commit 说明      |
| :----------------------- | :----------------- | :--------------- |
| :hammer: (锤子)          | :hammer:           | 页面重构         |
| :wrench: (扳手)          | :wrench:           | 修改配置文件     |
| :art: (画板)             | :art:              | 调整页面样式     |
| :heavy_plus_sign:(加号)  | :heavy_plus_sign:  | 新增功能或依赖   |
| :heavy_minus_sign:(减号) | :heavy_minus_sign: | 移除功能或依赖   |
| :fire: (火焰)            | :fire:             | 移除代码或文件   |
| :hankey: (便便)          | :hankey:           | 优化规范代码结构 |
| :memo: (编写文档)        | :memo:             | 编写文档         |
| :zap: (闪电)             | :zap:              | 提升性能         |
| :truck: (卡车)           | :truck:            | 移动或重命名文件 |
| :ambulance: (救护车)     | :ambulance:        | 紧急修复 bug     |
