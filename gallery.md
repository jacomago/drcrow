---
layout: default
title: GALLERY
permalink: /gallery.html
---

<section class="gallery-header">
  <div class="container">
    <h1>GALLERY</h1>
    <p class="lead">Visual manifestations of the Forbidden Zone.</p>
  </div>
</section>

<section class="gallery-content bg-surface-low">
  <div class="container">
    <div class="gallery-grid">
      {% for img in site.data.gallery %}
      <div class="gallery-item">
        <img src="{{ 'assets/images/' | append: img | relative_url }}" alt="Forbidden Zone Manifestation">
      </div>
      {% endfor %}
    </div>
  </div>
</section>
