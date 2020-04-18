---
layout: page
title: 카테고리가 "Java"인 글 목록
---
<!-- Java에 포함된 글들: -->
<!-- .code is a class with one font-family property. -->
<section>
	<!-- <ul>
		{% for post in site.categories.Java %}
		<li>
			<div class="post-date code">
				<span>{{ post.date | date: "%b %d" }}</span>
			</div>
			<div class="title">
				<a href="{{ post.url | prepend: site.baseurl | prepend: site.url }}">{{ post.title }}</a>
			</div>
		</li>
		{% endfor %}
	</ul> -->

	{% for post in site.categories.Java %}
		{% unless post.next %}
			<h3 class="code">{{ post.date | date: '%Y' }}</h3>
		{% else %}
			{% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
			{% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
			{% if year != nyear %}
				<h3 class="code">{{ post.date | date: '%Y' }}</h3>
			{% endif %}
		{% endunless %}

		<ul>
			<li>
				<div class="post-date code">
					<span>{{ post.date | date: "%b %d" }}</span>
				</div>
				<div class="title">
					<a href="{{ post.url | prepend: site.baseurl | prepend: site.url }}">{{ post.title }}</a>
				</div>
			</li>
		</ul>

	{% endfor %}
</section>
