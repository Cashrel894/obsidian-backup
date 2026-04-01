#FullStackOpen 
React 的核心思想是：将网页的组成部分抽象为一个个可复用的 **组件**(Component)。

基础使用非常简单，例如：
```jsx
const Hello = (props) => {
  console.log(props);
  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
    </div>
  );
};

const App = () => {
  const name = 'Peter';
  const age = 10;

  return (
    <div>
      <h1>Greetings</h1>

      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  );
};
```

注意：
- React 中的所有组件名称都应该**首字母大写**，否则可能被识别为原生 html 标签。
- 每个 React 组件都应该有一个 **根元素**，例如：
```jsx
const App = () => {
  return (
    <h1>Greetings</h1>
    <Hello name='Maya' age={26 + 10} />
    <Footer />
  )
}
```
这样会报错。

解决方案：
```jsx
const App = () => {
  return [
    <h1>Greetings</h1>,
    <Hello name='Maya' age={26 + 10} />,
    <Footer />
  ]
}
```
或者更常用的是用一个空标签包裹：
```jsx
const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
      <Footer />
    </>
  )
}
```
- React 不能直接渲染 JS 对象，只能直接渲染原生数据类型，例如：
```jsx
const App = () => {
  const friends = [
    { name: 'Peter', age: 4 },
    { name: 'Maya', age: 10 },
  ]

  return (
    <div>
      <p>{friends[0]}</p>
      <p>{friends[1]}</p>
    </div>
  )
}

export default App
```
无法正常渲染。
- React 支持直接渲染 Array。

## Stateful component
一个组件的状态可能随着用户交互等发生变化，当状态更新时，要自动重渲染这些状态，就需要使用 React 的 **状态 hook**（State hook）。
```jsx
import { useState } from 'react'

const App = () => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}

export default App
```

`useState(x)` 为组件添加了状态，并将其初始化为 `x`。该函数返回包含两个项目的数组，第一个 `counter` 为 *状态初始值*，第二个 `setCounter` 则为 *修改状态的函数*。

每当 *修改状态的函数* 被调用时，React 会重新渲染组件，即重新执行组件的函数体。此时，`counter` 会被赋予更新后的状态值，接着调用 `setCounter`，渲染组件；如此循环往复。

## Event Handling
在 React 中，为事件注册处理函数大致是这样的：
```jsx
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const handleClick = () => {
    console.log('clicked')
  }

  return (
    <div>
      <div>{counter}</div
      <button onClick={handleClick}>
        plus
      </button>
    </div>
  )
}
```

在这里，将 `onClick` 属性的值设为对代码中定义的 `handleClick` 函数的引用。

也可以直接在赋值中定义：
```jsx
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={() => console.log('clicked')}>
        plus
      </button>
    </div>
  )
}
```

注意，`onClick` 属性的值必须是一个 *函数* 对象，不能直接调用函数，这回将被调用函数的返回值存入属性，而不是将函数调用作为回调。

通常，使用 `onSomething` 命名代表时间的 props，用 `handleSomething` 命名相应的处理函数。

## Lifting State Up
有时，多个组件需要反映相同的变化数据，一个 **最佳实践** 是将共享状态 *提升* 到它们**最近的公共祖先**。

当父组件的一个状态更新时，该组件及其子组件都会被重新渲染，来适应状态的更新。

