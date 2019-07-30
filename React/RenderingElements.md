# 元素渲染 Rendering Elements
想要将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 ReactDOM.render()：
```
// 假设你的 HTML 文件某处有一个 <div>
<div id="root"></div>

const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```


# 更新已渲染的元素
计时器的例子
```
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```
- 在实践中，大多数 React 应用只会调用一次 ReactDOM.render()

