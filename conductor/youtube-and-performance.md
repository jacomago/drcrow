# Site Cleanup, Media Integration, and Optimization Plan

## Objective
Enhance the site's performance and aesthetics by fetching the latest YouTube videos, fixing embedded player formatting, renaming cryptic images, incorporating legacy illustrations, and optimizing image rendering.

## Proposed Solution
1. **YouTube Integration:** 
   - Scrape the latest videos from `https://www.youtube.com/@drcrowandtheforbiddenzone9192`.
   - Update `_data/videos.yml` to contain these YouTube video titles and embed links.
   - Modify `_includes/video_embed.html` to support responsive YouTube `<iframe>` embeds.

2. **Audio Player Formatting:** 
   - Wrap the iframes in `_includes/bandcamp_player.html` and `_includes/soundcloud_player.html` in constrained container divs (e.g., `max-width: 700px; margin: 0 auto;`). This prevents the players from stretching too wide on desktop monitors and aligns better with the Noir Stage aesthetic.

3. **Image Renaming & Gallery Cleanup:**
   - Rename all cryptic image files (e.g., `97e2c6_...~mv2...jpg`) in `assets/images/` to human-readable names (e.g., `gallery-1.jpg`, `gallery-2.jpg`).
   - Rebuild `_data/gallery.yml` with the new filenames.
   - Remove the first entry (`band-bw-1.jpg`) as it is an empty/black image.

4. **Incorporate Legacy Illustrations:**
   - Add one of the `gate-sized...jpg` illustrations from the `illustrations/` folder into `_includes/exposition.md`.
   - Replace the main `<h1>` textual headline in `_includes/hero.html` with the `illustrations/header.png` image for a stronger visual identity.

5. **Image Performance (Jekyll Advantage):** 
   - Leverage modern HTML attributes within Jekyll's layouts. Add `loading="lazy"` and `decoding="async"` to the `<img>` tags in `_layouts/gallery.html`, `_includes/hero.html`, and `_includes/exposition.md`. The browser will natively defer loading off-screen images, drastically improving initial page load times for the heavy gallery without needing complex plugins.

## Implementation Steps
1. Execute a shell script to batch rename all `*~mv2*.jpg` files in `assets/images/` to `gallery-*.jpg`.
2. Generate a new list of gallery images, exclude `band-bw-1.jpg`, and save to `_data/gallery.yml`.
3. Update `_includes/hero.html` to use `<img src="{{ 'illustrations/header.png' | relative_url }}" alt="{{ include.title }}" class="headline-logo">` instead of the `<h1>` text.
4. Update `_includes/exposition.md` to include `illustrations/gate-sized49b9.jpg` (or similar).
5. Add `loading="lazy" decoding="async"` to image tags in `_layouts/gallery.html`, `_includes/hero.html`, `_includes/exposition.md`, and `_layouts/default.html` (the lightbox script).
6. Fetch video data from YouTube and rewrite `_data/videos.yml`.
7. Update `_includes/video_embed.html` to render responsive YouTube iframes.
8. Update `_includes/bandcamp_player.html` and `_includes/soundcloud_player.html` with responsive wrapper classes, and add corresponding CSS to `assets/css/style.css`.
9. Verify all changes locally.