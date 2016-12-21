IOS safari中如果嵌入Iframe，当iframe页面依赖于cookie时，由于浏览器安全限制，将导航设置失败。

这个问题是由于safari默认开启了“限制第三方cookie”功能。

那如何解决这个问题呢？

1. 可以添加一个中间页，在这个页面设置一个cookie后再重定向到父页面，这时候父页面再嵌入iframe就可以访问cookie了。

2. 举个栗子：
```
module.exports = function (req, res, next) {
    var redirectUrl = req.query.redirectUrl;
    if(redirectUrl) {
        res.cookie('sessionReady',1);
        res.redirect(redirectUrl);
        return;
    }else {
        res.render('session-fix');
    }
};
```