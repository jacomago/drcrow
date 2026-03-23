# Astro & Decap CMS Migration Plan

## Objective
Convert the existing Jekyll-based static site to **Astro**, establishing a modern, TypeScript-first architecture while keeping the site fully static and guaranteeing seamless integration with **Decap CMS** (formerly Netlify CMS).

## Scope & Impact
- **Remove:** Ruby/Jekyll dependencies (`Gemfile`, `_config.yml`).
- **Add:** Node.js toolchain with Astro (`package.json`, `astro`, `@astrojs/react`).
- **Convert:** `_layouts` and `_includes` from Liquid (`.html`) to Astro components (`.astro`) and React components (`.tsx`).
- **Preserve:** All Markdown content, Decap CMS configuration (`admin/`), and image assets.

## Proposed Solution & Implementation Steps

### Phase 1: Toolchain Setup
1. Initialize the Astro project in the current directory: `npm create astro@latest -- --template minimal`
2. Add React support for components requiring it: `npx astro add react`
3. Configure `astro.config.mjs` to set up the build directory and integrate React.

### Phase 2: Decap CMS & Static Assets
1. Move the `admin/` folder (containing Decap CMS `index.html` and `config.yml`) into Astro's `public/` directory. Astro automatically serves everything in `public/` at the root URL.
2. Update `public/admin/config.yml`: change the `build_command` to `npm run build` (instead of `jekyll build`).
3. Move `assets/images` and `illustrations` into `public/`.
4. Ensure the Netlify Identity widget script (`https://identity.netlify.com/v1/netlify-identity-widget.js`) is included in the global `<head>` so CMS login routing still works.

### Phase 3: Content & Data Migration
1. Move all Markdown files (`about.md`, `index.md`, etc.) into `src/pages/` or `src/content/` (using Content Collections for better type safety).
2. Move `_data/*.yml` into `src/data/` or import them directly where needed.

### Phase 4: Layouts & CSS
1. Convert `_layouts/default.html` to `src/layouts/Layout.astro`.
2. Move `assets/css/style.css` to `src/styles/style.css` and import it directly into `Layout.astro`.
3. Re-implement your `{% include %}` snippets as reusable components (e.g., `src/components/Hero.astro`, `src/components/VideoEmbed.tsx`).
4. Port the custom image zooming JavaScript into a `<script>` tag within `Layout.astro`.

## Verification & Testing
1. Run `npm run dev` to start the Astro dev server.
2. Test `/admin/` to ensure Decap CMS loads and Netlify Identity login functions correctly.
3. Verify that all Markdown pages correctly render through the new Astro layouts.
4. Verify the custom JavaScript (image zooming script) still functions on the front end.

## Rollback
- Since this involves changing the build system, all work will be performed in a separate Git branch. Rollback involves simply checking out the previous branch.