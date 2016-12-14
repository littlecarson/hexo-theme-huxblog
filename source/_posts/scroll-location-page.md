---
layout:     post
title:      "Scroll Location Page"
subtitle:   "滚动定位导航特效制作"
date:       2016-12-14 22:32:00
author:     "Carson"
catalog:    true
tags:
    - JavaScript
    - jQuery
---

# scroll-location-page

页面滚动定位特效制作


## 特效分析

- 页面结构：
    
    - 左为主体内容，右为悬浮导航菜单
    - 页面滚动，导航随内容不同聚焦移动
    - 点击导航，左侧内容滚动到对应内容

- 特效分析：
    
    ![readme-01](image/readme-01.png)


## 技术支撑

页面滚动会发生滚动事件，通过滚动事件从而实现页面中相应内容的位置导航聚焦

可以通过jQuery的方法快捷实现。

```
// 适用于所有可以滚动的元素和window对象
$element.scroll(function(){...})

// 获取/设置匹配元素相对于滚动条顶部的偏移
$element.scrollTop([val])

// 获取匹配元素的相对偏移，返回对象包含两个整形属性（top,left）
$element.offset()
```


## 技术实现

1. 锚点链接: 实现点击导航内容跳转

```
<div id="menu">
    <ul>
        <li><a href="#item-one">item</a></li>
        <li><a href="#item-two">item</a></li>
    </ul>
</div>
<div id="content">
    <div id="item-one" class="item">item-one-content</div>
    <div id="item-two" class="item">item-two-content</div>
</div>
```
2. 滚动切换：

```
// 浏览器滚动条滚动事件监听
$(window).scroll(function() {

    // 获取文档滚动后到顶部的距离
    var top = $(document).scrollTop();
    var menu = $('#menu');
    var items = $('#content').find('.item');

    var currentId = ""; // 当前所在楼层的ID

    items.each(function(){
        var m = $(this);
        var itemTop = m.offset().top;
        if (top > itemTop-200) {
            currentId = '#' + m.attr('id');
        } else {
            return false;
        }
    });

    // 设置当前元素为所到楼层的ID, 取消上一次的当前ID
    var currentLink = menu.find('.current');
    if(currentId && currentLink.attr('href') != currentId) {
        currentLink.removeClass('current');
        menu.find('[href='+ currentId + ']').addClass('current');
    }

});
```
3. 兼容

```
解决IE6不兼容fixed属性的方法，在css中加入：

/*ie6 hack*/

*html,*html body{

background-image:url(about:blank);

background-attachment:fixed;

}

*html menu{

position:absolute;

top:expression(((e=document.documentElement.scrollTop)?e:document.body.scrollTop)+100+'px');

}
```
4. 原生实现

```
// 根据 className 获取元素 cls: className
function getByClassName(obj,cls){
  var elements=obj.getElementsByTagName('*');
  var result=[];
  for(var i=0;i<elements.length;i++){
    if(elements[i].className==cls){
      result.push(elements[i]);
    }
  }
  return result;
}
function hasClass(obj,cls){
  return obj.className.match(new RegExp("(\\s^)"+cls+"(\\s$)"));
}
function removeClass(obj,cls){
  if(hasClass(obj,cls)){
    var reg =new RegExp("(\\s^)"+cls+"(\\s$)");
    obj.className=obj.className.replace(reg,"");
  }
}
function addClass(obj,cls){
  if(!hasClass(obj,cls)){
    obj.className += " " +cls;
  }
}
window.onload = function() {

    window.onscroll = function() {

        var top = document.documentElement ? document.documentElement.scrollTop : document.body.scrollTop;

        var menus = document.getElementById('menu').getElementByTagName('a');

        // var items = document.getElementById('content').getElementByTagName('item');
        var items = getByClassName(document.getElementById('content', 'item'));

        var currentId = '';

        for(var i = 0; i<items.length; i++) {
            var _item = items[i];
            var _itemTop = _item.offsetTop;
            if(top > _itemTop -200) {
                currentId = _item.id;
            } else {
                break;
            }
        }

        if(currentId) {
            // 给正确的menu 下的 a 元素加上 class
            for(var j= 0; j < menus.length; j++) {
                var _menu = menus[j];
                var _href = _menu._href.split('#');
                if (_href[_href.length - 1] != currentId) {
                    removeClass(_menu,'current');
                } else {
                    addClass(_menu,'current');
                }
            }
        }

    };

};

```
