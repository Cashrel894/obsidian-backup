#FullStackOpen 
**单页应用**(Single Page App, SPA) 与传统的网页不同，它将页面更新的任务交给浏览器，而不是由服务器后端进行重定向。

具体来说，当页面更新时，SPA 会先通过前端的 JS 代码“局部更新”HTML 文件，再将更新推送到服务器，从而让页面操作变得十分丝滑。

示例：
```js
var form = document.getElementById('notes_form')
form.onsubmit = function(e) {
  e.preventDefault()

  var note = {
    content: e.target.elements[0].value,
    date: new Date(),
  }

  notes.push(note)
  e.target.elements[0].value = ''
  redrawNotes()
  sendToServer(note)
}
```