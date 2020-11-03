# Another-internationalization-solution

最近做了一个小项目，或者说几个简单的vue页面，由于页面太少用vue-cli搭建感觉适得其反，于是用h5做的，直接页面引入vue的script标签，自然也就没办法install各种插件了，但是又需要国际化，加上又用了thymeleaf模板，开始走了一些弯路，后来直接写了个函数实现了。

先上效果图

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b23ee81ed6654cb0a7c14212999af92a~tplv-k3u1fbpfcp-watermark.image)
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8f52476b53184f95a4b68ddc05ca9661~tplv-k3u1fbpfcp-watermark.image)

首先页面引入vue,由于第一次接触thymeleaf，刚开始连引入路径问题都搞半天。。

结构是这样的

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/00acf298d0364506a8fae0683bf5705a~tplv-k3u1fbpfcp-watermark.image)


核心方法：
```
async doTranslation() {
//我是根据url最后五位匹配对应的json文件，从而获取对应的翻译，这个看具体业务需求，也可能是api返回，也可能是用户手动选择，这里会略微不同
      const url = window.location.href;
      const typeId = url.substring(url.length-5)
      let currentlang
      switch (typeId) {
        case 'en-MY':
          currentlang = '/js/en_MY.json'
          break
        case 'zh-HK':
          currentlang = '/js/zh_HK.json'
          break
        case 'zh-TW':
          currentlang = '/js/zh_TW.json'
          break
        default:
          currentlang = '/js/en_MY.json'
      }
      //用的fetch方法获取本地json文件
      await fetch(currentlang)
        .then(res=>res.json())
        .then(data=>{
        //赋值给变量，从而页面可以直接读取
          this.langData = data;
      })
    },

```
核心就是fetch方法获取本地json文件的数据，拿到数据之后就是最简单的页面渲染了{{langData.lable_title}}

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7f76af1fef034ef2aa111b82eea010e2~tplv-k3u1fbpfcp-watermark.image)

以后再碰到类似场景可以更省事儿了~~!



