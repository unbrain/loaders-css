# [loaders.css](https://github.com/ConnorAtherton/loaders.css) 源码解读

`loader.css` 有许多的加载动态效果，我打算解读一下来深入对 `CSS` 的了解

[样式预览](https://unbrain.github.io/loaders-css/index.html)

对兼容性的代码大多已删除，只留在重要部分。

例如：

```css
/* 原本是：*/
0% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 1; }
/* 修改为：*/
0% {
    transform: scale(1);
    opacity: 1; }
```

---

## 球脉冲

- 一个球缩小放大加载效果
- infinite 无限循环
- animation-fill-mode  这个 CSS 属性用来指定在动画执行之前和之后如何给动画的目标应用样式。其实我把它注释后没变化。
- cubic-bezier 又称三次贝塞尔，主要是为 animation 生成速度曲线的函数 

---

### 知识点：

1. [transform](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)
2. [animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)
3. [:nth-child()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child)
4. [animation-fill-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-fill-mode)

```css
@keyframes scale {
  0% {
    transform: scale(1);
    opacity: 1; }
  45% {
    transform: scale(0.1);
    opacity: 0.7; }
  80% {
    transform: scale(1);
    opacity: 1; } }

.ball-pulse > div:nth-child(0) {
  animation: scale 0.75s -0.36s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.ball-pulse > div:nth-child(1) {
  animation: scale 0.75s -0.24s infinite }

.ball-pulse > div:nth-child(2) {
  animation: scale 0.75s -0.12s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.ball-pulse > div:nth-child(3) {
  animation: scale 0.75s 0s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.ball-pulse > div {
  background-color: #000;
  width: 15px;
  height: 15px;
  border-radius: 100%;
  margin: 2px;
  animation-fill-mode: both;
  display: inline-block; }
```

---

## 弹跳加载

- CSS transform 属性允许你修改CSS视觉格式模型的坐标空间。使用它，元素可以被转换（translate）、旋转（rotate）、缩放（scale）、倾斜（skew）。
- ease-in-out 规定以慢速开始和结束的过渡效果 


---

### 知识点

同上

```css

@keyframes ball-pulse-sync {
  33% {
    transform: translateY(10px); }
  66% {
    transform: translateY(-10px); }
  100% {
    transform: translateY(0); } }

.ball-pulse-sync > div:nth-child(0) {
  animation: ball-pulse-sync 0.6s -0.21s infinite ease-in-out; }

.ball-pulse-sync > div:nth-child(1) {
  animation: ball-pulse-sync 0.6s -0.14s infinite ease-in-out; }

.ball-pulse-sync > div:nth-child(2) {
  animation: ball-pulse-sync 0.6s -0.07s infinite ease-in-out; }

.ball-pulse-sync > div:nth-child(3) {
  animation: ball-pulse-sync 0.6s 0s infinite ease-in-out; }

.ball-pulse-sync > div {
  background-color: #000;
  width: 15px;
  height: 15px;
  border-radius: 100%;
  margin: 2px;
  animation-fill-mode: both;
  display: inline-block; }
```

---

## 淡出

- 一个淡出加载效果
- 修改大小缩放从 从 0 到 1
- 修改透明度从 ０ 到 １

```css
@keyframes ball-scale {
  0% {
    transform: scale(0); }
  100% {
    transform: scale(1);
    opacity: 0; } }

.ball-scale > div {
  background-color: #fff;
  width: 15px;
  height: 15px;
  border-radius: 100%;
  margin: 2px;
  animation-fill-mode: both;
  display: inline-block;
  height: 60px;
  width: 60px;
  animation: ball-scale 1s 0s ease-in-out infinite; }
```

---

## 多球淡出

- 一个多球淡出加载效果
- animation name 与上一个相同 只是增加延时来形成视觉差
- 若觉的 div 太多 其实都能用 ：after ：before 来实现 

```css
.ball-scale-random {
  width: 37px;
  height: 40px; }

.ball-scale-random > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    animation-fill-mode: both;
    position: absolute;
    display: inline-block;
    height: 30px;
    width: 30px;
    animation: ball-scale 1s 0s ease-in-out infinite; }

.ball-scale-random > div:nth-child(1) {
  margin-left: -7px;
  animation: ball-scale 1s 0.2s ease-in-out infinite; }

.ball-scale-random > div:nth-child(3) {
  margin-left: -2px;
  margin-top: 9px;
  animation: ball-scale 1s 0.5s ease-in-out infinite; }
```

---

## 球旋转

- 一个球旋转加载效果
- animation 旋转 360
- 用一个 div 展示出 3 个球

---

### 知识点

1. [:after](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::after)
2. [:before](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::before)

```css
@keyframes rotate {
  0% {
    transform: rotate(0deg); }
  50% {
    transform: rotate(180deg); }
  100% {
    transform: rotate(360deg); } }

.ball-rotate {
  position: relative; }

 .ball-rotate > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    animation-fill-mode: both;
    position: relative; }

.ball-rotate > div:first-child {
      animation: rotate 1s 0s cubic-bezier(0.7, -0.13, 0.22, 0.86) infinite; }

.ball-rotate > div:before, .ball-rotate > div:after {
      background-color: #fff;
      width: 15px;
      height: 15px;
      border-radius: 100%;
      margin: 2px;
      content: "";
      position: absolute;
      opacity: 0.8; }

.ball-rotate > div:before {
      top: 0px;
      left: -28px; }

.ball-rotate > div:after {
      top: 0px;
      left: 25px; }
/*很尴尬的就是在　loaders.css 中有重名的　@keyframes,会有事分析与看到的不一致,就比如在下一个就有另一个 rotate 的　@keyframes*/
```



### 还未看

```css
@keyframes rotate {
  0% {
    -webkit-transform: rotate(0deg) scale(1);
            transform: rotate(0deg) scale(1); }
  50% {
    -webkit-transform: rotate(180deg) scale(0.6);
            transform: rotate(180deg) scale(0.6); }
  100% {
    -webkit-transform: rotate(360deg) scale(1);
            transform: rotate(360deg) scale(1); } }

.ball-clip-rotate > div {
  background-color: #fff;
  width: 15px;
  height: 15px;
  border-radius: 100%;
  margin: 2px;
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  border: 2px solid #fff;
  border-bottom-color: transparent;
  height: 25px;
  width: 25px;
  background: transparent !important;
  display: inline-block;
  -webkit-animation: rotate 0.75s 0s linear infinite;
          animation: rotate 0.75s 0s linear infinite; }

@keyframes rotate {
  0% {
    -webkit-transform: rotate(0deg) scale(1);
            transform: rotate(0deg) scale(1); }
  50% {
    -webkit-transform: rotate(180deg) scale(0.6);
            transform: rotate(180deg) scale(0.6); }
  100% {
    -webkit-transform: rotate(360deg) scale(1);
            transform: rotate(360deg) scale(1); } }

@keyframes scale {
  30% {
    -webkit-transform: scale(0.3);
            transform: scale(0.3); }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1); } }

.ball-clip-rotate-pulse {
  position: relative;
  -webkit-transform: translateY(-15px);
      -ms-transform: translateY(-15px);
          transform: translateY(-15px); }
  .ball-clip-rotate-pulse > div {
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    top: 0px;
    left: 0px;
    border-radius: 100%; }
    .ball-clip-rotate-pulse > div:first-child {
      background: #fff;
      height: 16px;
      width: 16px;
      top: 7px;
      left: -7px;
      -webkit-animation: scale 1s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite;
              animation: scale 1s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite; }
    .ball-clip-rotate-pulse > div:last-child {
      position: absolute;
      border: 2px solid #fff;
      width: 30px;
      height: 30px;
      left: -16px;
      top: -2px;
      background: transparent;
      border: 2px solid;
      border-color: #fff transparent #fff transparent;
      -webkit-animation: rotate 1s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite;
              animation: rotate 1s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite;
      -webkit-animation-duration: 1s;
              animation-duration: 1s; }

@keyframes rotate {
  0% {
    -webkit-transform: rotate(0deg) scale(1);
            transform: rotate(0deg) scale(1); }
  50% {
    -webkit-transform: rotate(180deg) scale(0.6);
            transform: rotate(180deg) scale(0.6); }
  100% {
    -webkit-transform: rotate(360deg) scale(1);
            transform: rotate(360deg) scale(1); } }

.ball-clip-rotate-multiple {
  position: relative; }
  .ball-clip-rotate-multiple > div {
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    left: -20px;
    top: -20px;
    border: 2px solid #fff;
    border-bottom-color: transparent;
    border-top-color: transparent;
    border-radius: 100%;
    height: 35px;
    width: 35px;
    -webkit-animation: rotate 1s 0s ease-in-out infinite;
            animation: rotate 1s 0s ease-in-out infinite; }
    .ball-clip-rotate-multiple > div:last-child {
      display: inline-block;
      top: -10px;
      left: -10px;
      width: 15px;
      height: 15px;
      -webkit-animation-duration: 0.5s;
              animation-duration: 0.5s;
      border-color: #fff transparent #fff transparent;
      -webkit-animation-direction: reverse;
              animation-direction: reverse; }

@-webkit-keyframes ball-scale-ripple {
  0% {
    -webkit-transform: scale(0.1);
            transform: scale(0.1);
    opacity: 1; }
  70% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 0.7; }
  100% {
    opacity: 0.0; } }

@keyframes ball-scale-ripple {
  0% {
    -webkit-transform: scale(0.1);
            transform: scale(0.1);
    opacity: 1; }
  70% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 0.7; }
  100% {
    opacity: 0.0; } }

.ball-scale-ripple > div {
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  height: 50px;
  width: 50px;
  border-radius: 100%;
  border: 2px solid #fff;
  -webkit-animation: ball-scale-ripple 1s 0s infinite cubic-bezier(0.21, 0.53, 0.56, 0.8);
          animation: ball-scale-ripple 1s 0s infinite cubic-bezier(0.21, 0.53, 0.56, 0.8); }

@-webkit-keyframes ball-scale-ripple-multiple {
  0% {
    -webkit-transform: scale(0.1);
            transform: scale(0.1);
    opacity: 1; }
  70% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 0.7; }
  100% {
    opacity: 0.0; } }

@keyframes ball-scale-ripple-multiple {
  0% {
    -webkit-transform: scale(0.1);
            transform: scale(0.1);
    opacity: 1; }
  70% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 0.7; }
  100% {
    opacity: 0.0; } }

.ball-scale-ripple-multiple {
  position: relative;
  -webkit-transform: translateY(-25px);
      -ms-transform: translateY(-25px);
          transform: translateY(-25px); }
  .ball-scale-ripple-multiple > div:nth-child(0) {
    -webkit-animation-delay: -0.8s;
            animation-delay: -0.8s; }
  .ball-scale-ripple-multiple > div:nth-child(1) {
    -webkit-animation-delay: -0.6s;
            animation-delay: -0.6s; }
  .ball-scale-ripple-multiple > div:nth-child(2) {
    -webkit-animation-delay: -0.4s;
            animation-delay: -0.4s; }
  .ball-scale-ripple-multiple > div:nth-child(3) {
    -webkit-animation-delay: -0.2s;
            animation-delay: -0.2s; }
  .ball-scale-ripple-multiple > div {
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    top: -2px;
    left: -26px;
    width: 50px;
    height: 50px;
    border-radius: 100%;
    border: 2px solid #fff;
    -webkit-animation: ball-scale-ripple-multiple 1.25s 0s infinite cubic-bezier(0.21, 0.53, 0.56, 0.8);
            animation: ball-scale-ripple-multiple 1.25s 0s infinite cubic-bezier(0.21, 0.53, 0.56, 0.8); }

@-webkit-keyframes ball-beat {
  50% {
    opacity: 0.2;
    -webkit-transform: scale(0.75);
            transform: scale(0.75); }
  100% {
    opacity: 1;
    -webkit-transform: scale(1);
            transform: scale(1); } }

@keyframes ball-beat {
  50% {
    opacity: 0.2;
    -webkit-transform: scale(0.75);
            transform: scale(0.75); }
  100% {
    opacity: 1;
    -webkit-transform: scale(1);
            transform: scale(1); } }

.ball-beat > div {
  background-color: #fff;
  width: 15px;
  height: 15px;
  border-radius: 100%;
  margin: 2px;
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  display: inline-block;
  -webkit-animation: ball-beat 0.7s 0s infinite linear;
          animation: ball-beat 0.7s 0s infinite linear; }
  .ball-beat > div:nth-child(2n-1) {
    -webkit-animation-delay: -0.35s !important;
            animation-delay: -0.35s !important; }

@-webkit-keyframes ball-scale-multiple {
  0% {
    -webkit-transform: scale(0);
            transform: scale(0);
    opacity: 0; }
  5% {
    opacity: 1; }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 0; } }

@keyframes ball-scale-multiple {
  0% {
    -webkit-transform: scale(0);
            transform: scale(0);
    opacity: 0; }
  5% {
    opacity: 1; }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 0; } }

.ball-scale-multiple {
  position: relative;
  -webkit-transform: translateY(-30px);
      -ms-transform: translateY(-30px);
          transform: translateY(-30px); }
  .ball-scale-multiple > div:nth-child(2) {
    -webkit-animation-delay: -0.4s;
            animation-delay: -0.4s; }
  .ball-scale-multiple > div:nth-child(3) {
    -webkit-animation-delay: -0.2s;
            animation-delay: -0.2s; }
  .ball-scale-multiple > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    left: -30px;
    top: 0px;
    opacity: 0;
    margin: 0;
    width: 60px;
    height: 60px;
    -webkit-animation: ball-scale-multiple 1s 0s linear infinite;
            animation: ball-scale-multiple 1s 0s linear infinite; }

@-webkit-keyframes ball-triangle-path-1 {
  33% {
    -webkit-transform: translate(25px, -50px);
            transform: translate(25px, -50px); }
  66% {
    -webkit-transform: translate(50px, 0px);
            transform: translate(50px, 0px); }
  100% {
    -webkit-transform: translate(0px, 0px);
            transform: translate(0px, 0px); } }

@keyframes ball-triangle-path-1 {
  33% {
    -webkit-transform: translate(25px, -50px);
            transform: translate(25px, -50px); }
  66% {
    -webkit-transform: translate(50px, 0px);
            transform: translate(50px, 0px); }
  100% {
    -webkit-transform: translate(0px, 0px);
            transform: translate(0px, 0px); } }

@-webkit-keyframes ball-triangle-path-2 {
  33% {
    -webkit-transform: translate(25px, 50px);
            transform: translate(25px, 50px); }
  66% {
    -webkit-transform: translate(-25px, 50px);
            transform: translate(-25px, 50px); }
  100% {
    -webkit-transform: translate(0px, 0px);
            transform: translate(0px, 0px); } }

@keyframes ball-triangle-path-2 {
  33% {
    -webkit-transform: translate(25px, 50px);
            transform: translate(25px, 50px); }
  66% {
    -webkit-transform: translate(-25px, 50px);
            transform: translate(-25px, 50px); }
  100% {
    -webkit-transform: translate(0px, 0px);
            transform: translate(0px, 0px); } }

@-webkit-keyframes ball-triangle-path-3 {
  33% {
    -webkit-transform: translate(-50px, 0px);
            transform: translate(-50px, 0px); }
  66% {
    -webkit-transform: translate(-25px, -50px);
            transform: translate(-25px, -50px); }
  100% {
    -webkit-transform: translate(0px, 0px);
            transform: translate(0px, 0px); } }

@keyframes ball-triangle-path-3 {
  33% {
    -webkit-transform: translate(-50px, 0px);
            transform: translate(-50px, 0px); }
  66% {
    -webkit-transform: translate(-25px, -50px);
            transform: translate(-25px, -50px); }
  100% {
    -webkit-transform: translate(0px, 0px);
            transform: translate(0px, 0px); } }

.ball-triangle-path {
  position: relative;
  -webkit-transform: translate(-29.994px, -37.50938px);
      -ms-transform: translate(-29.994px, -37.50938px);
          transform: translate(-29.994px, -37.50938px); }
  .ball-triangle-path > div:nth-child(1) {
    -webkit-animation-name: ball-triangle-path-1;
            animation-name: ball-triangle-path-1;
    -webkit-animation-delay: 0;
            animation-delay: 0;
    -webkit-animation-duration: 2s;
            animation-duration: 2s;
    -webkit-animation-timing-function: ease-in-out;
            animation-timing-function: ease-in-out;
    -webkit-animation-iteration-count: infinite;
            animation-iteration-count: infinite; }
  .ball-triangle-path > div:nth-child(2) {
    -webkit-animation-name: ball-triangle-path-2;
            animation-name: ball-triangle-path-2;
    -webkit-animation-delay: 0;
            animation-delay: 0;
    -webkit-animation-duration: 2s;
            animation-duration: 2s;
    -webkit-animation-timing-function: ease-in-out;
            animation-timing-function: ease-in-out;
    -webkit-animation-iteration-count: infinite;
            animation-iteration-count: infinite; }
  .ball-triangle-path > div:nth-child(3) {
    -webkit-animation-name: ball-triangle-path-3;
            animation-name: ball-triangle-path-3;
    -webkit-animation-delay: 0;
            animation-delay: 0;
    -webkit-animation-duration: 2s;
            animation-duration: 2s;
    -webkit-animation-timing-function: ease-in-out;
            animation-timing-function: ease-in-out;
    -webkit-animation-iteration-count: infinite;
            animation-iteration-count: infinite; }
  .ball-triangle-path > div {
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    width: 10px;
    height: 10px;
    border-radius: 100%;
    border: 1px solid #fff; }
    .ball-triangle-path > div:nth-of-type(1) {
      top: 50px; }
    .ball-triangle-path > div:nth-of-type(2) {
      left: 25px; }
    .ball-triangle-path > div:nth-of-type(3) {
      top: 50px;
      left: 50px; }

@-webkit-keyframes ball-pulse-rise-even {
  0% {
    -webkit-transform: scale(1.1);
            transform: scale(1.1); }
  25% {
    -webkit-transform: translateY(-30px);
            transform: translateY(-30px); }
  50% {
    -webkit-transform: scale(0.4);
            transform: scale(0.4); }
  75% {
    -webkit-transform: translateY(30px);
            transform: translateY(30px); }
  100% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-transform: scale(1);
            transform: scale(1); } }

@keyframes ball-pulse-rise-even {
  0% {
    -webkit-transform: scale(1.1);
            transform: scale(1.1); }
  25% {
    -webkit-transform: translateY(-30px);
            transform: translateY(-30px); }
  50% {
    -webkit-transform: scale(0.4);
            transform: scale(0.4); }
  75% {
    -webkit-transform: translateY(30px);
            transform: translateY(30px); }
  100% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-transform: scale(1);
            transform: scale(1); } }

@-webkit-keyframes ball-pulse-rise-odd {
  0% {
    -webkit-transform: scale(0.4);
            transform: scale(0.4); }
  25% {
    -webkit-transform: translateY(30px);
            transform: translateY(30px); }
  50% {
    -webkit-transform: scale(1.1);
            transform: scale(1.1); }
  75% {
    -webkit-transform: translateY(-30px);
            transform: translateY(-30px); }
  100% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-transform: scale(0.75);
            transform: scale(0.75); } }

@keyframes ball-pulse-rise-odd {
  0% {
    -webkit-transform: scale(0.4);
            transform: scale(0.4); }
  25% {
    -webkit-transform: translateY(30px);
            transform: translateY(30px); }
  50% {
    -webkit-transform: scale(1.1);
            transform: scale(1.1); }
  75% {
    -webkit-transform: translateY(-30px);
            transform: translateY(-30px); }
  100% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-transform: scale(0.75);
            transform: scale(0.75); } }

.ball-pulse-rise > div {
  background-color: #fff;
  width: 15px;
  height: 15px;
  border-radius: 100%;
  margin: 2px;
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  display: inline-block;
  -webkit-animation-duration: 1s;
          animation-duration: 1s;
  -webkit-animation-timing-function: cubic-bezier(0.15, 0.46, 0.9, 0.6);
          animation-timing-function: cubic-bezier(0.15, 0.46, 0.9, 0.6);
  -webkit-animation-iteration-count: infinite;
          animation-iteration-count: infinite;
  -webkit-animation-delay: 0;
          animation-delay: 0; }
  .ball-pulse-rise > div:nth-child(2n) {
    -webkit-animation-name: ball-pulse-rise-even;
            animation-name: ball-pulse-rise-even; }
  .ball-pulse-rise > div:nth-child(2n-1) {
    -webkit-animation-name: ball-pulse-rise-odd;
            animation-name: ball-pulse-rise-odd; }

@-webkit-keyframes ball-grid-beat {
  50% {
    opacity: 0.7; }
  100% {
    opacity: 1; } }

@keyframes ball-grid-beat {
  50% {
    opacity: 0.7; }
  100% {
    opacity: 1; } }

.ball-grid-beat {
  width: 57px; }
  .ball-grid-beat > div:nth-child(1) {
    -webkit-animation-delay: 0.44s;
            animation-delay: 0.44s;
    -webkit-animation-duration: 1.27s;
            animation-duration: 1.27s; }
  .ball-grid-beat > div:nth-child(2) {
    -webkit-animation-delay: 0.2s;
            animation-delay: 0.2s;
    -webkit-animation-duration: 1.52s;
            animation-duration: 1.52s; }
  .ball-grid-beat > div:nth-child(3) {
    -webkit-animation-delay: 0.14s;
            animation-delay: 0.14s;
    -webkit-animation-duration: 0.61s;
            animation-duration: 0.61s; }
  .ball-grid-beat > div:nth-child(4) {
    -webkit-animation-delay: 0.15s;
            animation-delay: 0.15s;
    -webkit-animation-duration: 0.82s;
            animation-duration: 0.82s; }
  .ball-grid-beat > div:nth-child(5) {
    -webkit-animation-delay: -0.01s;
            animation-delay: -0.01s;
    -webkit-animation-duration: 1.24s;
            animation-duration: 1.24s; }
  .ball-grid-beat > div:nth-child(6) {
    -webkit-animation-delay: -0.07s;
            animation-delay: -0.07s;
    -webkit-animation-duration: 1.35s;
            animation-duration: 1.35s; }
  .ball-grid-beat > div:nth-child(7) {
    -webkit-animation-delay: 0.29s;
            animation-delay: 0.29s;
    -webkit-animation-duration: 1.44s;
            animation-duration: 1.44s; }
  .ball-grid-beat > div:nth-child(8) {
    -webkit-animation-delay: 0.63s;
            animation-delay: 0.63s;
    -webkit-animation-duration: 1.19s;
            animation-duration: 1.19s; }
  .ball-grid-beat > div:nth-child(9) {
    -webkit-animation-delay: -0.18s;
            animation-delay: -0.18s;
    -webkit-animation-duration: 1.48s;
            animation-duration: 1.48s; }
  .ball-grid-beat > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    display: inline-block;
    float: left;
    -webkit-animation-name: ball-grid-beat;
            animation-name: ball-grid-beat;
    -webkit-animation-iteration-count: infinite;
            animation-iteration-count: infinite;
    -webkit-animation-delay: 0;
            animation-delay: 0; }

@-webkit-keyframes ball-grid-pulse {
  0% {
    -webkit-transform: scale(1);
            transform: scale(1); }
  50% {
    -webkit-transform: scale(0.5);
            transform: scale(0.5);
    opacity: 0.7; }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 1; } }

@keyframes ball-grid-pulse {
  0% {
    -webkit-transform: scale(1);
            transform: scale(1); }
  50% {
    -webkit-transform: scale(0.5);
            transform: scale(0.5);
    opacity: 0.7; }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1);
    opacity: 1; } }

.ball-grid-pulse {
  width: 57px; }
  .ball-grid-pulse > div:nth-child(1) {
    -webkit-animation-delay: 0.58s;
            animation-delay: 0.58s;
    -webkit-animation-duration: 0.9s;
            animation-duration: 0.9s; }
  .ball-grid-pulse > div:nth-child(2) {
    -webkit-animation-delay: 0.01s;
            animation-delay: 0.01s;
    -webkit-animation-duration: 0.94s;
            animation-duration: 0.94s; }
  .ball-grid-pulse > div:nth-child(3) {
    -webkit-animation-delay: 0.25s;
            animation-delay: 0.25s;
    -webkit-animation-duration: 1.43s;
            animation-duration: 1.43s; }
  .ball-grid-pulse > div:nth-child(4) {
    -webkit-animation-delay: -0.03s;
            animation-delay: -0.03s;
    -webkit-animation-duration: 0.74s;
            animation-duration: 0.74s; }
  .ball-grid-pulse > div:nth-child(5) {
    -webkit-animation-delay: 0.21s;
            animation-delay: 0.21s;
    -webkit-animation-duration: 0.68s;
            animation-duration: 0.68s; }
  .ball-grid-pulse > div:nth-child(6) {
    -webkit-animation-delay: 0.25s;
            animation-delay: 0.25s;
    -webkit-animation-duration: 1.17s;
            animation-duration: 1.17s; }
  .ball-grid-pulse > div:nth-child(7) {
    -webkit-animation-delay: 0.46s;
            animation-delay: 0.46s;
    -webkit-animation-duration: 1.41s;
            animation-duration: 1.41s; }
  .ball-grid-pulse > div:nth-child(8) {
    -webkit-animation-delay: 0.02s;
            animation-delay: 0.02s;
    -webkit-animation-duration: 1.56s;
            animation-duration: 1.56s; }
  .ball-grid-pulse > div:nth-child(9) {
    -webkit-animation-delay: 0.13s;
            animation-delay: 0.13s;
    -webkit-animation-duration: 0.78s;
            animation-duration: 0.78s; }
  .ball-grid-pulse > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    display: inline-block;
    float: left;
    -webkit-animation-name: ball-grid-pulse;
            animation-name: ball-grid-pulse;
    -webkit-animation-iteration-count: infinite;
            animation-iteration-count: infinite;
    -webkit-animation-delay: 0;
            animation-delay: 0; }

@-webkit-keyframes ball-spin-fade-loader {
  50% {
    opacity: 0.3;
    -webkit-transform: scale(0.4);
            transform: scale(0.4); }
  100% {
    opacity: 1;
    -webkit-transform: scale(1);
            transform: scale(1); } }

@keyframes ball-spin-fade-loader {
  50% {
    opacity: 0.3;
    -webkit-transform: scale(0.4);
            transform: scale(0.4); }
  100% {
    opacity: 1;
    -webkit-transform: scale(1);
            transform: scale(1); } }

.ball-spin-fade-loader {
  position: relative;
  top: -10px;
  left: -10px; }
  .ball-spin-fade-loader > div:nth-child(1) {
    top: 25px;
    left: 0;
    -webkit-animation: ball-spin-fade-loader 1s -0.96s infinite linear;
            animation: ball-spin-fade-loader 1s -0.96s infinite linear; }
  .ball-spin-fade-loader > div:nth-child(2) {
    top: 17.04545px;
    left: 17.04545px;
    -webkit-animation: ball-spin-fade-loader 1s -0.84s infinite linear;
            animation: ball-spin-fade-loader 1s -0.84s infinite linear; }
  .ball-spin-fade-loader > div:nth-child(3) {
    top: 0;
    left: 25px;
    -webkit-animation: ball-spin-fade-loader 1s -0.72s infinite linear;
            animation: ball-spin-fade-loader 1s -0.72s infinite linear; }
  .ball-spin-fade-loader > div:nth-child(4) {
    top: -17.04545px;
    left: 17.04545px;
    -webkit-animation: ball-spin-fade-loader 1s -0.6s infinite linear;
            animation: ball-spin-fade-loader 1s -0.6s infinite linear; }
  .ball-spin-fade-loader > div:nth-child(5) {
    top: -25px;
    left: 0;
    -webkit-animation: ball-spin-fade-loader 1s -0.48s infinite linear;
            animation: ball-spin-fade-loader 1s -0.48s infinite linear; }
  .ball-spin-fade-loader > div:nth-child(6) {
    top: -17.04545px;
    left: -17.04545px;
    -webkit-animation: ball-spin-fade-loader 1s -0.36s infinite linear;
            animation: ball-spin-fade-loader 1s -0.36s infinite linear; }
  .ball-spin-fade-loader > div:nth-child(7) {
    top: 0;
    left: -25px;
    -webkit-animation: ball-spin-fade-loader 1s -0.24s infinite linear;
            animation: ball-spin-fade-loader 1s -0.24s infinite linear; }
  .ball-spin-fade-loader > div:nth-child(8) {
    top: 17.04545px;
    left: -17.04545px;
    -webkit-animation: ball-spin-fade-loader 1s -0.12s infinite linear;
            animation: ball-spin-fade-loader 1s -0.12s infinite linear; }
  .ball-spin-fade-loader > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute; }

@-webkit-keyframes ball-spin-loader {
  75% {
    opacity: 0.2; }
  100% {
    opacity: 1; } }

@keyframes ball-spin-loader {
  75% {
    opacity: 0.2; }
  100% {
    opacity: 1; } }

.ball-spin-loader {
  position: relative; }
  .ball-spin-loader > span:nth-child(1) {
    top: 45px;
    left: 0;
    -webkit-animation: ball-spin-loader 2s 0.9s infinite linear;
            animation: ball-spin-loader 2s 0.9s infinite linear; }
  .ball-spin-loader > span:nth-child(2) {
    top: 30.68182px;
    left: 30.68182px;
    -webkit-animation: ball-spin-loader 2s 1.8s infinite linear;
            animation: ball-spin-loader 2s 1.8s infinite linear; }
  .ball-spin-loader > span:nth-child(3) {
    top: 0;
    left: 45px;
    -webkit-animation: ball-spin-loader 2s 2.7s infinite linear;
            animation: ball-spin-loader 2s 2.7s infinite linear; }
  .ball-spin-loader > span:nth-child(4) {
    top: -30.68182px;
    left: 30.68182px;
    -webkit-animation: ball-spin-loader 2s 3.6s infinite linear;
            animation: ball-spin-loader 2s 3.6s infinite linear; }
  .ball-spin-loader > span:nth-child(5) {
    top: -45px;
    left: 0;
    -webkit-animation: ball-spin-loader 2s 4.5s infinite linear;
            animation: ball-spin-loader 2s 4.5s infinite linear; }
  .ball-spin-loader > span:nth-child(6) {
    top: -30.68182px;
    left: -30.68182px;
    -webkit-animation: ball-spin-loader 2s 5.4s infinite linear;
            animation: ball-spin-loader 2s 5.4s infinite linear; }
  .ball-spin-loader > span:nth-child(7) {
    top: 0;
    left: -45px;
    -webkit-animation: ball-spin-loader 2s 6.3s infinite linear;
            animation: ball-spin-loader 2s 6.3s infinite linear; }
  .ball-spin-loader > span:nth-child(8) {
    top: 30.68182px;
    left: -30.68182px;
    -webkit-animation: ball-spin-loader 2s 7.2s infinite linear;
            animation: ball-spin-loader 2s 7.2s infinite linear; }
  .ball-spin-loader > div {
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    background: green; }

@-webkit-keyframes ball-zig {
  33% {
    -webkit-transform: translate(-15px, -30px);
            transform: translate(-15px, -30px); }
  66% {
    -webkit-transform: translate(15px, -30px);
            transform: translate(15px, -30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

@keyframes ball-zig {
  33% {
    -webkit-transform: translate(-15px, -30px);
            transform: translate(-15px, -30px); }
  66% {
    -webkit-transform: translate(15px, -30px);
            transform: translate(15px, -30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

@-webkit-keyframes ball-zag {
  33% {
    -webkit-transform: translate(15px, 30px);
            transform: translate(15px, 30px); }
  66% {
    -webkit-transform: translate(-15px, 30px);
            transform: translate(-15px, 30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

@keyframes ball-zag {
  33% {
    -webkit-transform: translate(15px, 30px);
            transform: translate(15px, 30px); }
  66% {
    -webkit-transform: translate(-15px, 30px);
            transform: translate(-15px, 30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

.ball-zig-zag {
  position: relative;
  -webkit-transform: translate(-15px, -15px);
      -ms-transform: translate(-15px, -15px);
          transform: translate(-15px, -15px); }
  .ball-zig-zag > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    margin-left: 15px;
    top: 4px;
    left: -7px; }
    .ball-zig-zag > div:first-child {
      -webkit-animation: ball-zig 0.7s 0s infinite linear;
              animation: ball-zig 0.7s 0s infinite linear; }
    .ball-zig-zag > div:last-child {
      -webkit-animation: ball-zag 0.7s 0s infinite linear;
              animation: ball-zag 0.7s 0s infinite linear; }

@-webkit-keyframes ball-zig-deflect {
  17% {
    -webkit-transform: translate(-15px, -30px);
            transform: translate(-15px, -30px); }
  34% {
    -webkit-transform: translate(15px, -30px);
            transform: translate(15px, -30px); }
  50% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); }
  67% {
    -webkit-transform: translate(15px, -30px);
            transform: translate(15px, -30px); }
  84% {
    -webkit-transform: translate(-15px, -30px);
            transform: translate(-15px, -30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

@keyframes ball-zig-deflect {
  17% {
    -webkit-transform: translate(-15px, -30px);
            transform: translate(-15px, -30px); }
  34% {
    -webkit-transform: translate(15px, -30px);
            transform: translate(15px, -30px); }
  50% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); }
  67% {
    -webkit-transform: translate(15px, -30px);
            transform: translate(15px, -30px); }
  84% {
    -webkit-transform: translate(-15px, -30px);
            transform: translate(-15px, -30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

@-webkit-keyframes ball-zag-deflect {
  17% {
    -webkit-transform: translate(15px, 30px);
            transform: translate(15px, 30px); }
  34% {
    -webkit-transform: translate(-15px, 30px);
            transform: translate(-15px, 30px); }
  50% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); }
  67% {
    -webkit-transform: translate(-15px, 30px);
            transform: translate(-15px, 30px); }
  84% {
    -webkit-transform: translate(15px, 30px);
            transform: translate(15px, 30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

@keyframes ball-zag-deflect {
  17% {
    -webkit-transform: translate(15px, 30px);
            transform: translate(15px, 30px); }
  34% {
    -webkit-transform: translate(-15px, 30px);
            transform: translate(-15px, 30px); }
  50% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); }
  67% {
    -webkit-transform: translate(-15px, 30px);
            transform: translate(-15px, 30px); }
  84% {
    -webkit-transform: translate(15px, 30px);
            transform: translate(15px, 30px); }
  100% {
    -webkit-transform: translate(0, 0);
            transform: translate(0, 0); } }

.ball-zig-zag-deflect {
  position: relative;
  -webkit-transform: translate(-15px, -15px);
      -ms-transform: translate(-15px, -15px);
          transform: translate(-15px, -15px); }
  .ball-zig-zag-deflect > div {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    margin-left: 15px;
    top: 4px;
    left: -7px; }
    .ball-zig-zag-deflect > div:first-child {
      -webkit-animation: ball-zig-deflect 1.5s 0s infinite linear;
              animation: ball-zig-deflect 1.5s 0s infinite linear; }
    .ball-zig-zag-deflect > div:last-child {
      -webkit-animation: ball-zag-deflect 1.5s 0s infinite linear;
              animation: ball-zag-deflect 1.5s 0s infinite linear; }

/**
 * Lines
 */
@-webkit-keyframes line-scale {
  0% {
    -webkit-transform: scaley(1);
            transform: scaley(1); }
  50% {
    -webkit-transform: scaley(0.4);
            transform: scaley(0.4); }
  100% {
    -webkit-transform: scaley(1);
            transform: scaley(1); } }
@keyframes line-scale {
  0% {
    -webkit-transform: scaley(1);
            transform: scaley(1); }
  50% {
    -webkit-transform: scaley(0.4);
            transform: scaley(0.4); }
  100% {
    -webkit-transform: scaley(1);
            transform: scaley(1); } }

.line-scale > div:nth-child(1) {
  -webkit-animation: line-scale 1s -0.4s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08);
          animation: line-scale 1s -0.4s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.line-scale > div:nth-child(2) {
  -webkit-animation: line-scale 1s -0.3s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08);
          animation: line-scale 1s -0.3s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.line-scale > div:nth-child(3) {
  -webkit-animation: line-scale 1s -0.2s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08);
          animation: line-scale 1s -0.2s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.line-scale > div:nth-child(4) {
  -webkit-animation: line-scale 1s -0.1s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08);
          animation: line-scale 1s -0.1s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.line-scale > div:nth-child(5) {
  -webkit-animation: line-scale 1s 0s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08);
          animation: line-scale 1s 0s infinite cubic-bezier(0.2, 0.68, 0.18, 1.08); }

.line-scale > div {
  background-color: #fff;
  width: 4px;
  height: 35px;
  border-radius: 2px;
  margin: 2px;
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  display: inline-block; }

@-webkit-keyframes line-scale-party {
  0% {
    -webkit-transform: scale(1);
            transform: scale(1); }
  50% {
    -webkit-transform: scale(0.5);
            transform: scale(0.5); }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1); } }

@keyframes line-scale-party {
  0% {
    -webkit-transform: scale(1);
            transform: scale(1); }
  50% {
    -webkit-transform: scale(0.5);
            transform: scale(0.5); }
  100% {
    -webkit-transform: scale(1);
            transform: scale(1); } }

.line-scale-party > div:nth-child(1) {
  -webkit-animation-delay: -0.09s;
          animation-delay: -0.09s;
  -webkit-animation-duration: 0.83s;
          animation-duration: 0.83s; }

.line-scale-party > div:nth-child(2) {
  -webkit-animation-delay: 0.33s;
          animation-delay: 0.33s;
  -webkit-animation-duration: 0.64s;
          animation-duration: 0.64s; }

.line-scale-party > div:nth-child(3) {
  -webkit-animation-delay: 0.32s;
          animation-delay: 0.32s;
  -webkit-animation-duration: 0.39s;
          animation-duration: 0.39s; }

.line-scale-party > div:nth-child(4) {
  -webkit-animation-delay: 0.47s;
          animation-delay: 0.47s;
  -webkit-animation-duration: 0.52s;
          animation-duration: 0.52s; }

.line-scale-party > div {
  background-color: #fff;
  width: 4px;
  height: 35px;
  border-radius: 2px;
  margin: 2px;
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  display: inline-block;
  -webkit-animation-name: line-scale-party;
          animation-name: line-scale-party;
  -webkit-animation-iteration-count: infinite;
          animation-iteration-count: infinite;
  -webkit-animation-delay: 0;
          animation-delay: 0; }

@-webkit-keyframes line-scale-pulse-out {
  0% {
    -webkit-transform: scaley(1);
            transform: scaley(1); }
  50% {
    -webkit-transform: scaley(0.4);
            transform: scaley(0.4); }
  100% {
    -webkit-transform: scaley(1);
            transform: scaley(1); } }

@keyframes line-scale-pulse-out {
  0% {
    -webkit-transform: scaley(1);
            transform: scaley(1); }
  50% {
    -webkit-transform: scaley(0.4);
            transform: scaley(0.4); }
  100% {
    -webkit-transform: scaley(1);
            transform: scaley(1); } }

.line-scale-pulse-out > div {
  background-color: #fff;
  width: 4px;
  height: 35px;
  border-radius: 2px;
  margin: 2px;
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  display: inline-block;
  -webkit-animation: line-scale-pulse-out 0.9s -0.6s infinite cubic-bezier(0.85, 0.25, 0.37, 0.85);
          animation: line-scale-pulse-out 0.9s -0.6s infinite cubic-bezier(0.85, 0.25, 0.37, 0.85); }
  .line-scale-pulse-out > div:nth-child(2), .line-scale-pulse-out > div:nth-child(4) {
    -webkit-animation-delay: -0.4s !important;
            animation-delay: -0.4s !important; }
  .line-scale-pulse-out > div:nth-child(1), .line-scale-pulse-out > div:nth-child(5) {
    -webkit-animation-delay: -0.2s !important;
            animation-delay: -0.2s !important; }

@-webkit-keyframes line-scale-pulse-out-rapid {
  0% {
    -webkit-transform: scaley(1);
            transform: scaley(1); }
  80% {
    -webkit-transform: scaley(0.3);
            transform: scaley(0.3); }
  90% {
    -webkit-transform: scaley(1);
            transform: scaley(1); } }

@keyframes line-scale-pulse-out-rapid {
  0% {
    -webkit-transform: scaley(1);
            transform: scaley(1); }
  80% {
    -webkit-transform: scaley(0.3);
            transform: scaley(0.3); }
  90% {
    -webkit-transform: scaley(1);
            transform: scaley(1); } }

.line-scale-pulse-out-rapid > div {
  background-color: #fff;
  width: 4px;
  height: 35px;
  border-radius: 2px;
  margin: 2px;
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  display: inline-block;
  -webkit-animation: line-scale-pulse-out-rapid 0.9s -0.5s infinite cubic-bezier(0.11, 0.49, 0.38, 0.78);
          animation: line-scale-pulse-out-rapid 0.9s -0.5s infinite cubic-bezier(0.11, 0.49, 0.38, 0.78); }
  .line-scale-pulse-out-rapid > div:nth-child(2), .line-scale-pulse-out-rapid > div:nth-child(4) {
    -webkit-animation-delay: -0.25s !important;
            animation-delay: -0.25s !important; }
  .line-scale-pulse-out-rapid > div:nth-child(1), .line-scale-pulse-out-rapid > div:nth-child(5) {
    -webkit-animation-delay: 0s !important;
            animation-delay: 0s !important; }

@-webkit-keyframes line-spin-fade-loader {
  50% {
    opacity: 0.3; }
  100% {
    opacity: 1; } }

@keyframes line-spin-fade-loader {
  50% {
    opacity: 0.3; }
  100% {
    opacity: 1; } }

.line-spin-fade-loader {
  position: relative;
  top: -10px;
  left: -4px; }
  .line-spin-fade-loader > div:nth-child(1) {
    top: 20px;
    left: 0;
    -webkit-animation: line-spin-fade-loader 1.2s -0.84s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s -0.84s infinite ease-in-out; }
  .line-spin-fade-loader > div:nth-child(2) {
    top: 13.63636px;
    left: 13.63636px;
    -webkit-transform: rotate(-45deg);
        -ms-transform: rotate(-45deg);
            transform: rotate(-45deg);
    -webkit-animation: line-spin-fade-loader 1.2s -0.72s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s -0.72s infinite ease-in-out; }
  .line-spin-fade-loader > div:nth-child(3) {
    top: 0;
    left: 20px;
    -webkit-transform: rotate(90deg);
        -ms-transform: rotate(90deg);
            transform: rotate(90deg);
    -webkit-animation: line-spin-fade-loader 1.2s -0.6s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s -0.6s infinite ease-in-out; }
  .line-spin-fade-loader > div:nth-child(4) {
    top: -13.63636px;
    left: 13.63636px;
    -webkit-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
            transform: rotate(45deg);
    -webkit-animation: line-spin-fade-loader 1.2s -0.48s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s -0.48s infinite ease-in-out; }
  .line-spin-fade-loader > div:nth-child(5) {
    top: -20px;
    left: 0;
    -webkit-animation: line-spin-fade-loader 1.2s -0.36s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s -0.36s infinite ease-in-out; }
  .line-spin-fade-loader > div:nth-child(6) {
    top: -13.63636px;
    left: -13.63636px;
    -webkit-transform: rotate(-45deg);
        -ms-transform: rotate(-45deg);
            transform: rotate(-45deg);
    -webkit-animation: line-spin-fade-loader 1.2s -0.24s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s -0.24s infinite ease-in-out; }
  .line-spin-fade-loader > div:nth-child(7) {
    top: 0;
    left: -20px;
    -webkit-transform: rotate(90deg);
        -ms-transform: rotate(90deg);
            transform: rotate(90deg);
    -webkit-animation: line-spin-fade-loader 1.2s -0.12s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s -0.12s infinite ease-in-out; }
  .line-spin-fade-loader > div:nth-child(8) {
    top: 13.63636px;
    left: -13.63636px;
    -webkit-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
            transform: rotate(45deg);
    -webkit-animation: line-spin-fade-loader 1.2s 0s infinite ease-in-out;
            animation: line-spin-fade-loader 1.2s 0s infinite ease-in-out; }
  .line-spin-fade-loader > div {
    background-color: #fff;
    width: 4px;
    height: 35px;
    border-radius: 2px;
    margin: 2px;
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    position: absolute;
    width: 5px;
    height: 15px; }

/**
 * Misc
 */
@-webkit-keyframes triangle-skew-spin {
  25% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(0);
            transform: perspective(100px) rotateX(180deg) rotateY(0); }
  50% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(180deg);
            transform: perspective(100px) rotateX(180deg) rotateY(180deg); }
  75% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(180deg);
            transform: perspective(100px) rotateX(0) rotateY(180deg); }
  100% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(0);
            transform: perspective(100px) rotateX(0) rotateY(0); } }
@keyframes triangle-skew-spin {
  25% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(0);
            transform: perspective(100px) rotateX(180deg) rotateY(0); }
  50% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(180deg);
            transform: perspective(100px) rotateX(180deg) rotateY(180deg); }
  75% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(180deg);
            transform: perspective(100px) rotateX(0) rotateY(180deg); }
  100% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(0);
            transform: perspective(100px) rotateX(0) rotateY(0); } }

.triangle-skew-spin > div {
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  width: 0;
  height: 0;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
  border-bottom: 20px solid #fff;
  -webkit-animation: triangle-skew-spin 3s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite;
          animation: triangle-skew-spin 3s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite; }

@-webkit-keyframes square-spin {
  25% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(0);
            transform: perspective(100px) rotateX(180deg) rotateY(0); }
  50% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(180deg);
            transform: perspective(100px) rotateX(180deg) rotateY(180deg); }
  75% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(180deg);
            transform: perspective(100px) rotateX(0) rotateY(180deg); }
  100% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(0);
            transform: perspective(100px) rotateX(0) rotateY(0); } }

@keyframes square-spin {
  25% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(0);
            transform: perspective(100px) rotateX(180deg) rotateY(0); }
  50% {
    -webkit-transform: perspective(100px) rotateX(180deg) rotateY(180deg);
            transform: perspective(100px) rotateX(180deg) rotateY(180deg); }
  75% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(180deg);
            transform: perspective(100px) rotateX(0) rotateY(180deg); }
  100% {
    -webkit-transform: perspective(100px) rotateX(0) rotateY(0);
            transform: perspective(100px) rotateX(0) rotateY(0); } }

.square-spin > div {
  -webkit-animation-fill-mode: both;
          animation-fill-mode: both;
  width: 50px;
  height: 50px;
  background: #fff;
  border: 1px solid red;
  -webkit-animation: square-spin 3s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite;
          animation: square-spin 3s 0s cubic-bezier(0.09, 0.57, 0.49, 0.9) infinite; }

@-webkit-keyframes rotate_pacman_half_up {
  0% {
    -webkit-transform: rotate(270deg);
            transform: rotate(270deg); }
  50% {
    -webkit-transform: rotate(360deg);
            transform: rotate(360deg); }
  100% {
    -webkit-transform: rotate(270deg);
            transform: rotate(270deg); } }

@keyframes rotate_pacman_half_up {
  0% {
    -webkit-transform: rotate(270deg);
            transform: rotate(270deg); }
  50% {
    -webkit-transform: rotate(360deg);
            transform: rotate(360deg); }
  100% {
    -webkit-transform: rotate(270deg);
            transform: rotate(270deg); } }

@-webkit-keyframes rotate_pacman_half_down {
  0% {
    -webkit-transform: rotate(90deg);
            transform: rotate(90deg); }
  50% {
    -webkit-transform: rotate(0deg);
            transform: rotate(0deg); }
  100% {
    -webkit-transform: rotate(90deg);
            transform: rotate(90deg); } }

@keyframes rotate_pacman_half_down {
  0% {
    -webkit-transform: rotate(90deg);
            transform: rotate(90deg); }
  50% {
    -webkit-transform: rotate(0deg);
            transform: rotate(0deg); }
  100% {
    -webkit-transform: rotate(90deg);
            transform: rotate(90deg); } }

@-webkit-keyframes pacman-balls {
  75% {
    opacity: 0.7; }
  100% {
    -webkit-transform: translate(-100px, -6.25px);
            transform: translate(-100px, -6.25px); } }

@keyframes pacman-balls {
  75% {
    opacity: 0.7; }
  100% {
    -webkit-transform: translate(-100px, -6.25px);
            transform: translate(-100px, -6.25px); } }

.pacman {
  position: relative; }
  .pacman > div:nth-child(2) {
    -webkit-animation: pacman-balls 1s -0.99s infinite linear;
            animation: pacman-balls 1s -0.99s infinite linear; }
  .pacman > div:nth-child(3) {
    -webkit-animation: pacman-balls 1s -0.66s infinite linear;
            animation: pacman-balls 1s -0.66s infinite linear; }
  .pacman > div:nth-child(4) {
    -webkit-animation: pacman-balls 1s -0.33s infinite linear;
            animation: pacman-balls 1s -0.33s infinite linear; }
  .pacman > div:nth-child(5) {
    -webkit-animation: pacman-balls 1s 0s infinite linear;
            animation: pacman-balls 1s 0s infinite linear; }
  .pacman > div:first-of-type {
    width: 0px;
    height: 0px;
    border-right: 25px solid transparent;
    border-top: 25px solid #fff;
    border-left: 25px solid #fff;
    border-bottom: 25px solid #fff;
    border-radius: 25px;
    -webkit-animation: rotate_pacman_half_up 0.5s 0s infinite;
            animation: rotate_pacman_half_up 0.5s 0s infinite;
    position: relative;
    left: -30px; }
  .pacman > div:nth-child(2) {
    width: 0px;
    height: 0px;
    border-right: 25px solid transparent;
    border-top: 25px solid #fff;
    border-left: 25px solid #fff;
    border-bottom: 25px solid #fff;
    border-radius: 25px;
    -webkit-animation: rotate_pacman_half_down 0.5s 0s infinite;
            animation: rotate_pacman_half_down 0.5s 0s infinite;
    margin-top: -50px;
    position: relative;
    left: -30px; }
  .pacman > div:nth-child(3),
  .pacman > div:nth-child(4),
  .pacman > div:nth-child(5),
  .pacman > div:nth-child(6) {
    background-color: #fff;
    width: 15px;
    height: 15px;
    border-radius: 100%;
    margin: 2px;
    width: 10px;
    height: 10px;
    position: absolute;
    -webkit-transform: translate(0, -6.25px);
        -ms-transform: translate(0, -6.25px);
            transform: translate(0, -6.25px);
    top: 25px;
    left: 70px; }

@-webkit-keyframes cube-transition {
  25% {
    -webkit-transform: translateX(50px) scale(0.5) rotate(-90deg);
            transform: translateX(50px) scale(0.5) rotate(-90deg); }
  50% {
    -webkit-transform: translate(50px, 50px) rotate(-180deg);
            transform: translate(50px, 50px) rotate(-180deg); }
  75% {
    -webkit-transform: translateY(50px) scale(0.5) rotate(-270deg);
            transform: translateY(50px) scale(0.5) rotate(-270deg); }
  100% {
    -webkit-transform: rotate(-360deg);
            transform: rotate(-360deg); } }

@keyframes cube-transition {
  25% {
    -webkit-transform: translateX(50px) scale(0.5) rotate(-90deg);
            transform: translateX(50px) scale(0.5) rotate(-90deg); }
  50% {
    -webkit-transform: translate(50px, 50px) rotate(-180deg);
            transform: translate(50px, 50px) rotate(-180deg); }
  75% {
    -webkit-transform: translateY(50px) scale(0.5) rotate(-270deg);
            transform: translateY(50px) scale(0.5) rotate(-270deg); }
  100% {
    -webkit-transform: rotate(-360deg);
            transform: rotate(-360deg); } }

.cube-transition {
  position: relative;
  -webkit-transform: translate(-25px, -25px);
      -ms-transform: translate(-25px, -25px);
          transform: translate(-25px, -25px); }
  .cube-transition > div {
    -webkit-animation-fill-mode: both;
            animation-fill-mode: both;
    width: 10px;
    height: 10px;
    position: absolute;
    top: -5px;
    left: -5px;
    background-color: #fff;
    -webkit-animation: cube-transition 1.6s 0s infinite ease-in-out;
            animation: cube-transition 1.6s 0s infinite ease-in-out; }
    .cube-transition > div:last-child {
      -webkit-animation-delay: -0.8s;
              animation-delay: -0.8s; }

@-webkit-keyframes spin-rotate {
  0% {
    -webkit-transform: rotate(0deg);
            transform: rotate(0deg); }
  50% {
    -webkit-transform: rotate(180deg);
            transform: rotate(180deg); }
  100% {
    -webkit-transform: rotate(360deg);
            transform: rotate(360deg); } }

@keyframes spin-rotate {
  0% {
    -webkit-transform: rotate(0deg);
            transform: rotate(0deg); }
  50% {
    -webkit-transform: rotate(180deg);
            transform: rotate(180deg); }
  100% {
    -webkit-transform: rotate(360deg);
            transform: rotate(360deg); } }

.semi-circle-spin {
  position: relative;
  width: 35px;
  height: 35px;
  overflow: hidden; }
  .semi-circle-spin > div {
    position: absolute;
    border-width: 0px;
    border-radius: 100%;
    -webkit-animation: spin-rotate 0.6s 0s infinite linear;
            animation: spin-rotate 0.6s 0s infinite linear;
    background-image: -webkit-linear-gradient(transparent 0%, transparent 70%, #fff 30%, #fff 100%);
    background-image: linear-gradient(transparent 0%, transparent 70%, #fff 30%, #fff 100%);
    width: 100%;
    height: 100%; }

@-webkit-keyframes bar-progress {
  0% {
    -webkit-transform: scaleY(20%);
            transform: scaleY(20%);
    opacity: 1; }
  25% {
    -webkit-transform: translateX(6%) scaleY(10%);
            transform: translateX(6%) scaleY(10%);
    opacity: 0.7; }
  50% {
    -webkit-transform: translateX(20%) scaleY(20%);
            transform: translateX(20%) scaleY(20%);
    opacity: 1; }
  75% {
    -webkit-transform: translateX(6%) scaleY(10%);
            transform: translateX(6%) scaleY(10%);
    opacity: 0.7; }
  100% {
    -webkit-transform: scaleY(20%);
            transform: scaleY(20%);
    opacity: 1; } }

@keyframes bar-progress {
  0% {
    -webkit-transform: scaleY(20%);
            transform: scaleY(20%);
    opacity: 1; }
  25% {
    -webkit-transform: translateX(6%) scaleY(10%);
            transform: translateX(6%) scaleY(10%);
    opacity: 0.7; }
  50% {
    -webkit-transform: translateX(20%) scaleY(20%);
            transform: translateX(20%) scaleY(20%);
    opacity: 1; }
  75% {
    -webkit-transform: translateX(6%) scaleY(10%);
            transform: translateX(6%) scaleY(10%);
    opacity: 0.7; }
  100% {
    -webkit-transform: scaleY(20%);
            transform: scaleY(20%);
    opacity: 1; } }

.bar-progress {
  width: 30%;
  height: 12px; }
  .bar-progress > div {
    position: relative;
    width: 20%;
    height: 12px;
    border-radius: 10px;
    background-color: #fff;
    -webkit-animation: bar-progress 3s cubic-bezier(0.57, 0.1, 0.44, 0.93) infinite;
            animation: bar-progress 3s cubic-bezier(0.57, 0.1, 0.44, 0.93) infinite;
    opacity: 1; }

@-webkit-keyframes bar-swing {
  0% {
    left: 0; }
  50% {
    left: 70%; }
  100% {
    left: 0; } }

@keyframes bar-swing {
  0% {
    left: 0; }
  50% {
    left: 70%; }
  100% {
    left: 0; } }

.bar-swing {
  width: 30%;
  height: 8px; }
  .bar-swing > div {
    position: relative;
    width: 30%;
    height: 8px;
    border-radius: 10px;
    background-color: #fff;
    -webkit-animation: bar-swing 1.5s infinite;
            animation: bar-swing 1.5s infinite; }

@-webkit-keyframes bar-swing-container {
  0% {
    left: 0;
    -webkit-transform: translateX(0);
            transform: translateX(0); }
  50% {
    left: 70%;
    -webkit-transform: translateX(-4px);
            transform: translateX(-4px); }
  100% {
    left: 0;
    -webkit-transform: translateX(0);
            transform: translateX(0); } }

@keyframes bar-swing-container {
  0% {
    left: 0;
    -webkit-transform: translateX(0);
            transform: translateX(0); }
  50% {
    left: 70%;
    -webkit-transform: translateX(-4px);
            transform: translateX(-4px); }
  100% {
    left: 0;
    -webkit-transform: translateX(0);
            transform: translateX(0); } }

.bar-swing-container {
  width: 20%;
  height: 8px;
  position: relative; }
  .bar-swing-container div:nth-child(1) {
    position: absolute;
    width: 100%;
    background-color: rgba(255, 255, 255, 0.2);
    height: 12px;
    border-radius: 10px; }
  .bar-swing-container div:nth-child(2) {
    position: absolute;
    width: 30%;
    height: 8px;
    border-radius: 10px;
    background-color: #fff;
    -webkit-animation: bar-swing-container 2s cubic-bezier(0.91, 0.35, 0.12, 0.6) infinite;
            animation: bar-swing-container 2s cubic-bezier(0.91, 0.35, 0.12, 0.6) infinite;
    margin: 2px 2px 0; }

```

  