# Canvas
使用Canvas从0-1制作出精美的背景动画


### 什么是Canvas
> `<canvas>`是HTML5新增元素，可用于通过使用JavaScript来绘制图形，制作照片，创建动画，甚至可以进行实时视频处理和渲染

**要注意，`<canvas>`只是一个画布，本身并不具有绘图能力，必须使用Js等脚本语言**

`<canvas>`标签允许脚本语言动态渲染位图像，`<canvas>`标签创建出了一个可绘制区域，Js代码可以通过一套完整的绘图功能（类似于其他通过二维的API）访问该区域，从而生成动态的图形

**一句话总结：Canvas就是为了解决旧时代页面只能显示静态图片而提出的**

### SVG与Canvas的区别
svg是使用XML来描述2D图形的语言（svg创建的每一个元素都是独立的DOM元素，由于是DOM元素，我们就可以通过css和Js来操控DOM。可以对每一个DOM元素进行监听）
因为每一个元素都是DOM的机制，所以修改svg中的DOM,系统就会对页面进行重绘（过分的重绘对性能有影响）

Canvas对比svg，是通过javascript来绘制2D图形，Canvas只是一个HTML元素，并不会跟svg一样创建DOM元素，因此我们无法通过JS来操控Canvas内单独的图形，不能对其中的图形进行监听

一旦Canvas绘制完成，它就不再得到浏览器关注，此时如果其位置发生了变化，那么整个场景也需要重新绘制（包括任何或许已被图形覆盖的对象）


-   Canvas 是基于像素的即时模式图形系统，绘制完对象后不保存对象到内存中，当再次需要这个对象时，需要重新绘制
-   SVG 是基于形状的保留模式图形系统，绘制完对象后会将其保存在内存中，当需要修改这个对象信息时，直接修改就可以了

### Canvas应用场景
根据不同场景，选择不同的绘图操作，下面了解Canvas的大部分应用场景

-   绘制图表（Echart大部分也是用canvas实现）
-   背景特效
-   小游戏
-   活动页面
-   小特效

