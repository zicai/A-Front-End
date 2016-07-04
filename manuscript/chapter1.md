前端开发流程中涉及到的步骤有

1. 开发环境初始化（搭建项目脚手架）
2. 依赖管理
3. CSS、JS 预处理
4. 代码压缩，合并与混淆
5. Lint
6. 测试

通常，我们需要会借助一些工具（大部分都是 npm 包，这里用 base tool 指代）来完成这些任务：

1. 搭建项目脚手架--yeoman 等
2. 依赖管理--bower,npm 等
3. CSS、JS 预处理--Less,Sass,Babel 等
4. 代码压缩，合并与混淆--
5. Lint--ESlint
6. 测试


grunt、gulp 等 task runner 的出现，使我们可以把上面的步骤自动化，极大的节省了开发时间。实际上，我们使用 npm scripts 也完全可以做到前端自动化。

## 为何不使用 grunt、gulp 等 task runner

grunt、gulp 的原理类似：增加一个抽象层，通过将 base tool 封装成插件，调用这些插件。

有时候抽象很有用，但是同时也有代价：

- Grunt、gulp 等 task runner 都有自己的一套配置语法，有一定的学习成本
- 在很大程度上会依赖插件的作者，例如：插件有没有跟上 base tool 的更新，文档是否清晰
- 增加复杂度，引入新的 bug
- 难以调试：出了问题不知道是配置问题还是插件问题，还是base tool 的问题还是版本问题
- gulpfile 越来越大、依赖的插件越来越多
- when I use npm scripts, I consume tools directly without an extra layer of abstraction.
- 一些插件只暴露了底层包一部分API


if you are happy with your current build system and it accomplishes all that you need it to do, you can keep using it!
Keep focusing on writing your code instead of learning more tooling

那么为啥不去掉这层抽象，直接使用 base tool 呢？

## 常见的误解

- 使用 npm scripts 需要很强的命令行编程技能
- npm scripts 不够强大
- Gulp 使用 stream，构建起来会更快
- npm scripts 不能跨平台

这里先不澄清这些误解，看完之后，你应该就有自己的判断了。

## npm scripts
base tool 通常都会提供命令行接口，利用 npm scripts 直接调用这些 base tool
足够强大和简单
很多开源项目早已经开始使用 npm scripts

npm's scripts directive can do everything that these build tools can, more succinctly, more elegantly, with less package dependencies and less maintainence overhead.

## 基础准备

npm 不仅仅是一个包管理工具，它提供了很丰富的功能。利用它的 `npm run-script` 命令，来构建自动化流程。

npm run-script 语法如下：

```
npm run-script <command> [-- <args>...]
```
可以简写为：npm run。当我们执行 npm run 时，npm 会从 package.json 文件中找到 scripts 对象。


如果指明了 command，则将 script 对象对应的属性值作为命令执行。

假设 package.json 文件内容如下：

```
{
  "name": "myproject",
  "devDependencies": {
    "jshint": "latest",
    "browserify": "latest",
    "mocha": "latest"
  },
  "scripts": {
    "lint": "jshint **.js",
    "test": "mocha test/"
  }
}
```

当我们执行 npm run lint 时，npm 会spawn 一个 shell 并执行 jshint **.js。

如果不指明 command ，则列出所有可用脚本。

npm 还提供了几个常用命令的缩写：

- npm test 对应 npm run test
- npm start 对应 npm run start


### 钩子

### 传递参数

从 npm 2.0.0 开始，可以传递参数。





npm scripts
npm run 命令
缩写：

- npm test
- npm start
- npm stop

pre- post- 钩子

传递参数
npm config 变量


替换任务

- 使用多个文件
- 运行多个任务
- 流
- 版本号
- 清理
- 编译为唯一名字
- watch
- livereload
- 没有二进制包的任务

watch
监控文件的变化，一旦变化，则 reload server ，重新编译资源，重新执行任务。
有一些工具自带了该选项，例如：

- mocha
- stylus
- node-sas
- jade
- karma

有的工具没有这个选项，有时候，你想结合多个目标到一个任务中。
有很多工具用来监控文件，并执行任务：

- watch
- onchange
- dirwatch
- nodemon

如何并行？

live reload


node_modules/.bin 是如何出现的

npm-scripts
https://docs.npmjs.com/misc/scripts

## 实践

接手到一个旧项目，

## 添加浏览器刷新
当js、css 、模板文件文件更新时，刷新浏览器

```
npm install -g browser-sync

"watch": "browser-sync start --proxy '127.0.0.1:8000' --files 'kcon/static/styles/*.css'"
```
## CSS 预处理器

node-sass

```
npm install node-sass
```

```
node-sass --watch kcon/static/src/main.scss kcon/static/styles/test.css
```
## autoprefix
npm install --save-dev postcss-cli autoprefixer
"autoprefixer": "postcss -u autoprefixer --autoprefixer.browsers '&gt; 5%, ie 9' -r dist/css/*"

## lint
npm install --save-dev eslint
eslint --init
"lint": "eslint src/js"

## JS 预处理
"build-js": "browserify browser/main.js > static/bundle.js"
"build-js": "browserify browser/main.js | uglifyjs -mc > static/bundle.js"
"watch-js": "watchify browser/main.js -o static/bundle.js -dv"

## 合并、压缩
"build-css": "cat static/pages/*.css tabs/*/*.css > static/bundle.css"
"watch-css": "catw static/pages/*.css tabs/*/*.css -o static/bundle.css -v"

## 混淆

## 图片压缩
npm i -D imagemin-cli
"imagemin": "imagemin src/images dist/images -p"

sprite

## change
npm i -D onchange
"watch:css": "onchange 'src/scss/*.scss' -- npm run build:css",
 "watch:js": "onchange 'src/js/*.js' -- npm run build:js"

The problem is that && chains commands together and waits for each command to finish successfully before starting the next. However, since we are running watch commands, they never finish!

用

- parallelshell
- npm-run-all


## 任务
顺序执行 &&
"build": "npm run build-js && npm run build-css"
并列执行 &
"watch": "npm run watch-js & npm run watch-css"

## window 兼容问题

- 使用跨平台命令 http://www.yolinux.com/TUTORIALS/unix_for_dos_users.html
- 使用跨平台包
- 使用shelljs

## 痛点

- JSON 文件不支持 注释

http://www.barretlee.com/blog/2016/06/02/thing-about-taobao-homepage/
上线前的自动化检测

检测 HTML 是否符合规范
检测 https 升级情况
检测链接合法性
检测静态资源合法性
检测 JavaScript 报错
检测页面加载时是否有弹出框
检测页面是否调用 console.*
页面 JS 内存记录
当然，也可以自己添加测试用例，比如检测接口数据格式、模块天窗问题等。自动化检测也可以设定定时回归，还是比较有保障的。

https://www.h5jun.com/post/untangling-spaghetti-code-writing-maintainable-javascript.html


参考资料：

- gulp https://github.com/zicai/zicai.github.com/blob/master/_drafts/2015-05-12-gulp.md
- grunt https://github.com/zicai/zicai.github.com/blob/master/_posts/2013-03-21-grunt-tutorial.md
- http://substack.net/task_automation_with_npm_run
- http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/

    - https://github.com/keithamus/npm-scripts-example
- https://medium.freecodecamp.com/why-i-left-gulp-and-grunt-for-npm-scripts-3d6853dd22b8#.pjqu09t4b   为什么放弃 grunt 和 gulp , 转向 npm script
- https://css-tricks.com/why-npm-scripts/

    - https://github.com/damonbauer/npm-build-boilerplate