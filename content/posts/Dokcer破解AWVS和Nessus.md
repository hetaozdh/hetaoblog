---
title: "Dokcer破解AWVS和Nessus"
date: 2022-7-25T15:52:50+08:00
lastmod: 2022-07-25T15:52:50+08:00
author: ["吃核桃不吐核桃壳"]

categories:
- 技术

tags:
- 工具
- 网络安全
- AWVS
- Nessus
- Docker

keywords:
- Docker
- AWVS
- Nessus

description: "" # 文章描述，与搜索优化相关
summary: "" # 文章简单描述，会展示在主页
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
draft: false # 是否为草稿
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
autonumbering: true # 目录自动编号
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
searchHidden: false # 该页面可以被搜索到
showbreadcrumbs: true #顶部显示当前路径
mermaid: true
cover:
    image: ""
    caption: ""
    alt: ""
    relative: false
---

记录一下安装Docker版本的破解AWVS和Nessus

<!-- more -->

# Docker破解AWVS和Nessus

AWVS和Nessus，不多介绍，漏洞扫描器，前者多用于扫Web漏洞，后者多用于扫系统漏洞。

看到有人把自用的AWVS和Nessus都封装成Docker开源了，开箱即用也挺方便的就配置下。

~~开源大佬牛逼！！！~~

## 0x01 安装 Nessus

感谢大佬开源，[项目地址](https://github.com/elliot-bia/nessus)。全都帮我们配置好了，真的感谢大佬🙏。下面一行代码就搞定了安装，比起以前用Nessus可以简单了太多。

```shell
docker run -itd -p 8834:8834 ramisec/nessus
```

大佬也是个很有意思的人，给我们留了道作业题，要密码就去GitHub仓库自己找。相信有安全功底应该不难的，实在不会可以抄作业或者直接进Docker用nessuscli修改密码自己Google：

{% folding child:codeblock color:yellow 噫～不会这么简单一道题都要抄作业吧(･･;) %}

```markdown
# v2 20220621
更新20220620最新版插件，同时采用2个版本 nessus/nessuslite
前者是已经破解好并且编译好插件，开箱即用
后者开机后需要等待几分钟编译插件
password:
U2FsdGVkX19WZv+QOe8awVyJwXDPSNSIC1X4AMNA4+3rO8mL/3HZ+mS/Or3DhcWXKs0WHfvOH1q/YNtVdXnaHg==
**tips**: github/elliot-bia
```

​	password是AES加密，加密密码🔐我不说，解密完得到一串base64然后得到密码。

{% endfolding %}

完事打开<https://localhost:8834/#/>就OK啦。

![好耶](https://cdn.jsdelivr.net/gh/hetaozdh/hetaozdh.github.io@pic/img/image-20220725185117448.png)

## 0x02 安装AWVS

[项目传送门🔗](docker run -it -d --name awvs -p 3443:3443 xrsec/awvs:latest)

一行命令安装：

```shell
bash <(curl -skm 10 https://www.fahai.org/aDisk/Awvs/check.sh) xrsec/awvs
```

下载[`RootCA.cer`](https://cdn.jsdelivr.net/gh/XRSec/AWVS14-Update@main/.github/resources/ca.cer)加入根证书并设置信任。然后打开<https://127.0.0.1:3443/#/>

<img src="https://cdn.jsdelivr.net/gh/hetaozdh/hetaozdh.github.io@pic/img/image-20220725185807826.png" alt="image-20220725185807826" style="zoom:33%;" />

**用户名：awvs@awvs.lan**
**密码： Awvs@awvs.lan**

搞定🤝

## 0x03 感谢和参考文献

感谢开源大佬法海和elliot-bia

参考文献：

- <https://www.fahai.org/index.php/archives/5/>
- <https://mari0er.club/post/scanner.html/>