> ### css 盒模型

- css 盒模型本质是一个盒子，封装周围的 HTML 元素，它包括：边距，边框，填充和实际内容。

  - 盒子宽高计算如下：

    - 标准盒模型

      盒子大小 = content + border + padding +margin

    - 怪异盒模型(IE 盒模型)

      盒子大小 = content(width+border+padding) + margin

  - 如何选择盒模型

    如果是定义了完整的 doctype 的标准文档类型，无论是哪种模型情况，最终都会触发标准模式， (<!DOCTYPE html>)

    如果 doctype 协议缺失，会由浏览器自己界定，在 IE 浏览器中 IE9 以下（IE6.IE7.IE8）的版本触发怪异模式，其他浏览器中会默认为 W3c 标准模式。

  - 使用 box-sizing

        content-box： 默认值，border和padding不算到width范围内，可以理解为是W3c的标准模型(default)

        border-box：border和padding划归到width范围内，可以理解为是IE的怪异盒模型

        padding-box：将padding算入width范围

  - 什么情况下使用怪异盒模型

    当我们想让盒子的 width 变为 content+padding+border 的时候。比如我定义了 3 个元素，希望他们并排显示，宽度是 33.35,我再加 20px 的 padding 值，标准盒模型此时计算宽度就会是 33.3%+20px 的 padding 值。这样肯定会被挤下去。所以我们使用怪异盒模型。33.3% = width + padding + border.
