#   H5泛解析
什么是H5泛解析？这是我新造的一个名词，这里举个例子说明：<br />首先，我这里有一个 WebApp : <br />[https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png](https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png)<br />你会说，怎么可能，这不是一张图片吗？<br />但实际上，这个图片隐写了一个压缩包，里面包含了 html , js , css 等文件，是一个完整的前端 APP 。<br />当你要运行（分发）这个 APP ，也很简单：<br />[https://h5trigger.github.io/sw.html#url=https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png](https://h5trigger.github.io/sw.html#url=https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png)<br />这又是什么原理？<br />原来是 [https://h5trigger.github.io/sw.html](https://h5trigger.github.io/sw.html) 这个页面，自动下载了这个 png ，把隐写的zip解压，然后 使用ServiceWorker技术，实现了 WebApp.（此过程纯前端完成，不涉及任何后端）<br />H5泛解析就是  [https://h5trigger.github.io/sw.html](https://h5trigger.github.io/sw.html) 这个页面 ，针对任意指定的图片文件，均解包成 WebApp 的缝合技术。
# 背景：
某一次，我写了一个小型的H5项目。里面一共有3个文件，分别是 index.html , 1.js , 1.css。<br />我需要把这个小的WEB项目搭建成网站，从而提高我的工作效率。<br />这时，我可以把这几个文件上传到我的博客网站上，但我博客网站根目录的文件已经乱得不能再乱了。<br />而且这个WEB项目涉及一些自用的API，我只打算自己使用，不想公开发布。<br />我就想，有没有存在一种类似Docker的东西，可以把我的这几个文件打包，然后很简单地就能制作成WEBAPP并发布呢？<br />  <br />  于是，我的思路如下。<br />  1.首先我把这几个文件用zip打包，密码为 password。<br />  2.然后把这个 zip 内容隐写到图片 zip.png 里。<br />  3.上传到在线图床。 得到 [https://x.x/zip.png](https://x.x/zip.png)<br />  4.访问 [https://h5trigger.github.io/sw.html#url=https://x.x/zip.png&pass=password](https://h5trigger.github.io/sw.html#url=https://x.x/zip.png&pass=password) 即可。<br />  <br />  重点是第4步 sw.html 使用的技术。<br />访问 [https://h5trigger.github.io/sw.html#url=https://x.x/zip.png&pass=password](https://h5trigger.github.io/sw.html#url=https://x.x/zip.png&pass=password)  ，其加载流程如下：<br />  1.加载 location.hash ，远程下载 [https://x.x/zip.png](https://x.x/zip.png) <br />  2.使用 canvas，从 zip.png 中还原出 源zip文件。<br />  3.使用 zip.js , 纯前端解压文件。（支持zip加密） <br />  4.使用 ServiceWorker 技术，hook 请求路径。<br />  
#   案例：
  1.我访问 [https://h5trigger.github.io/sw.html](https://h5trigger.github.io/sw.html) 。<br />选择一个ZIP文件。然后点击下载图片。<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/2953360/1663500173559-87ff26a7-03e8-43b1-a9ac-c5dd16cdc257.png#clientId=ude4d15bd-3c38-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=564&id=u9294715c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=592&originWidth=1257&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90956&status=done&style=none&taskId=u4c4a457d-068c-4c65-8e66-53918434138&title=&width=1197.1429115087833)<br />  ![image.png](https://cdn.nlark.com/yuque/0/2022/png/2953360/1663500113019-8b9c8dab-6189-440f-930f-db61e29bd608.png#clientId=ude4d15bd-3c38-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=520&id=u77cdeaea&margin=%5Bobject%20Object%5D&name=image.png&originHeight=546&originWidth=651&originalType=binary&ratio=1&rotation=0&showTitle=false&size=903914&status=done&style=none&taskId=u4b102cf1-8ab4-4d09-92b0-884fe65cb44&title=&width=620.0000281561001)<br />  2.把图片上传到第三方图床，得到 [https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png](https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png) 。

3.访问   <br />[https://h5trigger.github.io/sw.html#url=https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png](https://h5trigger.github.io/sw.html#url=https://message.biliimg.com/bfs/im/bed8c43082ea6dc292657179f229cf29a43c9db6.png)<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/2953360/1663500202274-7f5b0d0b-039c-4fe4-927b-f545f9b43e95.png#clientId=ude4d15bd-3c38-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=775&id=u7ee1cc94&margin=%5Bobject%20Object%5D&name=image.png&originHeight=814&originWidth=1024&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1117488&status=done&style=none&taskId=ucdca1149-9823-44f6-bacb-0e0c95ecb92&title=&width=975.238139526646)

# 一些限制：
1.图床必须满足 https 且响应头 access-control-allow-origin: * 以满足跨域。否则需要一些技术手段中转。<br />2.服务端必须满足 根目录，https 以满足 ServiceWorker 工作条件

# 兼容性（高）：
支持ServiceWorker技术（经测试，Chrome 支持，安卓webview支持，大部分移动浏览器支持）

# 安全性（极高）：
1.图床端：只能获得 zip 没有秘钥<br />2.服务端：解压秘钥包含在 location.hash ，不通过网络发送<br />就是说，所有敏感信息皆是本地

访问速度（高）：<br />1.由于 WebApp 储存在图床，所以访问速度杠杠的。<br />2.一次加载，永久运行。也可以选择其他H5泛解析节点。
# 分发：
欢迎分发 H5trigger 。这是一个纯前端项目。核心文件sw.html , sw.js , (zip.min.js) .<br />把它们上传到您的 https网站的根目录，就能形成新的H5泛解析节点，让这个巧妙的构想焕发更大的活力。<br />访


访问速度***access-control-allow-origin: **<br />*

