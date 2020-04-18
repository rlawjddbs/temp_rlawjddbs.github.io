---
layout: page
title: 카테고리가 "Java"인 글 목록
---
<!-- .code is a class with one font-family property. -->
<section>
	{% for post in site.categories.Java %}	
		<ul>
			<li>
				<div class="post-date code">
					<span>{{ post.date | date: "%Y-%m-%d" }}</span>
				</div>
				<div class="title">
					<a href="{{ post.url | prepend: site.baseurl | prepend: site.url }}">{{ post.title }}</a>
				</div>
			</li>
		</ul>
	{% endfor %}
</section>