### 第一次尝试
****
#### canvas-demo/demo01.html
**圆、直线、弧、矩形、点**
- 创建Canva画布
```
<canvas id='canvas'></canvas>
var canvas = document.getElementById('canvas')

初始化canvas若不设置宽高，默认为300*150(px)
改变画布大小方式：
    HTML：设置width，height
    CSS：设置width，height  （使用CSS来设置宽高的话，画布就会按照300*150的比例进行缩放）
    JS：动态设置width，height
尽量使用HTML或者JS的方式来修改宽高

```
- 获取Canvas对象
```
var context = canvas.getContext('2d')

--------------------------------------------------------------
canvas.getContext(contextType,contextAttributes)

contextType 类型：2d | webgl | webgl2
contextAttributes 属性 (https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/getContext)
```
- 绘制路径
```
Canvas中创建路径的方式：
-   fill()      # 填充路径
-   stroke()        # 描边
-   arc()       # 创建圆弧
-   rect()      # 创建矩形
-   fillRect()      # 绘制矩形路径区域  （实心）
-   storkeRect()        # 绘制矩形路径描边  （空心）
-   clearRect()     # 在给定的矩形内清除指定的像素
-   arcTo()     # 创建两切线之间的弧/曲线
-   beginPath()     # 起始一条路径，或重置当前路径
-   moveTo()        # 把路径移动到画布中的指定点，不创建线条
-   lineTo()        # 添加一个新点，然后在画布中创建从该点到最后指定点的线条
-   closePath()     # 创建从当前点回到起始点的路径
-   clip()      # 从原始画布剪切任意形状和尺寸的区域
-   quadraticCurveTo()      # 创建二次方贝塞尔曲线
-   bezierCurveTo()     # 创建三次方贝塞尔曲线
-   isPointInPath()     # 如果指定的点位于当前路径中，则返回 true，否则返回 false


设置填充/描边颜色（可以设置渐变）
- fillStyle
- strokeStyle
```
- 使用canvas画一个点
```
画一个点也就是画一个半径为1的圆

var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d");
var cx = canvas.width = 400;
var cy = canvas.height = 400;

context.beginPath();
context.arc(100, 100, 1, 0, Math.PI * 2, true);     # 创建圆弧
context.closePath();
context.fillStyle = 'rgb(255,255,255)';
context.fill();

可以看出Canvas绘制图像的步骤：
开始路径（beginPath）→ 绘制路径 → 关闭路径（closePath）→ 设置填充颜色/描边颜色 → 填充颜色或描边（fill）


具体看看绘制路径arc()
    arc()用于创建圆或部分圆     # context.arc(x,y,r,sAngle,counterclockwise)
    x   # 圆心x坐标
    y   # 圆心y坐标
    r   # 圆半径
    sAngle  # 起始角，以弧度计（弧的圆形的三点钟方向是0度
    eAngle  # 结束角，以弧度计（半圆为1*PI）
    counterclockwise    # 可选，规定应该逆时针绘图。false为顺时针，true为逆时针
```
- 绘制直线
```
moveTo      # 把路径移动到画布中指定点，不创建线条
lineTo      # 添加一个新点，然后画布中创建从该点到最后指定点的线条


注意：
    -   如果没有moveTo，那么第一次lineTo就视为moveTo
    -   每次lineTo后如果没有moveTo，那么下次lineTo的开始点就为前一次lineTo的结束点


绘制直线之后，为直线添加样式
- lineCap       # 设置或返回线条的结束端点样式
- lineJoin      # 设置或返回两条线相交时，所创建的拐角类型
- lineWidth     # 设置或返回当前的线条宽度
- miterLimit    # 设置或返回最大斜接长度
```
- 绘制矩阵
```
-   fillRect(x,y,width,height)      # 绘制一个实心矩形
-   strokeRect(x,y,width,height)    # 绘制一个空心矩形
```
#### canvas-demo/demo02.html
**阴影、渐变**
```
给路径设置来改变其样式

- fillStyle     # 设置或返回用于填充绘画的颜色，渐变或模式
- strokeStyle       # 设置或返回用于描边的颜色，渐变或模式
- shadowColor       # 设置或返回用于阴影的颜色
- shadowBlur        # 设置或返回阴影的模糊级别
- shadowOffsetX     # 设置或返回阴影距形状的水平距离
- shadowOffsetY     # 设置或返回阴影距形状的垂直距离
```
- 设置阴影
```
    context.beginPath();
    context.arc(100,100,50,0,2*Math.PI,false);
    context.fillStyle = '#fff';
    context.shadowBlur = 20;
    context.shadowColor = '#fff';
    context.fill()
```
- 设置渐变
```
- createLinearGradient()      # 创建线性渐变（用在画布内容上）
- createPattern()       # 在指定的方向上重复指定的元素
- createRadialGradient()        # 创建放射状|环形的渐变（用在画布内容上）
- addColorStop()        # 规定渐变对象中的颜色和停止位置


createLinearGradient(x0,y0,x1,y1)
-   x0:开始渐变的x坐标
-   y0：开始渐变的y坐标
-   x1：结束渐变的x坐标
-   y1：结束渐变的y坐标

// 由上向下 粉色到白色渐变
var grd = context.createLinearGradient(100,100,100,200);
grd.addColorStop(0,'pink');
grd.addColorStop(1,'white');

context.fillStyle = grd;
context.fillRect(100,100,200,200);
```
#### canvas-demo/demo03.html
**图形转换（缩放，旋转）**
```
- sacle()       # 缩放当前绘图
- rotate()      # 旋转当前绘图
- translate()       # 重新映射画布上的（0，0）位置
- transform()       # 替换绘图的当前转换矩阵
- setTransform()        # 将当前转换重置为单位矩阵，然后允许transform()
```
- 缩放
```
// 绘制一个矩形：放大到200%，再放大200%
context.strokeStyle = 'white';
context.strokeRect(5,5,50,25);
context.scale(2,2);
context.strokeRect(5,5,50,25);
context.scale(2,2);
context.strokeRect(5,5,50,25);
```
- 旋转
```
context.fillStyle = 'white';
context.rotate(20*Math.PI/180);
context.fillRect(70,30,200,100);


context.rotate(angle)
-   angle       # 旋转角度，以弧度计。
如需将角度转换为弧度，请使用 degrees*Math.PI/180 公式进行计算。 举例：如需旋转 5 度，可规定下面的公式：5*Math.PI/180
```
**我们使用的图形变换的方法都是作用在画布上的，既然对画布进行了变换，那么在接下来绘制的图形都会变换。这点是需要注意的。
比如我对画布使用了 rotate(20*Math.PI/180) 方法，就是将画布旋转了 20°，然后之后绘制的图形都会旋转 20 度**

- 绘制图像
```
- drawImage()       # 向画布上绘制图像、画布或视频

context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height)
- img       # 规定要使用的图像，画布或视频
- sx        # 可选，开始剪切的x坐标位置
- sy        # 可选，开始剪切的y坐标位置
- swidth        # 可选，被剪切图像的宽度
- sheight       # 可选，被剪切图像的高度
- x     # 在画布上放置图像的x坐标位置
- y     # 在画布上放置图像的y坐标位置
- width     # 可选，要使用的图像的宽度（伸展或缩小图像）
- height    # 可选，要使用的图像的高度（伸展或缩小图像）
```



### 实现随机粒子
****
-   粒子特效的特点
    -   粒子
    -   规则图形
    -   随机
    -   数量多

