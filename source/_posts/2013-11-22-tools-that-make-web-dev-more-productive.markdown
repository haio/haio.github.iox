---
layout: post
title: "Tools that make web dev more productive"
date: 2013-11-22 22:02
comments: true
categories: [Node.js, Yeoman, Grunt, Bower]
---
{% img http://haio.u.qiniudn.com/toolset.png 600 200 Tools that make web dev more productive %}

# Grunt
[Grunt](http://gruntjs.com)是一个Javascript写的任务管理工具(Tasks Runner)。在Web开发中，出了写代码之外，我们还有一序
列的任务要做，比如:
<!-- more -->

* **minification** javascript,css,html代码的压缩
* **compilation** .less,.sass,.coffee等文件编译
* **testing** 单元测试，集成测试，各种测试
* **linting** js linting
......

以前，这些任务都分散在不同小任务中，我们也有很多工具来'自动化'这些任务，比如javaer有`maven`以及一堆xml代码搭成的`ant`
,这里就不细说了，实在感兴趣的可以去Google学习下。Grunt相比他们来说进步了很多，它更加的简易，强大和可扩展，你几乎可以
找到一切你想要或不想要的任务plugin来加速你的开发流程，当然你也可以很方面的编写自己的插件。并且在Grunt中你不需要写
shell，不需要写xml，只需要写我们最爱的JSON和Javascript。

## Install grunt CLI

```js
npm install -g grunt-cli
```

安装完`gurnt-cli`之后`grunt`就成为你系统中的可执行命令了，他并没有安装了任何版本的grunt，当你执行`grunt`时候它会在当
前目录下寻找已有的grunt来跑定义好的任务。

## 使用Grunt

### 安装Grunt

Grunt本身也是一个`npm package`，由于只是开发中使用，所以我们可以将它安装在devDependencies中：

```js
npm install grunt --save-dev
```

然后我们会安装一些常用的grunt插件，比如jshint,uglify等

```js
npm install grunt-contrib-jshint --save-dev
npm install grunt-contrib-uglify --save-dev
```

之后我们就可以来构建自己的Grunt任务了，Grunt的所有要运行的任务包含在一个叫`Gruntfile.js`的文件中

### Gruntfile.js

Gruntfile.js由下面几部分组成：

* The "wrapper" function
* Project and task configuration
* Loading Grunt plugins and tasks
* Custom tasks

```js
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // Load the plugin that provides the "uglify" task.
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Default task(s).
  grunt.registerTask('default', ['uglify']);

};
```

### Wrapper function

Grunt是由Node.js写的，从它的wrapper函数可以看出Grunt是被放到一个闭包中然后赋给module.exports供外部使用

```js
module.exports = function (grunt) {
    grunt.initConfig({
        // tasks 
    });
};
```

### Project and task configuration

```js
grunt.initConfig({
  pkg: grunt.file.readJSON('package.json'),
  uglify: {
    options: {
      banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
    },
    build: {
      src: 'src/<%= pkg.name %>.js',
      dest: 'build/<%= pkg.name %>.min.js'
    }
  }
});
```

首先，`grunt.file.readJSON('package.json')`获取了当前项目的配置信息，比如项目的名字`pkg.name`,然后是任务的定义，这里
以`uglify`为例，`uglify`是任务的名字，`options`中定义了要传给当前任务的一些参数或者配置信息，`build`是子任务的名字，
`src`和`dest`分别用来设置子任务的参数。
完成以上配置之后，我们既有一个叫做uglify:build的任务可以使用了。

### Loading Grunt plugins and tasks

要运行定义好的任务，我们就可能需要加载相应的插件，加载已安装的uglify插件：

```js
grunt.loadNpmTasks('grunt-contrib-uglify');
```

### Custom tasks

要执行上面定义好的uglify任务，只需要执行：

```js
grunt uglify
```

我们也可以定义默认的任务

```js
// Default task(s).
grunt.registerTask('default', ['uglify']);
```

`['uglify']`里可以包含一序列的其他任务，现在要执行uglify任务，我们可以执行`grunt default`或者直接`grunt`

我们也可以直接在Gruntfile.js中自定义任务，如

```js
grunt.registerTask('custom', 'This is a custom task', function () {
    console.log('custom!');
});
```

Grunt就说到这里，需要更多的干货可以看[官方文档](http://gruntjs.com/)或[Github](https://github.com/gruntjs)

# Bower

[Bower](http://bower.io/) is a package manager for the web. Bower是twitter推出的一个前端包管理工具，对于Javascript来
说，后端Node.js已经有了npm，Bower就是一个前端的npm，她的语法也是和npm一致的。

## Install Bower

```js
npm install bower -g
```

## Usage

### bower init

初始化一个bower.json文件, `bower init`

```js
{
  "name": "test",
  "version": "0.0.0",
  "authors": [
    "aio <mockingror@gmail.com>"
  ],
  "description": "test",
  "main": "index.js",
  "keywords": [
    "test"
  ],
  "license": "MIT",
  "private": true
}
```

### bower install

```js
bower install jquery --save
```

### bower uninstall

```js
bower uninstall jquery
```

### MORE...

bower search|register

# Yeoman

[Yeoman](http://yeoman.io/)是一个Node.js项目生成工具，与其说是一个工具，它更是一个workflow，Yeoman整合了grunt和bower
，运用了大量的web开发最佳实践，为开发者打造一个更好更高效的workflow

## Install Yeoman

```js
npm install -g yo
```

yo将自动为你安装好grunt和bower，所以上面说道的grunt和bower的安装方法都可以忘记啦，^_^.

## Install Generator

Yeoman提供了一序列的生成器，一个生成器为你生成特定类型的项目，比如说一个普通的web项目，一个mobile项目，一个angular项
目，以web项目为例，首先需要安装相应的generator

```js
npm install -g generator-webapp
```

现在我们就可以使用`yo`来创建一个web项目了

```js
yo webapp
```

你可以选择是否使用Bootstrap和Modernizr，需要注意的是Yeoman默认安装的是sass版的bootstrap，所以需要系统安装了ruby和
compass来编译sass。命令执行完毕之后yeoman也会自动安装好grunt及其用到的plugins

## 项目结构

```js
.
├── app
│   ├── 404.html
│   ├── bower_components
│   ├── favicon.ico
│   ├── images
│   ├── index.html
│   ├── robots.txt
│   ├── scripts
│   └── styles
├── bower.json
├── Gruntfile.js
├── package.json
└── test
    ├── index.html
    ├── lib
    └── spec
```

执行完`yo webapp`会产生如上的目录结构，可以看到Yeoman已经为我们生成好了Bower和Grunt需要的配置文件以及node项目默认的`package.json`，`app`目录是用来存放前端代码的，`app/bower_components`存放了项目所依赖的bower package，`app\scripts`存
放开发者自己的javascript代码，同理`app/styles`存放css代码，`test`存放我们的测试代码

下面我们来运行一下这个项目

```js
grunt server
```

# Conclusion

OK!我们已经如何用Yeoman+Bower+Grunt快速打造一个Vaible的Webapp，心动不如行动，快在你的项目中用起来吧！你会喜欢它的！
