# Java 中的 Ajax 与跨域

## 📌 什么是 Ajax
- **[Ajax](ca://s?q=Java_Ajax是什么)**（Asynchronous JavaScript and XML）是一种在网页中与服务器进行异步交互的技术。  
- 它允许在不刷新整个页面的情况下，向服务器发送请求并获取数据，从而提升用户体验。  
- 在 Java Web 中，Ajax 通常通过前端 JavaScript 发起请求，后端 Servlet、Spring MVC 等来处理响应。

---

## 📌 什么是跨域
- **[同源策略](ca://s?q=浏览器同源策略)**：浏览器安全机制，要求协议、域名、端口一致才能访问资源。  
- **跨域场景**：  
  - `http://a.com:8080` 调用 `http://a.com:8081` → **端口不同**，跨域。  
  - `http://a.com` 调用 `https://a.com` → **协议不同**，跨域。  
  - `http://a.com` 调用 `http://b.com` → **域名不同**，跨域。  

---

## 🔎 常见跨域解决方案

| 方法 | 原理 | 优缺点 |
|------|------|--------|
| **[CORS](ca://s?q=Java_CORS跨域解决方案)** | 服务端在响应头加 `Access-Control-Allow-Origin`，允许指定域访问 | 最标准，支持多种请求；需服务端支持 |
| **[JSONP](ca://s?q=Java_JSONP跨域解决方案)** | 利用 `<script>` 标签可跨域加载 JS，返回执行回调函数 | 只支持 GET；实现简单但功能有限 |
| **[反向代理/Nginx](ca://s?q=Nginx_反向代理跨域解决)** | 前端请求同源地址，由代理服务器转发到目标域 | 性能好，常用于生产环境；需额外配置 |
| **[后端转发](ca://s?q=Java_后端HttpClient跨域转发)** | Ajax 请求本地后端，再由后端调用跨域服务 | 安全性高，但增加一次请求，效率较低 |

---

## 📊 Java 中的实现示例

### 1. 使用 CORS Filter
```xml
<filter>
  <filter-name>CORS</filter-name>
  <filter-class>com.thetransactioncompany.cors.CORSFilter</filter-class>
  <init-param>
    <param-name>cors.allowOrigin</param-name>
    <param-value>*</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>CORS</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
