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
- [3. 方法](#3-方法)
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

大部分的配置选项能通过 scene 元素的 data-value 属性, 或者配置类的属性来设置。 编程式的设置的优先级要高于 HTML 的 data-value 属性。 
有些选项还可以在运行的时候通过实例方法来设置。.

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

Property: **relativeInput**  
Attribute: **data-relative-input**

Value: *boolean*  
Default: *false*

Makes mouse input relative to the position of the scene element.  
No effect when gyroscope is used.

### clipRelativeInput

Property: **clipRelativeInput**  
Attribute: **data-clip-relative-input**

Value: *boolean*  
Default: *false*

Clips mouse input to the bounds of the scene. This means the movement stops as soon as the edge of the scene element is reached by the cursor.  
No effect when gyroscope is used, or `hoverOnly` is active.

### hoverOnly

Property: **hoverOnly**  
Attribute: **data-hover-only**

Value: *boolean*  
Default: *false*

Parallax will only be in effect while the cursor is over the scene element, otherwise all layers move back to their initial position. Works best in combination with `relativeInput`.  
No effect when gyroscope is used.

### inputElement

Property: **inputElement**  
Attribute: **data-input-element**  
Method: **setInputElement(HTMLElement)**

Value: *null* or *HTMLElement* / *String*  
Default: *null*

Allows usage of a different element for cursor input.  
The configuration property expects an HTMLElement, the data value attribute a query selector string.  
Will only work in combination with `relativeInput`, setting `hoverOnly` might make sense too.  
No effect when gyroscope is used.

### calibrateX & calibrateY

Property: **calibrateX** & **calibrateY**  
Attribute: **data-calibrate-x** & **data-calibrate-y**  
Method: **calibrate(x, y)**

Value: *boolean*  
Default: *false* for X, *true* for Y

Caches the initial X/Y axis value on initialization and calculates motion relative to this.  
No effect when cursor is used.

### invertX & invertY

Property: **invertX** & **invertY**  
Attribute: **data-invert-x** & **data-invert-y**  
Method: **invert(x, y)**

Value: *boolean*  
Default: *true*

Inverts the movement of the layers relative to the input. Setting both of these values to *false* will cause the layers to move with the device motion or cursor.

### limitX & limitY

Property: **limitX** & **limitY**  
Attribute: **data-limit-x** & **data-limit-y**  
Method: **limit(x, y)**

Value: *false* or *integer*  
Default: *false*

Limits the movement of layers on the respective axis. Leaving this value at false gives complete freedom to the movement.

### scalarX & scalarY

Property: **scalarX** & **scalarY**  
Attribute: **data-scalar-x** & **data-scalar-y**  
Method: **scalar(x, y)**

Value: *float*  
Default: *10.0*

Multiplies the input motion by this value, increasing or decreasing the movement speed and range.

### frictionX & frictionY

Property: **frictionX** & **frictionY**  
Attribute: **data-friction-x** & **data-friction-y**  
Method: **friction(x, y)**

Value: *float* between *0* and *1*  
Default: *0.1*

Amount of friction applied to the layers. At *1* the layers will instantly go to their new positions, everything below 1 adds some easing.  
The default value of *0.1* adds some sensible easing. Try *0.15* or *0.075* for some difference.

### originX & originY

Property: **originX** & **originY**  
Attribute: **data-origin-x** & **data-origin-y**  
Method: **origin(x, y)**

Value: *float* between *0* and *1*  
Default: *0.5*

X and Y origin of the mouse input. The default of *0.5* refers to the center of the screen or element, *0* is the left (X axis) or top (Y axis) border, 1 the right or bottom.  
No effect when gyroscope is used.

### precision

Property: **precision**  
Attribute: **data-precision**

Value: *integer*  
Default: *1*

Decimals the element positions will be rounded to. *1* is a sensible default which you should not need to change in the next few years, unless you have a very interesting and unique setup.

### selector

Property: **selector**  
Attribute: **data-selector**

Value: *null* or *string*  
Default: *null*

String that will be fed to querySelectorAll on the scene element to select the layer elements. When *null*, will simply select all direct child elements.  
Use `.layer` for legacy behaviour, selecting only child elements having the class name *layer*.

### pointerEvents

Property: **pointerEvents**  
Attribute: **data-pointer-events**

Value: *boolean*  
Default: *false*

Set to *true* to enable interactions with the scene and layer elements. When set to the default of *false*, the CSS attribute `pointer-events: none` will be applied for performance reasons.  
Setting this to *true* alone will not be enough to fully interact with all layers, since they will be overlapping. You have to either set `position: absolute` on all layer child elements, or keep **pointerEvents** at *false* and set `pointer-events: all` for the interactable elements only.

### onReady

Property: **onReady**

Value: *null* or *function*  
Default: *null*

Callback function that will be called as soon as the Parallax instance has finished its setup. This might currently take up to 1000ms (`calibrationDelay * 2`).

# 3. Methods

In addition to the configuration methods outlined in the section above, there are a few more publicly accessible methods:

### enable()

Enables a disabled Parallax instance.

### disable()

Disables a running Parallax instance.

### destroy()

Completely destroys a Parallax instance, allowing it to be garbage collected.

### version()

Returns the version number of the Parallax library.

# 4. Development

## 4.1 Running the Project

1. Clone the Repository `git clone git@github.com:wagerfield/parallax.git`
2. Open the working directory `cd parallax`
3. Install dependencies `npm install`
4. Run development server `gulp watch`
5. Open [http://localhost:9000/](http://localhost:9000/) in browser

## 4.2 Opening an Issue

If you need help relating the direct usage of this library in a project of yours, provide us with a working, running example of your work. This can be a GitHub repository, a ZIP file containing your work, a project on CodePen or JSFiddle, you name it.  
*Do not complain about something not working without giving us some way to help you.* Thank you!

## 4.3 Known Issues

### SVG-Bug in MS Edge

It seems MS Edge does not support the `children` or `querySelectorAll` methods for SVG elements.

# 5. FAQ

### How can I use this Library with jQuery?

jQuery will not prevent you from using this library in any way. If you want to use jQuery for selecting your Parallax scene element, you can do so too.

```javascript
var scene = $('#scene').get(0);
var parallaxInstance = new Parallax(scene);
```

### How can I interact with my layers?

Check out the section on the configuration option `pointerEvents` above.

### How do I get the demo files to work?

Either download compiled_with_examples.zip from the [GitHub Releases](https://github.com/wagerfield/parallax/releases) section, or follow section 4.1


# 6. Information

## 6.1 License

This project is licensed under the terms of the  [MIT](http://www.opensource.org/licenses/mit-license.php) License. Enjoy!

## 6.2 Authors

Matthew Wagerfield: [@wagerfield](http://twitter.com/wagerfield)  
René Roth: [Website](http://reneroth.org/)