#### 实现一个随机粒子特效（实现随机粒子.html）
· 创建全屏Canvas
```
var ctx = document.getElementById('canvas'),
            content = ctx.getContext('2d'),
            WIDTH,
            HEIGHT;

WIDTH = document.documentElement.clientWidth;
HEIGHT = document.documentElement.clientHeight;

ctx.width = WIDTH;
ctx.height = HEIGHT;
```
· 设置Round_item类
```
Round_item类需要什么参数？（x,y,半径,透明度,index）
    我们需要设置的是位置随机，透明度随机，半径随机的圆，
    而为了区分不同的圆，我们还应该设置一个唯一的index参数

function Round_item(index,x,y){
    this.index = index
    this.x = x
    this.y = y
    this.r = Math.random()*2 + 1
    var alpha = (Math.floor(Math.random() * 10) + 1) / 10 / 2
    this.color = 'rgba(255,255,255,' + alpha + ')';
}

此外我们还需要一个initRoundPopulation变量来设置round的个数，然后我们可以通过for循环来创建initRoundPopulation个圆
```
· 设置draw()方法
```
draw() → 画圆方法
我们需要将draw()设置在Round_item的原型上，这样我们每个Round_item实例对象都拥有了draw()方法

Round_item.prototype.draw = function () {
    content.fillStyle = this.color;
    content.shadowBlur = this.r * 2;
    content.beginPath();
    content.arc(this.x, this.y, this.r, 0, 2 * Math.PI, false);
    content.closePath();
    content.fill();
};
```
· 初始化init()函数
```
在init()函数中，我们的主要任务是创建单个的round，然后使用每一个round的draw方法

function init() {
    for(var i = 0; i < initRoundPopulation; i++ ){
        round[i] = new Round_item(i,Math.random() * WIDTH,Math.random() * HEIGHT);
        round[i].draw();
    }
}

最后调用init()实现随机粒子
```

### 让随机粒子动起来
****
· `animate()`函数
```
animate()的作用是帮助我们形成动画，在这个函数中，首先需要清除当前屏幕（使用清除函数content.clearRect()

content.clearRect(x,y,width,height)
    - x     # 要清除的矩形左上角的x坐标
    - y     # 要清除的矩形左上角的y坐标
    - width         # 要清除的矩形的宽度，以像素计
    - height        # 要清除的矩形的高度，以像素计


由于我们需要清除的是一整个屏幕，所以clearRect要这样用
content.clearContent(0,0,WIDTH,HEIGHT)


清除了屏幕内容之后就需要重新绘制图形了
```
· 实现粒子匀速上升
```
实现粒子匀速上升，实际上就是改变y坐标，并且在是需要匀速的

我们将粒子位移的函数定义为move()，并且要写在Round_item的原型上，这样每个实例才都具有位移的能力

位移函数有了，接下来就要实现'不断'位移的能力，实现动画的效果，就需要'不断'地进行清除再重绘，并且时间间隔不能过长
```
· 实现**不断**
```
想要实现不停位移的效果，大部分人可能会想到JavaScript中的setTimeout | setInterval，但它们都不够精确，由于它们内在的运行机制（具体了解Js的事件循环EventLoop），如果队列前面已经存在其他任务，那么动画就会出现延迟问题，因为动画代码需要等前面的任务完成后才执行

所以我们需要另外一个函数 -- requestAnimationFrame()
    # window.requestAnimationFrame()会告诉浏览器，你需要执行动画，
    # 并请求浏览器调用指定的函数在下一次重绘之前更新动画。
    # requestAnimationFrame()使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用
```
```
animate()

function animate(){
    // 清除屏幕
    content.clearRect(0,0,WIDTH,HEIGHT)

    for(var i in round){
        round[i].move()
    }
    requestAnimationFrame(animate)
}
```
· 创建`move()`
```
由于我们要实现一个匀速上升的动画，所以在move()中我们要完成y坐标的改变，并且设置边界条件

当y坐标的值小于-10（或其他负值），则代表round已经超出了屏幕，这个时候我们要将其移动到屏幕的最底端，也就是重置位置，这样才可以保证我们屏幕的粒子数不变

总结起来就是：一个粒子不断上升，到达顶部的时候再移动到最底部的循环过程
```
```
move()

Round_item.prototype.move = function(){
    this.y -= 0.15      // 移动
    if(this.y <= -10){
        this.y = HEIGHT + 10     // 到顶重置位置
    }
    this.draw() // 重绘
}
```
· 整体代码
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>匀速上升粒子动画</title>
    <style>
        html, body {
            margin: 0;
            overflow: hidden;
            width: 100%;
            height: 100%;
            background: black;
        }
    </style>
</head>
<body>
<canvas id="canvas"></canvas>

