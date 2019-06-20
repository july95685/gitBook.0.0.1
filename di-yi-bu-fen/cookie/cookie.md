# Cookie
### cookie不可跨域

```javascript
// 域名为.baidu
document.cookie='myname=laihuamin;path=/;domain=.baidu.com';

// 域名为.google
document.cookie='myname=laihuamin;path=/;domain=.google.com';

// document.cookie可以获取所有cookie,以cookiekey+value;这种形式
```
真正能把cookie设置上去的只有domain是.baidu.com的cookie绑定到了域名上，所以上面所说的不可跨域性，就是不能在不同的域名下用，每个cookie都会绑定单一的域名

#### cookie的属性
**name**

这个显而易见，就是代表cookie的名字的意思，一个域名下绑定的cookie，name不能相同，相同的name的值会被覆盖掉

**value**

每个cookie拥有的一个属性，它表示cookie的值

cookie的值字符串可以用encodeURIComponent()来保证它不包含任何逗号、分号或空格(cookie值中禁止使用这些值).(出自mdn,验证了下';'确实存在问题，value字段只能保留;前面的内容，但',',' '未受影响，set-cookie对编码的描述提到编码不是必须的)

tips:写一个获取cookie value的方法

**domain**
这个是指的域名，这个代表的是，cookie绑定的域名，如果没有设置，就会自动绑定到执行语句的当前域，还有值得注意的点，统一个域名下的二级域名也是不可以交换使用cookie的，比如，你设置 www.baidu.com 和 image.baidu.com ,依旧是不能公用的

如何使一二级域名公用cookie？
域名之前的点号会被忽略。假如指定了域名，那么相当于各个子域名也包含在内了。比如.a.com 下的cookie 在 .b.a.com 下可以被访问

**path**
path这个属性默认是'/'，这个值匹配的是web的路由，为什么说的是匹配呢，就是如果 path=/docs，那么 "/docs", "/docs/Web/" 或者 "/docs/Web/HTTP" 都满足匹配的条件

**cookie的有效期**

Expires

cookie 的最长有效时间，形式为时间戳。如果没有设置这个属性，那么表示这是一个会话期 cookie 。一个会话结束于客户端被关闭时，这意味着会话期 cookie 在彼时会被移除。然而，很多Web浏览器支持会话恢复功能，这个功能可以使浏览器保留所有的tab标签，然后在重新打开浏览器的时候将其还原。与此同时，cookie 也会恢复，就跟从来没有关闭浏览器一样。

Max-Age

在 cookie 失效之前需要经过的秒数。一位或多位非零（1-9）数字。一些老的浏览器（ie6、ie7 和 ie8）不支持这个属性。对于其他浏览器来说，假如二者 （指 Expires 和Max-Age） 均存在，那么 Max-Age 优先级更高,

 当Max-Age为负数时，表示的是临时储存，不会生出cookie文件，只会存在浏览器内存中，且只会在打开的浏览器窗口或者子窗口有效，一旦浏览器关闭，cookie就会消失, 
 
 当Max-Age为0时，又会发生什么呢，删除cookie，因为cookie机制本身没有设置删除cookie，失效的cookie会被浏览器自动从内存中删除，所以，它实现的就是让cookie失效。

**secure**

一个带有安全属性的 cookie 只有在请求使用SSL和HTTPS协议的时候才会被发送到服务器

**HttpOnly**

如果这个属性设置为true，就不能通过js脚本来获取cookie的值, 即不能经由  Document.cookie 属性、XMLHttpRequest 和  Request APIs 进行访问

附张cookie与setcookie的图:![这是图片](
https://user-gold-cdn.xitu.io/2017/10/2/ac1f0d4e46b21da20d76b8136dd7583f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)