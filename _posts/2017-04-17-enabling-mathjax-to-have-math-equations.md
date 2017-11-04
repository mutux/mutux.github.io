---
layout: post
title: Enabling MathJax to Have Math Equations Supported in Your Google Blogger
date: '2017-04-17T15:36:00.000-07:00'
author: mutux
tags:
modified_time: '2017-04-17T15:36:16.215-07:00'
blogger_id: tag:blogger.com,1999:blog-7424095106938843032.post-8281179714167862593
blogger_orig_url: http://mutux.blogspot.com/2017/04/enabling-mathjax-to-have-math-equations.html
---

Blogspot, the Google's Blogger, is not officially supported by MathJax. A lot of guys tried to make it work for blogspot. I also saw a lot of blogs hosted on Wordpress that looks beautiful and attractive with perfect fonts, line spacing, and professional Mathematic Equations as well. I myself finally managed to have my blogspot support MathJax script too, today. If you like the appearance of my blog, here's what I did today:

# Instructions:

+ Get into the Design Board --> "Theme" page;
+ Click the Button at the upright corner, named "Backup/Restore", it will popup a window.
+ Click the "Download Theme" button, store it in your local disk.
+ Edit the Theme File (Usually xml type file) with some Text Editor.
+ Find the text `"<b:widget id='Blog1' locked='false' title='Blog Posts' type='Blog'>"`, change it into `"<b:widget id='Blog1' locked='true' title='Blog Posts' type='Blog'>"`
+ If you want add Math Equation in themes other than "dynamic views theme", just add script like this right before tag "</head>":
{% highlight javascript linenos %}
<script src='http://cdn.mathjax.org/mathjax/latest/MathJax.js' type='text/javascript'>
    MathJax.Hub.Config({
        HTML: ["input/TeX","output/HTML-CSS"],
        TeX: { extensions: ["AMSmath.js","AMSsymbols.js"],
               equationNumbers: { autoNumber: "AMS" } },
        extensions: ["tex2jax.js"],
        jax: ["input/TeX","output/HTML-CSS"],
        tex2jax: { inlineMath: [ ['′,′'], ["\\(","\\)"] ],
                   displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
                   processEscapes: true },
        "HTML-CSS": { availableFonts: ["TeX"], linebreaks: { automatic: true } }
    });
</script>
{% endhighlight %}
+ If you want add MathJax support for "Dynamic Views Theme", right before the tag "</head>", you will need this:
{% highlight javascript linenos %}
<script type="text/x-mathjax-config">
    // <![CDATA[
        MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']],
                      displayMath: [['\\[','\\]'], ['$$','$$']]}});
        blogger.ui().viewType_.prototype.onRenderComplete=function(){
            MathJax.Hub.Queue(['Typeset',MathJax.Hub])
        };
    // ]]>
</script>
<script type="text/javascript"
    src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
{% endhighlight %}
+ Then save the file, back to your blogger design page where you download the above file.
+ Click the small Button "choose file", in a new popup window, select your local file just edited, and click ok.
+ Then the file name should appear after the "choose file" button.
+ Click "Upload" button to submit you customized theme file to blogger, it will be applied to your blogger immediately.
+ You can write \( e=mc^2 \) to see the results, it should looks like this \( e = mc^2 \).
+ If you want adjust the font size and line spacing, for themes other than "Dynamic Views Theme", you just need to customize it with several clicks, while for "Dynamic Views Theme", you have add this code to "Theme" --> "Custom" --> "Advanced" --> "Add CSS":
{% highlight css linenos %}
/* Post Content - Size of Text */
.article-content {
  font-size:130%;
}
.article-content.entry-content{
  line-height:1.6
}
{% endhighlight %}

# Reference:
+ http://weiqigao.blogspot.ca/2013/03/enabling-mathjax-in-blogger.html
+ https://productforums.google.com/forum/#!topic/blogger/_LzlfFT7I10
+ http://sifta.blogspot.ca/2014/01/getting-mathjax-to-work-in-blogger-with.html
+ https://productforums.google.com/forum/#!topic/blogger/XVQQKlBgRAQ
