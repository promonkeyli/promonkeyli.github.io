---
title: css opacity透明度叠加色差问题
tags:
  - css
categories:
  - css
date: 2024-06-21 20:54:26
---

#### css中透明度

* `opacity` ：属性用于设置元素的整体透明度。它会影响元素本身及其所有子元素
* `rgba`：rgba 颜色值中的 `a` 代表 alpha 通道，用于设置颜色的透明度。rgba 可以用来设置背景色、边框色、文字颜色等
* `hsla`：hsla 颜色值类似于 rgba，但使用色相 (Hue)、饱和度 (Saturation)、亮度 (Lightness) 和 alpha 通道来定义颜色。

#### opacity叠加效果计算

opacity方式叠放：例如opacity 值都是60%，叠加元素的透明度 ：0.6 + 0.6 * （1 - 0.6）= 0.84

