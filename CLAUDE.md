# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page marketing site for 看海過日子 (Enjoy Ocean), a 海龜燒 (turtle-shaped pastry) shop on 小琉球 (Liuqiu Island), Taiwan. It is served by **GitHub Pages** at the custom domain in `CNAME` (`enjoyocean.tw`). Pushing to `main` deploys it. There is no build step, package manager, linter or test suite.

All page content is in Traditional Chinese (`zh-Hant`). Keep new copy in Traditional Chinese, and write code comments in Chinese to match the existing ones.

## Local preview

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

Opening `index.html` directly also works. The only external dependencies are Google Fonts and the Google Maps embed iframe.

## Structure of `index.html`

Everything (HTML, CSS in one `<style>` block, a small inline `<script>`) lives in `index.html`. Images sit in the repo root next to it.

- **Theme tokens**: all colors, radius, shadow and font stacks are CSS variables on `:root` at the top of the `<style>` block. Change colors there instead of in individual rules.
- **Icons**: an inline SVG sprite at the top of `<body>` defines `#i-phone`, `#i-map`, `#i-ig`, and buttons reference them with `<svg><use href="#i-..."/></svg>`.
- **Sections** (in order): sticky header/nav → hero (`#top`) → features strip → limited flavors (`#limited`) → best sellers (`#flavors`) → menu image (`#menu`) → IG call-to-action → visit info and map (`#visit`) → footer → mobile `.dock` (a fixed bottom button bar, shown only at ≤860px).
- **Adding a limited-edition flavor**: duplicate a whole `<article class="lim-card">…</article>` block inside `.limited` (the HTML comment above the section says so).
- **Scroll animation**: any element with class `.reveal` fades in via IntersectionObserver. It respects `prefers-reduced-motion`.
- **Breakpoints**: 860px (nav hidden, hero stacks, dock appears) and 760px (grids drop to one column).

## Business info appears in several places

The phone number, address, Instagram URL and Google Maps link are repeated throughout the page: header, hero CTAs, limited cards, menu note, visit section, dock, `<meta name="description">`, and the JSON-LD `Bakery` schema in `<head>`. When any of these change, update every occurrence. Search for `0980008291` / `980-008-291`, `enjoy_ocean_`, `maps.app.goo.gl` and `民生路27-1號`. The OG/JSON-LD image and logo URLs are absolute (`https://enjoyocean.tw/...`), so renaming those image files also means updating the meta tags.

## Asset gotchas

- GitHub Pages is case-sensitive, so image `src` values must match filenames exactly (e.g. `logo-img.PNG`, not `logo-img.png`).
- Git tracks both `menu.jpg` and `menu.JPG` (identical content). They collide on the default case-insensitive macOS filesystem. The page uses `menu.jpg`.
- Some images use Chinese filenames (`卡士達1.png`, `抹茶.png`) and are referenced unescaped in `src`.
- Several images are multi-MB originals (e.g. `logo-img.PNG` is about 8.5 MB and is also the favicon). `DSC05125.jpg` and `IMG_8881 2.JPG` are not referenced by the page.
