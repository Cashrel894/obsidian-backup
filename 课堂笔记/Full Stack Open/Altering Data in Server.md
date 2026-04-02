#FullStackOpen 
## Post
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

## Put
要修改一个现有的资源，可以使用 `Put` 动作替换整个目标对象：
```jsx
const toggleImportanceOf = id => {
  const url = `http://localhost:3001/notes/${id}`
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, important: !note.important }

  axios.put(url, changedNote).then(response => {
    setNotes(notes.map(note => note.id === id ? response.data : note))
  })
}
```