<script>
    var ctx = document.getElementById('canvas'),
        content = ctx.getContext('2d'),
        round = [],
        WIDTH,
        HEIGHT,
        initRoundPopulation = 80;


    WIDTH = document.documentElement.clientWidth;
    HEIGHT = document.documentElement.clientHeight;

    ctx.width = WIDTH;
    ctx.height = HEIGHT;

    function Round_item(index, x, y) {
        this.index = index;
        this.x = x;
        this.y = y;
        this.r = Math.random() * 2 + 1;
        var alpha = (Math.floor(Math.random() * 10) + 1) / 10 / 2;
        this.color = "rgba(255,255,255," + alpha + ")";
    }

    Round_item.prototype.draw = function () {
        content.fillStyle = this.color;
        content.shadowBlur = this.r * 2;
        content.beginPath();
        content.arc(this.x, this.y, this.r, 0, 2 * Math.PI, false);
        content.closePath();
        content.fill();
    };

    function animate() {
        content.clearRect(0, 0, WIDTH, HEIGHT);

        for (var i in round) {
            round[i].move();
        }
        requestAnimationFrame(animate)
    }

    Round_item.prototype.move = function () {
        this.y -= 0.15;
        if (this.y <= -10) {
            this.y = HEIGHT + 10;
        }
        this.draw();
    };


    function init() {
        for (var i = 0; i < initRoundPopulation; i++) {
            round[i] = new Round_item(i, Math.random() * WIDTH, Math.random() * HEIGHT);
            round[i].draw();
        }
        animate();

    }

    init();
</script>
</body>
</html>
```

### 实现鼠标与屏幕互动动画
****
```
效果概述：
    鼠标移动，经过的地方创建一个圆，圆的半径大小由小变大，达到某个固定大小时该圆消失，圆的颜色随机变化
```
#### 创建Canvas元素（全屏）
```
var canvas = document.getElementById('canvas'),
    ctx = canvas.getContext('2d'),
    WIDTH = canvas.width = document.documentElement.clientWidth,
    HEIGHT = canvas.height = document.documentElement.clientHeight,
    para = {
        num: 100,
        color: false,    //  颜色  如果是false 则是随机渐变颜色
        r: 0.9,          //   圆每次增加的半径 
        o: 0.09,         //      判断圆消失的条件，数值越大，消失的越快
        a: 1
    },
    color,
    color2,
    round_arr = [];     // 存放圆的数组 
```
#### 监听`onmousemove`事件
```
需求：在鼠标移动的过程中，不断在鼠标滑过的位置产生一个逐渐变大的圆

Canvas中创建动画的方式就是不断的清除屏幕然后重绘

由于移动的轨迹是由一个个圆构成，那我们就应该使用数组存储圆的信息（xy坐标，半径），然后在鼠标移动的时候将鼠标的位置信息存放在数组中

所以监听onmousemove事件就是为了拿到鼠标的信息
```
```
window.onmousemove = function(event) {
    Xmouse = event.clientX  // 当前在屏幕的x位置
    Ymouse = event.clientY  // 当前在屏幕的y位置

    // 将信息存入圆数组
    round_arr.push({
        Xmouse:Xmouse,
        Ymouse:Ymouse,
        r:para.r
        o:1
    })
}
```
· 设置`color
```
在onmousemove中，我们已经将坐标信息和半径存入round_arr圆数组中，接下来就设置颜色了

在para对象里，默认的color是false，说明圆的颜色是随机的，如果color不为false，则圆的颜色就为color的颜色
```
```
if(para.color){
    color2 = para.color
}else{
    color = Math.random() * 360
}
```
```
那么如何设置颜色渐变呢？
if (!para.color) {
    color += .1;
    color2 = 'hsl(' + color + ',100%,80%)';
}

如果要让颜色变，则要将颜色改变的代码放在一个一直执行的函数
```
· `animate()`函数
```
animate()负责动画的实现，我们就是在该函数里写动画
```
```
完整animate函数代码如下：
function animate() {

    if (!para.color) {         # 设置颜色
        color += .1
        color2 = 'hsl(' + color + ',100%,80%)'
    }

    ctx.clearRect(0, 0, WIDTH, HEIGHT)      # 清除屏幕

    for (var i = 0; i < round_arr.length; i++) {

        ctx.fillStyle = color2
        ctx.beginPath()
        ctx.arc( round_arr[i].Xmouse ,round_arr[i].Ymouse,round_arr[i].r,0, Math.PI * 2)        # 画圆
        ctx.closePath()
        ctx.fill()
        round_arr[i].r += para.r    # 增大半径
        round_arr[i].o -= para.o    # 消失快慢

        if( round_arr[i].o <= 0){       # 移除圆
            round_arr.splice(i,1)
            i--
        }
    }

    window.requestAnimationFrame(animate)
}

```
