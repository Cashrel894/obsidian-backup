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

## Connecting the backend to a database
有时我们想指定数据对象转换为 JSON 的格式，可以修改 schema 的 `toJSON` 方法：
```js
noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})
```

此外，直接在 schema 定义的时候指定 `toJSON` 也是可以的。

## Moving db config to its own module
一般而言，数据库相关操作应当独立为单独的模块使用。这里，在后端根目录新建目录 `models`，添加一个 `note.js` 文件：
```js
const mongoose = require('mongoose')

mongoose.set('strictQuery', false)

const url = process.env.MONGODB_URI

console.log('connecting to', url)
mongoose.connect(url, { family: 4 })

  .then(result => {
    console.log('connected to MongoDB')
  })
  .catch(error => {
    console.log('error connecting to MongoDB:', error.message)
  })

const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})

module.exports = mongoose.model('Note', noteSchema)
```
其中，`MONGODB_URI` 被定义在环境变量中，可防止 url 和密钥泄露。

## Defining env vars using the dotenv lib
一种更复杂的定义环境变量的方式是使用 `dotenv` 库。我们在项目根目录创建一个 `.env` 文件：
```js
MONGODB_URI=mongodb+srv://fullstack:thepasswordishere@cluster0.a5qfl.mongodb.net/noteApp?retryWrites=true&w=majority&appName=Cluster0
PORT=3001
```

注意，这里将服务器端口 `PORT` 也编码进了 `.env` 中。

**`.env` 文件必须被 gitignored！**

只需要 `require('dotenv').config()`，我们就可以像访问系统环境变量一样访问 `.env` 中的变量：
```js
require('dotenv').config()
const express = require('express')
const Note = require('./models/note')

const app = express()
// ..

const PORT = process.env.PORT
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```
在这个例子中，应当确保 `dotenv` 在 `Note` 导入之前被导入。

## A true full stack developer's oath
It is again time for the exercises. The complexity of our app has now taken another step since besides frontend and backend we also have a database. There are indeed really many potential sources of error.

So we should once more extend our oath:

Full stack development is _extremely hard_, that is why I will use all the possible means to make it easier

- I will have my browser developer console open all the time
- I will use the network tab of the browser dev tools to ensure that frontend and backend are communicating as I expect
- I will constantly keep an eye on the state of the server to make sure that the data sent there by the frontend is saved there as I expect
- _I will keep an eye on the database: does the backend save data there in the right format_
- I progress with small steps
- I will write lots of _console. log_ statements to make sure I understand how the code behaves and to help pinpoint problems
- If my code does not work, I will not write more code. Instead, I start deleting the code until it works or just return to a state when everything was still working
- When I ask for help in the course Discord channel or elsewhere I formulate my questions properly, see [here](https://fullstackopen.com/en/part0/general_info#how-to-get-help-in-discord) how to ask for help