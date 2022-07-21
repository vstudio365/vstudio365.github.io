---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: blog-home
---


This blog is a place to share knowledge around `SharePoint, Azure, M365 & Power Platform`

### Posts
{% for post in site.posts %}
  <a href="{{ post.url  | prepend: site.baseurl  }}">{{ post.title }}</a>
{% endfor %}