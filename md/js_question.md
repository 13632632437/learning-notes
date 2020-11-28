# javaScript学习笔记
## 1.JavaScript函数注释规范
   + @param 	@param {类型} 参数名 描述
   + @return 	@return {类型} 描述
   + @author 	@author 作者
   + @version 	@version 版本号
   + @api: 提供给第三方使用的接口
   + @author: 标明作者
   + @param: 参数
   + @return: 返回值
   + @todo: 待办
   + @version: 版本号
   + @inheritdoc: 文档继承
   + @property: 类属性
   + @property-read: 只读属性
   + @property-write: 只写属性
   + @const: 常量
   + @deprecated: 过期方法
   + @example: 示例
   + @final: 标识类是终态, 禁止派生
   + @global: 指明引用的全局变量
   + @static: 标识类、方法、属性是静态的
   + @ignore: 忽略
   + @internal: 限内部使用
   + @license: 协议
   + @link: 链接,引用文档等
   + @see: 与 link 类似, 可以访问内部方法或类
   + @method: 方法
   + @package: 命名空间
   + @since: 从指定版本开始的变动
   + @throws: 抛出异常
   + @uses: 使用
   + @var: 变量
   + @copyright: 版权声明
## 2.javaScript下载文件/导出文件
   + 使用a标签
```
       downloadModel({}, 'blob').then(res => {
      console.log('下载Excel', res);
      const blob = new Blob([res], { type: 'application/vnd.ms-excel' });
      var a = document.createElement('a');
      a.download = '下载文件的名字';
      a.href = URL.createObjectURL(blob);
      document.body.appendChild(a);
      var evt = document.createEvent("MouseEvents");
      evt.initEvent("click", false, false);
      a.dispatchEvent(evt);
      document.body.removeChild(a);
    })
```
   + 使用iframe
```
    // 下载文件
    if (isDownloadFile) {
        let iframe = document.createElement("iframe");
        iframe.src = `${url}?${qs.stringify(reqParams)}`;
        iframe.style.display = "none";

        let timer = setInterval(function () {
            let iframeDoc = iframe.contentDocument || iframe.contentWindow.document;
            if (
                iframeDoc.readyState == "complete" ||
                iframeDoc.readyState == "interactive"
            ) {
                document.body.removeChild(iframe);
                clearInterval(timer);
                return;
            }
        }, 4000);

        document.body.appendChild(iframe);
        return;
    }
```
   
