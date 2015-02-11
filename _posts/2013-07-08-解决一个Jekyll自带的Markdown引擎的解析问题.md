---
layout: post
---

# 解决一个Jekyll自带的Markdown引擎的解析问题

## 问题描述  

Markdown文件里面，如果有字符串的开头用到了`_`字符，那么Jekyll自带的Markdown引擎在解析时会提示错误，错误信息如图所示：  
![Alt](http://ww3.sinaimg.cn/large/6321ab24gw1e6f1wh4oh7j20h102x3yu.jpg)  
例如：  

    this _is a book

## 解决方法  

**方法一：**  

在`_`的前面加上转义字符。  
例如：  

    this \_is a book  

**方法二：**  

更换Jekyll使用的Markdown引擎。在站点的配置文件`_config.yml`中定义`markdown`属性即可。  
例如：  

markdown: rdiscount