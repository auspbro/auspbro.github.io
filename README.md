# auspbro.github.io
## Hexo 多终端部署及更新到 GitHub 方法

### 一、关于搭建的流程
1. 创建仓库，https://auspbro.github.io/
2. 创建两个分支：master(默认分支) 与 hexo
3. 在 GitHub 页面上 Setting-branch 中设置 hexo-blog 为默认分支（因为我们只需要手动管理这个分支上的 Hexo 网站源文件）
4. 使用git clone https://github.com/auspbro/auspbro.github.io.git 拷贝仓库
5. 拷贝完成后先备份 .git 隐藏文件夹到其他地方，后面还需要。否则执行 hexo init 会提示文件夹不是空的
6. 在本地`auspbro.github.io` 文件夹下通过 Git bash 依次执行 
```
$ npm install -g hexo-cli
$ hexo init
$ npm install
$ npm install hexo-deployer-git（此时当前分支应显示为 hexo-blog）
```
7. 修改 `_config.yml` 中的 deploy 参数，分支应为 master
8. 依次如下指令 

```
$ git add 
$ git commit -m "..."
$ git push origin hexo-blog
```
提交网站相关的文件
9. 执行 hexo g -d 生成网站并部署到 GitHub 上。这样一来，在 GitHub 上的 https://auspbro.github.io 仓库就有两个分支，一个 hexo-blog 分支用来存放网站的原始文件，一个 master 分支用来存放生成的静态网页

### 二、关于日常的改动流程在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理
1. 依次执行 git add .、git commit -m "..."、git push origin hexo指令将改动推送到GitHub（此时当前分支应为 hexo-blog）
2. 然后才执行 hexo g -d 发布网站到 master 分支上。虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催....的情况，调转顺序就有问题了）

### 三、本地资料丢失后的流程当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤
1. 使用 git clone https://github.com/auspbro/auspbro.github.io.git 拷贝仓库（默认分支为 hexo-blog）；
2. 在本地新拷贝的 auspbro.github.io 文件夹下通过 Git bash 依次执行下列指令(Mac 用户记得加 sudo)：
```
$ npm install -g hexo-cli
$ npm install
$ npm install hexo-deployer-git（记得，不需要 hexo init 这条指令）
```
