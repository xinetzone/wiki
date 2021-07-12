# SVG

```{glossary}
[SVG](https://developer.mozilla.org/zh-CN/docs/Web/SVG)
    可缩放矢量图形（Scalable Vector Graphics，SVG），是一种用于描述二维[矢量图形](https://en.wikipedia.org/wiki/Vector_graphics)，基于 {term}`XML` 的标记语言。作为一个基于文本的开放网络标准，SVG 能够优雅而简洁地渲染不同大小的图形，并和 {term}`CSS`，{term}`DOM`，{term}`JavaScript` 和 {term}`SMIL` 等其他网络标准无缝衔接。本质上，SVG 相对于图像，就好比 {term}`HTML` 相对于文本。

[XML](https://developer.mozilla.org/en-US/docs/Web/XML/XML_Introduction)
    ...
```

## HTML 相关元素

```{glossary}
[`<svg>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/svg)
    ...

[`<g>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/g)
    ...

[`<rect>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/rect)
    ...

[`<circle>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle)
    ...

[`<ellipse>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/ellipse)
    ...

[`<line>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/line)
    ...

[`<polyline>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/polyline)
    ...

[`<polygon>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/polygon)
    ...

[`<path>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path)
    ...

[`<defs>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/defs)
    `<defs>` 元素用于存储以后使用的图形对象。在 `<defs>` 元素中创建的对象不会直接呈现。要显示它们，你必须引用它们(例如使用 {term}`<use>` 元素)。

    图形对象可以从任何地方引用，但是，在 `<defs>` 元素中定义这些对象可以提高 SVG 内容的可理解性，并有利于文档的整体可访问性。

[`<use>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use)
    `<use>` 元素从 SVG 文档中获取节点，并将它们复制到其他地方。

    效果就像将节点深度克隆到一个非公开 DOM 中，然后粘贴到 `<use>` 元素所在的位置，很像 HTML5 中克隆的[模板元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template)。

[`<linearGradient>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/linearGradient)
    `<linearGradient>` 元素允许定义线性渐变，可以应用于图形元素的填充或描边。
    :::{note}
    不要与 CSS [linear-gradient()](https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient()) 混淆，因为 CSS 渐变只能应用于 HTML 元素，而 SVG 渐变只能应用于 SVG 元素。
    :::

[`<stop>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/stop)
    SVG `<stop>` 元素定义了在渐变中使用的颜色及其位置。这个元素总是 `<linearGradient> `或 `<radialGradient>` 元素的子元素。

[`<radialGradient>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/radialGradient)
    `<radialGradient>` 元素允许作者定义径向渐变，可以应用于图形元素的填充或描边。
    :::{note}
    不要与 CSS [radial-gradient()](https://developer.mozilla.org/en-US/docs/Web/CSS/radial-gradient()) 混淆，因为 CSS 渐变只能应用于 HTML 元素，而 SVG 渐变只能应用于 SVG 元素。
    :::

[`<pattern>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/pattern)
    `<pattern>` 元素定义了一个图形对象，该对象可以在重复的 `x` 和 `y` 坐标间隔(“平铺”)重新绘制，以覆盖一个区域。
```

## SVG 元素属性

```{glossary}
[`fill-rule`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/fill-rule)
    ...

[`stroke-miterlimit`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-miterlimit)
    ...

[`stroke-dashoffset`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dashoffset)
    ...
```