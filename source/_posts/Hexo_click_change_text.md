---
title: Hexo点击切换文字
toc: true
top: 100
date: 2018-11-24 15:12:43
tags:
 - hexo 
 - 鼠标效果
 - 文字切换
categories:
 - Hexo自定义
---
# 序
最近在工作中,有幸发现到了[C# Aspose.Cells.dll Excel操作总结](https://www.cnblogs.com/cang12138/p/8992506.html)这篇文章。  
觉得页面的鼠标点击效果还不错,故F12查看了一下源码,迁移到了个人博客网站上面，下面简单介绍一下实现步骤。
>希望原作者不要太介意哈~  

# 正文

在目录**themes\next\source\js\src**下新建**click_show_text.js**文件，内容如下

```
//单击显示随机文字
var a_idx = 0;
jQuery(document).ready(function($) {
    $("body").click(function(e) {
        var a = new Array(
          "若是不专一，生活把你欺",
          "活得有点二，倾慕大白菜",
          "爱好只有三，辣和甜和酸",
          "生日九月四，吃鱼得吐刺",
          "广州年有五，喜欢吃排骨", 
          "绰号为十六，断烟不断肉", 
          "年龄二十七，就懂零和一", 
          "身高一米八，身边缺个她",
          "体重一百九，木有女朋友",
          "二五得一十，爱就得一世", 
          "智商不够百，不怕有人怼", 
          "弱水有三千，一瓢可成仙", 
          "打赏没过万，可悲又可叹",
          "赚它一个亿，回家去种地"
                );
        var $i = $("<span/>").text(a[a_idx]);
        a_idx = (a_idx + 1) % a.length;
        var x = e.pageX,
        y = e.pageY;
        $i.css({
            "z-index": 5,
            "top": y - 20,
            "left": x,
            "position": "absolute",
            "font-weight": "bold",
            "color": "#FF69B4"
        });
        $("body").append($i);
        $i.animate({
            "top": y - 180,
            "opacity": 0
        },
			3000,
			function() {
			    $i.remove();
			});
    });
    setTimeout('delay()', 2000);
});

function delay() {
    $(".buryit").removeAttr("onclick");
}
```

具体要闪烁的文字，请自行替换。  
然后在**themes\next\layout\\_layout.swig**文件中 **</body>**前添加以下代码

```
<!--单击显示文字-->
<script type="text/javascript" src="/js/src/click_show_text.js"></script>
```

然后hexo clean ，hexo g ，hexo d 即可看到点击效果。
>注意：该事件可能和其余鼠标点击效果冲突。

# 总结

这是我自行做的页面优化，个人感觉还挺不错。

---
# 参考资料

* [C# Aspose.Cells.dll Excel操作总结](https://www.cnblogs.com/cang12138/p/8992506.html)
