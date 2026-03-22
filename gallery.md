---
layout: default
title: GALLERY
permalink: /gallery.html
---

<div class="gallery-container">

# GALLERY

Visual manifestations of the Forbidden Zone.

<div class="gallery-grid">
  {% for img in site.data.gallery %}
  <div class="gallery-item">
    <img src="{{ 'assets/images/' | append: img | relative_url }}" alt="Forbidden Zone Manifestation">
  </div>
  {% endfor %}
</div>

</div>
