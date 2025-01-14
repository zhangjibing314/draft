# Liquid
## 简介
Liquid 是一门模板语言，用 ruby 实现。多应用于 web 开发中，适合面向非开发者的应用场景，比如 在线商店、博客、CMS[1]等。

## 基本语法
* 变量和插值  
示例：将 `product.title` 的值插入到 <p> 标签中  
```Liquid
<p>{{product.title}}<p>
```
* 过滤器(filters)
示例：格式化输出"hello wordl"为"HELLO WORLD"  
```Liquid
{{"hello world" | upcase}}
```
* 控制流
```Liquid
{% if product.available %}
  <p>产品已上架</p>
{% else %}
  <p>产品已下架</p>
{% endif %}
```

* for 循环
```Liquid
<ul>
  {% for item in collection.items %}
    <li>{{ item.name }}</li>
  {% endfor %}
</ul>
```
* 对象和数组
```Liquid
{{ user.name }}  <!-- 获取对象 user 的 name 属性 -->
{% for product in products %}
  {{ product.title }}
{% endfor %}
```
* 自定义标签
```Liquid
{% my_custom_tag %}
```

[1]: content management system, 即内容管理系统
