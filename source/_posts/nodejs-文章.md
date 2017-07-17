---
title: nodejs 文章
date: 2017-07-17 14:21:45
tags: node
---
## 快速搭建 Node.js 开发环境[nvm](https://github.com/creationix/nvm)
### node,npm卸载
安装之前，先删除全局的node和npm
删除方法：
如果是通过brew install node 安装的：
首先通过brew uninstall node 卸载，然后删除系统剩余的node和node_modules

1. 删除/usr/local/lib中的所有node和node_modules,npm

2. 删除/usr/local/lib中的所有node和node_modules,npm的文件夹

3. 检查~/中所有的local, lib或者include文件夹, 删除里面所有node和node_modules,npm

4. 在/usr/local/bin中, 删除所有node,npm的可执行文件

5. 最后运行以下代码:
```
sudo rm /usr/local/bin/npm
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d
sudo rm -rf ~/.npm
sudo rm -rf ~/.node-gyp
sudo rm /opt/local/bin/node
sudo rm /opt/local/include/node
sudo rm -rf /opt/local/lib/node_modules
```

运行完毕后，重启终端，输入以下命令，查看是否卸载完毕
```node
npm
```


### 安装 nvm
#### 通过https://github.com/creationix/nvm 方式推荐

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
它会安装nvm,并自动会在.bashrc 文件中添加配置，傻瓜式安装

##### 升级nvm
直接到官网复制命令安装一遍来更新。
参考：http://www.jianshu.com/p/045df8e20ebe

#### 通过brew安装，这种方法不推荐,会出现[问题](https://www.v2ex.com/t/273237)
每次重新打开命令行工具都会出现这个问题，如果在.bashrc配置之后，并不能解决此问题

1.通过brew install nvm
2.在mac配置 .bashrc 文件中添加 
source $(brew --prefix nvm)/nvm.sh
3.在命令行中执行
source ~/.bashrc
4.重新打开你的终端, 输入 nvm

```
$ nvm

Node Version Manager

Usage:
    nvm help                    Show this message
    nvm --version               Print out the latest released version of nvm
    nvm install [-s] <version>  Download and install a <version>, [-s] from source
    nvm uninstall <version>     Uninstall a version
    nvm use <version>           Modify PATH to use <version>
    nvm run <version> [<args>]  Run <version> with <args> as arguments
    nvm current                 Display currently activated version
    nvm ls                      List installed versions
    nvm ls <version>            List versions matching a given description
    nvm ls-remote               List remote versions available for install
    nvm deactivate              Undo effects of NVM on current shell
    nvm alias [<pattern>]       Show all aliases beginning with <pattern>
    nvm alias <name> <version>  Set an alias named <name> pointing to <version>
    nvm unalias <name>          Deletes the alias named <name>
    nvm copy-packages <version> Install global NPM packages contained in <version> to current version

Example:
    nvm install v0.10.24        Install a specific version number
    nvm use 0.10                Use the latest available 0.10.x release
    nvm run 0.10.24 myApp.js    Run myApp.js using node v0.10.24
    nvm alias default 0.10.24   Set default node version on a shell

Note:
    to remove, delete or uninstall nvm - just remove ~/.nvm, ~/.npm and ~/.bower folders
```

### 使用 cnpm 加速 npm
同理 nvm , npm 默认是从国外的源获取和下载包信息, 不慢才奇怪. 可以通过简单的 ---registry 参数, 使用国内的镜像 http://registry.npm.taobao.org :
```
$ npm install koa --registry=http://registry.npm.taobao.org
```

其他帮助文档：https://fengmk2.com/blog/2014/03/node-env-and-faster-npm.html

### 安装切换各版本 node/npm
nvm install stable #安装最新稳定版 node，现在是 8.1.0
nvm install 4.2.2 #安装 4.2.2 版本
nvm install 0.12.7 #安装 0.12.7 版本

##### 特别说明：以下模块安装仅供演示说明，并非必须安装模块
nvm use 0 #切换至 0.12.7 版本
npm install -g mz-fis #安装 mz-fis 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v0.12.7/lib/mz-fis
nvm use 4 #切换至 4.2.2 版本
npm install -g react-native-cli #安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v4.2.2/lib/react-native-cli

nvm alias default 0.12.7 #设置默认 node 版本为 0.12.7


### 问题
如果按照上面的流程安装之后，每次新打开的命令行，都要执行nvm use [版本]
怎么解决：nvm alias default [版本]//设置默认选中的版本


### 资料
http://www.imooc.com/article/14617
http://www.cnblogs.com/kaiye/p/4937191.html
https://fengmk2.com/blog/2014/03/node-env-and-faster-npm.html

### nvm常用指令

```
nvm install <version> ## 安装指定版本，可模糊安装，如：安装v4.4.0，既可nvm install v4.4.0，又可nvm install 4.4
nvm uninstall <version> ## 删除已安装的指定版本，语法与install类似
nvm use <version> ## 切换使用指定的版本node
nvm ls ## 列出所有安装的版本
nvm ls-remote ## 列出所以远程服务器的版本（官方node version list）
nvm current ## 显示当前的版本
nvm alias <name> <version> ## 给不同的版本号添加别名
nvm unalias <name> ## 删除已定义的别名
nvm reinstall-packages <version> ## 在当前版本node环境下，重新全局安装指定版本号的npm包
```

### [nrm](https://github.com/Pana/nrm)切换源
开发的npm registry 管理工具 nrm,  能够查看和切换当前使用的registry, 最近NPM经常 down 掉, 这个还是很有用的哈哈

我在国内使用npm init的时候会一直卡在那里，一直动不了，使用nrm,可以很方便的解决这个问题。建议在国内使用cnpm源。

```
$ npm install -g nrm

$ nrm ls

* npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  eu ----- http://registry.npmjs.eu/
  au ----- http://registry.npmjs.org.au/
  sl ----- http://npm.strongloop.com/
  nj ----- https://registry.nodejitsu.com/
  
$ nrm use cnpm //switch registry to cnpm

  Registry has been set to: http://r.cnpmjs.org/
    
nrm help // show help
nrm list // show all registries
nrm use cnpm // switch to cnpm
nrm home // go to a registry home page
nrm test
```






