# Lessons Learned: Doing iOS 9.3.5 Web Development in 2026

_Thoughts and lessons learned by me, Donna J. Harris._

## Overview

The following lessons have been discovered through hand debugging and testing, commenting out code, viewing in a modern browser and comparing with the iOS 9.3.5 Safari browser on an iPad 3.

These are my own notes about developing for legacy environments while developing MonoVerse, and other websites. No AI was involved in generating this documentation, although AI has generated the initial POC code and has assisted with some troubleshooting and pattern checking.

### LESSON 0: Test locally
<sup>Monday, June 1, 2026</sup>

Most important for sanity! Test locally first.

iOS 9.3.5 development needs a lot of manually debugging and troubleshooting. Save yourself the hassle and run a small HTTP Python server locally on your network and connect your legacy (and other!) test devices to it while working through these things.

How-To Link: **[TEST_LOCALLY.md](TEST_LOCALLY.md)**

### LESSON 1: Trailing commas are bad
<sup>Monday, June 1, 2026</sup>

iOS 9.3.5 does not like trailing commas very much. Apparently this little thing (which is added by extensions like Prettier -- see Lesson 1a) totally makes iOS 9.3.5 Safari die.

According to Claude AI, the JavaScript engine doesn't support trailing commas in function calls. That's an ES2017 feature. So, the old WebKit simply chokes on it silently.

**DO THIS:**
```
        addResult(
          'rawBooks.length: ' + rawBooks.length,
          rawBooks.length > 0 ? 'pass' : 'fail'
        );
```

**NOT THIS:**
```
        addResult(
          'rawBooks.length: ' + rawBooks.length,
          rawBooks.length > 0 ? 'pass' : 'fail',
        );
```

_Note: the **NOT THIS** example results in the entire object not being rendered on the page, even if some code is good._

I experienced several red herrings, such as using ternary operators and inline concatentation used within function calls, etc., but all of the "fixes" I experienced also included removing the trailing commas. But simply removing the trailing commas from all function calls allowed the initial POC index page to load.


### LESSON 1a: Don't use modern code formatters for legacy code
<sup>Monday, June 1, 2026</sup>

Using Prettier in VS Code was actually introducing/re-introducing problems because it was looking to achieve a standard too modern for iOS 9.3.5

Don't use these tools for this kind of project work.

Beware of anything that could alter your code to the wrong standard.


### LESSON 2: Using SVG sprite sheet
<sup>Monday, June 8, 2026</sup>
((This tip needs a HOW-TO of its own!))

__**Style Rule to Adopt**__
> Anywhere you use SVG features, add the `xlink` namespace and take a dual attribute approach with referencing the SVG symbols in the `use` element. This gives you the legacy safety net automatically.

__**Details**__
Using an SVG sprite sheet at the top of the `body`'s code is a great way to make the code readable, while still being able to use CSS on the SVGs on your page. BUT... it needs some legacy support.

**DO THIS:**
```
        <svg aria-hidden="true" focusable="false">
                <use href="#kofi_icon_svg" xlink:href="#kofi_icon_svg" />
        </svg>
```

**NOT THIS:**
```
        <svg aria-hidden="true" focusable="false">
                <use href="#kofi_icon_svg" xlink:href="#kofi_icon_svg" />
        </svg>
```

The `<use href="#...">` is the problem with legacy browser. The `href` attribute on `<use>` isn't supported on older browsers and older WebKit (which is what iPad 3 runs). They need `xlink:href` instead.

For maximum compatibility, **include both** as we show here. Modern browsers use `href`, legacy browsers fall back to `xlink:href`. Both pointing to the same `symbol` ID so nothing changes visually.

You'll also need the `xlink` namespace declared on your sprite sheet's opening `svg` tag, for good measure:
```
    <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="display: none">
```

### Lesson 3: CSS + SVG controlling the width
<sup>Monday, June 15, 2026</sup>

The initial problem was independently controlling the width of a containing with an SVG image inside. The fix suggested was to set `aspect-ratio` explicitly within each `<svg>` tag's `style` attribute. The value is calculated from the SVG's viewBox, alongside the desired height.

For example.
```
<svg class="type-badge-icon" style="aspect-ratio: 343.96 / 60.46;" aria-hidden="true" focusable="false">
  <use href="#android_logo_svg" xlink:href="#android_logo_svg" />
</svg>
```

The `type-badge-icon` is a style for the container with a defined height.

But, the problem is that `aspect-ratio` is well-supported in modern browsers but not on iPad 3's iOS 9 WebKit. As a legacy fallback, the `width` value must be hardcoded into the `style` attribute as well:

```
<svg class="type-badge-icon" style="aspect-ratio: 343.96 / 60.46; width: 182px;" aria-hidden="true" focusable="false">
```

### Lesson 4: CSS + SVG accessibility "tool tips" ((ACCESSIBILITY))
<sup>Monday, June 15, 2026</sup>

SVG can be used as decorative elements on a website, and ignored explicitly using `aria-hidden="true" focusable="false"` on the `<svg>` element. But sometimes, the SVG is informative and needs to be presented, for both accessibility and for general usability, since it is not a regular image.

This is an example of how to provide good support:

```
        <div
          class="badge type-badge"
          role="img"
          aria-label="Web App"
          title="Web Application"
        >
          <svg class="type-badge-icon" style="aspect-ratio: 1 / 1; width: 32px">
            <use href="#web_app_icon_svg" xlink:href="#web_app_icon_svg" />
          </svg>
        </div>
```
- `role="img"` is set for the container element (could be directly on the `<svg>` in some cases) to identify its role as an image
- `aria-label=` at the same level provides the information to the shared with a screen reading device about the container
- `title=` at the same level provides the TOOL TIP that a web browser will see on hover over the container
- notice the absence of `aria-hidden="true"` and `focusable="false"` because we aren't trying to hide these elements from assistive technology


### Lesson X: CSS `gap` needs a workaround  (IN PROGRESS ... not sold on this one yet)
<sup>Monday, June 8, 2026</sup>

`gap:` on grid and flexbox containers isn't supported in the older WebKit on iPad 3.

The reliable legacy fallback is `margin:` on the **child** elements instead.

####For grid layouts:
```
.projects,
.available {
  display: grid;
  gap: 20px; /* modern browsers */
  margin-top: 4px;
}

.project-card,
.available-card {
  margin-bottom: 20px; /* legacy fallback */
}
```

####For flex layouts:
```
.badges {
  display: flex;
  flex-wrap: wrap;
  gap: 8px; /* modern browsers */
}

.badges > * {
  margin-right: 8px; /* legacy fallback */
  margin-bottom: 8px;
}
```
