---
title: How To Add The Netease Clould Music to Jekyll
layout: post
tags: [Plugins,Music,API]
music-id: 35201383
---

# Genetate Iframe 
Open the Netease Cloud Music on PC,then click the music  
[生成网易云音乐外链](/images/Plugins/outerchain.jpg)  
Usually it will show "由于版权限制，无法生成外链"，don't worry,just see the link 
> https://music.163.com/#/album?id=35201383  

You could change the id to this code

>  <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86" src="//music.163.com/outchain/player?type=2&id={{ page.music-id }}&auto=0&height=66"></iframe>

# Add to ervery page  
You could add this code to `_includes/_macro/post.html`
```ruby

  {% if page.music-id > 0 %} 
      <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86" src="//music.163.com/outchain/player?type=2&id={{ page.music-id }}&auto=0&height=66">
        </iframe>
  {% endif %}

```
Then in every page you only need add the tag `music-id:xxxxxx`  
Note:  
~ don't has blank between { and %  
~ Generally the weight and height not included the "",you need add  
~ Auto=1 means it will play auto,auto=0 means it won't

