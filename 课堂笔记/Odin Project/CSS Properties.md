#OdinProject 
## color & background-color 
```css 
p {
	/* hex example */
	color: #1100ff;
}

p {
	/* rgb example */
	color: rgb(100, 0, 127);
}

p {
	/* hsl example */
	color: hsl(15, 82%, 56%);
}
```
color 调整文本颜色，background-color 调整背景颜色。两者都可以通过 hex、rgb、hsl 编码颜色。

同时，它们还有对应的 alpha 版本，用于指定颜色的透明度。对于 hex，直接在最后用额外的两位十六进制表示；对于 rgb 和 hsl，分别有 rgba 和 hsla，在第四个参数表示即可。

## typography & text-align
```css 
p {
	font-family: "Times New Roman", serif;
}
```
每种字体分为两种类型：
- Font Family：指的是**一种**特定、具体的字体。其名称通常会包含空格或特殊符号，指定时需要加引号，如 `"Times New Roman"` `"Microsoft YaHei"` 等。
- Generic Family：指的是**一类**风格相近的字体。名称通常不需要加引号，而且主流浏览器都定义了这些它们的默认字体，因此总是能够成功显示。有 `serif` (衬线体)、`sans-serif`（无衬线体）、`monospace`（等宽字体）等等。
	- 事实上，这些 Generic Family 的名称都属于 CSS 的内置关键字。

使用 font-family 指定字体时，通常会指定多个字体，用逗号分隔；先尝试 Font Family，如果系统没有安装，就使用最后的 Generic Family 作为保底，防止文本完全无法显示。

```css 
p {
	font-size: 22px;
	font-weight: bold;
	/* font-weight: 700; 与使用bold关键字等价。数字范围1-1000 */
	
	text-align: center;
}
```

## Image height and width
```css 
img {
	height: auto; /* 使用auto后，height会随weight等比例缩放 */
	width: 500px;
}
```
显式指定图片高宽的好处：在加载页面时提前为图片预留显示空间，防止图片加载完成时，页面其余部分突然因图片的出现而移位，造成不好的视觉效果。

