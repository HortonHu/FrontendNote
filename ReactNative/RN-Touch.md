# 处理触摸事件
## 按钮
```
<Button
  onPress={() => {
    Alert.alert("你点击了按钮！");
  }}
  title="点我！"
/>
```
上面这段代码会在 iOS 上渲染一个蓝色的标签状按钮，在 Android 上则会渲染一个蓝色圆角矩形带白字的按钮。点击这个按钮会调用"onPress"函数，具体作用就是显示一个 alert 弹出框。你还可以指定"color"属性来修改按钮的颜色。
- button无法使用style prop 可以用自定义的TouchableOpacity 来代替button

多个button
```
import React, { Component } from 'react';
import { Alert, Button, StyleSheet, View } from 'react-native';

export default class ButtonBasics extends Component {
  _onPressButton() {
    Alert.alert('You tapped the button!')
  }

  render() {
    return (
        <View style={styles.container}>
          <View style={styles.buttonContainer}>
            <Button
                onPress={this._onPressButton}
                title="Press Me"
            />
          </View>
          <View style={styles.buttonContainer}>
            <Button
                onPress={this._onPressButton}
                title="Press Me"
                color="#841584"
            />
          </View>
          <View style={styles.alternativeLayoutButtonContainer}>
            <Button
                onPress={this._onPressButton}
                title="This looks great!"
            />
            <Button
                onPress={this._onPressButton}
                title="OK!"
                color="#841584"
            />
          </View>
        </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
  },
  buttonContainer: {
    margin: 20
  },
  alternativeLayoutButtonContainer: {
    margin: 20,
    flexDirection: 'row',
    justifyContent: 'space-between'
  }
});
```

## Touchable 系列组件
- 一般来说，你可以使用TouchableHighlight来制作按钮或者链接。注意此组件的背景会在用户手指按下时变暗。
- 在 Android 上还可以使用TouchableNativeFeedback，它会在用户手指按下时形成类似墨水涟漪的视觉效果。
- TouchableOpacity会在用户手指按下时降低按钮的透明度，而不会改变背景的颜色。
- 如果你想在处理点击事件的同时不显示任何视觉反馈，则需要使用TouchableWithoutFeedback。
- 某些场景中你可能需要检测用户是否进行了长按操作。可以在上面列出的任意组件中使用onLongPress属性来实现。
  
```
import React, { Component } from 'react';
import { Alert, AppRegistry, Platform, StyleSheet, Text, TouchableHighlight, TouchableOpacity, TouchableNativeFeedback, TouchableWithoutFeedback, View } from 'react-native';

export default class Touchables extends Component {
  _onPressButton() {
    Alert.alert('You tapped the button!')
  }

  _onLongPressButton() {
    Alert.alert('You long-pressed the button!')
  }


  render() {
    return (
      <View style={styles.container}>
        <TouchableHighlight onPress={this._onPressButton} underlayColor="white">
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableHighlight</Text>
          </View>
        </TouchableHighlight>
        <TouchableOpacity onPress={this._onPressButton}>
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableOpacity</Text>
          </View>
        </TouchableOpacity>
        <TouchableNativeFeedback
            onPress={this._onPressButton}
            background={Platform.OS === 'android' ? TouchableNativeFeedback.SelectableBackground() : ''}>
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableNativeFeedback</Text>
          </View>
        </TouchableNativeFeedback>
        <TouchableWithoutFeedback
            onPress={this._onPressButton}
            >
          <View style={styles.button}>
            <Text style={styles.buttonText}>TouchableWithoutFeedback</Text>
          </View>
        </TouchableWithoutFeedback>
        <TouchableHighlight onPress={this._onPressButton} onLongPress={this._onLongPressButton} underlayColor="white">
          <View style={styles.button}>
            <Text style={styles.buttonText}>Touchable with Long Press</Text>
          </View>
        </TouchableHighlight>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    paddingTop: 60,
    alignItems: 'center'
  },
  button: {
    marginBottom: 30,
    width: 260,
    alignItems: 'center',
    backgroundColor: '#2196F3'
  },
  buttonText: {
    padding: 20,
    color: 'white'
  }
});

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => Touchables);

```
