# 表单 form
Bootstrap 通过一些简单的 HTML 标签和扩展的类即可创建出不同样式的表单。

表单元素 `<input>`, `<textarea>`, 和 `<select>` `elements` 在使用 `.form-control` 类的情况下，宽度都是设置为 100%。


# 表单布局
- 堆叠表单 (全屏宽度)：垂直方向
- 内联表单：水平方向


## 堆叠表单

```
<div class="container">
  <h2>堆叠表单</h2>
  <form>
    <div class="form-group">
      <label for="email">Email:</label>
      <input type="email" class="form-control" id="email" placeholder="Enter email">
    </div>
    <div class="form-group">
      <label for="pwd">Password:</label>
      <input type="password" class="form-control" id="pwd" placeholder="Enter password">
    </div>
    <div class="form-check">
      <label class="form-check-label">
        <input class="form-check-input" type="checkbox"> Remember me
      </label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>
```

![](img/form-group.png)


## 内联表单
所有内联表单中的元素都是左对齐的。

注意：在屏幕宽度小于 576px 时为垂直堆叠，如果屏幕宽度大于等于576px时表单元素才会显示在同一个水平线上。

内联表单需要在 `<form>` 元素上添加 `.form-inline`类

![](img/form-inline.png)



# 表单控件
Bootstrap4 支持以下表单控件：
- input
- textarea
- checkbox
- radio
- select

## Input
Bootstrap 支持所有的 HTML5 输入类型: 
- text
- password
- datetime
- datetime-local
- date
- month
- time
- week
- number
- email
- url
- search
- tel
- color

如果 `input` 的 `type` 属性未正确声明，输入框的样式将不会显示。
```
<div class="container">
  <h2>表单控件: input</h2>
  <p>以下实例使用两个 input 元素，一个是 text，一个是 password ：</p>
  <form>
    <div class="form-group">
      <label for="usr">用户名:</label>
      <input type="text" class="form-control" id="usr">
    </div>
    <div class="form-group">
      <label for="pwd">密码:</label>
      <input type="password" class="form-control" id="pwd">
    </div>
  </form>
</div>
```


![](img/form-input.png)



## textarea

```
<div class="container">
  <h2>表单控件: textarea</h2>
  <p>以下实例演示了 textarea 的样式。</p>
  <form>
    <div class="form-group">
      <label for="comment">评论:</label>
      <textarea class="form-control" rows="5" id="comment"></textarea>
    </div>
  </form>
</div>
```

![](img/form-textarea.png)



## 复选框(checkbox)

```
<div class="container">
  <h2>表单控件: checkbox</h2>
  <p>以下实例包含了三个选项。最后一个是禁用的：</p>
  <form>
    <div class="form-check">
      <label class="form-check-label">
        <input type="checkbox" class="form-check-input" value="">Option 1
      </label>
    </div>
    <div class="form-check">
      <label class="form-check-label">
        <input type="checkbox" class="form-check-input" value="">Option 2
      </label>
    </div>
    <div class="form-check disabled">
      <label class="form-check-label">
        <input type="checkbox" class="form-check-input" value="" disabled>Option 3
      </label>
    </div>
  </form>
</div>
```

![](img/form-checkbot.png)

使用 `.form-check-inline` 类可以让选项显示在同一行上：

![](img/form-checkbot-inline.png)



## 单选框(Radio)
单选框用于让用户从一系列预设置的选项中进行选择，只能选一个。
```
<div class="container">
  <h2>表单控件: radio</h2>
  <p>以下实例包含了三个选项。最后一个是禁用的：</p>
  <form>
    <div class="radio">
      <label><input type="radio" name="optradio">Option 1</label>
    </div>
    <div class="radio">
      <label><input type="radio" name="optradio">Option 2</label>
    </div>
    <div class="radio disabled">
      <label><input type="radio" name="optradio" disabled>Option 3</label>
    </div>
  </form>
</div>
```

![](img/form-radio.png)


## select 下拉菜单

```
<div class="container">
  <h2>表单控件: select</h2>
  <p>以下表单包含了两个下拉菜单 (select 列表):</p>
  <form>
    <div class="form-group">
      <label for="sel1">单选下拉菜单:</label>
      <select class="form-control" id="sel1">
        <option>1</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
      </select>
      <br>
      <label for="sel2">多选下拉菜单(按住 shift 键，可以选取多个选项):</label>
      <select multiple class="form-control" id="sel2">
        <option>1</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
      </select>
    </div>
  </form>
</div>
```

![](img/form-options.png)




# 输入框组 input group
- 我们可以使用 `.input-group` 类来向表单输入框中添加更多的样式，如图标、文本或者按钮。
- 使用 `.input-group-prepend` 类可以在输入框的的前面添加文本信息， `.input-group-append` 类添加在输入框的后面。
- 最后，我们还需要使用 `.input-group-text` 类来设置文本的样式。

