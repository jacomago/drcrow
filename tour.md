---
layout: default
title: GIGS
permalink: /tour.html
---

<div class="tour-container">

# GIGS

Come one come all to witness the spectacle.

<div class="events-list">
  {% assign current_gigs = site.data.gigs | where: "status", "current" %}
  {% for gig in current_gigs %}
    {% include event_item.html 
      date=gig.date 
      venue=gig.venue 
      location=gig.location 
      maps_url=gig.maps_url %}
  {% endfor %}

  <div class="historical-gigs">
    ## LEGACY PORTALS

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
