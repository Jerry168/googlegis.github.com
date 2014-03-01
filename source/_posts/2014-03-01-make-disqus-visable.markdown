---
layout: post
title: "Octopress中Disqus设置"
date: 2014-03-01 10:49:47 +0800
comments: true
categories: Octopress
---
#### -- 让 disqus显示出来

  特意为Disqus写一篇，是因为过程中出现了错误，或者说我没有理解透别人说的意思。
   
   网上大多数的教程很简单，说是在Disqus注册个账号，然后把 short_name 写入 _config.yml中，我是菜鸟，写入了还是不能显示出来。
   
   {% img /images/disqus.png %}
   
   
   就是上图中画红色圈方框的部分没显示。
   
   
   擦！
   连续查了两天资料，终于搞定。
   
   
   1. disqus注册时，Shortname 随意写，Website Name 随意写， Website URL: 写入你的网站地址，如果你使用的github，那就写入你的Page Site： ***.github.com
   
   
   2. 如果还是出不来，请检查这两个文件。 /Source/_layout/page.html 和 /Source/_layout/post.html，  查看这部分：  
```  
   <section id="comment">  
    <h1 class="title">Comments</h1>  
    <div id="disqus_thread" aria-live="polite">{% include post/disqus_thread.html %}  </div>
</section>
``` 

 然后查看 _includes/post/disqus_thread.html 这个里面的内容，如果仅仅是下面这个部分，
```javascript  
    <noscript>Please enable JavaScript to view the <a href="http://disqus.com/? ref_noscript">comments powered by Disqus.</a></noscript>
```  
   那就说明，该修改 section 这部分代码了。
   
   查看 _includes/disqus.html 文件，查看源码，如果如下：
```javascript 
 {% comment %} Load script if disquss comments are enabled and `page.comments` 
  is either empty (index) or set to true {% endcomment %}
  {% if site.disqus_short_name and page.comments != false %}
  <script type="text/javascript">
      var disqus_shortname = '{{ site.disqus_short_name }}';
      {% if page.comments == true %}
        {% comment %} `page.comments` can be only be set to true on pages/posts, so we embed the comments here. {% endcomment %}
        // var disqus_developer = 1;
        var disqus_identifier = '{{ site.url }}{{ page.url }}';
        var disqus_url = '{{ site.url }}{{ page.url }}';
        var disqus_script = 'embed.js';
      {% else %}
        {% comment %} As `page.comments` is empty, we must be on the index page. {% endcomment %}
        var disqus_script = 'count.js';
      {% endif %}
    (function () {
      var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
      dsq.src = 'http://' + disqus_shortname + '.disqus.com/' + disqus_script;
      (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    }());
</script>
{% endif %}
```
  请修改 section 中的代码为：
  
```javascript
   <section id="comment">
    <h1 class="title">Comments</h1>
    <div id="disqus_thread" aria-live="polite">{% include disqus.html %}  </div>
</section>
```
 然后 rake generate ; rake deploy 查看页面了。