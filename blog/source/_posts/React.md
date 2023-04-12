---
title: React
date: 2023-04-11 22:02:15
tag:  front-end
---
- React 允许你创建组件，**应用程序的可复用 UI 元素。**
- 在 React 应用程序中，每一个 UI 模块都是一个组件。
- React 是常规的 JavaScript 函数，除了：
   1. 它们的名字总是以大写字母开头。
   2. 它们返回 JSX 标签。

例子：
```javascript
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>了不起的科学家</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}

```
## JSX规则：

1. 只能返回一个根元素：一个组件中含有多个标签，得用一个父元素将他们包裹起来，例如：
```html
<div>
  <h1>海蒂·拉玛的代办事项</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
    >
  <ul>
    ...
  </ul>
</div>
<!--或者你可以选择用<></>  -->
<>
  <h1>海蒂·拉玛的代办事项</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</>
```

2. 标签必须闭合：如img等自闭合标签必须写成<img/>
```html
<>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
   />
  <ul>
      <li>发明一种新式交通信号灯</li>
      <li>排练一个电影场景</li>
      <li>改进频谱技术</li>
  </ul>
</>
```

3. 使用驼峰式命名给~~所有~~大部分属性命名：因为JSX语法底层还是转化为JS代码，所以命名不允许出现-以及class，得用驼峰式命名和className代
```html
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  className="photo"
/>
```

4. {}内可以使用JS变量，JS函数以及JS对象
```javascript
//函数
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>To Do List for {formatDate(today)}</h1>
  );
}
```
```javascript
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}

```
JSX总结：

- JSX 引号内的值会作为字符串传递给属性。
- 大括号让你可以将 JavaScript 的逻辑和变量带入到标签中。
- 它们会在 JSX 标签中的内容区域或紧随属性的 = 后起作用。
- {{ 和 }} 并不是什么特殊的语法：它只是包在 JSX 大括号内的 JavaScript 对象
## props
props是在父组件和子组件中传递的内容，好比JS中传入函数的参数，父组件也可以给子组件传递参数
第一步：将props传递给子组件
```javascript
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```
第二步：在子组件中读取props
```javascript
function Avatar({ person, size }) { //不要忘记{}
  // 在这里 person 和 size 是可访问的
}
```

**P.S 渲染列表的时候要记得给每一个列表一个key**

## 关于组件的纯粹：

- 一个组件必须是纯粹的，就意味着：
   - **只负责自己的任务。** 不应更改渲染前存在的任何对象或变量。
   - **输入相同，则输出相同。** 给定相同的输入，组件应该总是返回相同的 JSX。
