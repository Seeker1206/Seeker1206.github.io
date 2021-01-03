---
layout:     post
title:      "jekyll添加百度统计工具"
subtitle:   "Baidu Analytics"
date:       2021-01-02 22:28:00
author:     "Seeker"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - jekyll
    - Baidu Analytics
---

# 正文
jekyll可以添加一些第三方的网站统计工具，像Baidu Analytics、Google Analytics、不蒜子等。这里介绍下添加百度统计的方式。

首先需要在[百度统计](https://tongji.baidu.com/)上面注册自己的账号，然后把自己的网站添加进去。

![img](/img/baidutongji/add.png)

然后点击左侧工具栏的代码获取，红框里面的内容就是你的track id。

![img](/img/baidutongji/code.png)

复制这个id到你的jekyll的_config.yml中，默认貌似会有下面这一条，把xxxxxx换成你自己的id。

    ba_track_id: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

我用的这个模板的作者直接在footer.html中添加了百度统计的代码字段，在yml中添加好track id后就能正常工作了，如果你用的模板没有的话，需要你自己把上面图里的代码复制下来添加到你自己的网页中。

    <!-- Baidu Tongji -->
    {% if site.ba_track_id %}
    <script>
        // dynamic User by Hux
        var _baId = '{{ site.ba_track_id }}';

        // Originial
        var _hmt = _hmt || [];
        (function () {
            var hm = document.createElement("script");
            hm.src = "//hm.baidu.com/hm.js?" + _baId;
            var s = document.getElementsByTagName("script")[0];
            s.parentNode.insertBefore(hm, s);
        })();
    </script>
    {% endif %}

之后Push到github上，然后去[百度统计](https://tongji.baidu.com/)网站上，上面图里工具栏代码获取下面有个代码安装检查，进去可以查看代码是否生效。

![img](/img/baidutongji/check.png)
