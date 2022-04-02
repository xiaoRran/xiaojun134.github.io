---
title: GitHub Actions自动部署Hexo博客
tags: 部署
categories: 技术
cover: 'https://tva3.sinaimg.cn/large/9bd9b167gy1fwrt93q40fj21hc0u0h1c.jpg'
abbrlink: b272844b
date: 2022-01-24 15:00:30
---



# GitHub Actions自动部署Hexo博客

> 转自[使用Github Action实现全自动部署](https://akilar.top/posts/f752c86d/)，仅供个人学习记录。

## 教程常量声明

| 常量名             | 常量释义                                                     |
| ------------------ | ------------------------------------------------------------ |
| **[Blogroot]**     | 本地存放博客源码的文件夹路径                                 |
| **[SourceRepo]**   | 存放博客源码的私有仓库名                                     |
| **[SiteBlogRepo]** | 存放编译好的博客页面的公有仓库名 Site指站点，教程中会替换成 Github、Gitee、Coding |
| **[SiteUsername]** | 用户名 Site指站点，教程中会替换成 Github、Gitee、Coding      |
| **[SiteToken]**    | 申请到的令牌码 Site指站点，教程中会替换成 Github、Gitee、Coding |
| **[GithubEmail]**  | 与github绑定的主邮箱，建议使用Gmail                          |
| **[TokenUser]**    | Coding配置特有的令牌用户名                                   |

```
PLAINTEXT

# 在记事本中逐个记录，方便替换
[Blogroot]：

[SourceRepo]：

[SiteBlogRepo]
  [GithubBlogRepo]：
  [GiteeBlogRepo]：
  [CodingBlogRepo]：

[SiteUsername]
  [GithubUsername]：
  [GiteeUsername]：
  [CodingUsername]：

[SiteToken]
  [GithubToken]：
  [GiteeToken]：
  [CodingToken]：

[GithubEmail]：

[TokenUser]：
```

## Github Action使用教程

### 获取Token

访问`Github`->`头像`（右上角）->`Settings`->`Developer Settings`->`Personal access tokens-`>`generate new token`,创建的Token名称随意，但必须勾选repo项.随后记录下了`Token`。

### 配置deploy项

打开站点配置文件`[Blogroot]/_config.yml`,找到`deploy`配置项，使用之前生成的`[SiteToken]`和各个站点仓库`URL`来组装地址。

```
PLAINTEXT

deploy:
- type: git
  repo:
    gitHub: https://[GithubUsername]:[GithubToken]@github.com/[GithubUsername]/[GithubBlogRepo].git[,branch]
    gitee: https://[GiteeUsername]:[GiteeToken]@gitee.com/[GiteeUsername]/[GiteeBlogRepo].git[,branch]
    coding: https://[TokenUser]:[CodingToken]@e.coding.net/[CodingUsername]/[CodingBlogRepo].git[,branch]
  # [,branch]为可选项，表示部署的分支
  #2020年10月后github新建仓库默认分支改为main
```

### 配置Github Action

在`[Blogroot]`新建`.github`文件夹,注意开头是有个`.`的。然后在`.github`内新建`workflows`文件夹，再在`workflows`文件夹内新建`autodeploy.yml`,在`[Blogroot]/.github/workflows/autodeploy.yml`里面输入

```
PLAINTEXT

# 当有改动推送到master分支时，启动Action
name: 自动部署

on:
  push:
    branches:
      - main #2020年10月后github新建仓库默认分支改为main，注意更改

  release:
    types:
      - published

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: 检查分支
      uses: actions/checkout@v2
      with:
        ref: main #2020年10月后github新建仓库默认分支改为main，注意更改

    - name: 安装 Node
      uses: actions/setup-node@v1
      with:
        node-version: "12.x"

    - name: 安装 Hexo
      run: |
        export TZ='Asia/Shanghai'
        npm install hexo-cli -g

    - name: 缓存 Hexo
      uses: actions/cache@v1
      id: cache
      with:
        path: node_modules
        key: ${{runner.OS}}-${{hashFiles('**/package-lock.json')}}

    - name: 安装依赖
      if: steps.cache.outputs.cache-hit != 'true'
      run: |
        npm install --save

    - name: 生成静态文件
      run: |
        hexo clean
        hexo generate

    - name: 部署
      run: |
        git config --global user.name "[GithubUsername]"
        git config --global user.email "[GithubEmail]"
        git clone https://github.com/[GithubUsername]/[GithubBlogRepo].git .deploy_git
        # 此处务必用HTTPS链接。SSH链接可能有权限报错的隐患
        # =====注意.deploy_git前面有个空格=====
        # 这行指令的目的是clone博客静态文件仓库，防止Hexo推送时覆盖整个静态文件仓库，而是只推送有更改的文件
        hexo deploy
```

### 重新设置远程仓库和分支

1. 删除或者先把`[Blogroot]/themes/butterfly/.git`移动到非博客文件夹目录下,原因是主题文件夹下的`.git`文件夹的存在会导致其被识别成子项目，从而无法被上传到源码仓库。

2. 在博客根目录`[Blogroot]`路径下运行指令

   ```
   PLAINTEXT
   git init #初始化
   git remote add origin git@github.com:[GithubUsername]/[SourceRepo].git #[SourceRepo]为存放源码的github私有仓库
   git checkout -b master # 切换到master分支，
   #2020年10月后github新建仓库默认分支改为main，注意更改
   # 如果不是，后面的所有设置的分支记得保持一致
   ```

3. 添加屏蔽项

   因为能够使用指令进行安装的内容不包括在需要提交的源码内，所有我们需要将这些内容添加到屏蔽项，表示不上传到github上。这样可以显著减少需要提交的文件量和加快提交速度。

   打开`[Blogroot]/.gitignore`,输入以下内容：

   ```
   PLAINTEXT
   
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/
   .deploy_git*/
   .idea
   themes/butterfly/.git
   ```

   如果不是`butterfly`主题，记得替换最后一行内容为你自己当前使用的主题。

4. 之后再运行git提交指令，将博客源码提交到github上。

   ```
   PLAINTEXT
   git add .
   git commit -m "github action update"
   git push origin main
   #2020年10月后github新建仓库默认分支改为main，注意更改
   ```

------

最后注意：一定要装[云端部署](https://sianx.com/posts/d36e9cd6/#部署云端)插件和[配置git](https://sianx.com/posts/d36e9cd6/#配置-Git-与-GitHub)