Clash如何编辑自定义规则不被更新订阅覆盖
===

Clash作为被广泛使用的网络代理软件，大量用户使用它配合机场订阅来实现魔法上网，大部分用户使用的都是机场订阅的默认规则，但是如果想自定义规则，大部分人应该都不知道怎么做，最近碰巧遇上访问chatgpt用不了，原以为是机场的问题，看了clash的logs日志后，发现是chatgpt.com这个域名被DNS劫持了，解析成了国内的IP，导致clash走了直连规则，自然连不上了，不想用全局模式来上网，这样正常的国内网站还有应用都要绕一圈，浪费流量，网速还慢，于是研究了下clash如何自定义规则。
Clash的配置文件路径

我们在Clash里导入订阅链接后，其实是在设备本地端生成了配置文件，里面含有线路信息，流量的分流规则等信息，Clash的.yaml格式的配置文件路径如下：
**Windows:**  `C:\Users\你的用户名.config\clash\profiles`
**Linux:**  `~/.config/clash/`
**Mac:** `~/.config/clash/`
**Clash for Android (Meta/Pro):** `/data/clash/`

## 添加自定义配置规则

Clash 支持订阅文件预处理，每次更新订阅时不会被覆盖，以我使用的clash for windows为例：
点击左侧Settings(设置),鼠标滑到Profies->Parsers这一项，右侧点击Edit编辑按钮，如下图

![](/docs/Spec/clash_Parsers.png)

## Clash预处理

在编辑器中增加如下内容，url后面的订阅链接换成你所用机场的订阅链接，最后一行的规则，你就可以按照你自己的想法添加规则。
```yml
parsers:
  - url: 你的订阅链接
    yaml:
      prepend-rules:
        - DOMAIN,test.com,DIRECT # rules最前面增加一个规则
```


## 自定义规则的编写形式

Clash支持多种的规则编写形式，我们常用的一般是一下几种：
```yml
DOMAIN-SUFFIX：域名后缀匹配
DOMAIN：域名匹配
DOMAIN-KEYWORD：域名关键字匹配
IP-CIDR：IP段匹配
```

比如我想增加一条规则，chatgpt.com走代理，那么这条规则是`DOMAIN-SUFFIX,chatgpt.com,Mutdot` 意思就是所有`chatgpt.com`域名的访问都通过`mutdot`机场代理出去，如果是要直连的话`DOMAIN-SUFFIX,baidu.com,DIRECT` 意思就是所有`baidu.com`的域名都直连访问，不走代理，`Mutdot`这个替换成你自己的机场名字，具体名字可以打开clash里面你的机场配置文件，看看应该写成什么。



执行了以上操作后每次载入机场的配置文件，clash都会把你自己定义的规则合并到机场的配置文件中，更新订阅文件也不会影响到你的自定义规则了。
