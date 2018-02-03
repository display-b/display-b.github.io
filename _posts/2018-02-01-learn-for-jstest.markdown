---
layout: default
title:  "learn for test"
date:   2018-02-02
categories: jsTest
---

> 说起测试让我想起来了部门的一个测试大佬，一个人部署起了一套自动化测试框架，能写Python、PHP、Java。但是身为一个小前端的我暂时只了解到Mocha、ShouldJS、Karma、Travis CI、Jasmine，OK不说这么多让我们来看看这些玩意儿能干啥东西。


> 我们接下来做的东西是基于**node环境**的哦，请确保大佬你的环境配置。


# Mocha

> Mocha：一个Javascript测试的框架，讲得简单点就是一个能跑在node环境中也能运行在浏览器中的一个测试工具。怎么玩它呢？跟着小哥哥的脚步，一步两步、摩擦摩擦.....

首先我们需要先创建一个floder，命名my_first_mocha，然后习惯性的跑一下下面的命令

```
cd my_first_mochal
npm init
//接下来就是安装mocha了
npm install --save-dev mocha
//当然你也可以全局安装你的抹茶
npm install --global mocha
```
OK，环境准备好了，让我们来写点什么吧。
创建一个js的folder，再往里面丢一个js(action.js)。

```
//action.js
function doSomething ( a , b ) {
  a = a || 0;
  b = b || 0;

  return a + b;
}

module.exports = doSomething
```
接下来重头戏了，在根目录下再创建一个test的folder,这里面放的都是你的测试案例。我们先来创建第一个测试案例吧。test.js，我们的测试用例写法如下：

```
//test.js
var assert = require('assert');
var doSomething = require('../js/action.js');

describe('TestAction', function() {
  describe('#doSomething()', function() {
    it('1 + 2 should be 3', function() {
      assert.equal(doSomething( 1 , 2 ), 3);
    });
  });
});
```
这里的**describe**就是一个测试套件，每个测试套件里面可以包含多个测试用例 **it**，也可以嵌套测试套件。

接下来让我们在package.json里面加上

```
"scripts": {
    "test": "mocha"
 }
```
现在就是见证奇迹的时刻了在你的terminal里面输入

```
npm run test
```
我们会看到一个成功的结果
![image](/assets/imgs/2018/02/02/1.png)

那失败是怎样的呢？我们把代码稍微修改一下

```
assert.equal(doSomething( 1 , 2 ), 4);
```

再跑命令结果如下：
![image](/assets/imgs/2018/02/02/2.png)

