# Astro Conversion Script

Run the following commands in your terminal to automatically perform the complete migration from Liquid templates to Astro components and layouts. 

First, ensure you have the necessary parser dependencies installed:

```bash
npm install marked @types/marked
```

Then, create and run the following Node script by saving it as \`migrate.js\` and executing \`node migrate.js\`:

```javascript
const fs = require('fs');
const path = require('path');
const yaml = require('js-yaml');

// 1. Convert Data Files to JSON
const dataDir = 'src/data';
const ymlFiles = fs.readdirSync(dataDir).filter(f => f.endsWith('.yml') && f !== 'navigation.yml' && f !== 'socials.yml');
for (const file of ymlFiles) {
  const content = fs.readFileSync(path.join(dataDir, file), 'utf8');
  const data = yaml.load(content);
  fs.writeFileSync(path.join(dataDir, file.replace('.yml', '.json')), JSON.stringify(data, null, 2));
}

// 2. Convert _includes to src/components/*.astro
const componentsDir = 'src/components';
if (!fs.existsSync(componentsDir)) fs.mkdirSync(componentsDir, { recursive: true });

const componentsMap = {
  'bandcamp_player.html': \`---
export interface Props {
  album_id?: string;
  track_id?: string;
  title: string;
  url: string;
}
const { album_id, track_id, title, url } = Astro.props;
---\n<div class="media-wrapper">
{album_id ? (
  <iframe style="border: 0; width: 100%; height: 470px;" src={\\\`https://bandcamp.com/EmbeddedPlayer/album=\${album_id}/size=large/bgcol=121212/linkcol=4b0082/tracklist=false/transparent=true/\\\`} seamless loading="lazy" decoding="async">
    <a href={url}>{title}</a>
  </iframe>
) : track_id ? (
  <iframe style="border: 0; width: 100%; height: 120px;" src={\\\`https://bandcamp.com/EmbeddedPlayer/track=\${track_id}/size=large/bgcol=121212/linkcol=4b0082/tracklist=false/artwork=small/transparent=true/\\\`} seamless loading="lazy" decoding="async">
    <a href={url}>{title}</a>
  </iframe>
) : null}
</div>\`,
  'event_item.html': \`---
export interface Props {
  date: string;
  venue: string;
  location: string;
  maps_url?: string;
}
const { date, venue, location, maps_url } = Astro.props;
---\n<div class="event-item">
  <div class="event-date">{date}</div>
  <div class="event-details">
    <h3 class="event-venue">{venue}</h3>
    <p class="event-location">{location}</p>
    {maps_url && <a href={maps_url} class="portal-link" target="_blank">LOCATE VIA PORTAL (MAPS)</a>}
  </div>
</div>\`,
  'exposition_renderer.html': \`---
import exposition from '../data/exposition.json';
import { marked } from 'marked';
---\n{exposition.map((item) => (
  <>
    {item.type === "hero" && (
      <div class="exposition-hero-img">
        <img src={item.image.replace(/^\\//, '')} alt={item.alt} loading="lazy" decoding="async" style="width: 100%; height: 60vh; object-fit: cover; filter: brightness(0.6);" />
      </div>
    )}
    {item.type === "row" && (
      <div class="container">
        <div class={\\\`editorial-row \${item.reverse ? 'reverse' : ''}\\\`}>
          <div class="editorial-text" set:html={marked.parse(item.text || '')} />
          <div class="editorial-image">
            <img src={item.image.replace(/^\\//, '')} alt={item.alt} loading="lazy" decoding="async" />
          </div>
        </div>
      </div>
    )}
    {item.type === "quote" && (
      <div class="container">
        <blockquote class="pull-quote">
          "{item.text}"
        </blockquote>
      </div>
    )}
    {item.type === "cta" && (
      <div class="container">
        <p class="cta text-center" style="font-size: 2rem;">
          <span set:html={marked.parseInline(item.text || '')} /><br />
          <span style="color: var(--accent-gold);">{item.subtext}</span>
        </p>
      </div>
    )}
  </>
))}\`,
  'hero.html': \`---
export interface Props {
  title?: string;
  tagline?: string;
  image?: string;
  cta_link?: string;
  cta_text?: string;
}
const { title, tagline = ". . . . stray from the path", image = "assets/images/heroes/home-hero.jpg", cta_link, cta_text = "LISTEN NOW" } = Astro.props;
---\n<div class="hero-section">
  <div class="hero-overlay">
    <p class="hero-tagline">{tagline}</p>
    <img src="illustrations/header.png" alt={title} class="headline-logo" loading="eager" decoding="async" />
    {cta_link && (
      <>
        <br />
        <a href={cta_link.replace(/^\\//, '')} class="hero-cta">{cta_text}</a>
      </>
    )}
  </div>
  <div class="hero-image">
    <img src={image.replace(/^\\//, '')} alt={title} loading="eager" decoding="async" />
  </div>
</div>\`,
  'page_hero.html': \`---
export interface Props {
  title?: string;
  tagline?: string;
  image?: string;
}
const { title, tagline, image } = Astro.props;
---\n<div class="page-hero">
  <div class="hero-overlay">
    <h1>{title}</h1>
    {tagline && <p class="hero-tagline">{tagline}</p>}
  </div>
  {image && (
    <div class="hero-image">
      <img src={image.replace(/^\\//, '')} alt={title} loading="lazy" decoding="async" />
    </div>
  )}
</div>\`,
  'portal_btn.html': \`---
export interface Props {
  url: string;
  name: string;
  class?: string;
}
const { url, name, class: className = '' } = Astro.props;
---\n<a href={url} class={\\\`portal-btn \${className}\\\`} target="_blank">
  <span class="portal-text">{name}</span>
</a>\`,
  'soundcloud_player.html': \`---
export interface Props {
  track_id: string;
}
const { track_id } = Astro.props;
---\n<div class="media-wrapper">
  <iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src={\\\`https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/\${track_id}&color=%234b0082&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true\\\`} loading="lazy" decoding="async"></iframe>
</div>\`,
  'video_embed.html': \`---
export interface Props {
  youtube_id?: string;
  facebook_url?: string;
  wix_video_url?: string;
}
const { youtube_id, facebook_url, wix_video_url } = Astro.props;
---\n{youtube_id ? (
<div class="video-responsive">
  <iframe src={\\\`https://www.youtube.com/embed/\${youtube_id}\\\`} title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen loading="lazy"></iframe>
</div>
) : facebook_url ? (
<div class="video-responsive">
  <iframe src={\\\`https://www.facebook.com/plugins/video.php?href=\${encodeURIComponent(facebook_url)}&show_text=0&width=560\\\`} style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowTransparency="true" allowFullScreen="true" loading="lazy"></iframe>
</div>
) : wix_video_url ? (
<div class="video-responsive">
  <video autoplay loop muted playsinline>
    <source src={wix_video_url} type="video/mp4" />
    Your browser does not support the video tag.
  </video>
</div>
) : null}\`
};

for (const [file, code] of Object.entries(componentsMap)) {
  const name = file.replace('.html', '').split('_').map(w => w.charAt(0).toUpperCase() + w.slice(1)).join('') + '.astro';
  fs.writeFileSync(path.join(componentsDir, name), code);
}

// 3. Convert _layouts to src/layouts/*.astro
const layoutsDir = 'src/layouts';
if (!fs.existsSync(layoutsDir)) fs.mkdirSync(layoutsDir, { recursive: true });

const layoutsMap = {
  'about.html': \`---
import Layout from './Layout.astro';
import PageHero from '../components/PageHero.astro';
import VideoEmbed from '../components/VideoEmbed.astro';
import ExpositionRenderer from '../components/ExpositionRenderer.astro';
const { frontmatter } = Astro.props;
---\n<Layout title={frontmatter.title}>
  <PageHero title={frontmatter.heading} tagline={frontmatter.lead} image={frontmatter.image} />
  <section class="about-intro">
    <div class="container">
      <slot />
    </div>
  </section>
  <section class="about-acid bg-surface-low">
    <div class="container">
      <h2>ACID</h2>
      <VideoEmbed youtube_id={frontmatter.acid_youtube_id} />
    </div>
  </section>
  <section class="about-exposition">
    <div class="container">
      <h2>EXPOSITION</h2>
    </div>
    <ExpositionRenderer />
  </section>
</Layout>\`,
  'contact.html': \`---
import Layout from './Layout.astro';
import PageHero from '../components/PageHero.astro';
import PortalBtn from '../components/PortalBtn.astro';
import { socials } from '../data/socials';
const { frontmatter } = Astro.props;
---\n<Layout title={frontmatter.title}>
  <PageHero title={frontmatter.title} tagline={frontmatter.lead} image={frontmatter.image} />
  <section class="contact-portals">
    <div class="container">
      <div class="portals-grid">
        {socials.map(social => (
          <PortalBtn url={social.url} name={social.name} class={social.class} />
        ))}
      </div>
    </div>
  </section>
  <section class="contact-direct bg-surface-low">
    <div class="container">
      <slot />
    </div>
  </section>
</Layout>\`,
  'default.html': \`---
import Layout from './Layout.astro';
const { frontmatter } = Astro.props;
---\n<Layout title={frontmatter?.title}>
  <slot />
</Layout>\`,
  'gallery.html': \`---
import Layout from './Layout.astro';
import PageHero from '../components/PageHero.astro';
import galleryHighlights from '../data/gallery_highlights.json';
import galleryArchive from '../data/gallery_archive.json';
const { frontmatter } = Astro.props;
---\n<Layout title={frontmatter.title}>
  <PageHero title={frontmatter.title} tagline={frontmatter.lead} image={frontmatter.image} />
  <section class="gallery-content bg-surface-low">
    <div class="container">
      <slot />
      <h2 style="font-family: 'Space Grotesk', sans-serif; font-size: 1.5rem; letter-spacing: 0.3em; margin-bottom: 2rem; color: var(--accent-gold);">MANIFESTATIONS</h2>
      <div class="gallery-grid">
        {galleryHighlights.map(img => (
          <div class="gallery-item">
            <img src={\\\`assets/images/highlights/\${img}\\\`} alt="Forbidden Zone Manifestation" loading="lazy" decoding="async" />
          </div>
        ))}
      </div>
      <h2 style="font-family: 'Space Grotesk', sans-serif; font-size: 1.5rem; letter-spacing: 0.3em; margin-top: 6rem; margin-bottom: 2rem; color: var(--accent-gold);">THE ARCHIVE</h2>
      <div class="gallery-grid" style="grid-template-columns: repeat(8, 1fr);">
        {galleryArchive.map(img => (
          <div class="gallery-item" style="grid-column: span 2; box-shadow: 0 10px 20px rgba(0,0,0,0.5);">
            <img src={\\\`assets/images/archive/\${img}\\\`} alt="Forbidden Zone Archive" loading="lazy" decoding="async" style="height: 250px;" />
          </div>
        ))}
      </div>
    </div>
  </section>
</Layout>\`,
  'media.html': \`---
import Layout from './Layout.astro';
import PageHero from '../components/PageHero.astro';
import BandcampPlayer from '../components/BandcampPlayer.astro';
import PortalBtn from '../components/PortalBtn.astro';
const { frontmatter } = Astro.props;
---\n<Layout title={frontmatter.title}>
  <PageHero title={frontmatter.title} tagline={frontmatter.lead} image={frontmatter.image} />
  <section class="music-feature">
    <div class="container">
      <h2>The new album: STROHAYR</h2>
      <p>The album is presented in its entirety. A story in 10 parts (just like life really).</p>
      <BandcampPlayer album_id="66352487" title="Strohayr by Dr Crow and The Forbidden Zone" url="https://drcrow.bandcamp.com/album/strohayr" />
    </div>
  </section>
  <section class="music-insight bg-surface-low">
    <div class="container">
      <h2>Crow Hall Insight</h2>
      <p>Get a special insight into the band's home environment......Crow Hall.</p>
      <div style="text-align: center; margin-bottom: 2rem;">
        <img src="assets/images/archive/crow-hall.jpg" alt="Crow Hall" style="border: 1px solid var(--border-ghost); max-width: 100%;" />
      </div>
      <BandcampPlayer track_id="3830604073" title="Radio Interview by Dr Crow and The Forbidden Zone" url="https://drcrow.bandcamp.com/track/radio-interview" />
    </div>
  </section>
  <section class="music-bones">
    <div class="container">
      <h2>A Fistful of Broken Bones</h2>
      <BandcampPlayer album_id="1201722343" title="Fistful Of Broken Bones by Dr Crow and The Forbidden Zone" url="https://drcrow.bandcamp.com/album/fistful-of-broken-bones" />
    </div>
  </section>
  <section class="music-archive bg-surface-low">
    <div class="container">
      <h2>THE SOUNDCLOUD ARCHIVE</h2>
      <p>Further frequencies from the zone.</p>
      <div class="portals-grid" style="max-width: 400px; margin: 40px auto;">
        <PortalBtn url="https://soundcloud.com/user-20555187" name="VISIT SOUNDCLOUD" class="soundcloud" />
      </div>
    </div>
  </section>
</Layout>\`,
  'tour.html': \`---
import Layout from './Layout.astro';
import PageHero from '../components/PageHero.astro';
import EventItem from '../components/EventItem.astro';
import gigs from '../data/gigs.json';
const { frontmatter } = Astro.props;

const currentGigs = gigs.filter(gig => gig.status === 'current');
const legacyGigs = gigs.filter(gig => gig.status === 'legacy');
---\n<Layout title={frontmatter.title}>
  <PageHero title={frontmatter.title} tagline={frontmatter.lead} image={frontmatter.image} />
  <section class="tour-content bg-surface-low">
    <div class="container">
      <slot />
      <div class="events-list">
        {currentGigs.map(gig => (
          <EventItem date={gig.date} venue={gig.venue} location={gig.location} maps_url={gig.maps_url} />
        ))}
        <div class="historical-gigs" style="margin-top: 80px;">
          <h2 style="font-family: 'Space Grotesk', sans-serif; font-size: 1.2rem; letter-spacing: 0.3em; opacity: 0.6;">LEGACY PORTALS</h2>
          {legacyGigs.map(gig => (
            <EventItem date={gig.date} venue={gig.venue} location={gig.location} maps_url={gig.maps_url} />
          ))}
        </div>
      </div>
    </div>
  </section>
</Layout>\`,
  'videos.html': \`---
import Layout from './Layout.astro';
import PageHero from '../components/PageHero.astro';
import VideoEmbed from '../components/VideoEmbed.astro';
import videos from '../data/videos.json';
const { frontmatter } = Astro.props;
---\n<Layout title={frontmatter.title}>
  <PageHero title={frontmatter.title} tagline={frontmatter.lead} image={frontmatter.image} />
  <section class="videos-content bg-surface-low">
    <div class="container">
      <slot />
      {videos.map(video => (
        <section class="video-feature">
          <h2>{video.title}</h2>
          <p>{video.description}</p>
          {(video.youtube_id || video.facebook_url || video.wix_video_url) && (
            <VideoEmbed youtube_id={video.youtube_id} facebook_url={video.facebook_url} wix_video_url={video.wix_video_url} />
          )}
        </section>
      ))}
    </div>
  </section>
</Layout>\`
};

for (const [file, code] of Object.entries(layoutsMap)) {
  const name = file.replace('.html', '').split('_').map(w => w.charAt(0).toUpperCase() + w.slice(1)).join('') + '.astro';
  fs.writeFileSync(path.join(layoutsDir, name), code);
}

// 4. Update src/pages/*.md Frontmatter Layouts
const pagesDir = 'src/pages';
const mdFiles = fs.readdirSync(pagesDir).filter(f => f.endsWith('.md'));
for (const file of mdFiles) {
  let content = fs.readFileSync(path.join(pagesDir, file), 'utf8');
  content = content.replace(/^layout:\\s*([a-zA-Z0-9_-]+)/m, (match, layoutName) => {
    const capitalized = layoutName.split('_').map(w => w.charAt(0).toUpperCase() + w.slice(1)).join('');
    return \\\`layout: ../layouts/\${capitalized}.astro\\\`;
  });
  fs.writeFileSync(path.join(pagesDir, file), content);
}

console.log('Conversion successful! You can now run npm run build');
```

Once executed, run \`npm run build\` to verify everything works flawlessly.
