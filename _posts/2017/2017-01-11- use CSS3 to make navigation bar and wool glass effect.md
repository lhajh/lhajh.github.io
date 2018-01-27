---
layout: post
title: 使用CSS3制作导航条和毛玻璃效果
categories: CSS
description: 使用CSS3制作导航条和毛玻璃效果
keywords: CSS3, 导航条, 毛玻璃
---

导航条对于每一个Web前端攻城狮来说并不陌生，但是毛玻璃可能会相对陌生一些。简单的说，毛玻璃其实就是让图片或者背景使用相应的方法进行模糊处理。这种效果对用户来说是十分具有视觉冲击力的。

把导航条和毛玻璃效果在一篇文章中分享其实是有原因的。因为这两个效果的实现离不开一个重要的思想。

**用语言来描述就是：父元素设置position:relative，其伪元素（after 或者before）设置 position:absolute，并且让 top,bottom,left,right 都为 0，伪元素占满父元素的整个空间，最后设置 z-index 将背景放在父元素后边。**

具体代码如下：
```
.container {
  position: relative;
}

.container::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
　z-index: -1;
}
```

什么意思呢？稍安勿躁，我会在接下来的两个实战例子中对这段代码的意思一一道来。

## 1. 导航条

### 1.1 平行四边形导航条

平行四边形制作的思想：平行四边形的制作运用了 CSS3 2D 变形中的 skew() 倾斜属性，因为我们只是在水平方向上倾斜，所以在使用 skew() 时需要将第二个参数指定为 0，否则 x，y 轴方向都会发生倾斜，这并不是我们想要的效果。或者是使用 skewX()。具体的代码实现如下。
```
css:

  <style type="text/css">
    ul {
      list-style: none;
    }
    .quadrangle li {
      position: relative;
      display: inline-block;
      width: 100px;
      height: 50px;
      line-height: 50px;
      color: #fff;
      text-align: center;
      cursor: pointer;
    }
    .quadrangle li::after {
      content: '';
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      top: 0;
      z-index: -1;
      border-radius: 5px;
      background: #2175BC;
      -moz-transform: skewX(-25deg);
      -ms-transform: skewX(-25deg);
      -webkit-transform: skewX(-25deg);
      transform: skewX(-25deg);
    }
    .quadrangle li:hover::after {
      background: #2586D7;
    }
  </style>

html:

  <div class="quadrangle">
    <ul>
      <li>Home</li>
      <li>Blog</li>
      <li>Source</li>
      <li>Bookmark</li>
      <li>About</li>
    </ul>
  </div>
```
上面代码中，只显示了部分重要部分。在设置平行四边形的时候需要注意以下几点：

1. 给 li 元素设置 relative，然后伪元素 after 设置 absolute 和 LRBT 四个方向的定位。原因在于我们需要让伪元素相对与 li 元素定位，并且让伪元素填满整个 li 元素的空间，这样的话给伪元素设置的背景就会铺满整个 li 元素 。最重要的是，在伪元素上设置 skewX()，只会对伪元素进行倾斜，并不会对父元素上的文字进行倾斜。

2. 设置 z-index:-1。这里如果不设置 z-index 值为负值的话，就看不到在 li 元素里面的文字了，因为 absolute 会提高自身元素的层级，所以要让伪元素 z-index 为-1，让其的层级位居 li 元素之后。

3. 使用 skewX() 函数让 伪元素（不是 li 元素） 元素旋转 25 度，注意写上属性前缀，防止浏览器兼容性问题。

4. 将伪元素和伪类结合使用的时候，必须要注意的是先伪类，再伪元素。如果是`li::after:hover` 这样设置的话是没有任何效果的。正确的写法：`li:hover::after`。

示例效果如下：

![](/assets/images/posts/css/e54GE.png)

hover 状态：

![](/assets/images/posts/css/qE8g4d.png)

## 1.2 梯形导航条

梯形导航条的是实现思想：梯形导航条使用了 CSS3 3D 变形中的三个属性：perspective()，rotateX() 和 transform-origin。

perspective() 是用于设置用户和元素 3D 空间 Z 平面之间的距离，值越小，用户与 3D 空间 Z 平面距离越近，视觉效果会明显；反之，值越大，用户与 3D 空间 Z 平面距离越远，视觉效果越小。

ratateX() 是用于 3D 空间中 x 轴的旋转，大家可以想象一下在高中时期学的空间直角坐标系，跟那个 x 轴的旋转是一样的道理。

transform-origin 是用于指定元素的旋转中心点位置。

