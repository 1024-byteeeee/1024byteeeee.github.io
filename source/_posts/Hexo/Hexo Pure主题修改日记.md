---
title: Hexo Pure主题修改日记
tags: Hexo
categories: Hexo
date: 2022-10-31 02:10:22
---



### 1.添加背景动画

背景动画基于canvas，在`\themes\pure\layout\layout.ejs`中添加

```html
<!-- 背景动画 -->
  <script type="text/javascript" color="0,0,0" opacity='0.8' zIndex="-2" count="88" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
  <!-- 
  color: 线条颜色，默认：‘0，0，0’；三个数字分别为(R,G,B),注意使用，分割
  opacity: 线条透明度0~1,默认0.5
  count: 线条总数量，默认150
  zIndex: 背景的z-Index属性，css用于控制所在层的位置，默认-1 
  -->
```

### 2.更改代码块样式

修改`.\themes\pure\source\css\style.css`

```css
pre,
.highlight {
  background: #cfcbcb;
  margin: 10px 0;
  padding: 15px 10px;
  overflow: auto;
  font-size: 18px;
  font-family: "Consolas";
  font-weight: bold;
  color: #4d4d4c;
  line-height: 1.5;
}
```

### 3.添加代码块一键复制按钮[[原文](https://blog.iwwee.com/posts/hexo-optimize.html#为代码块增加复制按钮)]

（1）、增加全局函数addLoadEvent  

在`/themes/pure/source/js`目录下打开`application.js`，在文件最后追加

```js
function addLoadEvent(func) {
    var oldonload = window.onload;
    if (typeof window.onload != 'function') {
        window.onload = func;
    } else {
        window.onload = function() {
            oldonload();
            func();
        }
    }
}
```

（2）、新增按钮

pure默认情况下是没有代码复制功能的，此时需要对hexo增加复制代码块功能。
首先在`/themes/pure/layout/_partial`目录下新增`article-copy-code.ejs`，增加以下内容：

```js
<% if(theme.codeblock.copy_button.enable){ %>
    <style>
      .copy-btn {
        display: inline-block;
        padding: 6px 12px;
        font-size: 13px;
        font-weight: 700;
        line-height: 20px;
        color: #333;
        white-space: nowrap;
        vertical-align: middle;
        cursor: pointer;
        background-color: #eee;
        background-image: linear-gradient(#fcfcfc, #eee);
        border: 1px solid #d5d5d5;
        border-radius: 3px;
        user-select: none;
        outline: 0;
      }
  
      .highlight-wrap .copy-btn {
        transition: opacity .3s ease-in-out;
        opacity: 0;
        padding: 2px 6px;
        position: absolute;
        right: 4px;
        top: 8px;
        z-index: 2;
      }
  
      .highlight-wrap:hover .copy-btn,
          .highlight-wrap .copy-btn:focus {
        opacity: 1
      }
  
      .highlight-wrap {
        position: relative;
      }
    </style>
    
    <script>
      addLoadEvent(()=>{
        $('.highlight').each(function (i, e) {
          var $wrap = $('<div>').addClass('highlight-wrap')
          $(e).after($wrap)
          $wrap.append($('<button>').addClass('copy-btn').append('<%= __("codeblock.copy_button") %>').on('click', function (e) {
            var code = $(this).parent().find(".code")[0].innerText
            <% if(theme.codeblock.copyright.enable){ %>
                code += "<%= theme.codeblock.copyright.content %>"
            <% } %>
            var ta = document.createElement('textarea')
            document.body.appendChild(ta)
            ta.style.position = 'absolute'
            ta.style.top = '0px'
            ta.style.left = '0px'
            ta.value = code
            ta.select()
            ta.focus()
            var result = document.execCommand('copy')
            document.body.removeChild(ta)
            <% if(theme.codeblock.copy_button.result){ %>
              if(result)$(this).text('<%= __("codeblock.copy_success") %>')
              else $(this).text('<%= __("codeblock.copy_failure") %>')
            <% } %>
            $(this).blur()
          })).on('mouseleave', function (e) {
            var $b = $(this).find('.copy-btn')
            setTimeout(function () {
              $b.text('<%= __("codeblock.copy_button") %>')
            }, 300)
          }).append(e)
        })
      })
    </script>
  <% } %>

```

