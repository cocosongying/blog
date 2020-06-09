---
title: Mac上安装brew
meta:
  donate: true
categories: [编程学习]
tag: [Mac]
abbrlink: 8d6d
date: 2020-06-09 21:55:44
---

介绍如何在 Mac 上安装 brew

<!-- more -->

brew 作为 Mac 平台下一个包管理工具。可以一条命令在 Mac 上安装、卸载和更新各种软件包。

## 安装brew
直接运行下面的命令就可以安装
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

但是如果执行这条命令报错，说是无法连接 `https://raw.githubusercontent.com/` 之类的

这个时候可以在浏览器中访问链接

[https://raw.githubusercontent.com/Homebrew/install/master/install](https://raw.githubusercontent.com/Homebrew/install/master/install)

内容如下：
```ruby
#!/usr/bin/ruby

STDERR.print <<EOS
Warning: The Ruby Homebrew installer is now deprecated and has been rewritten in
Bash. Please migrate to the following command:
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

EOS

Kernel.exec "/bin/bash", "-c", '/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"'
```

意思是直接执行下面的语句，但是我们遇到的问题是无法连接 `https://raw.githubusercontent.com/`
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

那么我们继续访问这里的链接

[https://raw.githubusercontent.com/Homebrew/install/master/install.sh](https://raw.githubusercontent.com/Homebrew/install/master/install.sh)

内容有点长，这里就不粘贴了，浏览器访问可以看到。将页面文本复制到 `brew_install.sh` 中

```bash
cd ~
touch brew_install.sh
chmod 777 brew_install.sh
```

然后执行这个脚本文件就可以正常安装了。过程可能有点慢，耐心等一等，如果长时间卡住不动也可以先强制退出再次执行。
```bash
/bin/bash -c brew_install.sh
```
