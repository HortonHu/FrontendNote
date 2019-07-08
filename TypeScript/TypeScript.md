# Install
npm install -g typescript

 
## Example
`tsc greeter.ts` 生成js文件
```
// greeter.ts
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";
document.body.innerHTML = greeter(user);
```

加上string的限制
```
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.innerHTML = greeter(user);
```
此时如果 `let user = [0, 1, 2];` 就会tsc编译报错



