# Styling a personal blog

This project is meant to provide the basis for a workshop / showcase of the required steps to styling a personal blog.

The provided HTML structure simulates a blog layout, and the only file modified will be `style.css`.

* * *

## Contents

- [The HTML structure](#the-html-structure)
- [Resetting default values](#resetting-default-values)
- [Typography](#typography)
- [Colors](#colors)
- [Article layout](#article-layout)
- [The sidebar](#the-sidebar)
- [General document layout](#general-document-layout)
- [Mobile layout](#mobile-layout)
- [Extra damage](#extra-damage)


* * *

### The HTML structure

The HTML document is structured as follows:

```
document
    head - Contains metadata describing the document contents
    body
        header
            h1 - blog title
            nav - main navigation links
        main
            article * 10
                header - aricle metadata 9title, author, posted date
                main - the blog post content
        aside
            section * 3 - link groupings
                h3 - section name
                nav - section links
        footer - copyright information and a copy of the main navigation
        script - a bit of JavaScript that will show/hide the navigation
```

No classes are defined in the HTML document, and other than the blog posts, no element has any `id`s -- this will allow us to target specific elements based on the document hierarchy, rather than on any arbitrary classes.

Note that both the document body and the articles themselves contain `header` and `main` elements -- this is not an error, as their meaning differs: on one hand, we have the data for the whole document, while on the other, we have the data for each article.

This is not important for the workshop, but it bears pointing out.

* * *

### Resetting default values

The first step is to make sure certain default behaviors are supressed; while the browser defaults are generally good, they are optimized for machine-readability. That is to say, the HTML should be accessible for all users, with aesthetic considerations taking step back. This is fine, but not what we want in this case -- we want the blog to *look* good.

The reset for this blog looks thus:

```css
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

html,
body {
    min-height: 100vh;
    line-height: 1.5;
    scroll-behavior: smooth;
    -webkit-font-smoothing: antialiased;
}

img,
svg {
    display: block;
    max-width: 100%;
}

input,
button {
    font: inherit;
}

p,
h1,
h2,
h3 {
    overflow-wrap: break-word;
}

ul {
    list-style: none;
}
```

First, we ensure that all elements on the page (`*`) have no spacee around their content, and we specify that the dimensions of the element take inner space into account (this is the `box-sizing` property).

Then, we specify that the document should be at least as tall as the browser window, we increase the spacing between lines of text, we have the scroll behavior animate smoothly, and set the characters be smoother.

Afterwards, we ensure that all image elements take up the full width of their respective containers, which makes them automatically responsive (ie, they look good at any zoom level).

For interactive elements (inputs, buttons and so on), we ensure that the display font is the same one used on the rest of the page, to prevent visual clashes.

The paragraphs and headers of the document have the `overflow-wrap` property set to `break-word`, ensuring that longer words wrap around and not break the layout.

Finally, the lists (unordered and ordered) have the list markers removed, again to prevent visual clashes.

* * *

### Typography

This is where we set the font family used throughout the document. We add an import at the top of the document, sourcing the two fonts we shall use from Google.

The document body will use the *Source Sans Pro* font, set to a size of 16 pixels and a medium weight; The headers will use the *Playfair Display* font, a semi-bold weight and relative sizes, to denote importance.

The sizes, by the way, are given in `rem`s, or `relative em units`. An `em` is the maximum character width, or the width of the `m` character, which is typically the widest letter in the Latin alphabet. The `em` unit is relative to the font size of its container, but if we want to use a more consisten style, we shall use the `rem` unit, which is relative to the font size of the document. 

Fallback fonts are also specified, in case the main ones do not load.

```css
@import url("https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&family=Source+Sans+Pro&display=swap");

html {
    font-family: "Source Sans Pro", -apple-system, BlinkMacSystemFont,
        avenir next, avenir, segoe ui, helvetica neue, helvetica, Cantarell,
        Ubuntu, roboto, noto, arial, sans-serif;
    font-size: 16px;
}

h1,
h2,
h3 {
    font-family: "Playfair Display", Iowan Old Style, Apple Garamond,
        Baskerville, Times New Roman, Droid Serif, Times, Source Serif Pro,
        serif, Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol;
    font-weight: 600;
}

h1 {
    font-size: 2.25rem;
}
h2 {
    font-size: 1.75rem;
}
h3 {
    font-size: 1.25rem;
}
```

* * *

### Colors

Three colors have been sourced from [huemint](https://huemint.com/website-1/#palette=f5f7e2-33373f-f55a2d). These will be, respectively, the background color for the page, the text color, and an accent color.

The colors themselves are defined in the HSL schema (Hue - Saturation - Lightness) in order to allow an additional color to be defined -- a darker shade of the accent color.

Since we're using modern CSS, these colors can be defined as custom properties (basically, CSS variables) on the `:root` element; this element does not appear as such in the document, but is in fact a logical property, being the ancestor of all other elements (which means that, in practical terms, it is mapped to the `html` element of the document).

Having defined these colors, they are then assigned to the document (background and text color), to the links and headers (accent color), and the darker accent is assigned to active, hovered and focused links. While we're here, we also remove the underline for the links, set a pointer cursor, and prettify a bit the look of the selected text.

The button, also, could use a bit of love. So, we put a bit of padding on it, and set the colors.

```css
:root {
    --background: hsl(66deg 57% 93%);
    --text: hsl(220deg 11% 22%);
    --text-alt: hsl(220deg 11% 52%);
    --accent: hsl(14deg 91% 57%);
    --accent-alt: hsl(14deg 91% 37%);
}

html {
    background-color: var(--background);
    color: var(--text);
}

footer div {
    color: var(--text-alt);
}

a,
h1,
h2 {
    color: var(--accent);
}

a:hover,
a:active,
a:focus {
    color: var(--accent-alt);
}

a,
button {
    cursor: pointer;
    text-decoration: none;
}

button {
    color: var(--accent);
    background-color: transparent;
    border: 1px solid;
    border-radius: 0.25rem;
    padding: 0.5rem;
}

svg {
    background: transparent;
    fill: var(--accent);
}

button:hover,
button:focus {
    color: var(--accent-alt);
}

button:hover svg,
button:focus svg {
    fill: var(--accent-alt);
}

::selection {
    background: var(--accent);
    color: var(--background);
}
```

* * *

### Article layout

To beautify the articles, we first must add some spacing below each article; this space will be twice the size of a regular letter.

Then, the headers of the articles have their color changed, to visually separate the content of the article from the information describing it.

Finally, in each article's content, we take every paragraph other than the first one (`p + p`) and add a bit of spacing abot each one.

```css
article {
    margin-bottom: 2rem;
}

article header {
    color: var(--text-alt);
    margin-bottom: 1rem;
}

article main * + p {
    margin-top: 0.5rem;
}
```

* * *

### The sidebar

We shall use the same trick as above to space out the link groups in the sidebar.

```css
aside section + section {
    margin-top: 1rem;
}
```

* * *

### General document layout

```css
body {
    display: grid;
    grid-template-columns: 3fr 1fr;
    gap: 1rem;

    max-width: 60rem;
    margin: 0 auto 1rem;
    padding: 0 1rem;
}

body > header,
body > footer {
    grid-column: 1 / -1;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

header nav > ul {
    display: flex;
    gap: 1rem;
}

body > footer {
    justify-content: space-around;
}
```

* * *

### Mobile layout

Finally, we're getting to the good bit -- making averything responsive.

First, we need to make sure that the button is not visible in desktop mode, so we hide it.

```css
body > header button {
    display: none;
}
```

Then, a *media query* is necessary, to specify a new layout when the sccreen width is less than a certain value; in this particular case, this width is 800px, because that proved to be the minimum width at which the desktop layout looked okay.

Now, since the base font size was defined to be 16 pixels, the width is `the_screen_width / the_font_size`, or `800px / 16px`, or 50rem.

```css
@media screen and (max-width: 50rem) {
    /* ... /*
}
```

Now, we can start laying out the document for  small screens.

The document will have only one column, with the sidebar going below the main content. Since we'll want the navigation menu to be displayed under the logo and toggler button, we specify that the header should wrap. And speaking of the toggler button, this will need to be displayed again.

The main navigation, as well as the footer, will need to be displayed vertically, rather than horizontally, which can be done by specifying the `column` flex direction.

```css
body {
    grid-template-columns: 1fr;
}

body > header {
    flex-wrap: wrap;
}

body > header button {
    display: block;
}

body > header ul,
body > footer {
    flex-direction: column;
}
```

Finally, the main navigation should look better. For that, we set its order to be 3, so it's placed *after* the button. Setting its flex basis to 100% will make it as wide as the header, which, in conjunction with the wrap property on this, will cause the list to flow underneath the other header elements.

The main navigation should also be hidden by default, so its opacity and height are both set to 0. As we have a CSS class, `.visible`, added via JavaScript, we set the opacity to 1 and the height to `auto` for the nav elements when they have this class.

Finally, since an element popping in and out of existence doesn't look all that good, we set a height transition of 750 miliseconds, and an opacity transition of 500ms, and the document now looks decent.

```css
body > header nav {
    order: 3;
    flex-basis: 100%;
    text-align: center;
    
    opacity: 0;
    height: 0;
    transition: height 0.75s, opacity 0.5s;
}

body > header nav.visible {
    height: auto;
    opacity: 1;
}
```

* * *

### Extra damage

What else are media queries good for? How about dark mode?

So, if the user has system-wide dark mode enabled, we can leverage that, and simbply flip some colors around. As we've defined the colors using CSS custom properties, we can simply target browsers with dark mode enabled, and change the values of these properties, as follows:

```css
@media screen and (prefers-color-scheme: dark) {
    :root {
        --background: hsl(220deg 11% 22%);
        --text: hsl(66deg 57% 93%);
        --text-alt: hsl(66deg 37% 63%);
    }
}
```

* * *