具体属性的使用方法可以去查阅相关文档，这里就不再赘述了。具体代码实现如下：
```
css:

  <style type="text/css">
    ul {
      list-style: none;
    }
    .trapezoid li {
      position: relative;
      display: inline-block;
      width: 100px;
      height: 50px;
      line-height: 65px;
      color: #fff;
      text-align: center;
      cursor: pointer;
    }
    .zIndex {
      z-index: 1;
    }
    .trapezoid li::after {
      content: '';
      position: absolute;
      top: 0;
      bottom: 0;
      left: -15px;
      right: -15px;
      z-index: -1;
      border-radius: 10px 10px 0 0;
      border: 1px solid #fff;
      background: #2175BC;
      -moz-transform: perspective(0.5em) rotateX(5deg);
      -ms-transform: perspective(0.5em) rotateX(5deg);
      -webkit-transform: perspective(0.5em) rotateX(5deg);
      transform: perspective(0.5em) rotateX(5deg);
      -moz-transform-origin: bottom;
      -webkit-transform-origin: bottom;
      transform-origin: bottom;
    }
    .trapezoid li:hover::after {
      background: #3B9BE5;
    }
  </style>

html:

  <div class=" trapezoid" id="trapezoid">
    <ul>
      <li class="zIndex">Home</li>
      <li>Blog</li>
      <li>Source</li>
      <li>Bookmark</li>
      <li>About</li>
    </ul>
  </div>

js:

  <script>
    window.onload = function () {
      var trp = document.getElementById('trapezoid')
      var oli = trp.getElementsByTagName('li')
      for (var i = 0; i < oli.length; i++) {
        oli[i].onclick = function() {
          for (var j = 0; j < oli.length; j++) {
            oli[j].className = ''
          }
          this.className = 'zIndex'
        }
      }
    }
  </script>
```
上面代码中，只显示重要部分。注意以下几个问题：

1. 前四个问题与平行四边形导航条的制作思路基本相同。其中，在伪元素上设置 perspective() 和 rotateX()，只会对伪元素进行 3D 处理和在空间中 X 轴的旋转，并不会对父元素上的文字进行任何的处理。文字还是会按照默认效果显示。如果在父元素上设置 perspective() 和 rotateX()，则会影响之后的所有子元素。也就是所有的子元素（包括文字）都会进行旋转。文字被旋转了，阅读十分困难的。这个逻辑应该不难理解。

2. 用于控制梯形是左倾斜还是右倾斜的属性是 transform-origin。梯形不倾斜：bottom。左倾斜：bottom left；右倾斜：bottom right。

示例效果如下：

![](/assets/images/posts/css/y4b3F.png)

点击状态：

![](/assets/images/posts/css/a34Rt34.png)


## 2. 毛玻璃效果

毛玻璃的实现思想：毛玻璃使用了 CSS3 中的 backgroung-size，fiter 滤镜的原理。

background-size 属性用于指定背景图片的尺寸，其中的一个参数 cover 是将背景图片放大，以适合铺满整个容器。但是这个属性使用的前提是需要设定一张足够大尺寸的图片，否则会导致背景图片失真。

fiter 滤镜中的 blur() 用于将图片进行高斯模糊处理，只接受单位值，值越大，模糊效果越明显。

在张鑫旭老师的一篇关于毛玻璃实现的文章中（会在参考文章中给出链接），给出了毛玻璃实现的效果，可是有一些小问题：如果在背景图片上加上文字，blur() 会将文字一起模糊掉，这样的话会用户体验不太好。当然，在不需要文字的背景图片下，张鑫旭老师的方案还是很棒的。

