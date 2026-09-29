# prfo-site

Source of [prfo.design](https://prfo.design), the personal site of Lev Skorokhodov: AI visual work in a masonry gallery with a lightbox.

History: this is a public snapshot of a private repository (29 commits, 24 Apr to 29 Sep 2026).

It is a one-page static site built with Astro 4 and about 120 lines of vanilla TypeScript. It is for anyone who wants to see the visual work, or read how it is put together without a JS framework.

The engineering portfolio lives at [lab.prfo.design/portfolio](https://lab.prfo.design/portfolio/).

![Hero, gallery series, and lightbox at 1440 px](docs/screenshot.webp)

## Key technical decisions

- Static output only. `npm run build` renders 1 page in under 1 second. The built HTML and CSS are 2.3 KB and 2.1 KB gzipped. The page ships 2.8 KB of inline JS and no framework runtime.
- No JS framework. The custom cursor, scroll reveal (IntersectionObserver), and lightbox are hand-written in `src/layouts/Base.astro`.
- Lightbox: keyboard (Esc, arrows), wheel paging with a 650 ms throttle, click to zoom 2.5x with drag to pan on desktop, swipe paging (48 px threshold) on touch screens, pinch zoom blocked inside the overlay only.
- Masonry with CSS `columns` (2 columns, 1 at 640 px and narrower) instead of a layout script. No JS computes the layout.
- Images: 9 WebP files, 0.8 MB total, max width 1600 px, each under 200 KB. Gallery images load with `loading="lazy"`; the hero reuses one of them with `fetchpriority="high"`.
- Every image is an AI render made by Lev. Some frames restage existing retail products as spec work and are not affiliated with or endorsed by their makers. Each file carries XMP metadata with the IPTC digital source type `trainedAlgorithmicMedia` and the creator name.
- Typography: EB Garamond and Instrument Sans from Google Fonts. The base palette, fonts and spacing are 8 CSS custom properties on `:root`; a few `rgba()` overlays repeat the background and text colors.

## Structure

```
src/
  layouts/Base.astro      shared head, global styles, cursor, lightbox, reveal script
  components/Hero.astro   full-screen hero, shown on / only
  pages/index.astro       gallery data (2 series, 9 works) and page markup
public/images/            AI renders, WebP
docs/screenshot.webp      README image
```

## Run

Requires Node 18.17.1, 20.3 or newer (tested on Node 25.9).

```bash
npm ci
npm run dev        # http://localhost:4321
npm run build      # writes dist/
npm run preview    # serves dist/
```

Deploy is a copy of `dist/` to any static host.

## Status

Live at [prfo.design](https://prfo.design). The live site still serves an earlier build (a different hero, one more series, heavier images without XMP metadata); this repo is the current source.

## License

Code: MIT, see [LICENSE](LICENSE). The images in `public/images/` and `docs/screenshot.webp` are AI renders by Lev Skorokhodov and are not covered by the MIT license; ask before reusing them.
