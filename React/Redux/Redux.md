# Installation
```
npm install redux
```

## Complementary Packages
```
npm install react-redux
npm install --save-dev redux-devtools
```

# Usage
```
npx create-react-app my-app --template redux
```

# Core Concepts


# Three Principles
## 单一数据源
整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

## State 是只读的
唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。

这样确保了视图和网络请求都不能直接修改 state，相反它们只能表达想要修改的意图。因为所有的修改都被集中化处理，且严格按照一个接一个的顺序执行，因此不用担心竞态条件（race condition）的出现。 Action 就是普通对象而已，因此它们可以被日志打印、序列化、储存、后期调试或测试时回放出来。

## 使用`pure functions`来执行修改
为了描述 action 如何改变 state tree ，你需要编写 `pure reducers`。

Remember to return new state objects, instead of mutating the previous state. 


# Learning Resources
https://redux.js.org/introduction/learning-resources


# Examples
https://redux.js.org/introduction/examples