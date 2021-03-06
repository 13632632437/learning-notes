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
## 3.正则验证>=0的小数或整数(0开头的只能是小数)
```
let reg = /^(0|([1-9]\d*))(\.\d+)?$/;
```
## 4.验证一个字符串能否被JSON.parse()解析
```
   function isJSON(str) {
        if (typeof str == 'string') {
            try {
                var obj = JSON.parse(str);
                console.log('转换成功：' + obj);
                return true;
            } catch (e) {
                return false;
            }
        }
    }
```
## 5.正则替换指定得开头和结尾
```
str = str.replace(/(^、)|(、$)/,'')
```
## 6.数据类型检测
   + typeof
      1. typeof 1             // Number
      2. typeof true          // Boolean
      3. typeof "str"         // String
      4. typeof undefined     // undefined
      5. typeof null          // object
      6. typeof function (){} // function
      7. typeof [1,2,3]       // object
      - 缺点：当类型为null、object和array时都会返回object，所以不能区分这三类
   + instanceof
      1. 1 instanceof Number              // false
      2. "str" instanceof String          // false
      3. true instanceof Boolean          // false
      4. function(){} instanceof Function // true
      5. [1,2,3] instancof Array          // true
      6. {} instancof Objec               // true
      - 缺点：不能检测number、string和boolean
