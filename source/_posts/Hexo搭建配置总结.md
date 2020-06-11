---
title: Hexo搭建配置总结
date: 2019-08-27 18:14:27
categories: Hexo
tags: Hexo
keywords: hexo, 搭建, 配置, 添加Favicon, 添加百度统计, RSS订阅, 添加看板娘, 网站地图, 评论系统 gitalk, hexo d 403
---

主题是material-x 最新版本叫：**[volantis](https://github.com/xaoxuu/hexo-theme-volantis)**
 
[主题最新仓库](https://github.com/xaoxuu/hexo-theme-volantis)

[本博客主题所用版本](https://github.com/lunhui1994/hexo-theme-material-x)

简介：
1. **Hexo 添加Favicon**
2. **Hexo 添加百度统计**
3. **Hexo RSS订阅**
4. **Hexo 添加看板娘**
5. **Hexo 网站地图**
6. **Hexo 评论系统 gitalk**
7. **常见问题**
    1. **Hexo deploy 报错**

<!-- more -->
### Hexo 添加Favicon

**根目录_config.yml**

- 根目录为public的时候,图片放在source下面的img里面就可以，没有的话创建个img文件夹。

``` 
    favicon: https://www.xxx.com/img/favicon.ico

```

### Hexo 添加百度统计

1. 首先肯定是要去百度统计注册一下了。。[百度统计](https://tongji.baidu.com/web/)注册完成之后生成统计代码，备用。
2. 在**主题配置文件_config.yml**里面添加一行
``` 
    # Analytics
    cnzz: true
```
3. 找到 **\hexo\themes\pacman\layout\_partial**,在这个文件夹中创建一个cnzz.ejs的文件然后将下面内容复制进去，记得替换中间的script
``` 
    
<% if (theme.cnzz){ %>

<!-- 将中间这一块script替换成你的统计代码 -->
<script type="text/javascript">
    var cnzz_protocol = (("https:" == document.location.protocol) ? " https://": " http://");
    document.write(unescape("%3Cspan id='cnzz_stat_icon_1000543074'%3E%3C/span%3E%3Cscript src='" + cnzz_protocol + "s19.cnzz.com/z_stat.php%3Fid%3D1000543074%26show%3Dpic' type='text/javascript'%3E%3C/script%3E"));
</script>


<% } %>
```

4. 在 **\hexo\themes\pacman\layout\_partial\footer.ejs**中加一行下面的代码，然后就结束了。重启生效就可以了。然后在百度中心检测代码有没有安装成功，生效了就可以看报告了。这个过程需要一定的时间。
```
<%- partial('cnzz') %>
```


### Hexo RSS订阅

1. 进入hexo目录

```
npm install hexo-generator-feed

```
2. **根目录_config.yml**中添加

```
#RSS订阅
plugin:
- hexo-generator-feed
#Feed Atom
feed:
type: atom
path: atom.xml
limit: 20
```

3. **主题目录下_config.yml**中添加
```
rss: /atom.xml
```


### Hexo 添加看板娘

1. 进入hexo目录

```
npm install --save hexo-helper-live2d

```
2. **根目录_config.yml**中添加

```
#看板娘
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  log: false
  model:
    use: live2d-widget-model-wanko #可选择不同的看板娘名称
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
  react:
    opacity: 0.7
```

3. **可以添加看板娘列表**中添加
```
live2d-widget-model-chitose
live2d-widget-model-epsilon2_1
live2d-widget-model-gf
live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16
```
想用哪个就 npm install --save xxx ， 然后在配置文件use: xxx 进行修改就好了。

4. **取消看板娘**

```
 npm uninstall hexo-helper-live2d 
```

然后去掉配置文件里面的配置就好了

### Hexo 网站地图

1. **添加sitemap**

```
    npm install hexo-generator-sitemap --save #sitemap.xml适合提交给谷歌搜素引擎
    npm install hexo-generator-baidu-sitemap --save #baidusitemap.xml适合提交百度搜索引擎
```
然后在站点配置文件_config.yml中添加以下代码

2. 自动生成sitemap,在**根目录_config.yml**中添加

sitemap:
    path: sitemap.xml
baidusitemap:
    path: baidusitemap.xml

最后修改**根目录_config.yml**中的url

url: http://你的网站

然后hexo g, 会在/public目录下生成sitemap.xml和baidusitemap.xml 网站地图。


### Hexo 评论系统 gitalk

1. **创建评论仓库**

    1. 首先到[github中创建repository](https://github.com/new)，库名称可以叫XXXtalk，因为这个仓库是用来存储我们的评论内容的。
    2. 创建好之后，进入这个仓库的settings界面找到issues选项，确保它的勾选着的。

2. **注册Github Application**

    1. 到[github中创建Github Application](https://github.com/settings/applications/new)。
    2. 名称和描述可以随意填写，两个URL填写你的博客地址就可以了。注册之后就可以看到Client ID 和 Client Secret。这两个是我们需要的东西。

3. **根目录下配置gitalk**
    1. 将下面的代码赋值到根目录下
    ```
    gitalk: 
        clientID: 你的clientID 
        clientSecret: 你的clientSecret
        repo: 你的repo名 //xxxtalk 刚才我们创建的仓库名
        owner: 你的GitHub名 
        admin: [你的GitHub名称]
    ```
    2. 接下来hexo g 执行之后，你会发现你的文章下面会出现评论了。

4. **错误处理**

    1. 第一次添加gitalk出现了 Error:Validation Failed 这样的错误具体原因是因为Github 限制 labal 长度不能超过 50引起的。 解决办法是使用md5对id进行加密。
    2. 解决方案：
       首先将下面的内容保存为md5.js放在 themes/material-X/source/js/ 下
    ``` 
        ! function(n) {
            "use strict";
            function t(n, t) {
                var r = (65535 & n) + (65535 & t);
                return (n >> 16) + (t >> 16) + (r >> 16) << 16 | 65535 & r
            }
            function r(n, t) {
                return n << t | n >>> 32 - t
            }
            function e(n, e, o, u, c, f) {
                return t(r(t(t(e, n), t(u, f)), c), o)
            }
            function o(n, t, r, o, u, c, f) {
                return e(t & r | ~t & o, n, t, u, c, f)
            }
            function u(n, t, r, o, u, c, f) {
                return e(t & o | r & ~o, n, t, u, c, f)
            }
            function c(n, t, r, o, u, c, f) {
                return e(t ^ r ^ o, n, t, u, c, f)
            }
            function f(n, t, r, o, u, c, f) {
                return e(r ^ (t | ~o), n, t, u, c, f)
            }
            function i(n, r) {
                n[r >> 5] |= 128 << r % 32, n[14 + (r + 64 >>> 9 << 4)] = r;
                var e, i, a, d, h, l = 1732584193,
                    g = -271733879,
                    v = -1732584194,
                    m = 271733878;
                for (e = 0; e < n.length; e += 16) i = l, a = g, d = v, h = m, g = f(g = f(g = f(g = f(g = c(g = c(g = c(g = c(g = u(g = u(g = u(g = u(g = o(g = o(g = o(g = o(g, v = o(v, m = o(m, l = o(l, g, v, m, n[e], 7, -680876936), g, v, n[e + 1], 12, -389564586), l, g, n[e + 2], 17, 606105819), m, l, n[e + 3], 22, -1044525330), v = o(v, m = o(m, l = o(l, g, v, m, n[e + 4], 7, -176418897), g, v, n[e + 5], 12, 1200080426), l, g, n[e + 6], 17, -1473231341), m, l, n[e + 7], 22, -45705983), v = o(v, m = o(m, l = o(l, g, v, m, n[e + 8], 7, 1770035416), g, v, n[e + 9], 12, -1958414417), l, g, n[e + 10], 17, -42063), m, l, n[e + 11], 22, -1990404162), v = o(v, m = o(m, l = o(l, g, v, m, n[e + 12], 7, 1804603682), g, v, n[e + 13], 12, -40341101), l, g, n[e + 14], 17, -1502002290), m, l, n[e + 15], 22, 1236535329), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 1], 5, -165796510), g, v, n[e + 6], 9, -1069501632), l, g, n[e + 11], 14, 643717713), m, l, n[e], 20, -373897302), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 5], 5, -701558691), g, v, n[e + 10], 9, 38016083), l, g, n[e + 15], 14, -660478335), m, l, n[e + 4], 20, -405537848), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 9], 5, 568446438), g, v, n[e + 14], 9, -1019803690), l, g, n[e + 3], 14, -187363961), m, l, n[e + 8], 20, 1163531501), v = u(v, m = u(m, l = u(l, g, v, m, n[e + 13], 5, -1444681467), g, v, n[e + 2], 9, -51403784), l, g, n[e + 7], 14, 1735328473), m, l, n[e + 12], 20, -1926607734), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 5], 4, -378558), g, v, n[e + 8], 11, -2022574463), l, g, n[e + 11], 16, 1839030562), m, l, n[e + 14], 23, -35309556), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 1], 4, -1530992060), g, v, n[e + 4], 11, 1272893353), l, g, n[e + 7], 16, -155497632), m, l, n[e + 10], 23, -1094730640), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 13], 4, 681279174), g, v, n[e], 11, -358537222), l, g, n[e + 3], 16, -722521979), m, l, n[e + 6], 23, 76029189), v = c(v, m = c(m, l = c(l, g, v, m, n[e + 9], 4, -640364487), g, v, n[e + 12], 11, -421815835), l, g, n[e + 15], 16, 530742520), m, l, n[e + 2], 23, -995338651), v = f(v, m = f(m, l = f(l, g, v, m, n[e], 6, -198630844), g, v, n[e + 7], 10, 1126891415), l, g, n[e + 14], 15, -1416354905), m, l, n[e + 5], 21, -57434055), v = f(v, m = f(m, l = f(l, g, v, m, n[e + 12], 6, 1700485571), g, v, n[e + 3], 10, -1894986606), l, g, n[e + 10], 15, -1051523), m, l, n[e + 1], 21, -2054922799), v = f(v, m = f(m, l = f(l, g, v, m, n[e + 8], 6, 1873313359), g, v, n[e + 15], 10, -30611744), l, g, n[e + 6], 15, -1560198380), m, l, n[e + 13], 21, 1309151649), v = f(v, m = f(m, l = f(l, g, v, m, n[e + 4], 6, -145523070), g, v, n[e + 11], 10, -1120210379), l, g, n[e + 2], 15, 718787259), m, l, n[e + 9], 21, -343485551), l = t(l, i), g = t(g, a), v = t(v, d), m = t(m, h);
                return [l, g, v, m]
            }

            function a(n) {
                var t, r = "",
                    e = 32 * n.length;
                for (t = 0; t < e; t += 8) r += String.fromCharCode(n[t >> 5] >>> t % 32 & 255);
                return r
            }

            function d(n) {
                var t, r = [];
                for (r[(n.length >> 2) - 1] = void 0, t = 0; t < r.length; t += 1) r[t] = 0;
                var e = 8 * n.length;
                for (t = 0; t < e; t += 8) r[t >> 5] |= (255 & n.charCodeAt(t / 8)) << t % 32;
                return r
            }

            function h(n) {
                return a(i(d(n), 8 * n.length))
            }

            function l(n, t) {
                var r, e, o = d(n),
                    u = [],
                    c = [];
                for (u[15] = c[15] = void 0, o.length > 16 && (o = i(o, 8 * n.length)), r = 0; r < 16; r += 1) u[r] = 909522486 ^ o[r], c[r] = 1549556828 ^ o[r];
                return e = i(u.concat(d(t)), 512 + 8 * t.length), a(i(c.concat(e), 640))
            }

            function g(n) {
                var t, r, e = "";
                for (r = 0; r < n.length; r += 1) t = n.charCodeAt(r), e += "0123456789abcdef".charAt(t >>> 4 & 15) + "0123456789abcdef".charAt(15 & t);
                return e
            }
            function v(n) {
                return unescape(encodeURIComponent(n))
            }
            function m(n) {
                return h(v(n))
            }
            function p(n) {
                return g(m(n))
            }
            function s(n, t) {
                return l(v(n), v(t))
            }
            function C(n, t) {
                return g(s(n, t))
            }
            function A(n, t, r) {
                return t ? r ? s(t, n) : C(t, n) : r ? m(n) : p(n)
            }
            "function" == typeof define && define.amd ? define(function() {
                return A
            }) : "object" == typeof module && module.exports ? module.exports = A : n.md5 = A
        }(this);
    ```
        保存之后，修改主题目录下 layout/_partial/scripts.ejs 138行，修改如下

    ```
        <% if (enableGitalk) { %>
            <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
            <script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>
            <script src="/js/md5.js"></script> // 添加！！！！
            <script type="text/javascript">
                var gitalk = new Gitalk({
                    clientID: "<%- config.gitalk.clientID %>",
                    clientSecret: "<%- config.gitalk.clientSecret %>",
                    repo: "<%- config.gitalk.repo %>",
                    owner: "<%- config.gitalk.owner %>",
                    admin: "<%- config.gitalk.admin %>",
                    <% if(page.gitalk && page.gitalk.id) { %>
                    id: "<%= page.gitalk.id %>",
                    <% } else { %>
                    id: md5(location.pathname), // 修改！！！！
                    <% } %>
                    distractionFreeMode: false // Facebook-like distraction free mode
                });
                gitalk.render('gitalk-container');
            </script>
        <% } %>
    ```
        如上一处添加，一处修改，当然也可以将这块代码完全替换了也可以，然后hexo g 之后问题应该就解决了。


### 常见问题

#### Hexo deploy 报错
**报错信息一**

```
remote: Weak credentials. Please Update your password to continue using GitHub.
remote: See https://help.github.com/articles/creating-a-strong-password/.
fatal: unable to access 'https://github.com/xxx/xxx.github.io.git/': The requested URL returned error: 403
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: Spawn failed
    at ChildProcess.<anonymous> (/opt/hexo/node_modules/_hexo-util@0.6.3@hexo-util/lib/spawn.js:52:19)
    at ChildProcess.emit (events.js:198:13)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:248:12)


```
**问题描述**
这种报错是突然出现的，一看就知道是github的问题，大概意思就是说你的密码强度太低。
所以造成了连接不上GitHub仓库地址的报错。刚开始还比较迷茫，不知道是哪个密码比较弱了。
直到我想添加ssh key的时候，GitHub官网提醒，说我的GitHub密码比较弱，需要修改，否则一个月后自动修改。

这简直比较狗血，没想到还会影响到git提交。 但是目前只发现会影响hexo deploy的自动部署，账号上的其他clone的仓库不会影响。

**解决办法(linux)**

1. 更改GitHub密码。
2. 重新生成ssh key。
```
    sudo ssh-keygen -C 'xxx@email.com' -t rsa
```
    一路 y 就可以了。

3. 将重新生成的key添加到GitHub上。
