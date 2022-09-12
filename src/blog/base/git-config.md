# Git 配置

为了将代码放到 [GitHub](https://github.com/) 中进行托管。

## 初始化 Git 仓库{#init-git}

首先需要使用 Git 对代码仓库进行初始化：

```bash
$ git init
```

其次增加 `.gitignore`：

```
# 忽略项目及系统的文件
.DS_Store
node_modules
/dist*
/lib*
/theme*
/storybook-static*
/_site*
.history
/coverage*
dis
cache
.cache
auto-imports.d.ts
dest

# 本地开发所用到的文件
.env.local
.env.*.local

# 包管理工具自动生成的日志文件
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-error.log*
.pnpm-debug.log*

# 编辑器自动生成的配置文件
.idea
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

# 锁定版本文件
package-lock.json
yarn.lock
```

## 配置用户名和邮箱{#config-name-email}

配置用户名和邮箱：

```bash
$ git config --local user.name "沫俱宏"
$ git config --local user.email "negivup@sina.com"
```

如果没有 ssh key，最好生成一个（一路回车即可）：

```bash
$ ssh-keygen -t rsa -C "negivup@sina.com"
```

把生成的 ssh key 加入到 GitHub 中，方便后期提交代码。

## 创建远程仓库{#config-remote}

在 [GitHub](https://github.com/) 中创建一个名称为当前登录名的仓库。

创建成功后将本地仓库与远程仓库建立联系：

```bash
$ git remote add origin git@github.com:negivup/negivup.git
```

提交代码并上传到远程仓库：

```bash
$ git add .
$ git commit -m "first commit"
$ git branch -M main
$ git push -u origin main
```

## GitHub Pages{#github-pages}

创建发布脚本 `deploy.sh`：

```sh
#!/usr/bin/env sh

# 忽略错误
set -e

# 构建
npm run docs:build

# 进入待发布的目录
cd dist

# 如果是发布到自定义域名
# echo 'www.example.com' > CNAME

git init
git add -A
git commit -m 'deploy'

# 如果部署到 https://<USERNAME>.github.io
git push -f git@github.com:negivup/negivup.github.io.git master:gh-pages

# 如果是部署到 https://<USERNAME>.github.io/<REPO>
# git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages

cd -

rm -f -r dist
```

创建一个名称为 `<USERNAME>.github.io.git` 的远程仓库，然后执行脚本：

```bash
$ sh deploy.sh
```

如果想查看效果，可以通过 [https://negivup.github.io/](https://negivup.github.io/) 查看。
