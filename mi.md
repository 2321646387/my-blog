## 1.项目搭建

#### base.css：公共样式

```css
.clearfix::before,
.clearfix::after{
    content: '';
    display: table;
    clear: both;
}

/* 去除a的下划线 */
a{
    text-decoration: none;
    color: #333;
}

body{
    font:14px/1.5 Helvetica Neue,Helvetica,Arial,Microsoft Yahei,Hiragino Sans GB,Heiti SC,WenQuanYi Micro Hei,sans-serif;
    color: #333;
   	/*min-width 最小宽度，，在开发中思想重要*/
    /*不让容器过小让内容溢出*/
    min-width: 1226px;
}

/* 设置一个类，用来表示中间容器的宽度 */
.w{
    /* 固定容器的宽度 */
    width: 1226px;
    /* 设置容器居中 */
    margin: 0 auto;
}
```

## 2.	顶部导航条

- 购物车iconfont：阿里妈妈iconfont

- 因为右侧导航条和购物车都要向右浮动，所以购物车先浮动，才会在右侧导航条右侧 

- 购物车hover状态必须绑定给ul而不是li或者a标签。因为移入后有弹出层，防止弹出鼠标移入弹出层，，弹出层消失，，给他们绑定公共的父元素 

- hover状态检查：控制台----->Element----->右键----->force state----->:hover

------



### ·下拉二维码

<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210506163720146.png" alt="image-20210506163720146" style="zoom: 67%;" />

##### Q:文字被挤到外面的原因？

<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210506163823342.png" alt="image-20210506163823342" style="zoom: 80%;" />	继承了line-height，有40px的行高，将qrcode的行高变成1px

------



### ·小三角生成原理

<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210506165306038.png" alt="image-20210506165306038" style="zoom:50%;" />边框相交的地方是斜的，将宽高都设为0，border有10px，将上，左，右的颜色设为透明，就得到一个倒三角形<img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210506165624721.png" alt="image-20210506165624721" style="zoom: 33%;" />

```css
 .box1{
            width: 0px;
            height: 0px;
        
            border: 10px red solid；

            border-color: transparent transparent blue transparent
        }
```

- 用a的伪类来设置

##### 设置绝对定位水平居中

（倒三角相对于.app）

```css
		left: 0;
          right: 0;
          margin: auto;
```

##### 再次将二维码放出来的效果

![image-20210506215903167](C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210506215903167.png)因为太大了将上面撑开了，也同时给.qrcode添加绝对定位，left:-40px解决

### ·添加过度效果

none不可见，且不占位置。hidden是不可见，占位置。但是元素开启了绝对定位，脱离文档流，就不占位置了，相当于bfc

```css
.qrcode{
    height: 0;
 	overflow: hidden;
} 
```

## 3.头部导航条

### 	·logo

过渡三要素：起始位置、过度时间、终止位置

> ------>所以设置过渡动画时，position:absolute虽然默认left为0，但也要设置----->起始位置

背景：backgro-position:center

> logo水平切换home：小房子从左边滑入，因为设置a标签display：block，导致两个图片垂直排列----->

> 直接给定位absolute，然后将宽高给父元素logo；此时只能看见一个logo，因为此时两个图片处于重叠状态------>

> 给home设置单独的样式，，让它往左移，然后给logo标签设置overflow

- 给h1标题给title：小米，这样爬索引擎可以检索到
- 给内容“小米官网”---》同样是为了让搜索引擎看到，字显示出来了，-----》
  - text-indent:-9999px----->常用隐藏文字的手段

------

### 	·中间部分

| 首页导航                                                     | 商品详情页导航                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210510095514583.png" alt="image-20210510095514583" style="zoom:50%;" /> | <img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210510095421863.png" alt="image-20210510095421863" style="zoom:33%;" /> |
| 都是用的同一个样式，只不过首页的全部商品分类隐藏了，所以要给全部的宽度 | 细节                                                         |

