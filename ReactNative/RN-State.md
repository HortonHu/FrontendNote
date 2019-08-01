# State（状态）
我们使用两种数据来控制一个组件：props和state。
- props是在父组件中指定，而且一经指定，在被指定的组件的生命周期中则不再改变。 
- 对于需要改变的数据，我们需要使用state。
- 需要在 constructor 中初始化state（译注：这是 ES6 的写法，早期的很多 ES5 的例子使用的是 getInitialState 方法来初始化 state，这一做法会逐渐被淘汰），然后在需要修改时调用setState方法

假如我们需要制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个prop。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到state中。
```
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Blink extends Component {
  constructor(props) {
    super(props);
    this.state = { isShowingText: true };

    // 每1000毫秒对showText状态做一次取反操作
    setInterval(() => {
      this.setState(previousState => {
        return { isShowingText: !previousState.isShowingText };
      });
    }, 1000);
  }

  render() {
    // 根据当前showText的值决定是否显示text内容
    if (!this.state.isShowingText) {
      return null;
    }

    return (
      <Text>{this.props.text}</Text>
    );
  }
}

export default class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text='I love to blink' />
        <Blink text='Yes blinking is so great' />
        <Blink text='Why did they ever take this out of HTML' />
        <Blink text='Look at me look at me look at me' />
      </View>
    );
  }
}
```

- 一切界面变化都是状态state变化
- state的修改必须通过setState()方法
    - this.state.likes = 100; // 这样的直接赋值修改无效！
    - setState 是一个 merge 合并操作，只修改指定属性，不影响其他属性
    - setState 是异步操作，修改不会马上生效
