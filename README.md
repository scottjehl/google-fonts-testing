# google-fonts-testing
This repo is intended for performance tests comparing Google Fonts' default embed code vs. more optimal approaches.

## Reduced Test Pages

All of these test pages load the SUSE Google Font from Google's gstatic font server in a variety of ways. When fonts files are referenced directly, the slightly heavier hinted version of the fonts that are typically delivered only to Windows desktop are used, as this represents a single font format that can be served to any device.

### Font Display Swap Tests

- [Default Google Fonts Embed](https://scottjehl.github.io/google-fonts-testing/loading/default-embed.html): Standard embed from Google Fonts site.
- [Direct Font-Face Embed with a hinted font](https://scottjehl.github.io/google-fonts-testing/loading/font-face-hinted.html): Same test page but instead of the standard embed link, this test page places the contents of the CSS file (as it is served on Windows) inside a style element in the page. The fonts are hinted windows woff2 files, which include additional information that only windows devices use. 
- [Direct Font-Face Embed with a hinted font](https://scottjehl.github.io/google-fonts-testing/loading/font-face-hinted-preload.html): Same as the previous test page with a rel=preload added for the woff2 file that ends up being used in the page.

### Font-Display Default (blocking) Tests

The initial test pages use `font-display: swap` because that is the current google fonts default. The following test pages are the same as the first three without using `font-display: swap`, which despite allowing test to render a little faster using fallback fonts while the custom font loads, has fallen out of favor compared to a short delay that renders text solely in a custom font. 

These test pages likely represent a more realistic set of use cases for how folks want fonts to load currently (without the swap).

- [Default Google Fonts Embed](https://scottjehl.github.io/google-fonts-testing/loading/default-embed.html): Standard embed from Google Fonts site.
- [Direct Font-Face Embed with a hinted font](https://scottjehl.github.io/google-fonts-testing/loading/font-face-hinted.html): Same test page but instead of the standard embed link, this test page places the contents of the CSS file (as it is served on Windows) inside a style element in the page. The fonts are hinted windows woff2 files, which include additional information that only windows devices use. 
- [Direct Font-Face Embed with a hinted font](https://scottjehl.github.io/google-fonts-testing/loading/font-face-hinted-preload.html): Same as the previous test page with a rel=preload added for the woff2 file that ends up being used in the page.


## Performance Tests and Comparison

Each test page was run through WebPageTest using 6 test runs on a 4G connection speed in Chrome browser from Virginia, USA with a custom user agent string to ensure that Google serves the default embed as it would for Windows devices, to present a fair comparison. (UA: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36`)

Bulk Test Run, median runs compared in a timeline: https://www.webpagetest.org/video/compare.php?tests=240830_BiDcV6_DBD%2C240830_BiDc0G_DBE%2C240830_BiDcHP_DBF%2C240830_BiDcXG_DBG%2C240830_BiDcHS_DBH%2C240830_BiDcH5_DBJ&thumbSize=100&ival=100&end=visual

### Observations:
- Loading fonts with font-face is faster than the render-blocking default embed.
- The greatest performance improvements show in the tests that use font-face with `font-display: swap`, as the removal of the render-blocking embed link means they can begin showing text as soon as the HTML arrives. This examples show text rendering a full second sooner than the default google embed under these conditions, and the FCP metric reports that 1 second improvement as well. Given that Google fonts currently uses `font-display: swap` as its default, this approach allows the feature to truly work as designed.
- For test pages that do not use `font-display: swap`, the performance improvements of moving to `font-face` are again significant, though less so from a perceived performance perspective due to the blocking nature of browsers' default font display behavior. Still, in the `font-face` examples that do not use `font-display: swap`, Text renders visually 300ms earlier than it does for pages using the default embed, and the First Contentful Paint metric is almost a full second faster than the default embed.
- In these examples, preloading does not show a significant benefit, but it's likely that it would on sites with more complicated CSS that can cause later font discovery.



