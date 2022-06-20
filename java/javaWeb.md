## 注意事项

### 1.静态资源路径处理问题

filter在某些情况下会过滤静态资源，导致静态资源加载失败，需要特殊处理静态资源路径

在filter函数中加入

```java
if (requestURI.contains(".css")||requestURI.contains(".js")||requestURI.contains(".png")||requestURI.contains(".jpg")) {
   chain.doFilter(request,response);
   return;
}
```

