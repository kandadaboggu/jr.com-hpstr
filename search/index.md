---
layout: page
title: "Search"
date:
modified:
description:
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-8-abstract-wallpaper-pack-for-iphone-5s-5c-and-ipod-touch-retina/
share: false
comments: false
---
<header class="post-header">
	<h1>Search: {{ site.title }}</h1>
</header>

<div id="search">
	<form action="/search" method="get">
		<input type="text" id="search-query" name="q" placeholder="Search" autocomplete="on">
	</form>
</div>

<section id="search-results" style="display: none;">
	<h1>Search Results</h1>
	<div class="entries">
	</div>
</section>

{% raw %}
<script id="search-results-template" type="text/mustache">
	{{#entries}}
	<article>
		<h3>
			{{#date}}<small><time datetime="{{pubdate}}" pubdate>{{displaydate}}</time></small>{{/date}}
			- <a href="{{url}}">{{title}}</a>
		</h3>
	</article>
	{{/entries}}
</script>
{% endraw %}
