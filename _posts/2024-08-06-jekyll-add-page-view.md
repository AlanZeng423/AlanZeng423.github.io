---
layout: post
title: Jekyll 网站添加访问量统计分析
author: AlanZeng
description: RAG 
date: 2024-07-24
tags: Development
---

## Google Analysis

谷歌分析是谷歌提供的免费网络分析服务，用于跟踪和报告网站流量。将谷歌分析添加到 Jekyll 网站十分简单。登录  [谷歌分析](https://www.google.com/intl/zh-CN/analytics/)  并新建一个媒体资源，以获取网站的跟踪 ID。可在`管理 > 媒体资源 > 跟踪信息 > 跟踪代码`下找到跟踪 ID。

在 Jekyll 网站上部署谷歌分析，首先在`_includes`文件夹新建名为`google-analytics.html`的文件,并写入以下代码：

```javascript
<script async src="https://www.googletagmanager.com/gtag/js?id=***"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', '***');
</script>
```

然后在`_config.yml`中添加跟踪 ID：

```yaml
# Google Analytics
google_analytics: ***
```

最后添加`google-analytics.html`到网页，谷歌建议把跟踪代码放在每个页面的`<head>`中，以确保正确跟踪所有访问。

    {% raw %}{%- if jekyll.environment == 'production' -%}
      {%- include google-analytics.html -%}
    {%- endif -%}
    {% endraw %}


