---
layout: default
title: HOME
permalink: /
---

<div class="hero-section">
  <div class="hero-overlay">
    <h1>Dr Crow and the Forbidden Zone</h1>
    <p class="hero-tagline">. . . . stray from the path</p>
    <a href="{{ '/media.html' | relative_url }}" class="portal-btn hero-cta">LISTEN NOW</a>
  </div>
  <div class="hero-image">
    <img src="{{ 'assets/images/home-hero.jpg' | relative_url }}" alt="Dr Crow and the Forbidden Zone">
  </div>
</div>

<div class="home-intro text-center">
  <p class="lead">Abandon the straight and narrow. Enter the Forbidden Zone.</p>
</div>

<style>
  .hero-section {
    position: relative;
    height: 70vh;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: -60px; /* Offset the main padding */
    border-bottom: 5px solid var(--accent-purple);
  }

  .hero-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
  }

  .hero-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    filter: grayscale(0.5) contrast(1.5) brightness(0.6);
  }

  .hero-overlay {
    position: relative;
    z-index: 2;
    text-align: center;
    background: rgba(0,0,0,0.4);
    padding: 40px;
    border: 1px solid var(--accent-purple);
    backdrop-filter: blur(2px);
  }

  .hero-tagline {
    font-family: 'Playfair Display', serif;
    font-size: 1.5rem;
    font-style: italic;
    margin-bottom: 30px;
  }

  .hero-cta {
    display: inline-block;
    min-width: 200px;
  }

  .home-intro {
    padding: 60px 0;
  }
</style>
