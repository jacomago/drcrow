---
layout: default
title: GIGS
permalink: /tour.html
---

<section class="tour-header">
  <div class="container">
    <h1>GIGS & TOUR DATES</h1>
    <p class="lead">Come one come all to witness the spectacle.</p>
  </div>
</section>

<section class="tour-content bg-surface-low">
  <div class="container">
    <div class="events-list">
      {% assign current_gigs = site.data.gigs | where: "status", "current" %}
      {% for gig in current_gigs %}
        {% include event_item.html 
          date=gig.date 
          venue=gig.venue 
          location=gig.location 
          maps_url=gig.maps_url %}
      {% endfor %}

      <div class="historical-gigs" style="margin-top: 80px;">
        <h2 style="font-family: 'Space Grotesk', sans-serif; font-size: 1.2rem; letter-spacing: 0.3em; opacity: 0.6;">LEGACY PORTALS</h2>

        {% assign legacy_gigs = site.data.gigs | where: "status", "legacy" %}
        {% for gig in legacy_gigs %}
          {% include event_item.html 
            date=gig.date 
            venue=gig.venue 
            location=gig.location 
            maps_url=gig.maps_url %}
        {% endfor %}
      </div>
    </div>
  </div>
</section>
