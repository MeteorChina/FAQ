# Meteor部署

不少刚接触 Meteor 的同学，在完成自己第一个 Meteor 应用后，直接运行 `meteor run` 或  `meteor` 来运行部署 Meteor 应用，代码更新了应用会自动重新加载，顿时觉得 Meteor 真是 *简单、方便、快捷*。但细心的同学马上就会发现问题，一个简单的 Meteor 应用居然占用大量内存，CPU占用也很高，如果服务器配置不行，可能就经常重启，引起一些莫名其妙的问题，不知道如何处理， **醒悟** 时才发现，原来又掉进 **坑** 了啊。。。

该怎么 **爬** 出去呢？

**1. Meteor官方提供的免费空间**

Meteor命令行工具提供了 `meteor deploy`，可以把我们的Meteor应用直接部署到Meteor提供的免费空间上，通过Meteor的二级域名来访问，例如：运行 `meteor deploy xxx` 部署完后，就可以通过 xxx.meteor.com 来访问。另外，

- 支持自定义域名
> If you deploy to a custom domain, such as myapp.mydomain.com, then you'll need to make sure the DNS for that domain is configured to point at origin.meteor.com.

- 操作服务器上的mongodb

    直接连接：`meteor mongo xxx.cmeteor.com` 
    
    返回一个临时的连接地址(可用于导入导出数据)：`meteor mongo xxx.cmeteor.com --url` 

更多具体的介绍可以通过 `'meteor help <command>'`查看。
这里有一个 [meteor-cluster][1]  ([相应的代码][2]) 大家可以去试下。

既然是免费的，稳定性什么的就没保障了，用作 DEMO 展示还是挺不错的。

**2. 自定义部署**

想必你也想像其他应用一样自己控制每一个步骤，Meteor 归根还是基于 Node.js 的，所以，你完全可以像部署一般的 Node.js 应用一样部署。具体步骤如下：
 
```
  $ meteor #1 先保证应用是可以执行运行的
  $ meteor build --directory ../rel # 构建 Node.js 项目
  $ cd  ../rel/bundle

  # 该目录下 README 有详细的介绍
  $ (cd programs/server && npm install)
  $ export MONGO_URL='mongodb://user:password@host:port/databasename'
  $ export ROOT_URL='http://example.com'
  $ export MAIL_URL='smtp://user:password@mailhost:port/'
  $ export PORT=3000 # 默认 80
  $ node main.js
```
这样就可以直接用 `node` 来运行了，你也可以用其他的 `node` 进程管理工具来跑，例如：[pm2][3] 、[forever][4] 等。
    
值得注意点是 `#1` 这里，在 build 之前单独执行一下，原因之前也说过，这里再重申一下。


> 在部署的时候，如果有packages更新，最好，先执行meteor运行程序安装npm包，然后在build。另外要注意系统的node版本跟Meteor自带的node版本是否一致。因为执行meteor安装npm包的时候是用Meteor自带的node版本来安装编译的，不一致的话可能会导致有些需要编译的包不能正常运行。
> 全文见[这里][6]


这个部署的过程虽然看起来很麻烦，但很有效，最好先了解了之后再去尝试一些别人提供的工具(例如：[meteor-up][5])。


  [1]: http://meteor-cluster.meteor.com/
  [2]: https://github.com/cmeteor-org/meteor-cluster
  [3]: https://github.com/Unitech/PM2
  [4]: https://github.com/foreverjs/forever
  [5]: https://github.com/arunoda/meteor-up
  [6]: http://cmeteor.org/t/meteor-npm/31
