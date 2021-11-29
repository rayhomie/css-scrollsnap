# css 滚动贴合 ScrollSnap

参考资料：

[2 行 CSS 实现滚动贴合 Scroll Snap 特效](https://www.bilibili.com/video/BV1R3411r79d)

## [scroll-snap-type](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scroll-snap-type)

```css
/*滚动方向x轴，任何时候都强制滚动*/
scroll-snap-type: x mandatory;

/*滚动方向y轴，接近容器边缘时才滚动*/
scroll-snap-type: y proximity;

/*滚动方向任意，强制滚动*/
scroll-snap-type: both mandatory;
```

语法：

```
none | [ x | y | block | inline | both ] [ mandatory | proximity ]?
```

- `mandatory`

  如果它当前没有被滚动，这个滚动容器的可视视图将静止在临时点上。意思是当滚动动作结束，如果可能，它会临时在那个点上。如果内容被添加、移动、删除或者重置大小，滚动偏移将被调整为保持静止在临时点上。

- `proximity`

  如果它当前没有被滚动，这个滚动容器的可视视图将基于基于用户代理滚动的参数去到临时点上。如果内容被添加、移动、删除或者重置大小，滚动偏移将被调整为保持静止在临时点上。

### 需要把滚动条设置在直接父容器上，`scroll-snap-type`才能生效：

下图是滚动条在 body 上而非直接父容器上，导致 scroll-snap-type 失效

![非父容器滚动](https://img-blog.csdnimg.cn/d322a819b97e491ca618ad0cd479e413.gif)

下图是在直接父容器中使用`overflow:scroll`使得 scroll-snap 生效

![生效scroll-snap](https://img-blog.csdnimg.cn/af79e74839aa4cada14a2c9320ac685f.gif)

## scroll-snap-align

`scroll-snap-align: center`

![center](https://img-blog.csdnimg.cn/f50e33ecd092419782d6b6a1c328c537.gif)

`scroll-snap-align: start`

![start](https://img-blog.csdnimg.cn/6aa5fcc5d70949a88c7edd2c05c015bd.gif)

`scroll-snap-align: end`

![end](https://img-blog.csdnimg.cn/70862e6b88ce40e3866c57f901e19470.gif)

源码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>css-scrollsnap</title>
  </head>
  <style>
    body {
      margin: 0;
    }

    iframe {
      border: none;
      width: 80%;
      height: 80%;
    }

    section {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      font-size: 20px;
    }

    section:nth-child(odd) {
      background: paleturquoise;
    }

    section:nth-child(even) {
      background: palevioletred;
    }
  </style>

  <style>
    main {
      scroll-snap-type: y mandatory;
      /* scroll-snap-type: y proximity; */
      /* 需要把滚动条设置在直接父容器上，
    scroll-snap-type才能生效，
    默认是body上，现在使用了overflow:scroll滚动条在main上了
    */
      overflow-y: scroll;
      height: 100vh;
    }

    section {
      width: 100vw;
      height: 80vh;
      scroll-snap-align: center;
      /* scroll-snap-align: start; 容器顶部和元素顶部对齐 */
      /* scroll-snap-align: end; 容器底部和元素底部对齐 */
    }
  </style>

  <body>
    <main>
      <section>1</section>
      <section>2</section>
      <section>3</section>
      <section>4</section>
      <section>5</section>
    </main>
  </body>
</html>
```

## [scroll-padding](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-padding)

用于有 sticky 元素时的滚动内边距。

但我们容器中有一个 sticky 元素时，直接父级容器元素不使用 scroll-padding，则会将 sticky 元素的顶部作为滚动临界线，效果如下：

![不用scroll-padding](https://img-blog.csdnimg.cn/44eeaf55bc044ba28ea8b42a283ec6ea.gif)

当我们使用 scroll-padding 时，则会将 sticky 元素的底部作为滚动临界线，并达到预期效果。

![用scroll-padding](https://img-blog.csdnimg.cn/db7d7cffa88e4b27a64fedb65f4ed8ab.gif)

源码：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>scroll-padding</title>
  </head>
  <style>
    body {
      margin: 0;
    }

    h1 {
      margin: 0;
    }

    section {
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 20px;
    }

    section:nth-child(odd) {
      background: paleturquoise;
    }

    section:nth-child(even) {
      background: palevioletred;
    }
  </style>

  <style>
    main {
      scroll-snap-type: y mandatory;
      overflow-y: scroll;
      height: 100vh;
      scroll-padding: 50px;
    }

    section {
      width: 100vw;
      height: 80vh;
      scroll-snap-align: start;
    }

    h1 {
      background: white;
      height: 50px;
      position: sticky;
      top: 0;
    }
  </style>

  <body>
    <main>
      <h1>scroll-padding</h1>
      <section>1</section>
      <section>2</section>
      <section>3</section>
      <section>4</section>
      <section>5</section>
    </main>
  </body>
</html>
```
