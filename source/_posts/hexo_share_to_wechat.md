---
title: hexo 调用share.js
toc: true
top: 100
date: 2018-11-24 17:39:56
tags:
- hexo
- 朋友圈
categories:
- Hexo教程
---
# 序
我本来想做的分享按钮预期效果如下：
![分享图标](share.png),本来也打开了一个实现了该效果的个人网站，结果一不小心关掉了,然后就只能自己动手制作分享功能。
# 正文
首先在**themes\next\layout**中新建一个文件**socialshare.swig**
编辑内容如下
```
<script src="../lib/jquery/index.js"></script>
<link href="https://cdn.bootcss.com/social-share.js/1.0.16/css/share.min.css" rel="stylesheet">
<script src="https://cdn.bootcss.com/social-share.js/1.0.16/js/jquery.share.min.js"></script>

<script> var $config = {
      url                 : window.location.href,// 网址，默认使用 window.location.href
      source              : '', // 来源（QQ空间会用到）, 默认读取head标签：<meta name="site" content="http://overtrue" />
      title               : '', // 标题，默认读取 document.title 或者 <meta name="title" content="share.js" />
      description         : '', // 描述, 默认读取head标签：<meta name="description" content="PHP弱类型的实现原理分析" />
      image               : '', // 图片, 默认取网页中第一个img标签
      sites               : ['qzone', 'qq', 'weibo','wechat'], // 启用的站点
      disabled            : ['google', 'facebook', 'twitter'], // 禁用的站点
      wechatQrcodeTitle   : '微信扫一扫：分享', // 微信二维码提示文字
      wechatQrcodeHelper  : '<p>微信里点“发现”，扫一下</p><p>二维码便可将本文分享至朋友圈。</p>',
      target : '_blank' //打开方式
  };
  $('.social-share').share($config);
</script>
```
然后找到**themes\next\layout**中的文件**post.swig**中的这部分代码
```
    <footer class="post-footer">
```
之前贴上以下代码
```
    {% if theme.social_share and not is_index %}
       {% include '../_partials/share/socialshare.swig' %}
      <div class="social-share"></div>  
    {% endif %}
```
在主题_config.yml文件中增加以下代码
```
social_share:
  enable: true
```
保存修改后，然后hexo clean ，hexo g ，hexo d 即可看到点击效果。

# 总结
虽说分享功能不如我的预期完整,但是也是解决了baidu share组件和赞赏组件冲突的问题。
如果有时间,还有兴趣的时候会继续折腾Share.js这个组件。
然后希望有已经部署成功的人指点一二，不胜感激。
# 后记
于2018年12月14日修正并解决了该问题。

---
# 参考资料
* [一键转发工具 Share.js](https://www.oschina.net/p/share-js)
