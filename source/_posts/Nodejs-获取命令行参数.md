---
title: Nodejs-获取命令行参数
meta:
  donate: true
categories: [编程学习]
tag: [Nodejs]
references:
  - name: commander-Github
    url: 'https://github.com/tj/commander.js'
abbrlink: '12e9'
date: 2020-06-06 18:27:35
---

介绍一下使用 Nodejs 写脚本如何获取命令行参数。

<!-- more -->

## 直接获取
```js
const argv = process.argv.slice(2);
```

比如写个脚本 `script.js`
```js
const argv = process.argv.slice(2);
console.log('传递的参数为：', argv);
```

执行脚本并传入参数 a b c
```bash
node script.js a b c
```

执行结果为：
```
传递的参数为： [ 'a', 'b', 'c' ]
```

## 使用第三方模块commander
首先安装 `commander` 模块
```bash
npm install commander --save
```

写个脚本 `script.js`
```js
const { program } = require('commander');
program
    .version('0.0.1')
    .option('-d, --debug', 'output extra debugging')
    .option('-s, --small', 'small pizza size')
    .option('-p, --pizza-type <type>', 'flavour of pizza');

program.parse(process.argv);
console.log(program.opts());
if (program.debug) {
    console.log('debugging');
}
```

执行
```bash
node script.js -d
```

结果为
```
{
  version: '0.0.1',
  debug: true,
  small: undefined,
  pizzaType: undefined
}
debugging
```

执行
```bash
node script.js -d -s -p abc
```

结果为
```
{ version: '0.0.1', debug: true, small: true, pizzaType: 'abc' }
debugging
```

## 使用心得
使用直接获取的方式需要注意参数顺序。而使用第三方模块则不必关心参数顺序问题，可以更好的设计脚本参数。
