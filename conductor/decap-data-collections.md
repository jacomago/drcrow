# Decap CMS Data Collections Plan

## Objective
Make the data files (e.g., gigs, videos, exposition, navigation, socials, gallery items) editable via Decap CMS by configuring new data collections.

## Scope & Impact
- Convert `navigation.ts` and `socials.ts` to `.json` files to ensure seamless compatibility with Decap CMS.
- Update Astro component imports to use `.json` instead of `.ts` for navigation and socials.
- Add file collections to `public/admin/config.yml` for each JSON data file.

## Key Files & Context
- `src/data/navigation.ts` -> `src/data/navigation.json`
- `src/data/socials.ts` -> `src/data/socials.json`
- `src/layouts/Layout.astro` (update imports)
- `src/layouts/Contact.astro` (update imports)
- `public/admin/config.yml` (add data collections)

## Implementation Steps

### Phase 1: Convert TS Data Files to JSON
1. Convert `src/data/navigation.ts` to `src/data/navigation.json`.
2. Convert `src/data/socials.ts` to `src/data/socials.json`.
3. Delete the original `.ts` files.
4. Update `src/layouts/Layout.astro` and `src/layouts/Contact.astro` to import the `.json` files instead. Astro handles JSON imports out-of-the-box.

### Phase 2: Update Decap CMS Configuration
1. Edit `public/admin/config.yml` to append a new collection for data files (or add individual collections for each file using a "files" collection).
2. Configure fields for `navigation` (list of objects with name and link).
3. Configure fields for `socials` (list of objects with name, url, and class).
4. Configure fields for `gigs` (list of objects with status, date, venue, location, maps_url).
5. Configure fields for `videos` (list of objects with title, description, youtube_id, facebook_url, wix_video_url).
6. Configure fields for `exposition` (list of objects with type, text, subtext, image, alt, reverse).
7. Configure fields for `gallery_highlights` and `gallery_archive` (lists of strings/images).

### Phase 3: Testing
1. Run `npm run build` to verify the site builds successfully after JSON conversions.

## Rollback
- Revert the changes to `public/admin/config.yml` and `src/data/`.