（3）、插入到页面：
编辑`/themes/pure/layout/layout.ejs`，在`</body>`前面一行增加`<%- partial('_partial/article-copy-code')%>`

```ejs
  <%- body %>
  <%- partial('_common/footer', null, {cache: !config.relative_link}) %>
  <%- partial('_common/script', {post: page}) %>
  <%- partial('_partial/article-copy-code') %>
</body>
</html>
```

（4）、增加语言文件： 
在`/themes/pure/languages`目录下选择对应的语言文件，在文件后面增加：

```yaml
codeblock:
  copy_button: 复制
  copy_success: 复制成功
  copy_failure: 复制失败
```

（5）、增加主题配置文件
打开`themes/pure/_config.yml`，在文件末尾添加

```yaml
codeblock: 
  copy_button: 
    enable: true
    result: true
  copyright:
    enable: true
    content: false
```

### 4.代码块滚动条[[原文](https://blog.iwwee.com/posts/hexo-optimize.html#为代码块增加复制按钮)]

```css
.highlight::-webkit-scrollbar {
  /*滚动条整体样式*/
  /*高宽分别对应横竖滚动条的尺寸*/
  /*width: 10px;*/
  height: 8px;
}

 /* 代码块滚动条 */
.highlight::-webkit-scrollbar-thumb {
  /*滚动条里面小方块*/
  border-radius: 45px;
  /*background-color: #D62929;*/
  background-color: #6f6969;
  background-image: -webkit-linear-gradient(168deg,
      rgba(255, 255, 255, 0.2) 100%, /*12.5*/
      transparent 12.5%,
      transparent 25%,
      rgba(255, 255, 255, 0.2) 25%,
      rgba(255, 255, 255, 0.2) 37.5%,
      transparent 37.5%,
      transparent 50%,
      rgba(255, 255, 255, 0.2) 50%,
      rgba(255, 255, 255, 0.2) 62.5%,
      transparent 62.5%,
      transparent 75%,
      rgba(255, 255, 255, 0.2) 75%,
      rgba(255, 255, 255, 0.2) 87.5%,
      transparent 87.5%);
}

.highlight::-webkit-scrollbar-track {
  /*滚动条里面轨道*/
  background-color: #0f111a;
}
```

### 5.添加回到顶部 [[ 原文 ]](https://hwame.top/20200520/hello-hexo-troubleshooting.html)

文件位置：`./themes/pure/layout/_common/script.ejs`，在合适位置添加如下代码：

```ejs
<div id="go-top"></div>
<style type="text/css">
#go-top {
 width:40px;height:36px;
 background-color:#DDA0DD;
 position:relative;
 border-radius:2px;
 position:fixed;right:10px;bottom:60px;
 cursor:pointer;display:none;
}
#go-top:after {
 content:" ";
 position:absolute;left:14px;top:14px;
 border-top:2px solid #fff;border-right:2px solid #fff;
 width:12px;height:12px;
 transform:rotate(-45deg);
}
#go-top:hover {
 background-color:#8A2BE2;
}
</style>
<script>
$(function () {
  var top=$("#go-top");
  $(window).scroll(function () {
    ($(window).scrollTop() > 300) ? top.show(300) : top.hide(200);
    $("#go-top").click(function () {
      $('body,html').animate({scrollTop:0});
      return false();
    })
  });
});
</script>
```

### 6.使文章图片居中[[ 原文 ]](https://hwame.top/20200520/hello-hexo-troubleshooting.html)

第一步：在`./themes/pure/source/css/style.css`下

第二步：125行`img`修改：

```css
img {
  border: 0;
  box-sizing: border-box;
  margin: auto;
  padding: 3px;
  text-align: center;
  display: block;
}
```

