# vue-iframe
vue页面嵌入iframe 传参问题

```
<template>
  <iframe :src="src" ref="iframe" v-if="authorizeDialog" frameborder="0" scrolling="no" width="100%"></iframe>
</template>
<script>
export default {
  data () {
    return {
      src: "/static/fingerHtml/finger.html",//也可以通过?id=123形式传递参数
      iframeWin: {},
    }
  },
   methods: {
    /** 
     * 向iframe页面传递参数
    */
    sendMessage () {
      // 外部vue向iframe内部传数据
      this.iframeWin.postMessage({
        cmd: 'getFormJson',
        params: {}
      }, '*')
   },
    handleMessage (event) {
      // 根据上面制定的结构来解析iframe内部发回来的数据
      const data = event.data
     //dosomething

    }
   },
   
   mounted(){
    this.iframeWin = this.$refs.iframe.contentWindow
    window.addEventListener('message', this.handleMessage)
   }
}

</script>
```
```
//finger.html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

<body>
  <div class="login-install"></div>
<script>
//发送给父页面
  window.parent.postMessage({
      cmd: 'returnFData',
      params: {
        success: true,
        data: successMsg.FData
      }
    }, '*');
    //接受父页面发来的信息
     window.addEventListener('message', function (event) {
      // 通过origin属性判断消息来源地址
      // if (event.origin == 'localhost') {
      console.log(event.data);
      //console.log(event.source);
      //}
    }, false);

  </script>
</body>

</html>
```
