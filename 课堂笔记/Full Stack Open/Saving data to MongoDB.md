#FullStackOpen 
## Mongoose
```js
const mongoose = require('mongoose')

if (process.argv.length < 3) {
  console.log('give password as argument')
  process.exit(1)
}

const password = process.argv[2]

const url = `mongodb+srv://fullstack:${password}@cluster0.a5qfl.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0`

mongoose.set('strictQuery',false)

mongoose.connect(url, { family: 4 })

const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

const Note = mongoose.model('Note', noteSchema)

const note = new Note({
  content: 'HTML is easy',
  important: true,
})

note.save().then(result => {
  console.log('note saved!')
  mongoose.connection.close()
})
```

其中，这个指令建立了与数据库的连接：
```js
mongoose.connect(url, { family: 4 })
```
`{family: 4}` 指定了 IPv4 连接，因为 MongoDB Atlas 只支持 IPv4。

### Schema
建立连接后，我们为数据建立 **模式**(Schema) 以及相应的 **模型**(Model)：
```js
const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

const Note = mongoose.model('Note', noteSchema)
```

模式定义了该数据对象如何存储。模型应命名为单数名词，而对应的集合被命名为复数名词。

注意，MongoDB 本身是无模式的，模式是由应用层提供的功能。

## Creating and Saving Objects
借助 `Model` 可以轻松创建新的数据对象：
```js
const note = new Note({
  content: 'HTML is Easy',
  important: false,
})
```
Mongoose 中，`Model` 是一种构造函数，包含了将对象保存到数据库等功能的方法。

使用 `save` 可以保存对象，并允许接受一个异步事件处理函数：
```js
note.save().then(result => {
  console.log('note saved!')
  mongoose.connection.close()
})
```
这里，对象保存后，就调用 `mongoose.connection.close()` 关闭了数据库。

## Fetching objects from the database
通过 `find({})` 获取全部数据：
```js
Note.find({}).then(result => {
  result.forEach(note => {
    console.log(note)
  })
  mongoose.connection.close()
})
```

也可以在 `{}` 添加一些约束条件，详见 https://www.mongodb.com/docs/manual/tutorial/query-documents/ 。
