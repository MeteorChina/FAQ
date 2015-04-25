#如何在Meteor中使用npm模块？

首先，请在[AtmosphereJS](https://atmospherejs.com)上搜索有无相关的封装包。尽量采用已有的封装包，而不是自己封装。

有两种方法在项目中使用来自npm的模块。

1. 封装为Meteor包并在项目中添加包。使用`meteor create 包名 --package`来创建包，并通过将包目录放置于项目的`packages`文件夹等方法向项目引入包。包中使用`Npm.depends`和`Npm.require`来引入npm模块。[Meteor文档-包中引入Npm模块](http://docs.meteor.com/#/full/Npm-depends)
2. 使用`meteorhacks:npm`。[meteorhacks:npm @ AtmosphereJS](https://atmospherejs.com/meteorhacks/npm)
