## 添加301跳转
* 原因：解决原站点地址失效问题
* 文件：src\main\java\com\mtons\mblog\web\interceptor\BaseInterceptor.java
* 内容：preHandle方法中添加如下内容：

```Java
String url = request.getRequestURL().toString();

if (url.endsWith("default.aspx")) {
    response.setStatus(301);
    response.setHeader("Location", "/");
    return false;
}

if (url.endsWith("category.aspx")) {
    response.setStatus(301);
    response.setHeader("Location", "/channel/" + StringUtils.substring(request.getQueryString(),3));
    return false;
}

if (url.endsWith("article.aspx")) {
    response.setStatus(301);
    response.setHeader("Location", "/post/" + StringUtils.substring(request.getQueryString(),3));
    return false;
}

int index = url.indexOf("/article/");
if (index > 0) {
    response.setStatus(301);
    response.setHeader("Location", "/post/" + StringUtils.substring(url, index + 9, url.indexOf("/", index + 9)));
    return false;
}
```

## 启用注册邮箱验证，关闭发表文章
* 原因：避免恶意注册或发表垃圾文章
* 文件：src\main\resources\application.yml
* 内容：
```yml
site:
    controls:
        # 注册开启邮箱验证
        register_email_validate: true
        # 发布文章开关
        post: false
```

## 移除Html Logo和版本号
* 原因：避免特定版本漏洞被利用，减少Html体积
* 文件：src\main\resources\templates\classic\inc\layout.ftl
* 内容：
```html
    <!--
    ------------------------------------------------------
     _____ ______   ________  ___       ________  ________
    |\   _ \  _   \|\   __  \|\  \     |\   __  \|\   ____\
    \ \  \\\__\ \  \ \  \|\ /\ \  \    \ \  \|\  \ \  \___|
     \ \  \\|__| \  \ \   __  \ \  \    \ \  \\\  \ \  \  ___
      \ \  \    \ \  \ \  \|\  \ \  \____\ \  \\\  \ \  \|\  \
       \ \__\    \ \__\ \_______\ \_______\ \_______\ \_______\
        \|__|     \|__|\|_______|\|_______|\|_______|\|_______|
    ------------------------------------------------------------
    version: ${site.version}
    github : https://github.com/langhsu/mblog
    ------------------------------------------------------------
    -->
<meta name="mtons:mblog" content="${site.version}">
```

# 移除阅读全部功能，默认显示全部文章
* 原因：IE浏览器兼容性有问题
* 文件：src\main\resources\templates\classic\channel\view.ftl
* 内容：移除样式topic，移除阅读全部相关代码
```html
 <div class="topic panel panel-default">

 <div class="more-box">
    <a class="btn btn-fulltext" data-toggle="fulltext">
        <i class="icon icon-arrow-down" aria-hidden="true"></i> 阅读全部
    </a>
</div>
```

# 添加百度统计
* 原因：统计分析
* 文件：src\main\resources\templates\classic\inc\footer.ftl
```javascript
<script>
    var _hmt = _hmt || [];
    (function() {
        var hm = document.createElement("script");
        hm.src = "https://hm.baidu.com/hm.js?e4401a6e4dbe027be8577ac37e621f89";
        var s = document.getElementsByTagName("script")[0];
        s.parentNode.insertBefore(hm, s);
    })();
</script>

```

### 打包
```bash
mvn clean package -Dmaven.test.skip=true -Pprd
```
