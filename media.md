---
layout: default
title: MUSIC
permalink: /media.html
---

<div class="music-container">
  <h1>A message from outer space . .</h1>
  
  <p class="lead">Here you will find a pan-dimensional portal to the album.</p>

  <section class="album-feature">
    <h2>The new album: STROHAYR</h2>
    <p>The album is presented in its entirety. A story in 10 parts (just like life really).</p>
    {% include bandcamp_player.html album_id="66352487" title="Strohayr by Dr Crow and The Forbidden Zone" url="https://drcrow.bandcamp.com/album/strohayr" %}
  </section>

  <section class="music-track">
    <h2>Crow Hall Insight</h2>
    <p>Get a special insight into the band's home environment......Crow Hall.</p>
    <img src="{{ 'assets/images/crow-hall.jpg' | relative_url }}" alt="Crow Hall" style="border: 5px solid #000; margin-bottom: 20px;">
    {% include bandcamp_player.html track_id="3830604073" title="Radio Interview by Dr Crow and The Forbidden Zone" url="https://drcrow.bandcamp.com/track/radio-interview" %}
  </section>

  <section class="album-feature">
    <h2>A Fistful of Broken Bones</h2>
    {% include bandcamp_player.html album_id="1201722343" title="Fistful Of Broken Bones by Dr Crow and The Forbidden Zone" url="https://drcrow.bandcamp.com/album/fistful-of-broken-bones" %}
  </section>

  <section class="soundcloud-section">
    <h2>THE SOUNDCLOUD ARCHIVE</h2>
    <p>Further frequencies from the zone.</p>
    {% include soundcloud_player.html track_id="112000000" %} <!-- Placeholder ID, will verify if needed -->
  </div>
</div>
