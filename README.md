jekyll-taxonomy-pagination
=========================

This is a plugin for the [Jekyll](http://github.com/mojombo/jekyll) static site generator, 
to enable pagination of posts organized by tags and categories.

Though Jekyll provides pagination, it is only available in index pages. This plugin enables
pagination also on tag and category pages.

It's based on [the jekyll-tagging-pagination plugin](https://github.com/ilyakhokhryakov/jekyll-tagging-pagination) by Ilya Khokhryakov.

Installation
------------
Copy the file `taxon_pagination.rb` into your Jekyll site's `_plugins` directory 
(you might need to create it). You may also need to install the [activesupport gem](https://rubygems.org/gems/activesupport/versions/5.0.0);
note that this requires at least Ruby 2.2.2.

Usage
-----

In your post's YAML front matter, add tags like this:

    tags:
    - jekyll
    - pagination

In your `_layouts` directory, create the Liquid template files `tag_index.html` and `category_index.html`,
or just the file `taxon_index.html`, which acts as a default for either of those. Posts live in the `paginator.posts` 
array, just like on your homepage:

    {% for post in paginator.posts %}
        {{ post.title }}
    {% endfor %}

Other useful properties of the `paginator`:

- `previous_page_path` and `next_page_path` include the correct URIs to the previous and next pages.
- `taxon_type` is either 'category' or 'tag', which may be useful in your `taxon_index.html` file.
- `taxon` includes the current page's category or tag.
- `taxon_root_path` includes the base uri of this category or tag's pages, something like '/tag/things' 
or '/category/stuff'.

Example
-------

---
layout: default
---

This shows a list of post titles followed by a set of links for every page in the current `paginator`.

    <div id="taxolist">
        <h2>Posts in {{page.taxon_type}} &#8220;{{page.taxon}}&#8221;</h2>
        {% for post in paginator.posts %}
            {{ post.title }}
        {% endfor %}
    </ul>
    </div>

    {% if paginator.total_pages > 1 %}
    <nav>
        <ul class="pagination">
            {% if paginator.previous_page_path %}
            <li><a href="{{ paginator.previous_page_path }}">&larr;Newer</a></li>
            {% endif %}

            {% for page in (1..paginator.total_pages) %}
                {% if page == paginator.page %}
                <li><span>{{ page }}</span></li>
                {% elsif page == 1 %}
                    {% if paginator.taxon %}
                    <li><a href="{{paginator.taxon_root_path}}">1</a></li>
                    {% else %}
                    <li><a href="/">1</a></li>
                    {% endif %}
                {% else %}
                    {% if paginator.taxon %}
                    <li><a href="{{paginator.taxon_root_path}}page{{page}}">{{page}}</a></li>
                    {% else %}
                    <li><a href="/{{ site.paginate_path | replace: '//', '/' | replace: ':num', page }}">{{ page }}</a></li>
                    {% endif %}
                {% endif %}
            {% endfor %}

            {% if paginator.next_page_path %}
            <li><a href="{{ paginator.next_page_path }}">Older&rarr;</a></li>
            {% endif %}
        </ul>
    </nav>
    {% endif %}

