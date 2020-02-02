---
layout: page
title: 카테고리가 "javascript"인 글 목록
---
<!--temp에 포함된 글들:-->
<section>
	<p>{{ categoryNm[1] }}</p>
	<p>{{ page.url }}</p>
	<ul>
		{% for post in site.categories.javascript %}
		<li style="list-style:disc">
			<a href="{{ post.url }}">{{ post.title }}</a>
			<div class="post-date code float_right"><span id="koreanSpan">{{post.date | date : '%Y년 %m월 %d일'}}</span></div>
		</li>
		{% endfor %}
	</ul>
</section>
