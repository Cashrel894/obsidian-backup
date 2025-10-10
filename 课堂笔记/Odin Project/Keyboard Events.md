#OdinProject 
https://www.javascripttutorial.net/javascript-dom/javascript-keyboard-events/

以下是对您提供网址（`https://www.javascripttutorial.net/javascript-dom/javascript-keyboard-events/`）内容的简要概括，该网页主要介绍 **JavaScript 键盘事件** 及其应用。

---

### **1. 键盘事件概述**
键盘事件是指当用户在键盘上按下、松开或持续按键时触发的事件。页面通过这些事件可以获取用户的键盘输入，并作出响应。适用于表单处理、自定义快捷键等功能。

---

### **2. 常见键盘事件**
解释了 JavaScript 提供的三种主要键盘事件及其用途：
- **`keydown`**: 当用户按下键盘上的任意键时触发（键未抬起）。
- **`keypress`**: 在键按下时触发，并且仅适用于字符键（已废弃，建议使用 `keydown` 代替）。
- **`keyup`**: 当用户释放键盘上的按键时触发。

---

### **3. 键盘事件的使用**
- **绑定键盘事件**: 使用 `addEventListener()` 方法绑定键盘事件到指定的 DOM 元素（如文档或输入字段）。
- **事件对象（`event`）**: 提供了有关事件的详细信息，包括按键的值或代码。

常见属性：
1. **`event.key`**:
   表示被按下的键的值，返回具体字符（如 `'a'`、`'Enter'`）。
2. **`event.code`**:
   表示键的位置键值（如 `'KeyA'`、`'Space'`），与键盘布局无关。
3. **`event.shiftKey`、`event.ctrlKey`、`event.altKey`、`event.metaKey`**:
   布尔值，用于检测按键时是否同时按下了 Shift、Ctrl、Alt 或 Meta 键。

---

### **4. 键盘响应示例**
通过一些代码示例展示了如何处理键盘事件：

#### 示例 1：检测按下的键
```javascript
document.addEventListener('keydown', (event) => {
  console.log(`Key pressed: ${event.key}`);
});
```

#### 示例 2：实现快捷键
实现按下指定组合键来执行特定功能（如 Ctrl+C 或 Alt+D）：
```javascript
document.addEventListener('keydown', (event) => {
  if (event.ctrlKey && event.key === 'c') {
    console.log('复制命令被触发');
  }
});
```

#### 示例 3：限制用户输入
通过监听输入字段的键盘事件逻辑，限制用户只能输入数字：
```javascript
const input = document.querySelector('input');

input.addEventListener('keydown', (event) => {
  if (!/[0-9]/.test(event.key) && event.key !== 'Backspace') {
    event.preventDefault(); // 阻止非数字键的输入
  }
});
```

---

### **5. 防止默认行为**
使用 `event.preventDefault()` 阻止某些按键的默认行为，例如防止按下 Backspace 时页面返回上一页：
```javascript
document.addEventListener('keydown', (event) => {
  if (event.key === 'Backspace') {
    event.preventDefault(); // 防止返回上一页
  }
});
```

---

### **6. `keydown` 与 `keyup` 的对比**
- **`keydown`**: 在按下键时**立即**触发，适合用于实时响应（如监听方向键来移动元素）。
- **`keyup`**: 在释放键时触发，适合用于检测完整动作（如确认输入）。

---

### **7. 常见应用场景**
浏览器中键盘事件的典型应用包括：
- 表单验证：实时检查用户输入。
- 快捷键：捕捉特定键组合以触发功能（如 Ctrl+S 保存）。
- 游戏控制：通过方向键或其他键控制游戏中的角色。
- 键盘导航：允许使用键盘操控网页功能。

---

### **总结**
该网页完整介绍了键盘事件的基础知识和常见应用场景，强调了 **`keydown`** 和 **`keyup`** 的重要性，以及结合键值（`event.key` 和 `event.code`）使用的实际操作。文章内容适合初学者，希望学习如何通过键盘事件增强网页功能与交互性。