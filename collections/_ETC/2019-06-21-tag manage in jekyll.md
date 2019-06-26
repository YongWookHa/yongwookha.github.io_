---
layout: post
title: jekyll에서 tag link 관리하기
subtitle: (collection으로 category 관리하는 경우)
tags: [ETC, jekyll, tag, collection, category]
comments: true
---

jekyll에 익숙해지려면 아직 시간이 좀 걸릴 듯 하다. 사실 [Beautiful-jekyll 템플릿](https://github.com/daattali/beautiful-jekyll)이 워낙 잘 만들어져 있어서 웹 개발에 익숙한 분들은 금방 할 수 있을 내용이지만 학부 수준에서 아주 기초적인 지식 밖에 없는 나는 꽤 고전하고 있다. 그래도 Ruby를 통해 localhost로 변경 사항을 확인해가며 꾸역꾸역 원하는 블로그 모양을 만들어 내고 있음에 힘이 난다. Front-end는 눈에 보이는 진전들이 생기니 더 재미있는 것 같다.

## 서론

오늘은 블로그에 tag 정보 관리 기능을 갖다 붙이느라 겪었던 수고로움의 결과를 기록한다. 현재 나는 `_post`폴더 없이 `collection`폴더 내부에 `_category`폴더들을 만들어 blog category를 관리하고 있다. 전체적인 구조를 트리로 표현하면 다음과 같다.

```
yongwookha.github.io/
├─collections/
│  ├─_category1/
│  │      2019-06-19-first.md
│  │      index.html
│  │      
│  ├─_category2/
│  │      2019-06-19-second.md
│  │      index.html
│  │      
│  └─_category3/
│         2019-06-19-third.md
│         index.html
├─css/
├─img/
├─js/
├─_data/
├─_includes/
├─_layouts/
...
```
jekyll은 [markdown](https://ko.wikipedia.org/wiki/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4)으로 씌인 plain text를 static website나 블로그로 변환시켜주는 것을 목표로 하는데, 무슨 이유에서인지 blog에 필수적인 요소인 category 관리 기능은 공식적으로 지원하지 않는 듯 하다. 그럼에도 불구하고, 우리는 방법을 찾는다. 늘 그랬듯. 내가 쓰고 있는 [collection](https://jekyllrb-ko.github.io/docs/collections/) 기능을 이용한 categorize 방법도 그 중 하나다. collection은 본래 jekyll에서 page, post를 나누듯, 문서의 큰 분류를 나누는 것을 돕는 기능이지만, 나는 post의 소분류를 나누는 곳에 사용하기로 했다. [jekyll docs](https://jekyllrb-ko.github.io/docs/collections/#step1)를 보면, collection을 여러개 사용하는 경우, `_config.yml`에 `collections_dir`을 입력하여 여러 collection(category)들을 한 디렉토리에 모아놓을 수 있다고 한다. 

나는 여러 category들을 사용할 예정이라, 이 instruction을 그대로 따랐다. 그런데 문제는, 이렇게 collection 별로 post들을 정리해놓으니 jekyll의 `site`변수를 이용해 `posts`를 긁어모으는게 안되는 것이었다. `Beautiful-jekyll`의 많은 기능들이 `site`변수를 통해 post 정보에 접근하는 방식이었기에 이 문제는 꽤 심각했다. 역시, 아무리 템플릿을 가져왔다 해도, 손도 안대고 그대로 사용할 수 있을거라는 생각은 오산이었다. 하는 수 없이 `jekyll docs`를 정독하고, Liquid 문법을 읽어봤다.

## 본론

방법은 간단하다. `site.posts`가 아무것도 반환하지 않는다해도, collection으로 관리되는 post들에 대한 접근은 `site.collections`로 할 수 있다. 이것은 collection array를 반환하기 때문에 `site.posts`와 같이 각각의 post에 접근할 수 있기 위해서는 다음과 같은 코드를 사용할 수 있다.

{% raw %}
```html
{% for c in site.collections %}
    {% assign label = c.label %}
    {% assign collection = site[label] %}
    {% for post in collection %}
        {% comment %} ### use post ### {% endcomment %}
    {% endfor %}
{% endfor %}
```
{% endraw %}

jekyll native에서 제공하는 tag 기능을 활용하기 위해서는 `_config.yml`에 `link-tags: true`를 추가하고, root 디렉토리에 `tags.html` 파일을 만들어야 한다. 위의 코드를 이용해서 다음과 같은 `tags.html` 파일을 만들 수 있다. Liquid에서는 {% raw %}`{% comment %}`와 `{% endcomment %}`{% endraw %}로 주석을 작성하니 참고하기 바란다.

{% raw %}
```
---
layout: page
title: 'Tag Index'
---
{% comment %} collections의 모든 tag를 tags_list에 저장 {% endcomment %}
{%- assign tags_list = "" | split: "" -%}
{% for c in site.collections %}
    {% assign collection = c.label %}
    {% assign collection_tags =  site[collection] | map: 'tags' %}
    {% assign tags_list = tags_list | push: collection_tags %}
{% endfor %}
{% assign tags_list = tags_list | uniq | sort %}

{% comment %} tag에 따른 post 나열 {% endcomment %}
{%- for tag in tags_list -%}
    <h2 id="{{ tag }}" class="linked-section">
        <i class="fa fa-tag" aria-hidden="true"></i>
        &nbsp;{{ tag }}&nbsp;
    </h2>
    {% for c in site.collections %}
            {% assign label = c.label %}
            {% assign collection = site[label] %}
            {% for post in collection %}
                {% unless post.tags contains tag %}
                    {% comment %} post에 해당 tag가 없으면 pass {% endcomment %}
                    {% continue %}
                {% endunless %}
                <div class="tag-entry">
                <a href="{{- site.url -}}{{- post.url -}}">{{- post.title -}}</a>
                </div>
            {% endfor %}
    {%- endfor -%}
{%- endfor -%}
```
{% endraw %}

## 결론

syntax highlighting이 적용되지 않아 읽기 불편할 수 있다. 하지만 많이 복잡한 코드는 아니니 차근차근 보면 분명 이해할 수 있을 거라 생각한다. 별건 아니지만, Liquid를 쓸 일이 그렇게 자주 있을 것 같진 않아 잊어버리기 전에 기록해놓는다. 처음에는 익숙하지 않아 만드는데 오랜 시간이 걸리지만, 한번 구축해 놓으면 은근히 뿌듯한게 블로그요, 개인 페이지더라. 가늘더라도 길고 꾸준하게 갔으면 좋겠다. 