但是不能直接给第一个diaplay:none ,因为这样不占位，右侧元素左移了----》

```css
visibility:hidden
```

------

- hover后的下拉层

  针对header-wrapper定位，因为宽度和它一样

  <img src="C:\Users\zyy\AppData\Roaming\Typora\typora-user-images\image-20210510142044974.png" alt="image-20210510142044974" style="zoom: 33%;" />

  将下拉层的css样式放在li前面，因为li层级比info的层级高会覆盖掉

  > tips：有时候模块消失：考虑层级是不是不够，或者是否需要加宽高

  ```css
   // 必须设置宽度，否则不会显示
            width: 100%;
            top: 100px;
            left: 0;
            box-shadow: 0 2px 1px rgba(0,0,0,.3);
  ```

  调下拉框阴影：0 ，不加2px会导致上下都会有阴影，只需要下面有效果。

  - 考虑：goods-info 与li是   兄弟关系，，，怎么通过li找到goods-info

  - | 选择下面所有兄弟 | 选择下一个兄弟 |
    | ---------------- | -------------- |
    | 兄~弟            | 兄+弟          |

```css
&:hover~.goods-info{
            height: 228px;
            border-top: 1px solid rgb(224, 224, 224);
            box-shadow: 0 2px 1px rgba(0,0,0,.3);
          }
```

​		但是此时hover到弹出层，弹出层就消失。。解决：

```css
li:hover~.goods-info,
          .goods-info:hover{}
```

​	hover时不想让第一个和最后两个显示弹出层

### ·搜索框部分

> - input元素、button元素都属于行内块元素，行内块很多特点与文字相似，换行会有缝隙

​		input存在默认的padding，写的时候要把取消掉

```css
 	.search-ipt:focus,
      .search-ipt:focus+button {
        border-color: #ff6700;
      }
```

## 4.左侧导航条

​	看似是中间banner部分，其实在另外页面属于“所有商品”的下拉页面，所以不能直接写给中间，还是属于头部

## 5.轮播图

### 	·轮播图

​		本身宽度为1226px，图片1920px--->设置图片宽度100%就和父元素的宽度一样

​		banner必须继承w的样式，

​		给li加了绝对定位，所有图片就会重合在一起，但是此时宽度发生变化，因为此时相对于。。。。定位，给banner添加相对定位以后就恢复；

> ​		vertical-align : top------>垂直对齐一幅图像

### 	·导航小圆点

​		首先加绝对定位，否则看不见，

> ​		float : left 样式必须要设置给a，否则会重叠。。。
>
> border-radius：50%----》圆形

```css
.pointer {
      position: absolute;

      a {
        float: left;
        width: 5px;
        height: 5px;
        border: 3px blue solid;
        border-radius: 50%;
        background-color: red;
      }
    }
```

​		要给pointer定位到banner右下角，bottom：0，right：0；

​		但是此时因为banner内容里只有图片而图片设置了绝对定位脱离了文档流，导致banner没有高度，所以pointer不能定位到右下角，，，所以必须给banner设置高度

### 	·两侧箭头

​		雪碧图，，，要用a标签，

​		给a标签加绝对定位，然后background-url用雪碧图，然后在各个的class里面用background-position定位到相应的位置，然后hover

## 6.右侧工具条

> right：50%
>
> ​	 /* 
>
> ​    布局的等式
>
>  left + margin-left + width + margin-right + right = 视口的宽度
>
> auto + 0 + 26 + 0 + 60 = 视口宽度
>
>   auto + 0 + 26 + -639px + 50% = 视口宽度
>
>   */

1226/2+26=639

## 7.下部广告部分

​	左侧i和a，i是a的子元素，导致高度折叠问题---》开启bfc：-----》给父元素加overflow：hidden

### 	边框：

​		上边框用before，左边框用after

​		设置条条居中显示

```css
	left: 0;
    right: 0;
    top: 0;
    margin: 0 auto
```

