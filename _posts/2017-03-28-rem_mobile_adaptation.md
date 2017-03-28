---
layout: post
title: "rem 移动端最终适配方案"
date: 2017-03-28
description: "介绍 rem 移动端适配方案"
tag: webapp
---


> 此方案解决：依据设计师设计稿px,可直接得出映射的rem值，最终实现对不同大小的手机屏幕还原相同设计稿效果

>> 另：对于不同retina屏手机清晰展示的解决方案 不在此考虑

举个例子 便于理解

假设
设计稿屏宽 `designWidth = 640px`

手机屏宽 `innerWidth = 320px`

某元素设计稿宽度 `designContainWidth = 20px`

### 利用rem to px适配

1. 设计稿映射的手机像素

    ```
    pxContainWidth = innerWidth / designWidth * designContainWidth
                  = 320 / 640 * 20 = 10(px)
    ```

2. 由rem转px的规则得

    ```
    remContainWidth = pxContainWidth / htmlFontsize
                   = innerWidth / designWidth * designContainWidth / htmlFontsize
                   = innerWidth / designWidth / htmlFontsize * designContainWidth
    ```

3. 假设 rem2px = 100 即 1 rem = 100px

    这样，当设计稿 designContainWidth = 375px时，通过移动小数点，就可以映射 rem值： 3.75rem

    ```
    那么 htmlFonsize = innerWidth / designWidth * (designContainWidth / remContainWidth)
                    = innerWidth / designWidth * rem2px
    ```

### 利用rem to 百分比适配

（巧妙关联了htmlFontsize）

1. 一般情况，默认根节点fontsize为

    ```
      defaultHtmlFontsize = 16px
      假设 设置根节点fontsize = persentHtmlFonsize 百分比
      其实 htmlFonsize = defaultHtmlFontsize * persentHtmlFonsize
    ```

2. 由以上 htmlFonsize = innerWidth / designWidth * rem2px 可得

    ```
      defaultHtmlFontsize * persentHtmlFonsize = innerWidth / designWidth * rem2px
    ```

3. 所以

    ```
    persentHtmlFonsize（%） = innerWidth / designWidth * rem2px / defaultHtmlFontsize *100%
                           = innerWidth / designWidth * rem2px / defaultHtmlFontsize * 100%
                           = innerWidth / designWidth * rem2px / 16 * 100%
    ```

> 对于特殊情况 defaultHtmlFontsize != 16 px

需要通过解决
```
window.getComputedStyle(d, null).getPropertyValue('width') 计算得出 defaultHtmlFontsize
```

### 最终解决方案
```js
function adapt(designWidth, rem2px){
  var d = window.document.createElement('div');
  d.style.width = '1rem';
  d.style.display = "none";
  var head = window.document.getElementsByTagName('head')[0];
  head.appendChild(d);
  var defaultFontSize = parseFloat(window.getComputedStyle(d, null).getPropertyValue('width'));
  d.remove();
  document.documentElement.style.fontSize = window.innerWidth / designWidth * rem2px / defaultFontSize * 100 + '%';
  var st = document.createElement('style');
  var portrait = "@media screen and (min-width: "+window.innerWidth+"px) {html{font-size:"+ ((window.innerWidth/(designWidth/rem2px)/defaultFontSize)*100) +"%;}}";
  var landscape = "@media screen and (min-width: "+window.innerHeight+"px) {html{font-size:"+ ((window.innerHeight/(designWidth/rem2px)/defaultFontSize)*100) +"%;}}"
  st.innerHTML = portrait + landscape;
  head.appendChild(st);
  return defaultFontSize
};
var defaultFontSize = adapt(640, 100);

```

