微信内置浏览器location.reload处理
===

使用安卓微信浏览器调用location.reload方法或设置location.href为页面自身时，内置浏览器会丢弃此次请求。 

因此，刷新时请求路径改为携带一个时间戳参数

```
function reload () {
    var search = window.location.search,  
        newSearch = search,
        href = '';
    if(search) {
        newSearch = search.replace(/(&|\?)_=\d+/,'');
        newSearch = '?_=' + new Date().getTime() + newSearch.replace('?','&');
        href = window.location.href.replace(search,newSearch);
    }else {
        href = window.location.protocol + '//' + window.location.host + window.location.port + window.location.pathname + '?_=' + new Date().getTime() + window.location.hash;
    }
    window.location.href = href;
}
```
