# Eleventy & TypeScript (TSX) Conversion Plan

## Objective
Convert the existing Jekyll-based static site to **Eleventy v3**, utilizing **TypeScript** for configuration and data, **TSX** (React JSX via `react-dom/server`) for layouts and includes, and setting up a modern **CSS build pipeline**.

## Scope & Impact
- **Remove:** Ruby/Jekyll dependencies (`Gemfile`, `_config.yml`).
- **Add:** Node.js toolchain (`package.json`, `@11ty/eleventy`, `tsx`, `react`, CSS processors).
- **Convert:** `_layouts` and `_includes` from Liquid (`.html`) to TSX (`.11ty.tsx`).
- **CSS:** Implement a PostCSS-based pipeline (or similar) to process `assets/css/style.css`.
- **Preserve:** Markdown content, Netlify CMS / Decap CMS configuration, image assets, client-side scripts (Netlify Identity, image zoom).

## Trade-offs & Alternatives Considered

### 1. Template Engine: TSX (via React `renderToStaticMarkup`)
**Arguments For:**
- **Familiarity & Ecosystem:** React's JSX is an industry standard. You get excellent IDE support, component-driven architecture, and strong typing.
- **Type Safety:** Catch errors at build time if you pass the wrong data to a layout or component.
- **Maintainability:** Easier to break down complex layouts into smaller, strictly-typed TSX functions.

**Arguments Against (and Alternatives):**
- **Overhead:** Installing `react` and `react-dom` just to render static HTML adds dependencies and slightly increases build times.
- *Alternative:* **`vhtml`** instead of React. It parses JSX to strings natively without the React library. *Pros:* Much faster, lightweight. *Cons:* Not a full virtual DOM, lacks React-specific features (though rarely needed for static SSG).
- *Alternative:* **Nunjucks / Liquid**. *Pros:* Built-in to Eleventy, incredibly fast, easiest migration from Jekyll. *Cons:* No native TypeScript support in the templates.

### 2. Configuration: TypeScript (`eleventy.config.ts` via `tsx`)
**Arguments For:**
- **IntelliSense:** Excellent autocomplete when configuring Eleventy's APIs.
- **Data Integrity:** Allows writing `_data` files in TypeScript, enabling you to fetch external APIs with typed responses.

**Arguments Against (and Alternatives):**
- **Build Step:** Requires executing Eleventy through a transpiler (like `tsx`), which adds a small startup delay compared to pure CommonJS/ESM.
- *Alternative:* **Vanilla JavaScript (`eleventy.config.js`)**. *Pros:* Native, fastest execution. *Cons:* No type safety.

### 3. CSS Pipeline: PostCSS
**Arguments For:**
- **Modern Standards:** Allows usage of `autoprefixer` for cross-browser support, and `cssnano` for minification.
- **Extensibility:** Easy to drop in TailwindCSS or nesting plugins later.

**Arguments Against (and Alternatives):**
- **Complexity:** Requires managing separate `npm run build` scripts alongside Eleventy.
- *Alternative:* **Passthrough Copy**. Simply copy the existing `assets/css/style.css` without processing. *Pros:* Zero configuration, instant build. *Cons:* No minification or modern CSS features (like nesting/prefixing).

## Key Files & Context
- **Global Config:** `eleventy.config.ts` will replace `_config.yml`.
- **Data:** `_data/` files (e.g., `navigation.yml`, `socials.yml`) will either remain YAML or be converted to `.ts` files depending on complexity.
- **Templates:** `_layouts/default.html` and others will become TSX components. Liquid tags like `{% for %}` will become JavaScript `.map()` calls.
- **CMS:** `admin/config.yml` will need build command updates (from Jekyll to npm scripts).

## Proposed Solution & Implementation Steps

### Phase 1: Toolchain Setup
1. Initialize `package.json` (`npm init -y`).
2. Install dependencies:
   - **Core:** `npm install @11ty/eleventy@^3.0.0 tsx`
   - **TSX Engine:** `npm install react react-dom @types/react @types/react-dom`
   - **CSS Pipeline:** `npm install postcss postcss-cli cssnano autoprefixer`
3. Set up build scripts in `package.json`:
   - `"start": "eleventy --serve"`
   - `"build": "postcss assets/css/style.css -o _site/assets/css/style.css && eleventy"`

### Phase 2: Eleventy Configuration (TypeScript)
1. Create `eleventy.config.ts`.
2. Configure Eleventy to support TSX files using the `tsx` package and `renderToStaticMarkup` from React.
3. Configure passthrough copies for `admin/`, `assets/images/`, and `illustrations/`.
4. Migrate site-wide variables (like `site.title`) to `_data/site.ts` or `_data/site.json`.

### Phase 3: Converting Layouts and Includes to TSX
1. Move `_layouts` to `_includes/layouts` (Eleventy convention).
2. Rewrite `_layouts/default.html` into `_includes/layouts/default.11ty.tsx`:
   - Replace Liquid interpolations (`{{ page.title }}`) with JSX expressions (`{data.title}`).
   - Replace loop constructs with mapping over `data.navigation` and `data.socials`.
   - Pass `data.content` (using `dangerouslySetInnerHTML`) to inject the Markdown body.
3. Convert other layouts (`about`, `contact`, `gallery`, `media`, `tour`, `videos`) to TSX components that inherit or wrap the `default` layout.
4. Convert `_includes/*.html` snippets (e.g., `hero.html`, `video_embed.html`) to reusable TSX exported functions.

### Phase 4: CSS Pipeline
1. Create `postcss.config.js` with plugins like `autoprefixer` and `cssnano` (for minification in production).
2. Update the CSS link in the `<head>` of `default.11ty.tsx` to point to the processed output file if the path changes.

### Phase 5: Markdown & CMS Updates
1. Review Markdown files (`about.md`, `index.md`, etc.) to ensure frontmatter layout references map correctly to the new TSX layouts.
2. Update `admin/config.yml`:
   - Change `build_command` to `npm run build` or similar.

## Verification & Testing
1. Run `npm start` and verify that Eleventy builds the site without errors.
2. Navigate through the local site to ensure all pages render with correct TSX output.
3. Verify that the CSS is built and applied correctly.
4. Verify client-side JavaScript (Netlify Identity login routing and the custom zoom-in image script) functions as expected.
5. Check that Netlify CMS (`/admin/`) is still accessible and configured correctly.

## Migration & Rollback
- Since this is a destructive change regarding the build system, it will be done in a new Git branch.
- Rollback will involve simply checking out the previous Jekyll branch.