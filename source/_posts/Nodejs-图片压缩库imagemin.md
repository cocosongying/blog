---
title: Nodejs-图片压缩库imagemin
meta:
  donate: true
categories: [编程学习]
tag: [Nodejs]
references:
  - name: imagemin-Github
    url: 'https://github.com/imagemin/imagemin'
abbrlink: 9de
date: 2020-05-25 11:31:57
---

记录一个 `Nodejs` 的图片压缩库 - `imagemin`

<!-- more -->

## 简介
`imagemin` 本身不具备图片压缩能力，而是通过支持很多具有压缩不同类型图片的插件实现压缩。这个库只适合在服务端使用，可以写成脚本执行，也可以写成接口的形式给前端调用。

## 安装使用

```bash
npm install imagemin --save
```

常用插件列表
{% note link, [Imagemin plugin for Gifsicle](https://www.npmjs.com/package/imagemin-gifsicle) %}
{% note link, [SVGO imagemin plugin](https://www.npmjs.com/package/imagemin-svgo) %}
{% note link, [Imagemin plugin for pngquant](https://www.npmjs.com/package/imagemin-pngquant) %}
{% note link, [Imagemin plugin for OptiPNG](https://www.npmjs.com/package/imagemin-optipng) %}
{% note link, [WebP imagemin plugin](https://www.npmjs.com/package/imagemin-webp) %}
{% note link, [Imagemin plugin for mozjpeg](https://www.npmjs.com/package/imagemin-mozjpeg) %}
{% note link, [jpeg-recompress imagemin plugin](https://www.npmjs.com/package/imagemin-jpeg-recompress) %}
{% note link, [jpegtran imagemin plugin](https://www.npmjs.com/package/imagemin-jpegtran) %}

使用示例如下：
```js
const imagemin = require('imagemin');
const imageminJpegtran = require('imagemin-jpegtran');
const imageminPngquant = require('imagemin-pngquant');
 
(async () => {
    const files = await imagemin(['images/*.{jpg,png}'], {
        destination: 'build/images',
        plugins: [
            imageminJpegtran(),
            imageminPngquant({
                quality: [0.6, 0.8]
            })
        ]
    });
 
    console.log(files);
    //=> [{data: <Buffer 89 50 4e …>, destinationPath: 'build/images/foo.jpg'}, …]
})();
```

这个工具支持处理多张图片，既可以将压缩后的文件输出到指定目录，也可以返回压缩后文件的数据。

## 使用心得
在使用 `npm` 安装插件的时候，安装过程特别慢，且有很大几率出现安装不成功，且需要在自己电脑上编译的情况，编译这些文件又需要根据出错提示安装一些基础库。自从改为通过 `cnpm` 安装后，安装速度很快，且不需要再次编译的过程。这里强烈推荐使用 `cnpm` 安装。

对于压缩效果，那是非常明显且高效的，说的再多不如体验一下，大小减少了很多，而画质却看不出来太大差别，当然这个和设置也有关系，压缩比例太高了画质自然是不行的，使用推荐设置就可以了。
