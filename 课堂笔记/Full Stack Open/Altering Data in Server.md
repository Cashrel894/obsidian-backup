#FullStackOpen 
通过 `axios.post`，我们可以向服务器发送请求，请求修改服务器数据。

```jsx
const addNote = event => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    important: Math.random() > 0.5,
  }

  axios
    .post('http://localhost:3001/notes', noteObject)
    .then(response => {
      setNotes(notes.concat(response.data))
      setNewNote('')
    })
}
```
这里，`post` 请求的 `response` 将包含新创建的目标对象。注意，`json-server` 帮我们自动生成了对象的 `id` 属性，无需自行配置。