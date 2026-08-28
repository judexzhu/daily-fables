---
layout: page
title: Concepts
permalink: /concepts/
---

<h1 class="en-only">Every concept, by territory</h1>
<h1 class="zh-only zh">按领域浏览全部概念</h1>
<p class="en-only page-lede">{{ site.posts | size }} fables so far. If you are studying for something in particular, start here rather than at the top of the archive.</p>
<p class="zh-only zh page-lede">至今 {{ site.posts | size }} 则寓言。如果你正在准备某个特定主题，从这里开始，比从存档顶端往下翻要快。</p>

{% assign order = "distributed-systems,kubernetes,networking,linux,storage,security,sre,aws,microservices,performance,memory,runtime,observability,operations,scheduling,databases,virtualization,ci-cd,etcd" | split: "," %}
{% assign labels = "Distributed systems,Kubernetes,Networking,Linux,Storage,Security,SRE,AWS,Microservices,Performance,Memory,Runtime,Observability,Operations,Scheduling,Databases,Virtualization,CI/CD,etcd" | split: "," %}
{% assign labels_zh = "分布式系统,Kubernetes,网络,Linux,存储,安全,SRE,AWS,微服务,性能,内存,运行时,可观测性,运维,调度,数据库,虚拟化,CI/CD,etcd" | split: "," %}

<div class="concept-grid">
{% for tag in order %}
  {% assign tagged = site.tags[tag] %}
  {% if tagged %}
  <div class="concept-group">
    <h2>
      <span class="en-only">{{ labels[forloop.index0] }}</span>
      <span class="zh-only zh">{{ labels_zh[forloop.index0] }}</span>
      <span class="mono dim">{{ tagged | size }}</span>
    </h2>
    {% for post in tagged %}
    <a href="{{ post.url | relative_url }}">{{ post.concept }}</a>
    {% endfor %}
  </div>
  {% endif %}
{% endfor %}
</div>
