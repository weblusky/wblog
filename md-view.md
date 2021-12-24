# md-view 
浏览MD文件的小工具

### 起初
在github上看到一些开源图书是用md格式来发布的，下载到本地后，系统却不能自动预览，看起来很不方便。

### 于是
需求有了，正好我常用的一个工具local-web-server，是用来快速搭建静态文件web server的，但问题是浏览器不会自动渲染md文件，只能当成文本文件显示。

### 此时我的思路是
1. 利用local web server做本地服务器，浏览md文件
2. 在解析md文件时，利用插件把md内容格式转换为html格式
3. 需要一套适配的css样式

### 在看了lws的文档后
发现lws支持middleware，接下来搜 md2转html 找到了 showdown，css文件也是搜来的。。。然后照猫画虎调试出来了 [md2html.js](https://github.com/weblusky/md-view/blob/main/md2html.js)。

### 主要功能代码如下
```
  if (ctx.type == 'text/markdown') {
    const responseBodyBuffer = await streamReadAll(ctx.response.body)
    const htmlHead = '<!DOCTYPE html>\n<html>\n<head>\n<meta http-equiv="content-type" content="text/html; charset=utf-8">\n'
    const stylestr = '<style>' + cssstr + '</style>\n</head>\n<body>\n'
    const htmlFoot = '\n</body>\n</html>'
    let body = responseBodyBuffer.toString()
    body = converter.makeHtml(body)
    ctx.response.type = 'text/html'
    ctx.response.body = htmlHead + stylestr + body + htmlFoot
  }
```
通过 **ctx.type == 'text/markdown'** 来判断访问的是md格式文件

通过 **body = converter.makeHtml(body)** 来转换内容格式

通过 **ctx.response.type = 'text/html'** 来修改输出格式

现在本地的local-web-server可以支持md文件浏览了

### 最后
小工具做好，还没用几次，看到一篇文章说win10的资源管理器可以预览md文件。。。

另：功能简陋，欢迎[试用](https://github.com/weblusky/md-view)
