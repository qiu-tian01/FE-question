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


> ### BFC

BFC全称是块级格式上下文 (Block Formatting Context) 。

BFC是一个独立的布局环境，其中的子元素布局是不受外界的影响，同时子元素不会影响到外面元素，

- 实现BFC
  
  - 根元素HTML

  - float属性不为none (left,right)

  - position属性为absolute,fixed

  - display值为 inline-block、table-cell、table-caption、table、inline-table、flex、inline-flex、grid、inline-grid

  - overflow属性不为visible,为hidden，scroll，auto

- BFC区域的约束规则

  - 属于同一BFC的子元素垂直排列

  - 属于同一BFC的子元素的margin会重叠

  - 生成BFC元素的子元素中，每一个子元素左外边距与包含块的左边界相接触（对于从右到左的格式化，右外边距接触右边界），即使浮动元素也是如此（尽管子元素的内容区域会由于浮动而压缩），除非这个子元素也创建了一个新的BFC（如它自身也是一个浮动元素）。

  - BFC 的区域不会与 float 的元素区域重叠

  - 计算 BFC 的高度时，浮动子元素也参与计算

  - 文字层不会被浮动层覆盖，环绕于周围

- BFC解决的问题

 - 清除浮动

 - 阻止margin重叠

 - 实现自适应两栏，三栏布局

 - 可以阻止元素被浮动元素覆盖


