# Off The Tape (sealyoulater.github.io) - changes

## Real bugs fixed
- `_includes/head.html` had an **unclosed `<link>` tag**
  (`<link rel="stylesheet" href="/css/main.css"` with no closing `>`)
  on every single page. This is the single biggest fix in this
  project - it was malformed markup in the `<head>` of every page on
  the site.
- `_layouts/default.html` was missing `<!DOCTYPE html>` and `lang`,
  and had a `<meta viewport>` tag sitting inside `<body>` instead of
  `<head>` (meta tags only belong in `<head>`).
- `.dropdown { positon: relative; }` - typo (`positon`), which broke
  the dropdown menu's positioning context entirely.
- `.drop-menu { right: 185; top: 60; }` - unitless values where a CSS
  length is required. Invalid CSS gets silently dropped by the
  browser, so this positioning never actually applied. Fixed to
  `right: 0; top: 100%`.
- `.home-section h2 { margin-botton: 1.5rem; }` - typo, silently
  ignored by the browser. Fixed to `margin-bottom`.
- `.blog-title-list span { ... font-style: italic margin-top: 0.5em; }`
  - missing semicolon, breaks parsing of that declaration.
- `.blog-title-lost` - typo selector that never matched the real
  `.blog-title-list` class used in the HTML, so the recent-posts list
  never had its bullets/padding reset.
- `.post-header .scoial-link` - typo, should be `.social-link`, dead
  selector.
- `.bio-box { margin: 2rem 5 0; }` - unitless value, invalid, ignored.
  Fixed to `margin: 2rem 0.5rem 0`.
- `index.html`: `<div class="bio-wrapper>"` - the closing quote was in
  the wrong place, breaking the tag. There was also an empty
  `<div></div>` immediately after it for no reason.
- `index.html`: a `<h2>` was nested directly inside a `<p>` (invalid -
  block elements can't go inside paragraph content), and a
  `<br>...</br>` pair (`<br>` is a void element, it can't have a
  closing tag).
- `_posts/2020-08-17-Episode 2 .md` had a trailing space in the
  filename before `.md`. Jekyll derives post slugs from filenames, so
  this could cause inconsistent URL generation. Renamed.
- The `episodes/index.html` gallery page was unfinished scaffolding:
  all 5 thumbnails pointed at the same single `slide1.jpg` (the only
  image that actually existed in `images/small` and `images/large`),
  generic "Image title 1/2" alt text, and no JavaScript anywhere in
  the repo to make clicking a thumbnail actually do anything. I didn't
  have real episode photos to fill in 5 unique slides, so rather than
  ship fake placeholder content I rebuilt this page as a real "All
  Episodes" grid pulling from your actual post data (same data the
  blog page already uses). It was also previously an orphaned page -
  nothing in the nav linked to it at all.

## Accessibility
- Added a skip link.
- Added real `alt` text on every bio photo (was empty/missing on all
  4).
- Added `alt=""` (correctly empty/decorative) on gravatar images,
  since the author's name is already shown as visible text right next
  to them - repeating it as alt text would just be noise for screen
  reader users.
- Every icon-only social link (Twitter/YouTube/Spotify/Reddit/Apple
  Podcasts, on the home page and in all 4 episode posts) had no
  accessible name at all - a screen reader would just announce "link"
  with no indication of where it goes. Added visually-hidden text
  describing each one.
- Added `title` attributes to the YouTube embed iframes (one per
  episode, naming the actual episode) - previously these had no title
  at all, so a screen reader just announced "iframe."
- Added `rel="noreferrer"` to every `target="_blank"` link - opening a
  link in a new tab without this lets the destination page access
  `window.opener`.
- Added `:focus-within` alongside the existing `:hover` on the
  dropdown menu so keyboard users tabbing through nav can actually see
  and reach the submenu (previously mouse-hover only).
- Added a nav-level distinction between "Episodes" (now the real
  episode grid at `/episodes/`) and "Blog" (the post write-ups at
  `/blog/`) - these were both confusingly labeled "Episodes" in the
  old nav despite being two different pages.

## Cleanup
- Removed `_site/`, `.jekyll-cache/`, and `.htaccess` (GitHub Pages
  doesn't use Apache, so this file did nothing).
- Removed a stray duplicate `top: 0;` declaration on
  `.mobile-menu-check:checked ~ .show-mobile-menu:after`.
- Removed an old uncertainty comment on `.header-logo img`
  ("MIGHT BE AN ISSUE...") - the existing `width: auto` with a fixed
  height is actually the correct way to preserve the logo's aspect
  ratio, so there was nothing to fix there.
- Removed a stray unmatched opening quotation mark in Spencer's bio
  text.

## Didn't touch
- The dropdown and mobile menu still use CSS-only patterns
  (`:hover`/`:checked`) rather than JS-driven `aria-expanded` toggling.
  A fully robust accessible disclosure widget really wants a few lines
  of JS, but this repo has zero JavaScript anywhere and adding a
  script file just for one menu felt like a bigger architectural
  change than the rest of this pass. The `:focus-within` addition gets
  keyboard users most of the way there without that change.
- No `_config.yml` exists for this Jekyll site at all - GitHub Pages
  builds it fine with defaults, but there's no `site.title` or other
  site-wide config defined anywhere. Worth adding if you want
  `{{ site.title }}` available in templates instead of the hardcoded
  "Off The Tape Podcast" strings.
