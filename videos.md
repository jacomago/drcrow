---
layout: default
title: VIDEO
permalink: /videos.html
---

<div class="video-page-container">

# VIDEOS

{% for video in site.data.videos %}
<section class="video-feature">

## {{ video.title }}

{{ video.description }}

{% if video.facebook_url or video.wix_video_url %}
{% include video_embed.html facebook_url=video.facebook_url wix_video_url=video.wix_video_url %}
{% endif %}

</section>
{% endfor %}

</div>
