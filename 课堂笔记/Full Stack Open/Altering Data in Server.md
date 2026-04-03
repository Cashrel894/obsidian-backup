#FullStackOpen 
## Creating new Resource
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

## Update Resource
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

## Extracting Comms with the Backend into a Separate Module
按照惯例，React 应用与后端的通信模块一般放在 `src/services` 目录，并以相关资源的名称命名：
```js
// src/services/notes.js
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  return axios.get(baseUrl)
}

const create = newObject => {
  return axios.post(baseUrl, newObject)
}

const update = (id, newObject) => {
  return axios.put(`${baseUrl}/${id}`, newObject)
}

export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```
模块对外导出了一个对象，将处理资源的相关函数作为其属性。

`App` 模块只需要导入这个对象即可：
```jsx
import noteService from './services/notes'
```

由于 JS 的对象字面量特性，可以写成：
```js
export default { getAll, create, update }
```
这种简洁的性质。

## Promises and Errors
有时，前端向后端发出的请求可能失败，此时后端会向前端发送相关的状态码（如 404），前端需要根据回复的错误向用户做出合适的反馈。

使用 Promise，可以通过 `catch` 方法绑定错误处理的回调函数：
```jsx
axios
  .get('http://example.com/probably_will_fail')
  .then(response => {
    console.log('success!')
  })
  .catch(error => {
    console.log('fail')
  })
```

对于一个 Promise Chain，`catch` 通常放在链的末尾，可以捕捉链上的所有异常：
```jsx
axios
  .get('http://example.com/probably_will_fail')
  .then(response => {
    console.log('success!')
  })
  .catch(error => {
    console.log('fail')
  })
```

