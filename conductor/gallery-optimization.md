# Gallery Optimization Plan

## Objective
Make the gallery load significantly faster by implementing **Astro's native image optimization** (`astro:assets`). This will automatically convert images to modern formats (WebP/AVIF) and resize them to fit the layout, reducing the total payload size by up to 80-90%.

## Scope & Impact
- **Image Migration:** Move gallery and site images from `public/assets/images/` to `src/assets/images/`.
- **Component Update:** Update `Gallery.astro` to use the `<Image />` component from `astro:assets`.
- **CMS Update:** Update `public/admin/config.yml` so Decap CMS continues to manage images in their new location.
- **Dynamic Loading:** Use `import.meta.glob` to resolve dynamic image paths from JSON data files at build time.

## Proposed Changes

### 1. File Reorganization
Move images to `src/` so Astro's build engine can access and optimize them:
- `public/assets/images/highlights/` -> `src/assets/images/highlights/`
- `public/assets/images/archive/` -> `src/assets/images/archive/`

### 2. Update `Gallery.astro`
- Import the `Image` component.
- Use `import.meta.glob('/src/assets/images/**/*.{jpg,jpeg,png}')` to get a map of all images.
- Replace the `<img>` tags with `<Image />`.
- Specify optimized widths (e.g., `width={800}` for highlights, `width={400}` for archive).

### 3. Adjust Decap CMS Configuration
- Update `media_folder` to `src/assets/images`.
- Update `public_folder` to `/src/assets/images` (or keep it relative depending on how the CMS interacts with the repo). This ensures new images uploaded via the CMS are also optimized by the Astro build pipeline.

### 4. Fix other components
- Update `Hero.astro` and `PageHero.astro` to also use `astro:assets` where possible for background images.

## Verification & Testing
- Run `npm run build` and compare the size of the `dist/assets` folder before and after.
- Inspect the network tab in the browser to verify WebP/AVIF delivery.
- Ensure the CMS still correctly uploads and displays images.
