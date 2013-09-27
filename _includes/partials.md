{% capture partials_cache %}
    {% if page.partials %}
        {% assign sorted_pages = site.pages | sort:'path' %}
        {% assign partials_content = '' %}
        {% for item in sorted_pages %}
            {% if item.path contains page.partials %}
                {% assign item_id = item.path | split:'/' | last | remove:'.md' %}
                {% capture replace_example %} class="example:/{{item.path | remove:item_id | remove:'.md'}}tests/{% endcapture %}

                {% capture partial_code %}
<span class="small-pseudo-button toggle-button js-outer-toggler"><span class="button-content">исходный код скина</span></span>  
[оно же на Гитхабе](https://github.com/kizu/stylobate/tree/master/{{ page.partials }}{{ item.path | split:'/' | last | split:'_' | first | remove:'.md' | remove:'kind-' }}/){:.outer-link}

<pre class="language-styl is-hidden" data-src="s/stylobate/{{ page.partials }}{{ item.path | split:'/' | last | split:'_' | first | remove:'.md' | remove:'kind-' }}/{{ item.path | split:'/' | last | replace:'.md','.styl' }}"></pre>
                {% endcapture %}

                {% capture partials_content %}{{ partials_content }}
<article markdown="1">
<aside markdown="1">
{{ partial_code }}
</aside>

{:#{{ item_id }}}
{{ item.content | replace:' class="example:',replace_example }}
</article>
                {% endcapture %}
            {% endif %}
        {% endfor %}

        {% assign partials_content = partials_content | markdownify %}

        {% assign partials_toc = '' %}
        {% assign toc_input = partials_content %}
        {% include tenkan/toc.md %}
        {% for toc_id in toc_headers_id %}
            {% if toc_headers_indexes[forloop.index0] != '' and toc_headers_indexes[forloop.index0] != '1' %}
                {% capture partials_toc %}{{ partials_toc }}<a class="aside-nav-item{% if toc_headers_indexes[forloop.index0] != '2' %} aside-nav-item_{{ toc_headers_indexes[forloop.index0] | minus:1 }}{% endif %}" href="#{{ toc_id }}">{{ toc_headers_contents[forloop.index0] }}</a>{% endcapture %}
            {% endif %}
        {% endfor %}
    {% endif %}
{% endcapture %}