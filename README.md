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

Bulk Test Run:


