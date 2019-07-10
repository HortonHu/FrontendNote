# Jumbotron
Jumbotron（超大屏幕）会创建一个大的灰色背景框，里面可以设置一些特殊的内容和信息。

我们可以通过在 `<div>` 元素 中添加 `.jumbotron` 类来创建 `jumbotron`:

```
<div class="jumbotron">
  <h1>菜鸟教程</h1> 
  <p>学的不仅是技术，更是梦想！！！</p> 
</div>
```

# 全屏幕的 Jumbotron
如果你想创建一个没有圆角的全屏幕，可以在 `.jumbotron-fluid` 类里头的 div添加 `.container` 或 `.container-fluid` 类来实现:
```
<div class="jumbotron jumbotron-fluid">
  <div class="container">
      <h1>菜鸟教程</h1> 
      <p>学的不仅是技术，更是梦想！！！</p>
  </div>
</div>
```

![](img/Jumbotron.png)