- 渲染随时可能发生，因此组件不应依赖于彼此的渲染顺序。
- 你不应该改变组件用于渲染的任何输入。这包括 props、state 和 context。通过setEffect 来更新界面，而不要改变预先存在的对象。
- 努力在你返回的 JSX 中表达你的组件逻辑。当你需要“改变事物”时，你通常希望在事件处理程序中进行。作为最后的手段，你可以使用 useEffect。
- 编写纯函数需要一些练习，但它充分释放了 React 范式的能力。
## 响应事件
### 添加响应事件：
如需添加一个事件处理函数，你需要先定义一个函数，然后 [将其作为 prop 传入](https://react.docschina.org/learn/passing-props-to-a-component) 合适的 JSX 标签。
```javascript
export default function Button() {
  return (
    <button>
      未绑定任何事件
    </button>
  );
}
```
按照如下三个步骤，即可让它在用户点击时显示消息：

1. 在 Button 组件 _内部_ 声明一个名为 handleClick 的函数。
2. 实现函数内部的逻辑（使用 alert 来显示消息）。
3. 添加 onClick={handleClick} 到 <button> JSX 中。
```javascript
export default function Button() {
  function handleClick() {
    alert('你点击了我！');
  }

  return (
    <button onClick={handleClick}>
      点我
    </button>
  );
}
```
> 按照惯例，通常将事件处理程序命名为 handle，后接事件名。你会经常看到 onClick={handleClick}，onMouseEnter={handleMouseEnter} 等。

### 阻止事件传播：
如果你想阻止一个事件到达父组件，你需要像下面 Button 组件那样调用 e.stopPropagation() 
### 阻止默认事件
你可以调用事件对象中的 e.preventDefault() 来阻止这种情况发生
### 摘要：

- 你可以通过将函数作为 prop 传递给元素如 <button> 来处理事件。
- 必须传递事件处理函数，**而非函数调用！** onClick={handleClick} ，不是 onClick={handleClick()}。
- 你可以单独或者内联定义事件处理函数。
- 事件处理函数在组件内部定义，所以它们可以访问 props。
- 你可以在父组件中定义一个事件处理函数，并将其作为 prop 传递给子组件。
- 你可以根据特定于应用程序的名称定义事件处理函数的 prop。
- 事件会向上传播。通过事件的第一个参数调用 e.stopPropagation() 来防止这种情况。
- 事件可能具有不需要的浏览器默认行为。调用 e.preventDefault() 来阻止这种情况。
- 从子组件显式调用事件处理函数 prop 是事件传播的另一种优秀替代方案。
## 关于State

- 当一个组件需要在多次渲染间“记住”某些信息时使用 state 变量。
- State 变量是通过调用 useState Hook 来声明的。
- Hook 是以 use 开头的特殊函数。它们能让你 “hook” 到像 state 这样的 React 特性中。
- Hook 可能会让你想起 import：它们需要在非条件语句中调用。调用 Hook 时，包括 useState，仅在组件或另一个 Hook 的顶层被调用才有效。
- useState Hook 返回一对值：当前 state 和更新它的函数。
- 你可以拥有多个 state 变量。在内部，React 按顺序匹配它们。
- State 是组件私有的。如果你在两个地方渲染它，则每个副本都有独属于自己的 state。

使用方法：
```javascript
import { useState } from 'react'; //要添加 state 变量，先从文件顶部的 React 中导入 useState

const [index, setIndex] = useState(0); //创建state状态

function handleClick() {
  setIndex(index + 1);
} //改变状态
```
**请记住，必须在条件语句外并且始终以相同的顺序调用 Hook！**
## 渲染：

- 在一个 React 应用中一次屏幕更新都会发生以下三个步骤：
   1. 触发
   2. 渲染
   3. 提交
- 您可以使用严格模式去找到组件中的错误
- 如果渲染结果与上次一样，那么 React 将不会修改 DOM
- 设置 state 只会为 _下一次_ 渲染变更 state 的值
- 批处理渲染：
   - 设置 state 不会更改现有渲染中的变量，但会请求一次新的渲染。	
   - React 会在事件处理函数执行完成之后处理 state 更新。这被称为批处理。
   - 要在一个事件中多次更新某些 state，你可以使用 setNumber(n => n + 1) 更新函数。
## 更新状态中的对象
State 可以保存任何类型的 JavaScript 值，包括对象。但是你不应该直接改变你在 React 状态下持有的对象。相反，当你想更新一个对象时，你需要创建一个新对象（或复制一个现有对象），然后设置状态以使用该副本。
```javascript
onPointerMove={e => {
  position.x = e.clientX;
  position.y = e.clientY;
}} //错误示范


onPointerMove={e => {
  setPosition({
    x: e.clientX,
    y: e.clientY
  });
}}//应该这样
```
你也可以使用`...`对象扩展语法
```javascript
setPerson({
  ...person, // Copy the old fields
  firstName: e.target.value // But override this one
});
```
但一碰到嵌套的对象，复制一个新对象就显得非常麻烦
所以我们可以使用`Immer`写更简洁的更新
步骤：

1. 运行 npm install use-immer 将 Immer 添加为依赖项
2. 然后将 import { useState } from 'react' 替换为 import { useImmer } from 'use-immer'
3. 将const [] = useState() 变成 const [] = useImmer() 
4. 再使用update(draft=>{draft.name = xxx}) 进行修改

**摘要**：

- Treat all state in React as immutable.
将 React 中的所有状态视为不可变的。
- When you store objects in state, mutating them will not trigger renders and will change the state in previous render “snapshots”.
当您将对象存储在状态中时，改变它们不会触发渲染，并且会更改先前渲染“快照”中的状态。
- Instead of mutating an object, create a _new_ version of it, and trigger a re-render by setting state to it.
与其改变对象，不如创建它的新版本，并通过为其设置状态来触发重新渲染。
- You can use the {...obj, something: 'newValue'} object spread syntax to create copies of objects.
您可以使用 {...obj, something: 'newValue'} 对象传播语法来创建对象的副本。
- Spread syntax is shallow: it only copies one level deep.
传播语法很浅：它只复制一层深。
- To update a nested object, you need to create copies all the way up from the place you’re updating.
要更新嵌套对象，您需要从要更新的地方一直向上创建副本。
- To reduce repetitive copying code, use Immer.
要减少重复复制代码，请使用 Immer。
## 更新数组：
更新数组和对象一样，需要复制再修改
善于利用map，fliter，slice等数组方法
例子：
```javascript
function handleIncreaseClick(productId) {
    setProducts(products.map(product=>{
      if(productId===product.id){
        return{
          ...product,
          count : product.count+1
        }
      }else{
        return product
      }
    }))
  } //id+1
```

