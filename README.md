# 设置快捷键

[![](https://img.shields.io/github/issues/jaywcjlove/hotkeys.svg)](https://github.com/jaywcjlove/hotkeys/issues) [![](https://img.shields.io/github/forks/jaywcjlove/hotkeys.svg)](https://github.com/jaywcjlove/hotkeys/network) [![](https://img.shields.io/github/stars/jaywcjlove/hotkeys.svg)](https://github.com/jaywcjlove/hotkeys/stargazers) [![](https://img.shields.io/github/release/jaywcjlove/hotkeys.svg)](https://github.com/jaywcjlove/hotkeys/releases)

这是一个强健的 Javascript 库用于捕获键盘输入和输入的组合键，它没有依赖，压缩只有只有(~3kb)，gzip:1.9k。`hotkey` 可以算是临摹参考[madrobby/keymaster](https://github.com/madrobby/keymaster)的作品，重写了一遍，修复多个兼容问题，键支持，添加UMD支持和 **测试用例**，[官方文档DEMO预览](http://jaywcjlove.github.io/hotkeys/?lang=cn) [En](http://jaywcjlove.github.io/hotkeys/?lang=en)

[![jaywcjlove/sb](https://jaywcjlove.github.io/sb/lang/chinese.svg)](http://jaywcjlove.github.io/hotkeys/?lang=cn) [![jaywcjlove/sb](https://jaywcjlove.github.io/sb/lang/english.svg)](http://jaywcjlove.github.io/hotkeys/?lang=en)

```
  __            __    __                         
 |  |--..-----.|  |_ |  |--..-----..--.--..-----.
 |     ||  _  ||   _||    < |  -__||  |  ||__ --|
 |__|__||_____||____||__|__||_____||___  ||_____|
                                   |_____|   
```

## 创建

您将需要在您的系统上安装的 Node.js。

```sh
# bower 安装
$ bower install hotkeysjs

# npm 安装
$ npm install hotkeys-js

$ npm run build # 编译
$ npm run watch # 开发模式
```

## React中使用

[react-hotkeys](https://github.com/jaywcjlove/react-hotkeys)，安装如下：

```sh
npm i -S react-hot-keys
```

例子

```js
import React, { Component } from 'react';
import Hotkeys from 'react-hot-keys';

export default class HotkeysDemo extends Component {
  constructor(props) {
    super(props);
    this.state = {
      output: 'Hello, I am a component that listens to keydown and keyup of a',
    }
  }
  onKeyUp(keyName, e, handle) {
    console.log("test:onKeyUp", e, handle)
    this.setState({
      output: `onKeyUp ${keyName}`,
    });
  }
  onKeyDown(keyName, e, handle) {
    console.log("test:onKeyDown", keyName, e, handle)
    this.setState({
      output: `onKeyDown ${keyName}`,
    });
  }
  render() {
    return (
      <Hotkeys 
        keyName="shift+a,alt+s" 
        onKeyDown={this.onKeyDown.bind(this)}
        onKeyUp={this.onKeyUp.bind(this)}
      >
        <div style={{ padding: "50px" }}>
          {this.state.output}
        </div>
      </Hotkeys>
    )
  }
}
```

## 使用 

传统调用

```
<script type="text/javascript" src="./js/hotkeys.js"></script>
```

包加载

```js
import hotkeys from 'hotkeys-js';

hotkeys('shift+a,alt+d, w', function(e){
    console.log('干点活儿',e);
    if(hotkeys.shift) console.log('大哥你摁下了 shift 键！');
    if(hotkeys.ctrl) console.log('大哥你摁下了 ctrl 键！');
    if(hotkeys.alt) console.log('大哥你摁下了 alt 键！');
});
```


## 定义快捷键

```js
// 定义a快捷键
hotkeys('a', function(event,handler){
    //event.srcElement: input 
    //event.target: input
    if(event.target === "input"){
        alert('你在输入框中按下了 a!')
    }
    alert('你按下了 a!') 
});

// 定义a快捷键
hotkeys('ctrl+a,ctrl+b,r,f', function(event,handler){
    switch(handler.key){
        case "ctrl+a":alert('你按下了ctrl+a!');break;
        case "ctrl+b":alert('你按下了ctrl+b!');break;
        case "r":alert('你按下了r!');break;
        case "f":alert('你按下了f!');break;
    }
    //handler.scope 范围
});

// 返回false将停止活动，并阻止默认浏览器事件
hotkeys('ctrl+r', function(){ alert('停止刷新!'); return false });

// 多个快捷方式做同样的事情
hotkeys('⌘+r, ctrl+r', function(){ });

// 对所有摁键执行任务
hotkeys('*','wcj', function(e){
    console.log('干点活儿',e);
    console.log("key.getScope()::",hotkeys.getScope());
    if(hotkeys.shift) console.log('大哥你摁下了 shift 键！');
    if(hotkeys.ctrl) console.log('大哥你摁下了 ctrl 键！');
    if(hotkeys.alt) console.log('大哥你摁下了 alt 键！');
});
```

## 支持的键

`⇧`, `shift`, `option`, `⌥`, `alt`, `ctrl`, `control`, `command`, `⌘`。 

`⌘` Command()  
`⌃` Control  
`⌥` Option(alt)  
`⇧` Shift  
`⇪` Caps Lock(大写)   
~~`fn` 功能键就是fn(不支持)~~  
`↩︎` return/enter
`space` 空格键

## 修饰键判断
可以对下面的修饰键判断 `shift` `alt` `option` `ctrl` `control` `command`，特别注意`+`和`=`键值相同，组合键设置`⌘+=`

```js
hotkeys('shift+a,alt+d, w', function(e){
    console.log('干点活儿',e);
    if(hotkeys.shift) console.log('大哥你摁下了 shift 键！');
    if(hotkeys.ctrl) console.log('大哥你摁下了 ctrl 键！');
    if(hotkeys.alt) console.log('大哥你摁下了 alt 键！');
});
```

## 切换快捷键

如果在单页面在不同的区域，相同的快捷键，干不同的事儿，之间来回切换。O(∩_∩)O ！

```js
// 一个快捷键，有可能干的活儿不一样哦
hotkeys('ctrl+o, ctrl+alt+enter', 'scope1', function(){
    console.log('你好看');
});
hotkeys('ctrl+o, enter', 'scope2', function(){ 
    console.log('你好丑陋啊！');
});
// 你摁 “ctrl+o”组合键
// 当scope等于 scope1 ，执行 回调事件打印出 “你好看”，
// 当scope等于 scope2 ，执行 回调事件打印出 “你好丑陋啊！”，

// 通过setScope设定范围scope 
hotkeys.setScope('scope1'); // 默认所有事儿都干哦 
```

## 删除标记快捷键

删除区域范围标记

```js
hotkeys.deleteScope('issues');
```

## 解除绑定

`hotkeys.unbind("ctrl+o, ctrl+alt+enter")` 解除绑定两组快捷键  
`hotkeys.unbind("ctrl+o","files")` 解除绑定名字叫files钟的一组快捷键  


## 键判断

判断摁下的键是否为某个键

```js
hotkeys('a', function(){
    console.log(hotkeys.isPressed("A")); //=> true
    console.log(hotkeys.isPressed(65)); //=> true
});
```

## 获取摁下键值

获取摁下绑定键的键值 `hotkeys.getPressedKeyCodes()`

```js
hotkeys('command+ctrl+shift+a,f', function(){
    console.log(hotkeys.getPressedKeyCodes()); //=> [17, 65] 或者 [70]
})
```


## 过滤
`INPUT`  `SELECT` `TEXTAREA` 默认不处理。  
`hotkeys.filter` 返回 `true` 快捷键设置才会起作用，`flase` 快捷键设置失效。   

```javascript
hotkeys.filter = function(event){
  return true;
}
//如何增加过滤可编辑标签 <div contentEditable="true"></div>
//contentEditable老浏览器不支持滴 
hotkeys.filter = function(event) {
    var tagName = (event.target || event.srcElement).tagName;
    return !(tagName.isContentEditable || 
    tagName == 'INPUT' || 
    tagName == 'SELECT' || 
    tagName == 'TEXTAREA');
}

//
hotkeys.filter = function(event){
    var tagName = (event.target || event.srcElement).tagName;
    hotkeys.setScope(/^(INPUT|TEXTAREA|SELECT)$/.test(tagName) ? 'input' : 'other');
    return true;
}
```

## 兼容模式

```js
var k = hotkeys.noConflict();
k('a', function() {
    console.log("这里可以干一些事儿")
});

hotkeys()
// -->Uncaught TypeError: hotkeys is not a function(anonymous function) 
// @ VM2170:2InjectedScript._evaluateOn 
// @ VM2165:883InjectedScript._evaluateAndWrap 
// @ VM2165:816InjectedScript.evaluate @ VM2165:682
```
