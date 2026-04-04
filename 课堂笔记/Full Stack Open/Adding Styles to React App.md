#FullStackOpen 
在 React 中，添加 CSS 样式的方式和传统比较相似。

创建 `src/index.css` ，将其导入 `main.jsx`:
```jsx
import './index.css'
```
注意：在 React 中，我们应该用 `className` 而非 `class` 来指定组件的类。

## Example: Error Message
```jsx
const Notification = ({ message }) => {
  if (message === null) {
    return null
  }

  return (
    <div className="error">
      {message}
    </div>
  )
}

export default Notification
```

```jsx
import { useState, useEffect } from 'react'
import Note from './components/Note'
import noteService from './services/notes'

import Notification from './components/Notification'

const App = () => {
  const [notes, setNotes] = useState([]) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)

  const [errorMessage, setErrorMessage] = useState('some error happened...')

  // ...

  return (
    <div>
      <h1>Notes</h1>

      <Notification message={errorMessage} />
      <div>
        <button onClick={() => setShowAll(!showAll)}>
          show {showAll ? 'important' : 'all' }
        </button>
      </div>      
      // ...
    </div>
  )
}
```

```css
.error {
  color: red;
  background: lightgrey;
  font-size: 20px;
  border-style: solid;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 10px;
}
```

```jsx
const toggleImportanceOf = id => {
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, important: !note.important }

  noteService
    .update(id, changedNote).then(returnedNote => {
      setNotes(notes.map(note => note.id !== id ? note : returnedNote))
    })
    .catch(error => {

      setErrorMessage(
        `Note '${note.content}' was already removed from server`
      )
      setTimeout(() => {
        setErrorMessage(null)
      }, 5000)
      setNotes(notes.filter(n => n.id !== id))
    })
}
```

![[Pasted image 20260404143752.png]]

## Inline Styles
通过 `style` 属性，我们可以将 CSS 属性包装为 JS 对象传入到组件中。如：
```jsx
{
  color: 'green',
  fontStyle: 'italic'
}
```
注意：字符串属性值应加双引号，hyphen 连接的属性名应改为 camelCase。

例：
```jsx
const Footer = () => {
  const footerStyle = {
    color: 'green',
    fontStyle: 'italic'
  }

  return (
    <div style={footerStyle}>
      <br />
      <p>
        Note app, Department of Computer Science, University of Helsinki 2025
      </p>
    </div>
  )
}

export default Footer
```

当然，内联样式也存在一些局限，如不能直接使用伪类等。这种样式定义方式将 HTML 与 CSS 绑定在同一个组件下，与传统的样式分离模式完全不同，这也是 React 的一大设计理念。 