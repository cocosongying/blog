---
title: Shell-获取命令行参数
meta:
  donate: true
categories: [编程学习]
tag: [Shell]
abbrlink: 3d67
date: 2020-06-06 19:12:33
---

介绍一下使用 Shell 写脚本如何获取命令行参数。

<!-- more -->

## 直接获取
比如直接写个脚本文件 `script.sh`
```shell
#!/usr/bin/env bash

echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

执行
```bash
bash script.sh a b c
```

结果为
```
第一个参数为：a
第二个参数为：b
第三个参数为：c
```

## 使用getopts
比如直接写个脚本文件 `script.sh`
```shell
#!/usr/bin/env bash

while getopts 'a:b:c:' opt; do
    case $opt in
        a)
            echo $OPTARG
            ;;
        b)
            echo $OPTARG
            ;;
        c)
            echo $OPTARG
            ;;
        ?)
            echo "Unknow option"
            ;;
    esac
done
```

执行
```bash
bash script.sh -a 1 -b 2 -c 3
```

结果为
```
1
2
3
```

## 使用心得
直接获取参数的方式需要注意参数顺序。getopts 方式不必关注参数顺序，同时可以将参数赋值以便后续处理。
