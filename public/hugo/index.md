# VPS配置之六hugo

>hugo是快速的静态博客系统，可部署于github。我选择的是LoveIt主题,当然even也是很好的主题，但更适合文字博客，偏技术类博客loveit更现代，更适合。而且loveit功能很全，评论，搜索简单设置开箱即用，比较方便，而且设置文档也比较丰富。

## 安装hugo
archlinux下安装hugo非常简单，只需一条命令，一个文件。
```
sudo pacman -S hugo
```
## 配置hugo
* 建立博客
使用hugo建立名为blog的本地博客。
```
hugo new site blog
```
* 选择主题并编辑config

进入blog目录，初始化git，下载主题，可以使用github的submodule功能添加你喜欢的主题。下面以LoveIt主题为例，一般主题设置可以copy主题目录内 examplesite内内容到blog下使用。比较重要的是config.coml内设置。
```
cd blog
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```
* 新建文章并编译
不同主题要求的位置不同，需要注意，这点可以参照主题说明。
```
hugo new posts/<文章名>.md
hugo
```
* 本地测试
```
hugo serve --buildDrafts
```
## 使用gihub管理blog和文章

我们需要建立两个repo，分别用于文章管理和博客发布。其中博客发布我使用的是建立<用户名>.github.io，相对比较简单。
```
git submodule add <用于发布的repo地址> public
```

这里注意建立时public目录不能为空。
* 在blog根目录下建立depoly.sh。
```
#!/bin/sh
   
   # If a command fails then the deploy stops
   set -e
   
   # Print out commands before executing them
   set -x
   
   printf "\033[0;32mDeploying updates to GitHub...\033[    0m\n"
  
  # Build the project.
  hugo
  
  # Go To Public folder
  cd public
  
  # Add changes to git.
  git add .
  
  # Commit changes.
  msg="rebuilding site $(date)"
  if [ -n "$*" ]; then
          msg="$*"
  fi
  git commit -m "$
# Push source and build repos.
  git push origin master
   
# Back to the origin folder
# cd ..
 
# rm -rf public
``` 
* blog目录下submodudle初始化
```
git submodule init                                
```

至此设置基本完成，如需上传文章需在blog下执行：
```
git add .
git commit ""
git push -u origin master
```
如需更新博客，需提前为的deploy.sh增加可运行属性：
```
chmod +x deploy.sh
./deploy.sh
```
## LoveIt设置
编辑config.coml,

* 设置baseURL中文显示
```
baseURL = "https://gihub博客repo/"
defaultContentLanguage = "zh-cn"
```
* 设置header.title
```
logo = "/images/kiss-wink-heart-regular.svg
```
 此svg可从awesome font网站下载放入images目录。

## 其他机器使用
```
git clone <文章管理repo> blog
cd blog
git submodule init
git submodule update
```
{{< admonition >}}
以后使用前先`git pull`同步一下。
{{< /admonition >}}

## 配置自己域名
* 设置域名CNAME
在域名管理页面设置域名CNAME指向你的博客eopo。
* 设置博客repo
在博客仓库设置页面custom domain处填入你的域名，可勾选下面enforce HTTPS。

