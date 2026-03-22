# Fix Markdown Rendering and Add Image Zoom

## Objective
Fix the markdown rendering issues where headers display as `#` instead of actual formatting, and add a click-to-zoom feature for all images on the site.

## Proposed Solution
1. **Fix Markdown Rendering:**
   - Remove the surrounding block-level `<div class="...-container">` tags from all `.md` files (`about.md`, `contact.md`, `gallery.md`, `media.md`, `tour.md`, `videos.md`). Jekyll's markdown parser requires empty lines or `markdown="1"` to parse markdown inside HTML block elements. Removing them is the cleanest fix since these container classes are no longer used for specific CSS styling.
   - For `videos.md`, add `markdown="1"` to the `<section>` tags.
2. **Add Image Zoom (Lightbox):**
   - Add a simple, vanilla JavaScript script to `_layouts/default.html` right before the closing `</body>` tag.
   - This script will attach click listeners to all `<img>` tags on the site, opening a full-screen, dark overlay displaying the clicked image zoomed in.
   - The cursor will be set to `zoom-in` for images and `zoom-out` for the overlay.

## Implementation Steps
1. Modify `about.md`, `contact.md`, `gallery.md`, `media.md`, `tour.md` to remove the outer `<div>` tags.
2. Modify `videos.md` to update `<section class="video-feature">` to `<section class="video-feature" markdown="1">`.
3. Modify `_layouts/default.html` to insert the vanilla JS lightbox script.
4. Verify changes by checking the markdown rendering and clicking images to test the zoom functionality.