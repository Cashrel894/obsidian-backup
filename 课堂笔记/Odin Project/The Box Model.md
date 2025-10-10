#CS61C 
![[../../附件/Pasted image 20251002130936.png]]
## box-sizing
对于 `box-sizing` 属性，有两种可能值：
- `content-box`：指定给元素的长宽仅仅是 content 区域的大小。
- `border-box`：指定的长宽包含元素的 content、padding 和 border。
`box-sizing` 的默认值是 `content-box`。

## display
盒子拥有两种显示类型：inner display type 和 outer display type。

outer display type 决定了一个盒子如何和其周围的其他盒子交互，主要包含：
- block：`h1` `p` 等
	- 盒子会在新的一行中显示。
	- width 和 height 属性生效。
	- padding margin 和 border 会“推开”其他元素。
	- 如果 width 没有指定，那么盒子就会延伸填满容器中的当前行。
- inline：`a` `span` `em` `strong` 等
	- 盒子不会在新行显示。
	- width 和 height 属性失效。
	- 顶部和底部的 padding margin 和 border 不会对上下行的 inline box 产生影响。
	- 左右的 padding margin border 依然会“推开”i 其他 inline box。
- inline-block：
	- 和 inline 类似，但 width 和 height 属性生效，且会影响其他元素。

而 inner display type 则决定盒子内部的元素如何布局，包含 `flex` `grid` 等。
`inline` `inline-flex` `block` `flex` (即 block+flex)

## margin collapsing
Depending on whether two elements whose margins touch have positive or negative margins, the results will be different:
- Two positive margins will combine to become one margin. Its size will be equal to the largest individual margin.
- Two negative margins will collapse and the smallest (furthest from zero) value will be used.
- If one margin is negative, its value will be _subtracted_ from the total.