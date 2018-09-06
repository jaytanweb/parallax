![Parallax.js](logo.png)

[![CDNJS](https://img.shields.io/cdnjs/v/parallax.svg)](https://cdnjs.com/libraries/parallax)

Parallax 引擎能对只能设备的方向作出反应。 如果设备没有陀螺仪或不支持手势， 将会使用鼠标的位置数据作为替代。

查看 **[demo](http://wagerfield.github.com/parallax/)** 看看它是怎么工作的！

# Table of Contents

- [1. 开始使用](#1-开始使用)
  - [1.1 安装](#11-安装)
  - [1.2 准备工作](#12-准备工作)
  - [1.3 使用 Parallax](#13-使用-parallax)
- [2. 配置](#2-配置)
  - [2.1 编程式 vs 声明式](#21-编程式-vs-声明式)
  - [2.2 配置选项](#22-配置-选项)
- [3. 方法函数](#3-方法函数)
- [4. 参与开发](#4-参与开发)
  - [4.1 克隆项目](#41-克隆项目)
  - [4.2 Opening an Issue](#42-opening-an-issue)
  - [4.3 Known Issues](#43-known-issues)
- [5. FAQ](#5-faq)
- [6. 资料](#6-资料)
   - [6.1 License](#61-license)
   - [6.2 贡献者](#62-贡献者)

# 1. 开始使用

## 1.1 安装

### 1.1 a) 使用 CDN

1. 将 `<script src="https://cdnjs.cloudflare.com/ajax/libs/parallax/3.1.0/parallax.min.js"></script>` 加入你的代码
2. 完成！

非常感谢 [cdnjs](https://cdnjs.com/) 的朋友们保存着我们这个工具库。

### 1.1 b) 初学者

1. 前往[releases](https://github.com/wagerfield/parallax/releases) 章节
2. 从最新的release中将 `compiled.zip` 下载下来
3. 将下载的 ZIP 文件解压，并找到 `parallax.js` 和 `parallax.min.js` 文件
	- 如果你想学习源码，请使用`parallax.js`
	- 将`parallax.min.js` 用作部署, 因为它的体积更小
4. 将你选择的文件复制到项目目录
5. 到目前为止，一切都很顺利！

### 1.1 c) 专业人士

`npm i -s parallax-js`

然后你会在 `node_modules/parallax-js/src/parallax.js`看到源码，而 browserified, babelified, uglified, production-ready 的版本则在 `node_modules/parallax-js/dist/parallax.min.js`

## 1.2 准备工作

### 添加 Script

如果你使用编译后的版本，无论是从发布页下载， 还是从 `dist` 文件夹复制， 像添加其他Javascript库一样加入下面的`script`代码  
`<script src="path/to/parallax.js"></script>`

当然，如果你使用npm来安装， 并且使用了 browserify/babel，你只需简单的加入下面的代码：  
`import Parallax from 'parallax-js'` 或者  
`const Parallax = require('parallax-js')`

### 生成你的 HTML 元素

每个 Parallax.js 实例 需要一个 container 元素， 也就是 scene。你可以自由定义它你喜欢的名字，但现在，让我们先使用这个 ID:

```html
<div id="scene">
</div>
```

默认情况下，scene元素的所有直接子元素都会变成移动对象， the layers. 你可以将这个改做一个定制的查询选择器，不过现在我们还是先用这个最容易的方式开始：

```html
<div id="scene">
  <div>My first Layer!</div>
  <div>My second Layer!</div>
</div>
```

所有其他的选项和参数都是可选的，因为它们有默认值，并且可以通过编程式设置，但是`data-depth` 属性是每个图层都需要设置的。 应用到每个图层的动作都会乘上这个 depth 属性。

```html
<div id="scene">
  <div data-depth="0.2">My first Layer!</div>
  <div data-depth="0.6">My second Layer!</div>
</div>
```

## 1.3 使用 Parallax

当你的 DOM 已经设置好，你可以创建一个 new Parallax.js 实例， 使用你的 scene 元素作为它的首个参数。

```javascript
var scene = document.getElementById('scene');
var parallaxInstance = new Parallax(scene);
```

就是这样，你现在已经开始使用 Parallax.js 了！

# 2. 配置

## 2.1 编程式 vs 声明式

大部分的配置选项能通过 scene 元素的 data-value 属性, 或者编程式声明时传入的参数对象的属性来设置。 编程式的设置的优先级要高于 HTML 的 data-value 属性。 
有些选项还可以在运行的时候通过实例方法来设置。

声明式：

```html
<div data-relative-input="true" id="scene">
  <div data-depth="0.2">My first Layer!</div>
  <div data-depth="0.6">My second Layer!</div>
</div>
```

编程式：

```javascript
var scene = document.getElementById('scene');
var parallaxInstance = new Parallax(scene, {
  relativeInput: true
});
```

在运行时使用实例方法：

```javascript
parallaxInstance.friction(0.2, 0.2);
```

## 2.2 配置选项

### relativeInput
> 输入数据： 鼠标或智能设备的运动数据

选项: **relativeInput**  
HTML属性: **data-relative-input**

类型: *boolean*  
默认值: *false*

将鼠标的输入与 scene 元素关联起来。

当陀螺仪可以使用时此属性失效。

### clipRelativeInput

选项: **clipRelativeInput**  
HTML属性: **data-clip-relative-input**

类型: *boolean*  
默认值: *false*

限制鼠标输入在 scene 元素的范围内。 这意味着当鼠标箭头接触到 scene 元素的边沿时图层的运动会立即停止。

当陀螺仪可以使用时，或者 `hoverOnly` 属性激活时此属性失效。

### hoverOnly

选项: **hoverOnly**  
HTML属性: **data-hover-only**

类型: *boolean*  
默认值: *false*

Parallax 将只会在鼠标在 scene 元素之上时产生效果， 其他的情况下所有的图层会回到它们的初始位置。与 `relativeInput`属性搭配使用效果更佳。  
当陀螺仪可以使用时此属性失效。

### inputElement

选项: **inputElement**  
HTML属性: **data-input-element**  
Method: **setInputElement(HTMLElement)**

类型: *null* or *HTMLElement* / *String*  
默认值: *null*

允许使用不同的元素作为鼠标输入。 
这个配置参数需要一个HTML元素， 其属性值属于一个元素选择器字符串。  
必需搭配 `relativeInput`属性来使用， 设置 `hoverOnly` 也可能生效。  

当陀螺仪可以使用时此属性失效。

### calibrateX & calibrateY

选项: **calibrateX** & **calibrateY**  
HTML属性: **data-calibrate-x** & **data-calibrate-y**  
Method: **calibrate(x, y)**

类型: *boolean*  
默认值: *false* for X, *true* for Y

在初始化的时候缓存初始的 X/Y 坐标值，并计算出相应的动作。  
当使用鼠标时，此属性时效。

### invertX & invertY

选项: **invertX** & **invertY**  
HTML属性: **data-invert-x** & **data-invert-y**  
Method: **invert(x, y)**

类型: *boolean*  
默认值: *true*

将输入数据转化为关联图层的运动。 同时将两个属性设置为 *false* 会导致图层与设备的运动或鼠标同步运动。

### limitX & limitY

选项: **limitX** & **limitY**  
HTML属性: **data-limit-x** & **data-limit-y**  
Method: **limit(x, y)**

类型: *false* or *integer*  
默认值: *false*

限制各个图层在相应坐标系中的运动。将其设置为*false*会给运动带来完全的自由。

### scalarX & scalarY

选项: **scalarX** & **scalarY**  
HTML属性: **data-scalar-x** & **data-scalar-y**  
Method: **scalar(x, y)**

类型: *float*  
默认值: *10.0*

将输入数据乘以这个值， 以此来增加或减少运动的速度和幅度。

### frictionX & frictionY

选项: **frictionX** & **frictionY**  
HTML属性: **data-friction-x** & **data-friction-y**  
Method: **friction(x, y)**

类型: *float* between *0* and *1*  
默认值: *0.1*

应用到图层上的摩擦总数。设置为 *1* 的时候图层会立即到达它们的新位置，所有低于 1 的值都会增加一些缓冲。  
默认值 *0.1* 增加了一些合理的缓冲。 尝试下 *0.15* or *0.075* 看看其他效果。

### originX & originY

选项: **originX** & **originY**  
HTML属性: **data-origin-x** & **data-origin-y**  
Method: **origin(x, y)**

类型: *float* 取值在 *0* 到 *1* 之间  
默认值: *0.5*

鼠标输入的 X 和 Y 原点。默认值 *0.5* 代表于屏幕或者元素的中心， *0* 代表左侧 (X 轴) or 上方 (Y 轴) 的边，1 代表右边或者底部。  
当陀螺仪可以使用时此属性失效。

### precision

选项: **precision**  
HTML属性: **data-precision**

类型: *integer*  
默认值: *1*

元素位置四舍五入位数。 *1* 是一个合理的默认值， 你在接下来的几年都应该不需要去改变它， 除非你有一个非常有趣并且独特的设置。

### selector

选项: **selector**  
HTML属性: **data-selector**

类型: *null* or *string*  
默认值: *null*

这个字符串将会传给 querySelectorAll 作为选择 scene 元素子图层元素的依据。 当它是 *null*，的时候将会简单的选择所有的直系子元素。  
使用 `.layer` 作为例子，将会选择子元素里面 class 名字含有 *layer* 的元素。

### pointerEvents

选项: **pointerEvents**  
HTML属性: **data-pointer-events**

类型: *boolean*  
默认值: *false*

设置为 *true* 以激活与 scene 和 layer 元素的相互作用。 当设置为 *false* 的时候， CSS 属性 `pointer-events: none` 将会产生作用。  
单单设置这个值为 *true* 不足以影响到所有的 layers， 因为它们会互相覆盖。要么为所有的 layer 子元素设置 `position: absolute`，要么保持 **pointerEvents**  为  *false* 并且为相关的交互元素单独设置`pointer-events: all` 。

### onReady

选项: **onReady**

类型: *null* or *function*  
默认值: *null*

当 Parallax 实例完成了初始化设置，这个回调函数会立刻执行。 这一般会占用 1000ms (`calibrationDelay * 2`).

# 3. 方法函数

除了上一章的描述的设置方法，这里还有一些公开的更容易使用的函数：

### enable()

启动一个暂停的 Parallax 实例。

### disable()

暂停一个正在运行的 Parallax 实例。

### destroy()

完全摧毁一个 Parallax 实例，让它被垃圾回收机制回收。

### version()

返回 Parallax 库的版本编号。

# 4. 参与开发

## 4.1 克隆项目

1. 克隆仓库 `git clone git@github.com:wagerfield/parallax.git`
2. 打开工作目录 `cd parallax`
3. 安装依赖 `npm install`
4. 开启开发服务器 `gulp watch`
5. 在浏览器打开 [http://localhost:9000/](http://localhost:9000/) 

## 4.2 Opening an Issue

如果你使用了这个库的项目需要用法上的指导，给我们提供一个可运行的项目demo。可以是一个 GitHub 项目仓库，一个压缩着你的项目的 ZIP 文件 ， 一个放在 CodePen 或者 JSFiddle 上的项目， you name it.  
*不要抱怨有一些东西没正确运行，又不给我们帮主你的途径。* 感谢！

## 4.3 Known Issues

### SVG-Bug in MS Edge

It seems MS Edge does not support the `children` or `querySelectorAll` methods for SVG elements.

# 5. FAQ

### 我怎么配合jQuery来使用这个库呢？

jQuery 不会使用任何方式阻止你使用这个库。 如果你想用 jQuery 去选择你的 Parallax scene 元素，你也可以那样做。

```javascript
var scene = $('#scene').get(0);
var parallaxInstance = new Parallax(scene);
```

### 我怎样与我的 layers 互动？

查看上面的设置选项 `pointerEvents` 。

### 我怎样才能使 demo 文件正常运行？

要么就从 [GitHub Releases](https://github.com/wagerfield/parallax/releases) 章节下载 compiled_with_examples.zip，要么按照章节 section 4.1的做法。


# 6. 资料

## 6.1 License

本项目使用的协议是 [MIT](http://www.opensource.org/licenses/mit-license.php) License. Enjoy!

## 6.2 贡献者

Matthew Wagerfield: [@wagerfield](http://twitter.com/wagerfield)  
René Roth: [Website](http://reneroth.org/)
