兼容性
===
* android嵌入H5页面时，输入框被键盘遮盖。聚焦时强制让其显示，需要延时处理，100毫秒没有作用，暂用500毫秒
```
$('input').on('focus', function () {
  var elem = this;
  setTimeout(function () {
    elem.scrollIntoView && elem.scrollIntoView(true);
  },500);
});
```

* iphone上百度浏览器底部输入框被遮盖，特殊处理：
```
if(/iphone/i.test(navigator.userAgent) && /baidu/i.test(navigator.userAgent)) {
  $('.js-bottom-input').on('focus', function () {
    var elem = this;
    $('body').css({'padding-bottom': '260px'});
    elem.scrollIntoView && elem.scrollIntoView();
  })
  .on('blur', function () {
    $('body').css({'padding-bottom': 0});
  });
```

* android端有些浏览器不支持location.replace。考虑使用如下方法。
注意：history.replaceState不支持跨域
```
if(navigator.userAgent.toLowerCase().indexOf('android ') > -1) {
  window.history.replaceState({}, document.title, url);
  window.location.reload();
}else {
  window.location.replace(url);
}
```

* 华为荣耀6默认浏览器，设置background出现问题。
```
if(/H60-l12.*cHROME\/30\.0\.0\.0/i.test(navigator.userAgent)) {
  //荣耀6默认浏览器手动重绘页面，解决body背景图不扩展的问题。具体原因不知
  document.body.style.marginTop = '1px';
```
----------------------------------------------
1. IOS fixed布局元素脱离body，通过冒泡绑定事件无效。

2. UC低版本不支持CSS calc，flex布局子元素设置有float规则时，flex布局失效。

3. iPhone上第三方浏览器，嵌入iframe使用fastclick时，当连续点击输入框时将导致输入框不能输入。针对这种情况，不使用此插件 

4. 当父子框架使用document.domain设置允许跨域访问时，子页面中原来正常的iframe会报安全错误，这种情况必须同样设置document.domain才有效。小心My97date、lightbox等其他依赖iframe的插件。

5. 百度eChart在三星("S3安卓4.3.0","Note2,4.0")手机上绘制折线时，结果会出现双倍数量的线条。
**可设置animation:false解决此问题**

6. Android 4.3 不支持background-size缩写，不支持百分比单位

7. 新浪微博自带浏览器background不支持rgb设置颜色

8. 谨慎使用zepto的swipe事件。Zepto.js swipe事件在android 4.4版本多个浏览器下没有作用
9. safari开启无痕浏览模式，不能使用本地存储功能
10. 页面多个fixed元素存在时，不要嵌套元素。在使用浮层时，如果嵌套，低版本android在页面滚动时，可能出现闪跳的现象。
11. 移动端100%高度渲染错误，页面底部可能与系统导航条重合了，设置高度为可见区域高度

12. 手机QQ（Android 4.3,MQQBrowser/6.1）浏览器，history.back方法不起作用。操作历史记录不增加

