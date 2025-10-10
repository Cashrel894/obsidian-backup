#CS61C 
[IMPORTANT CHEATSHEET](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

![[../../附件/Pasted image 20251003210443.png]]
## Flex containers & Flex items
Flexbox 包含了一系列用于安排元素位置的实用属性。

这些属性可被分为两类，分别作用于 Flex container 和 Flex item。

所谓 Flex Container，指的是具有 `display: flex` 属性的元素。而其中所有的元素都是 Flex Item。

## The flex declaration
flex 声明，指的是声明作用于 flex item 的 `flex-grow` `flex-shrink` 和  `flex-basis` 三种属性的简写形式。
```css 
div {
	flex: 1;
	/*flex-grow: 1; flex-shrink: 1; flex-basis: 0;*/
}

div {
	flex: auto;
	/*flex: 1 1 auto;*/
}
```
当没有显式指定时，flex 的默认值是 `0 1 auto`。即，元素不会在有多余空间时放大，会在空间不足时缩小，且以元素内容的宽/高为基础放大或缩小。
### flex-grow
接受单个数字，作为 flex item 的“放大因子”。当容器放大时，元素的增长倍率与其 flex-grow 成正比。特别地，当 flex-grow 为 0 时，元素不会因容器的放大而放大。 ![[../../附件/Pasted image 20251002152036.png]]

### flex-shrink
与 flex-grow 类似，但 flex-shrink 是 flex item 的“缩小因子”。当 flex-shrink 为 0 时，元素不会因容器的缩小而缩小。

### flex-basis
决定了 flex item 在未放大或缩小前的初始宽度（若主轴为 row）或高度（主轴为 colomn）。如果用 auto 填充，则会用其 box 模型的大小决定 flex-basis。

### In practice
一般只会使用 `flex: 1;` 让元素平均地增长，以及 `flex-shrink: 0;` 来阻止元素缩小。

> [!warning] 
> 使用 `flex: 1;` 会导致 flex-basis 被设为 0，使得 grow 和 shrink 从 0 开始。这可能导致预期之外的结果。修复这个问题很容易，用 `flex: 1 1 auto;` 替代即可。

## Axes
flexbox 对于水平和垂直两个方向的元素排布都有效。

```css 
.flex-container {
	flex-direction: column; /*row*/
}
```

flex-container 总是有两个轴：**主轴**和**交叉轴**。修改 flex container 的 flex-direction，实际上就是在改变主轴的方向。默认情况下，主轴就是 row，即水平方向。

> [!attention] 
> 默认情况下，块级元素的宽等于其父级容器的宽，但高等于其 content 本身的高。因此，在改变主轴时，这点可能会带来困惑。

## Alignment
```css 
.flex-container {
	justify-content: space-between;
	align-items: center;
}
```
flex container 属性 `justify-content` 和 `align-items` 分别指定 flex item 在主轴和交叉轴上的行为。

默认情况下，元素会在主轴的起始处堆叠，同时伸展填满整个交叉轴。

`justify-content` 的常用值有：
- flex-start 
- flex-end 
- center 
- space-between 
- space-around
- space-evenly

`align-items` 的常用值有：
- stretch
- flex-start 
- flex-end 
- center 
- baseline

此外，还有一个 flex item 属性，`align-self`，可以单独指定该元素在交叉轴上的行为。这个属性的可能值与 `align-items` 完全相同。可以把 `align-items` 看作为每个元素设置相同 `align-self` 的简写形式。
## Gap
```css 
.container {
	gap: 8px;
}
```
用于指定项之间分布的间距。