## Refactor Components
像这样的组件：
```jsx
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

可以通过 js 的 *解构* 特性来简化组件定义：
```jsx
const Display = ({ counter }) => {
  return (
    <div>{counter}</div>
  )
}
```

甚至可以进一步简化：
```jsx
const Display = ({ counter }) => <div>{counter}</div>
```

这样，我们只需要取出组件所需的 `props` 属性，使得组件定义更清晰、简洁。

## Complex states
如果我们需要更复杂的状态，可以使用 **对象** 表示状态：
```jsx
const App = () => {
  const [clicks, setClicks] = useState({
    left: 0, right: 0
  })

  const handleLeftClick = () => {
    const newClicks = {
      left: clicks.left + 1,
      right: clicks.right
    }
    setClicks(newClicks)
  }

  const handleRightClick = () => {
    const newClicks = {
      left: clicks.left,
      right: clicks.right + 1
    }
    setClicks(newClicks)
  }

  return (
    <div>
      {clicks.left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {clicks.right}
    </div>
  )
}
```

然而，如果这个对象较为复杂，一遍遍复制未修改的其他属性并不简洁美观。可以使用 **对象展开** 语法：
```jsx
const handleLeftClick = () => {
  const newClicks = {
    ...clicks,
    left: clicks.left + 1
  }
  setClicks(newClicks)
}

const handleRightClick = () => {
  const newClicks = {
    ...clicks,
    right: clicks.right + 1
  }
  setClicks(newClicks)
}
```

这样，原对象未被修改的剩余属性就会被自动赋值到新的对象中。

或者更简洁地：
```jsx
const handleLeftClick = () =>
  setClicks({ ...clicks, left: clicks.left + 1 })

const handleRightClick = () =>
  setClicks({ ...clicks, right: clicks.right + 1 })
```

React 推崇函数式编程，如果尝试改变原来的对象属性，可能会导致意想不到的副作用，需要尽可能避免。

## Handling arrays
```jsx
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
  }

  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}

      <p>{allClicks.join(' ')}</p>
    </div>
  )
}
```

注意，这里更新数组使用的是 `concat` 而非 `push`，这依然是函数式风格的做法，返回新状态而非修改旧状态。

## State Updates are Async
```jsx
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const [total, setTotal] = useState(0)

  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
    setTotal(left + right)
  }

  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
    setTotal(left + right)
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p>

      <p>total {total}</p>
    </div>
  )
}
```
在这里，当 `setLeft(left + 1)` 函数执行后，`left` 并没有立即更新为 `left + 1`，因此后续 `total` 的更新依赖的是旧的 `left` 值，故更新不正确。

在 React 中，所有的状态更新是在 **组件重渲染** 时发生的。

## Debugging React App
### Use `console.log`
不要：
```jsx
console.log('props value is ' + props)
```
这样只会输出 `props value is [Object object]`

应该：
```jsx
console.log('props value is', props)
```
这样就能打印完整的 `props`

## Use `debugger`
在代码中写下 debugger 命令，就可以在控制台的调试器中打断点，进而进行调试。

或者，使用 Chrome 浏览器的 React 开发者工具拓展，就可以在开发者工具中显示 React 组件状态等信息。![[Pasted image 20260330220030.png]]

## Rules of Hook
使用状态 Hook 有一些必须遵循的规则：
- 不能在 **循环、条件表达式或任何不是定义组件的函数** 中调用 `useState` 函数。这是为了确保 Hook 总是以相同的顺序被调用。

## Never define components inside components
**永远不要在组件内部定义组件**，这会导致很多问题。

## References
互联网上有很多 React 相关的资料。然而，因为我们使用的是 React 的新风格，所以对于我们来说，网上发现的大部分材料已经过时。

你可能会发现下面的链接很有用：
- [React官方文档](https://react.dev/learn)值得在某些时候查看，尽管它的大部分内容在课程的后期才会变得相关。另外，基于类的组件有关的一切都与我们无关。
- [Egghead.io](https://egghead.io/) 上的一些课程，比如 [Start learning React](https://egghead.io/courses/start-learning-react) 的质量很高，最近更新的 [Beginner's Guide to React](https://egghead.io/courses/the-beginner-s-guide-to-reactjs) 也比较好；两个课程介绍的概念也将在本课程的后面介绍。**注意**前者使用类式组件，后者使用新的函数式组件。

## Rendering Collections
在渲染 Array 时，通常需要用 `map` 将其映射为 HTML 标签的列表。

然而需要注意的是，渲染得到的每个标签/组件需要有各自 **唯一的** `key` 属性，使得 React 可以区分和追踪每个组件，这在重渲染和状态更新中非常重要。
```jsx
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>
            {note.content}
          </li>
        )}
      </ul>
    </div>
  )
}
```

要点：
- `key` 属性必须是唯一的，否则 React 无法区分各个组件。
- 不要用数组下标作为 `key`，因为数组本身可能会插入、删除元素，而 React 只会根据 `key` 区分组件，导致 React 误认。

## Refactoring Modules
将所有组件都定义在 App. jsx 中不是个好习惯。一般而言，对于较小的 app，组件模块会被存储在 `src/components` 目录下，文件名即为组件名。例如：
```
src/
├── main.jsx
├── App.jsx
└── components/           # Folder for reusable components
    ├── Footer.jsx        # File named after the component
    ├── Note.jsx
    └── Notification.jsx
```

在每个组件模块的最后一行，通过 `export` 语句进行导出：
```jsx
export default Note
```

在使用该组件的模块中，使用 `import` 进行导入：
```jsx
import Note from `./components/Note`
```
这里可以看出，模块文件拓展名 `.jsx` 是可省略的。

## Forms
也可以在组件中定义 HTML 表单元素，让组件变得可交互：
```jsx
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)


  const addNote = (event) => {
    event.preventDefault()
    console.log('button clicked', event.target)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>

      <form onSubmit={addNote}>
        <input />
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```

其中，`addNote` 为表单提交时的处理函数，`event.preventDefault()` 是为了防止表单提交的默认行为，其中包括防止页面刷新。

## Controlled Component
那么，我们如何访问表单的 `input` 元素呢？

一个方式是添加一个状态，专门用于存储用户输入：
```jsx
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState(
    'a new note...'
  ) 

  const addNote = (event) => {
    event.preventDefault()
    console.log('button clicked', event.target)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>

        <input value={newNote} />
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```
嘣！控制台发出警告，我们的 `input` 变成了只读元素。事实上，当我们将组件状态作为 `value` 属性时，该组件就 **控制** 了 `input` 组件的行为。

为了让被控制的 `input` 组件能被编辑，我们需要指定 `onChange` 对应的处理函数。
```jsx
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState(
    'a new note...'
  ) 

  // ...


  const handleNoteChange = (event) => {
    console.log(event.target.value)
    setNewNote(event.target.value)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>
        <input
          value={newNote}

          onChange={handleNoteChange}
        />
        <button type="submit">save</button>
      </form>   
    </div>
  )
}
```
注意，这里的 `onChange` 属性的处理函数 *不需要* 调用 `event.preventDefault()`，因为这个事件没有默认行为。

