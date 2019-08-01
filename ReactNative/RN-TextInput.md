# 处理文本输入
TextInput是一个允许用户输入文本的基础组件。
- `onChangeText`，此属性接受一个函数，而此函数会在文本变化时被调用。
- `onSubmitEditing`，会在文本被提交后（用户按下软键盘上的提交键）调用。

```
import React, { Component } from 'react';
import { AppRegistry, Text, TextInput, View } from 'react-native';

export default class PizzaTranslator extends Component {
  constructor(props) {
    super(props);
    this.state = {text: ''};
  }

  render() {
    return (
        <View style={{flex: 1, flexDirection: 'column', justifyContent: 'center', alignItems: 'center'}}>
          <TextInput
              style={{height: 40}}
              placeholder="Type here to translate!"
              onChangeText={(text) => this.setState({text})}
          />
          <Text style={{padding: 10, fontSize: 42}}>
            {this.state.text.split(' ').map((word) => word && '🍕').join(' ')}
          </Text>
        </View>
    );
  }
}

```