```
<div class="container mt-3">
  <h3>输入框组</h3>
  <p> .input-group .input-group 类来向表单输入框中添加更多的样式，如图标、文本或者按钮，.input-group-prepend 类添加在前面，.input-group-append 添加在后面。</p>
  <p>使用 .input-group-text 类来设置文本的样式。</p>
  
  <form action="/action_page.php">
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text">@</span>
      </div>
      <input type="text" class="form-control" placeholder="Username" id="usr" name="username">
    </div>

    <div class="input-group mb-3">
      <input type="text" class="form-control" placeholder="Your Email" id="mail" name="email">
      <div class="input-group-append">
        <span class="input-group-text">@runoob.com</span>
      </div>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>
```

![](img/input-group.png)



## 输入框大小
使用 `.input-group-sm` 类来设置小的输入框， `.input-group-lg` 类设置大的输入框：

```
<div class="container mt-3">
  <h1>输入框大小</h1>
  <p>使用 .input-group-sm 类来设置小的输入框， .input-group-lg 类设置大的输入框：</p>
  <form>
    <div class="input-group mb-3 input-group-sm">
      <div class="input-group-prepend">
        <span class="input-group-text">Small</span>
      </div>
      <input type="text" class="form-control">
    </div>
  </form>
  <form>
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text">Default</span>
      </div>
      <input type="text" class="form-control">
    </div>
  </form>
  <form>
    <div class="input-group mb-3 input-group-lg">
      <div class="input-group-prepend">
        <span class="input-group-text">Large</span>
      </div>
      <input type="text" class="form-control">
    </div>
  </form>
</div>
```

![](img/input-group-size.png)



## 多个输入框和文本
```
<div class="container mt-3">
  <h3>多个输入框和文本</h3>
  
  <form>
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text">Person</span>
      </div>
      <input type="text" class="form-control" placeholder="First Name">
      <input type="text" class="form-control" placeholder="Last Name">
    </div>  
  </form>
  
  <form>
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text">One</span>
        <span class="input-group-text">Two</span>
        <span class="input-group-text">Three</span>
      </div>
      <input type="text" class="form-control">
    </div>
  </form>
</div>
```

![](img/input-group-text.png)



## 复选框与单选框
```
<div class="container mt-3">
  <h3>复选框与单选框</h3>
  <p>文本信息可以使用复选框与单选框替代：</p>
  <form>
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <div class="input-group-text">
          <input type="checkbox"> 
        </div>
      </div>
      <input type="text" class="form-control" placeholder="RUNOOB">
    </div>
  </form>
  
  <form>
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <div class="input-group-text">
          <input type="radio"> 
        </div>
      </div>
      <input type="text" class="form-control" placeholder="GOOGLE">
    </div>
  </form>
</div>
```

![](img/input-checkbox-radio.png)



## 输入框添加按钮组
```
<div class="container mt-3">
  <h1>输入框添加按钮组</h1>
  <div class="input-group mb-3">
    <div class="input-group-prepend">
      <button class="btn btn-outline-secondary" type="button">Basic Button</button>  
    </div>
    <input type="text" class="form-control" placeholder="Some text">
  </div>

  <div class="input-group mb-3">
    <input type="text" class="form-control" placeholder="Search">
    <div class="input-group-append">
      <button class="btn btn-success" type="submit">Go</button>  
     </div>
  </div>

  <div class="input-group mb-3">
    <input type="text" class="form-control" placeholder="Something clever..">
    <div class="input-group-append">
      <button class="btn btn-primary" type="button">OK</button>  
      <button class="btn btn-danger" type="button">Cancel</button>  
     </div>
  </div>
</div>
```

![](img/input-group-button.png)



## 设置下拉菜单
输入框中添加下拉菜单不需要使用 `.dropdown` 类。
```
<div class="container mt-3">
  <h3>设置下拉菜单</h3>
  <p>输入框中添加下拉菜单不需要使用 .dropdown 类。</p>
  <form>
    <div class="input-group mt-3 mb-3">
      <div class="input-group-prepend">
        <button type="button" class="btn btn-outline-secondary dropdown-toggle" data-toggle="dropdown">
          选择网站
        </button>
        <div class="dropdown-menu">
          <a class="dropdown-item" href="https://www.google.com">GOOGLE</a>
          <a class="dropdown-item" href="https://www.runoob.com">RUNOOB</a>
          <a class="dropdown-item" href="https://www.taobao.com">TAOBAO</a>
        </div>
      </div>
      <input type="text" class="form-control" placeholder="网站地址">
    </div>
  </form>
</div>
```

![](img/input-dropdown.png)



## 输入框组标签
在输入框组通过在输入框组外围的 `label` 来设置标签，标签的 `for` 属性需要与输入框组的 `id` 对应，点击标签后可以聚焦输入框：

```
<div class="container mt-3">
  <h2>输入框组标签</h2>
  <p>在输入框组通过在输入框组外围的 label 来设置标签，标签的 for 属性需要与输入框组的 id 对应。</p>
  <p>点击标签后可以聚焦输入框：</p>
  <form>
    <label for="demo">这里输入您的邮箱:</label>
    <div class="input-group mb-3">
      <input type="text" class="form-control" placeholder="Email" id="demo" name="email">
      <div class="input-group-append">
        <span class="input-group-text">@runoob.com</span>
      </div>
    </div>
  </form>
</div>
```

![](img/input-group-label.png)
