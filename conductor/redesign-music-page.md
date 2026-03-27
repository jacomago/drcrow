# Music Page Redesign Plan (Noir Editorial)

## Objective
Redesign the Music/Media page to align with "The Noir Stage" design system gathered from Stitch. The new design will prioritize cinematic atmosphere, asymmetrical layouts, and high-fidelity typography while providing a seamless Bandcamp listening experience.

## Key Files & Context
- `src/layouts/Media.astro`: Main layout for the music page.
- `src/components/BandcampPlayer.astro`: Component for embedding Bandcamp iframes.
- `src/styles/style.css`: Global styles (to be refined with Noir tokens).
- `src/pages/media.md`: Markdown content for the music page.

## Proposed Changes

### 1. Typography & CSS Refinements
- Enforce the **"No-Line" Rule**: Remove any remaining 1px borders in the music section and use tonal shifts (`surface` vs `surface_container_low`) to define boundaries.
- Refine headline spacing (-0.02em for `Noto Serif`) and metadata typography (`Space Grotesk`).

### 2. Redesign `Media.astro` (Layout)
- **Featured Release Section:** Create a high-impact section for the latest album ("STROHAYR") using an asymmetrical layout. The album art/player should overlap the background or text blocks.
- **Tonal Layering:** Alternate background colors between sections using the Noir palette to create depth without lines.
- **Discography List:** Re-implement the discography as a curated editorial list rather than a simple grid. Each entry will have its own atmospheric description and player.
- **Soundcloud Archive:** Move the Soundcloud portal to a "Noir Well" (deep black background `surface_container_lowest`) to signify its archival nature.

### 3. Enhance `BandcampPlayer.astro` (Comprehensive Options)
- Update the component to support all standard Bandcamp embed parameters:
    - `size` ('large', 'small')
    - `minimal` (boolean for square artwork-only)
    - `tracklist` (boolean)
    - `artwork` ('none', 'small', 'big')
    - `bgcol` and `linkcol` hex overrides
    - `transparent` (boolean)
- Automatically calculate the correct iframe height based on the selected mode.
- Maintain the "Noir Frame" styling for larger players while allowing slim layouts for interview/track snippets.

### 4. Implementation Steps
1.  **Draft Component:** Refine `BandcampPlayer.astro` with better styling and prop handling.
2.  **Draft Layout:** Rewrite `Media.astro` with the new asymmetrical grid structures.
3.  **Update Page:** Adjust `media.md` frontmatter or content if necessary to support new layout features.
4.  **Verify:** Run `npm run build` and check for visual consistency.

## Verification & Testing
- Visual inspection of the built `/media` page.
- Ensure all Bandcamp players load and function correctly.
- Verify responsiveness (asymmetrical layouts must collapse gracefully on mobile).
