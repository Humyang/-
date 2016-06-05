JavaScript 补基础系列

function createXHR() {
    if (typeof XMLHttpRequest != "undefined") {
        return new XMLHttpRequest();
    } else if (typeof ActiveXObject != "undefined") {
        if (typeof arguments.callee.activeXString != "string") {
            var versions = [
                    "MSXML2.XMLHttp.6.0 ", "MSXML2.XMLHttp.3.0 ", "MSXML2.XMLHttp"
                ],
                i,
                len;
            for (i = 0, len = versions.length; i < len; i++) {
                try {
                    new ActiveXObject(versions[i]);
                    arguments.callee.activeXString = versions[i];
                    break;
                } catch (ex) { //skip
                }
            }
        }
        return new ActiveXObject(arguments.callee.activeXString);
    } else {
        throw new Error("No XHR object available.");
    }
}

var xhr = createXHR();

xhr.getAllResponseHeaders();



GET 请求通常用于从服务器获取数据，POST 请求通常用于提交数据给服务器。

POST 请求最初设计来协同 XML 工作的，所以你可以直接传递 XML DOM 文档，他会自动 serialized 并作为 request body。你也可以传递任何字符串到服务器。

默认，POST 请求不会像表单提交一样提交数据到服务器，服务器需要读取原生 POST 数据接收你的数据。但你可以使用 XHR 模仿表单提交，首先设置 Content-Type 为 `application/ x-www-form-urlencoded`，他是表单提交的 Content-Type。第二步是创建相应格式的字符串，如果页面已经存在表单，如果要通过 XHR 发送他可以使用 serialize() 方法创建字符串。

```javascript

function submitData(){
var xhr = createXHR(); xhr.onreadystatechange = function(){
if (xhr.readyState == 4){
if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
} };
alert(xhr.responseText); } else {
alert(“Request was unsuccessful: “ + xhr.status); }
       xhr.open(“post”, “postexample.php”, true);
       xhr.setRequestHeader(“Content-Type”, “application/x-www-form-urlencoded”);
       var form = document.getElementById(“user-info”);
       xhr.send(serialize(form));
}

```


POST 请求比 GET 请求拥有更大的开销，传输同样数量的数据，GET 请求可以比 POST 快两倍。



## XMLHttpRequest Level 2

XMLHttpRequest Level 1 simply defined the already existing implementation details of the XHR object
XMLHttpRequest Level 1 简单的定义已有的 XHR 对象的实现细节。

XMLHttpRequest Level 2 went on to evolve the XHR object further.
XMLHttpRequest Level 2 进一步加强 XHR 对象的功能。

不是所有浏览器都实现了所有功能，但所有浏览器都实现了这些功能的一部分。