> OK，一个简单的mocha栗子就这样跑起来了，这里我们涉及了几个知识点 [assert](http://nodejs.cn/api/assert.html) 、[mocha](https://mochajs.org/#running-mocha-in-the-browser)
。

> 动手下载 [my_first_mocha](https://github.com/display-b/my_test_demos) 跑起来试试

> next

# Should.js
> 如果你觉得使用node 的assert模块觉得太low了，没关系。你还有ShouldJS、Chai等一些封装好的断言库，使用起来比较语义化。我们就来玩一玩 [ShouldJs](https://shouldjs.github.io/)吧

让我们接着上个栗子，先安装一下should

```
npm install --save-dev should
```
接下来我们来直接修改一下代码

```
var doSomething = require('../js/action.js');

describe('TestAction', function() {
  describe('#doSomething()', function() {
    it('1 + 2 should be 3', function() {
        //使用起来比较语义化，通俗易懂
      doSomething( 1 , 2 ).should.be.equal(3);
    });
  });
});
```
> 接着让我们来跑一下

```
npm run test
```
OMG.报错了，但是别着急，我们来看看它说啥
![image](/assets/imgs/2018/02/02/3.png)
这里的意思明显就是说找不到should嘛。这不简单，我们来手动引入一下

```
require('should');
```
当然还有另外一种方式自动引入should，就是在你的test目录下创建一个文件名为 mocha.opts 。内容如下
```
--require should
```
这样你就不需要手动在每个测试用例文件里面引入了。

> 封装好的断言库使用起来就是如此简单美妙。更多API请看 [Should](https://shouldjs.github.io/)



# Karma
> [karma](https://karma-runner.github.io/2.0/index.html) 是一个强大的测试集成框架，能集成测试框架，测试环境，测试覆盖率等，那我们为什么要使用karma呢？不能直接使用mocha吗？想了解karma可以去看看这篇文章 [karma的前世今生](http://taobaofed.org/blog/2016/01/08/karma-origin/)淘宝出品必属精品。

看看karma能提供的功能：
- 在真实环境中测试
- 支持远程控制
- 执行速度快
- 可以跟第三方 IDE 进行交互
- 支持 ci 服务
- 高扩展性，支持插件开发
- 支持调试

接着让我们动起来吧！**在我们原来的项目上继续,先复制一份然后把该删掉的删除 node_module 、package.json里面的依赖**。action.js修改如下：

```
function doSomething ( a , b ) {
  a = a || 0;
  b = b || 0;

  return a + b;
}
```


然后我们需要安装一些模块先
- karma       #karma官方推荐局部安装在你的项目中哦～
- karma-mocha    #把mocha以插件的形式集成到karma
- karma-chrome-launcher    #需要运行在chrome浏览器的插件，当然你也可以使用火狐这些浏览器
- should   #断言库
 

你可以运行以下命令
```
npm install --save-dev karma karma-mocha karma-chrome-launcher should
```
安装完模块了，接下来我们还需要初始化生成一下karma的配置了,跟着命令一步一步跑就OK了

```
//你也可以指定你的配置名字哦，不然就会默认生成karma.conf.js
//ex:karma init my.conf.js
karma init
```
这时候是不是发现 **-bash: karma: command not found** 😄，这是因为你没有安装karma-ci(可以让你在你的命令行里面使用karma的模块)

```
npm install -g karma-cli
```
这时候你就可以在任何地方使用你的 karma命令了

生成的配置里面我们来了解下
- frameworks：使用到的测试框架，我们选择了mocha
- files：写在这里的文件都是可以自动引入到测试案例中使用的哦，不需要再手动引入拉
- autoWatch：监听文件的变动自动测试
- browsers：需要测试的浏览器，这里我们只选择了Chrome
- singleRun：是否运行完测试之后自动关闭推出浏览器。这里我们选择了false

这些就是我们暂时使用到的配置拉。

```
//接下来就karma start去跑跑测试吧
karma start
```



# Travis CI
> [travis](https://travis-ci.org/)提供集成服务，可以绑定GitHub上的项目，进行监听变动自动测试。听起来是不是很爽，可以自动化了。接入也是很简单的哦

1. 进入官网[travis](https://travis-ci.org/)使用你的GitHub登入，然后就可以看到你的项目了
    ![image](/assets/imgs/2018/02/02/4.png)
2. 接着你只要点击左边的按钮打开监听
3. 然后还有一个最重要的东西.travis.yml（在你的项目根目录下存在）不然travis就不知道你的项目要干嘛了

ok,我们来看看这个配置文件长什么样子

```
#指定运行环境
 language: node_js
 #指定nodejs版本，可以指定多个
 node_js:
   - 8
 #指定在执行script里面的命令之前需要做什么，我们需要安装需要的依赖（每次自动构建都会去执行的哦）
 before_script:
   - npm install -g karma
   - npm install

#执行你需要执行的命令，会按照顺序执行下去的哦
 script:
   - npm run test

 #指定分支，只有指定的分支提交时才会运行脚本
 branches:
   only:
     - master
```
接着我们就是提交GitHub来触发自动构建拉

嘿嘿嘿，是不是没有想象中那么顺利呢～我们打开 [构建页面](https://travis-ci.org)看看它报什么错误呢～
![image](/assets/imgs/2018/02/02/5.png)

就是打不开chrome浏览器嘛，想想在服务端上怎么可能打开chrome呢？那得看看karma如何接入travis解决这个问题？有 [两种方法](https://karma-runner.github.io/2.0/plus/travis.html)：

- 使用PhantomJS启动一个虚拟的浏览器
- Travis支持用虚拟屏幕运行真正的浏览器（比如Firefox）

我们这里就用第一种方法来实现以下吧

```
//我们现在本地安装karma-phantomjs-launcher
npm install --save-dev karma-phantomjs-launcher
//接着再修改下package.json里面的script命令
"scripts": {
   "test": "karma start --single-run --browsers PhantomJS"
}
```
我们可以现在本地运行下npm run test测试下是否成功的哦。

好了我们再次提交GitHub看看效果如何！
看到我们这个图标![image](/assets/imgs/2018/02/02/6.png)是不是很Happy。

整个练习的 [地址](https://github.com/display-b/my_test_demos)。


> 第一篇博客就从总结开始吧～，希望能给大家带来一些小知识。