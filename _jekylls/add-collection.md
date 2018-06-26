---
title: "Jekyll. Добавление раздела"
---
Добавление раздела. How to...<!--more-->

## Дирректория раздела
Нужно создать в корне проекта директорию с именем **_ИмяРаздела**
например **_angular**

## Конфигурация
в файле **_config.yml** добавить следующие секции
```javascript
collections:
...
  angular:
    output: true
    permalink: /:collection/:name
...

defaults:
...
  - scope:
      path: ""
      type: angular
    values:
      layout: single
      author_profile: false
      share: true
      comments: true
...
```

	  
## Навигация
В файле **_data\navigation.yml** добавить следующие строки
```javascript
main:
...
  - title: "Angular"
    url: /angular/
```
	
## Создать шаблон списка статей в разделе
Например такой **_pages\angular-archive.html**
{% raw %}
```javascript
---
layout: archive
title: "Angular(2+) и все вокруг"
permalink: /angular/
author_profile: false
---
{% for post in site.angular %}
  {% include archive-single.html %}
{% endfor %}
```
{% endraw %}