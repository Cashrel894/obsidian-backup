#OdinProject 
## Links
```html
<a href="https://example.com" target="_blank" rel="noreferer">
```
`<a>` 标签，即 Anchor 元素，可以创建一个可跳转的链接。该元素包含多种属性：
- `href`：即 hypertext reference，用于指定跳转到的链接。支持绝对路径和相对路径（相对于当前页面的当前目录）。
- `target`：指定链接网站在浏览器中的位置。`_blank` 就是在新标签页中打开，而 `_self` 则直接在当前标签页打开。
- `rel`：指定链接与当前页面的关系。`noopener` 可以阻止链接网站使用 JavaScript 操作当前页面，而 `noreferer` 不仅可以阻止，还可以防止链接网站获取当前页面的地址，从而保证安全性和隐私性。
	- 注：新版现代浏览器通常会在 `target="_blank"` 时提供一些网络防护功能，所以忘记指定 rel 问题不大，不过最好还是加上。

## Images
```html
<img src="https://www.theodinproject.com/mstile-310x310.png" alt="The Odin Project Logo" width="310" height="310">
```
- `src`：即 source，指定图片的 url。同样支持绝对路径和相对路径。
- `alt`：指定当图片加载出现问题时，页面作为替代显示的内容。
- `width` 和 `height`：指定图片的尺寸。一般不加也没事，加了可以帮助浏览器更好地排版图片。