---
layout: archive
permalink: /year-archive/
title: "Blogy"
author_profile: true
redirect_from:
  - /wordpress/blog-posts/
---

<div style="margin-top: 20px; margin-bottom: 20px;">
  <p>各種資料や<a href="https://note.com/hadron_kyoto">note記事</a>、留学報告書などの更新を投稿します。内容はもっぱら日本語です。</p>
</div>

<div style="margin-bottom: 20px; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">
  <strong style="margin-right: 10px;"><i class="fas fa-filter"></i> Filter by Tag:</strong>
  <a href="#" data-tag="all" onclick="filterPosts(this.getAttribute('data-tag')); return false;" style="margin-right: 15px; font-weight: bold; text-decoration: underline;">All</a>
  {% for tag in site.tags %}
    <a href="#" data-tag="{{ tag | escape }}" onclick="filterPosts(this.getAttribute('data-tag')); return false;" style="margin-right: 15px; text-decoration: underline;">{{ tag }}</a>
  {% endfor %}
</div>

{% include base_path %}
{% capture written_year %}'None'{% endcapture %}

{% for post in site.posts %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% if year != written_year %}
    <h2 id="{{ year | slugify }}" class="archive__subtitle year-heading" data-year="{{ year }}">{{ year }}</h2>
    {% capture written_year %}{{ year }}{% endcapture %}
  {% endif %}
  
  <div class="post-item" data-tags="{{ post.tags | join: ',' }}" data-year="{{ year }}" style="display: contents;">
    {% include archive-single.html %}
  </div>
{% endfor %}

<script>
function filterPosts(selectedTag) {
  const posts = document.querySelectorAll('.post-item');
  const visibleYears = new Set();

  posts.forEach(post => {
    const tags = post.getAttribute('data-tags').split(',').map(t => t.trim());
    const year = post.getAttribute('data-year');

    if (selectedTag === 'all' || tags.includes(selectedTag)) {
      // 💡変更点: block ではなく contents に戻すことでデザイン崩れを防止
      post.style.display = 'contents';
      visibleYears.add(year);
    } else {
      post.style.display = 'none';
    }
  });

  document.querySelectorAll('.year-heading').forEach(heading => {
    const headingYear = heading.getAttribute('data-year');
    if (visibleYears.has(headingYear)) {
      heading.style.display = 'block';
    } else {
      heading.style.display = 'none';
    }
  });
}
</script>
