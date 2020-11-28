## axios的使用
```
import axios from "axios";
const defaultTableOption = { pageIndex: 1, pageRows: 10 }; // 默认分页参数
const commonRequestPath = '基础url';
export const upLoadUrl = "文件下载url";
export const partSignPath = (() => {
    const reg = /localhost|10\./g;
    let host = window.location.host;
    if (reg.test(host)) { host = "开发环境服务" } // 用于签名
    return `https://${host}`;                    // 不是开发环境用地址栏的地址
})();
const createRequest = (method, url, params) => {
    let queryString = qs.stringify(params, { arrayFormat: 'indices', allowDots: true });
    if (method === "get") { return axios.get(`${url}?${queryString}`) }
    if (method === "post") { return axios.post(url, queryString) }
    return Promise.reject("不被支持的请求");
};
// 过滤请求返回值
const filterResponse = response => {
    if (response.status === 200 || response.status === 206 || response.status === 304) {
        return response.data;
    }
};
/**
 * @param {Object} options
 * @param {String} options.method
 * @param {String} options.url
 * @param {Object} options.params
 * @param {Boolean} options.isTableData 是否是表格数据
 * @param {Boolean} options.keepEmpty 请求参数是否保留空数据
 * @param {Boolean} options.isDownloadFile 是否是文件下载请求
 */
export default function request(options) {
    const { method, url, params, isTableData, mockDuration, keepEmpty, isDownloadFile } = options;
    let reqParams = filterEmptyParams(params, keepEmpty); // 清理空参数
    if (isTableData) { reqParams = Object.assign({}, defaultTableOption, reqParams) } // 如果请求忘记，添加分页默认参数
    reqParams = Object.assign({ timestamp: new Date().getTime() }, defaultOption, reqParams); // 添加额外参数（时间戳等）
    let reqUrl = "";
    if (/检测下是否需要加基础路径/.test(url)) { reqUrl = url } else { reqUrl = commonRequestPath + url }
    const signUrl = partSignPath + reqUrl;                  // 签名url
    reqParams.sign = getSign(method, signUrl, reqParams);   // 获取签名的方法
    // 下载文件
    if (isDownloadFile) {
        let iframe = document.createElement("iframe");
        iframe.src = `${url}?${qs.stringify(reqParams)}`;
        iframe.style.display = "none";
        let timer = setInterval(function () {
            let iframeDoc = iframe.contentDocument || iframe.contentWindow.document;
            if (iframeDoc.readyState == "complete" || iframeDoc.readyState == "interactive") {
                document.body.removeChild(iframe);
                clearInterval(timer);
                return;
            }
        }, 4000);
        document.body.appendChild(iframe);
        return;
    }
    return new Promise((resolve, reject) => {
        createRequest(method, reqUrl, reqParams)
            .then(res => {
                const response = filterResponse(res);
                const { code } = response;
                if (code !== 0) {
                    !Logout &&
                        '提示方法'({
                            type: "warning",
                            text: response.msg || "发生异常"
                        });
                    if (code === 100010110) {
                        // 未登录 跳转登录页面
                        Logout = true;
                        setTimeout(() => {
                            Logout = false;
                            backToLogin();
                        }, 2000);
                    }
                }
                setTimeout(() => { resolve(response) }, mockDuration || 0);
            })
            .catch(err => {
                const { message = "请求失败", response = {} } = err;
                '提示方法'({ type: "error", title: "网络异常", text: `${response.status || ""} ${message}` });
                reject(err);
            });
    });
}

```
