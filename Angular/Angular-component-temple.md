# 组件 component & 模板 template
## Component的组成
- `selector` 一旦在模板 HTML 中找到了这个选择器对应的标签，就创建并插入该组件的一个实例。
- `templateUrl` 该组件的 HTML 模板文件相对于这个组件文件的地址。 另外，你还可以用 `template` 属性的值来提供内联的 HTML 模板
- `styleUrls` 该组件的 `Style` 模板文件相对于这个组件文件的地址。 另外，你还可以内联的 `Style` 模板 或者直接为空
- `Providers`: 当前组件所需的服务的一个数组


## 模板语法
-  `<script>` 元素，它被禁用了
- 有些合法的 `HTML` 被用在模板中是没有意义的。`<html>、<body> 和 <base>` 元素这个舞台上中并没有扮演有用的角色。剩下的所有元素基本上就都一样用了。