以下给出具体代码：
```
css:

  <style type="text/css">
    body {
      background: url("./3.jpg") no-repeat center center fixed;
      -moz-background-size: cover;
      -o-background-size: cover;
      -webkit-background-size: cover;
      background-size: cover;
    }
    h1, h2 {
      text-align: center;
    }
    .rascal {
      position: relative;
      background: rgba(255, 255, 255, 0.3);
      overflow: hidden;
    }
    .rascal::after {
      z-index: -1;
      content: '';
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: url("./3.jpg") no-repeat center center fixed;
      -moz-background-size: cover;
      -o-background-size: cover;
      -webkit-background-size: cover;
      background-size: cover;
      -webkit-filter: blur(20px);
      filter: blur(20px);
      margin: -30px;
    }
  </style>

html:

<div class="rascal">
  <h1>王羲之</h1>
  <p>王羲之（303年—361年，一作321年—379年），字逸少，汉族，东晋时期著名书法家，有“书圣”之称。琅琊临沂（今山东临沂）人，后迁会稽山阴（今浙江绍兴），晚年隐居剡县金庭。历任秘书郞、宁远将军、江州刺史，后为会稽内史，领右将军。其书法兼善隶、草、楷、行各体，精研体势，心摹手追，广采众长，备精诸体，冶于一炉，摆脱了汉魏笔风，自成一家，影响深远。风格平和自然，笔势委婉含蓄，遒美健秀。代表作《兰亭序》被誉为“天下第一行书”。在书法史上，他与其子王献之合称为“二王”。</p>
  <p>王羲之的《兰亭集序》为历代书法家所敬仰，被誉作“天下第一行书”。王兼善隶、草、楷、行各体，精研体势，心摹手追，广采众长，备精诸体，冶于一炉，摆脱了汉魏笔风，自成一家，影响深远。其书法平和自然，笔势委婉含蓄，遒美健秀，世人常用曹植的《洛神赋》中：“翩若惊鸿，婉若游龙，荣曜秋菊，华茂春松。仿佛兮若轻云之蔽月，飘飖兮若流风之回雪。”一句来赞美王羲之的书法之美。</p>
  <h2>《兰亭集序》</h2>
  <p>永和九年，岁在癸丑，暮春之初，会于会稽山阴之兰亭，修禊事也。群贤毕至，少长咸集。此地有崇山峻岭，茂林修竹，又有清流激湍，映带左右，引以为流觞曲水，列坐其次。虽无丝竹管弦之盛，一觞一咏，亦足以畅叙幽情。</p>
  <p>是日也，天朗气清，惠风和畅。仰观宇宙之大，俯察品类之盛，所以游目骋怀，足以极视听之娱，信可乐也。</p>
  <p>夫人之相与，俯仰一世。或取诸怀抱，悟言一室之内；或因寄所托，放浪形骸之外。虽趣舍万殊，静躁不同，当其欣于所遇，暂得于己，快然自足，不知老之将至；及其所之既倦，情随事迁，感慨系之矣。向之所欣，俯仰之间，已为陈迹，犹不能不以之兴怀，况修短随化，终期于尽！古人云：“死生亦大矣。”岂不痛哉！</p>
  <p>每览昔人兴感之由，若合一契，未尝不临文嗟悼，不能喻之于怀。固知一死生为虚诞，齐彭殇为妄作。后之视今，亦犹今之视昔，悲夫！故列叙时人，录其所述，虽世殊事异，所以兴怀，其致一也。后之览者，亦将有感于斯文。</p>
</div>
```
上面代码中，需要注意几个问题：

1. 同样这里也是使用父元素 relative，伪元素 absolute 的方法，并且设置了 TBLR 和 z-index。使用这种方法的关键之处在于我们是对伪元素进行了 blur() 处理，这样并不会影响到父元素中的文字效果。

2. 需要给背景图片添加 background-size 属性，这个是为了让图片自适应整个屏幕的宽度。另外，这个属性需要添加两次。一是在 body 元素上，一是在伪元素上。在伪元素上添加的原因是我们要让 blur() 处理模糊的图片与背景图片相同。如果在伪元素中给 background 设置 inherit 的话，只会继承父容器 rascal 的背景，而 rascal 容器是一个白色背景的容器，这样就与我们的效果不相同了。下图是在伪元素中使用 `background:inherit;` 的毛玻璃效果。  
![](/assets/images/posts/css/li6De2.png)  
这并不是我们想要的毛玻璃效果。所以伪元素上 background 的设置应该与背景图片是相同的。

3. 在为伪元素设置正确的 background 之后，我们要使用 margin 负值模糊边缘消退的问题。  
![](/assets/images/posts/css/wet5u.png)  
可以看到，毛玻璃中的blur()效果有点过犹不及了，一圈模糊效果超出了容器，给父元素设置overflow:hidden，可以将超出的部分剪切掉。最终的示例效果如下。  
![](/assets/images/posts/css/op4F3.png)  
最终效果看起来就很自然了。

## 3. 结束语

三个实例中，有一个共同的思想：将 CSS3 的倾斜，透视，旋转和滤镜效果都放在伪元素中，并且给父元素设置 relative，伪元素设置 absolute，让伪元素的宽度和高度撑满父元素的整个区域，最后设置伪元素的 z-index 为负值。这样做的好处就是不会影响父容器中的文字。

具体的代码如下：
```
.container {
  position: relative;
}

.container::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
　z-index: -1;
}
```


## 4. 参考文章

张鑫旭 - [使用CSS将图片转换成模糊(毛玻璃)效果](http://www.zhangxinxu.com/wordpress/2013/11/css-svg-image-blur/)

阮一峰 - [高斯模糊的算法](http://www.ruanyifeng.com/blog/2012/11/gaussian_blur.html)
