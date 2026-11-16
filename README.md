# uHost — Academind CSS Course Project

### 🔗 Live site: **[rickrieger.github.io/CSS_course_project_Acadamind](https://rickrieger.github.io/CSS_course_project_Acadamind/)**

**uHost** is a multi-page website for a fictional web-hosting company. It's the practice project built step by step through [Academind](https://academind.com/)'s CSS course ([course page](https://pro.academind.com/courses/enrolled/767968), enrollment required). I built it between **September 7 and October 4, 2020**. It was my second GitHub repository, and the commit history shows the course's progression: layout, then "changed many percentages to rem", then a JavaScript side menu, responsive design, and finally a sign-up form.

The site is written in **plain HTML, CSS and a little vanilla JavaScript**, with no frameworks (no Bootstrap) and no build tools. The point of the project was to learn how CSS itself works.

![uHost home page](images/freedom.jpg)

---

## Contents

- [Pages](#pages)
- [Built with](#built-with)
- [CSS concepts practiced](#css-concepts-practiced)
- [JavaScript](#javascript)
- [Run it locally](#run-it-locally)
- [Deployment](#deployment)
- [Cleanup log (November 2026)](#cleanup-log-november-2026)
- [Known limitations](#known-limitations)
- [Project structure](#project-structure)

---

## Pages

| Page | File | What's on it |
|---|---|---|
| Home | `index.html` | Hero image ("Get the freedom you deserve."), three pricing plans (Free / Plus / Premium), "Many Good Reasons to Stick Around" feature icons. Choosing a plan opens a confirmation modal. |
| Packages | `packages/index.html` | Each plan as a large card over a full-screen background photo, with cards alternating left and right |
| Customers | `customers/index.html` | Two customer testimonials with large photos, image and text alternating sides |
| Start Hosting | `start-hosting/index.html` | Sign-up form with styled inputs, a custom checkbox and validation styling |

Every page shares the same header (desktop nav plus a hamburger menu on phones), footer, `shared.css` and `shared.js`.

## Built with

| Technology | Used for |
|---|---|
| HTML5 | Semantic page structure (`header`, `nav`, `main`, `section`, `article`, `footer`) |
| CSS3 | All layout and styling, with no framework. `shared.css` holds site-wide styles; each page has its own stylesheet. |
| Vanilla JavaScript | Mobile side menu, plan confirmation modal (`shared.js`) |
| Google Fonts | Montserrat (body text), Anton (hero headline). Roboto is imported in `shared.css` but not applied anywhere. |
| Inline SVG | The three feature icons on the home page |
| GitHub Pages | Free static hosting (set up Nov 2026) |

## CSS concepts practiced

This is the part the project was built to teach. Here's where each concept appears in the code.

**BEM naming.** Classes follow *Block__Element--Modifier*, so it's clear what each class styles and how they relate:

```html
<article class="plan plan--highlighted">     <!-- block + modifier -->
  <h1 class="plan__annotation">RECOMMENDED</h1>  <!-- element -->
  <h1 class="plan__title">PLUS</h1>
```

**Mobile-first responsive design.** Base styles target phones, and `@media (min-width: 40rem)` (640px) layers on the desktop layout. For example, pricing plans stack full-width on phones and sit side by side on larger screens:

```css
.plan { width: 100%; }                 /* phones: stacked */

@media (min-width: 40rem) {
  .plan {
    display: inline-block;             /* desktop: side by side */
    width: 30%;
  }
}
```

The same pattern switches between the hamburger menu and the full nav (`.toggle-button` / `.main-nav`).

**`rem` units.** Sizes use `rem` (relative to the root font size) instead of fixed pixels or percentages, so spacing scales if a visitor changes their browser's font size. One commit was literally "Changed many percentages to rem :)".

**Positioning.**
- `position: fixed` keeps the header at the top while scrolling, and pins the backdrop, side drawer and modal over the page.
- `position: absolute` places the hero headline at the bottom-left of its section, and the "RECOMMENDED" badge in the corner of a package card (the parent has `position: relative`).
- `z-index` layers them: header 1, backdrop 100, mobile nav 101, modal 200.

**Layered backgrounds.** The hero combines a gradient overlay, a photo and a fallback color in one `background` shorthand:

```css
#product-overview {
  background: linear-gradient(to top, rgba(80, 68, 18, 0.6) 10%, transparent),
              url("images/freedom.jpg") center/cover no-repeat border-box,
              #ff1b68;
}
```

The Packages page uses a `position: fixed` full-screen background with `filter: grayscale(40%)`.

**Inline-block layout (pre-Flexbox).** Pricing plans, feature icons, the nav and the testimonials are laid out with `display: inline-block` plus `vertical-align`. That's how layouts were built before Flexbox and Grid, and the course teaches it first.

**Floats and clearfix.** On the Packages page the Free plan uses `float: right`, and a `.clearfix { clear: both; }` element stops later content from wrapping around it.

**`@font-face`.** `packages/packages.css` shows how to load a self-hosted font (Anonymous Pro) from `.ttf` files.

**Advanced selectors.** The sign-up form uses attribute selectors, sibling selectors and pseudo-classes:

```css
.signup-form input[id*="terms"] + label { ... }   /* label right after the terms checkbox */
.signup-form input:not([type="checkbox"]) { ... }
.signup-form input[type="checkbox"]:checked { background: #2ddf5c; }
```

The checkbox is restyled from scratch with `appearance: none`.

**Pseudo-classes for interaction.** `:hover`, `:active` and `:focus` styles on nav links, buttons, cards and inputs. `.button[disabled]` styles a disabled button.

## JavaScript

`shared.js` is about 40 lines and uses `classList` to add or remove an `open` class, which `shared.css` turns into `display: block`:

- **Hamburger menu (phones):** tapping ☰ opens the side drawer (`.mobile-nav`) and a dark backdrop. Tapping the backdrop closes both.
- **Plan modal:** any "CHOOSE PLAN" button opens "Do you want to continue?". **Yes** goes to the sign-up page, and **No** (or tapping the backdrop) closes it.

The commented-out lines in the file show the course's first approach (setting `element.style.display` directly) before it was refactored to toggle classes.

## Run it locally

No build step needed:

```bash
git clone https://github.com/RickRieger/CSS_course_project_Acadamind.git
cd CSS_course_project_Acadamind
# open index.html in a browser
```

## Deployment

Hosted on **GitHub Pages** from the root of the `master` branch (turned on November 2026). Every push to `master` redeploys the site automatically within a minute or two. All links between pages are relative (`../packages/index.html`), so the site works from the `/CSS_course_project_Acadamind/` subfolder without changes.

---

## Cleanup log (November 2026)

All changes below were made on **2026-11-16** on the branch `cleanup-and-deploy`, checked in a browser at desktop and phone sizes, and merged into `master`. Each is its own commit, so it can be reviewed or reverted on its own.

### 1. Removed duplicate "file 2" / "file 3" copies

**Why:** Five files had exact duplicates with " 2" or " 3" added to the name. macOS Finder makes these when a file is copied into a folder that already has one with the same name. Nothing used them, and they made it unclear which file was real.

**Before:**
```
favicon.png      favicon 2.png
index.html       index 3.html
main.css         main 2.css
shared.css       shared 2.css
shared.js        shared 2.js
```

**After:**
```
favicon.png
index.html
main.css
shared.css
shared.js
.gitignore       ← new: ignores .DS_Store / Thumbs.db
```

I confirmed each pair was byte-for-byte identical before deleting.

### 2. Fixed the plan confirmation modal on phones

**Why:** The modal was always 40% of the screen wide. On a typical 375px phone that's only **150px**, so "Do you want to continue?" spilled out of the box and the Yes/No buttons stacked on top of each other. Everything else on the site is mobile-first, but the modal wasn't.

**Before** (`main.css`):
```css
.modal {
  ...
  left: 30%;
  width: 40%;
}
```

**After:**
```css
.modal {
  ...
  left: 5%;
  width: 90%;        /* phones: nearly full width */
}

@media (min-width: 40rem) {
  .modal {
    left: 30%;
    width: 40%;      /* desktop: same as before */
  }
}
```

On a 375px screen the modal is now 338px wide and the text fits. Desktop looks the same as before.

### 3. Sign-up form no longer shows errors before you type

**Why:** The `:invalid` pseudo-class matches any empty `required` field, including when the page first loads. So the whole form appeared in red "error" styling before a visitor had touched it. `:user-invalid` only matches after the user has edited a field or tried to submit, which is when an error message actually makes sense.

**Before** (`start-hosting/start-hosting.css`):
```css
.signup-form input.invalid,
.signup-form select.invalid,
.signup-form :invalid {
    border-color: red !important;
    background: #faacac;
}
```

**After:**
```css
.signup-form input.invalid,
.signup-form select.invalid,
.signup-form :user-invalid {
    border-color: red !important;
    background: #faacac;
}
```

| | Field background on page load | After clicking "Sign Up" with it empty |
|---|---|---|
| Before | red (`#faacac`) | red |
| After | white | red |

### 4. Fixed a broken link in a customer testimonial

**Why:** The link had no `https://`, so the browser treated `tech-analysis.com` as a file in the current folder. It went to `customers/tech-analysis.com`, which is a 404. The testimonial is fictional, so the company name is now plain text instead of a link to a real outside website.

**Before** (`customers/index.html`):
```html
<h2 class="testimonial__subtitle">Founder of
    <a href="tech-analysis.com">tech-analysis.com</a>
</h2>
```

**After:**
```html
<h2 class="testimonial__subtitle">Founder of
    tech-analysis.com
</h2>
```

### 5. Set an explicit white page background

**Why:** `body` had no background color, so it used the browser's default. In viewers with a dark default (such as forced dark mode), the black text on the Customers and Start Hosting pages became nearly unreadable. The design assumes a white page, so the CSS now says so.

**Before** (`shared.css`):
```css
body {
  font-family: "Montserrat", sans-serif;
  margin: 0;
}
```

**After:**
```css
body {
  background: white;
  font-family: "Montserrat", sans-serif;
  margin: 0;
}
```

### 6. Compressed photos (2.0 MB → 950 KB)

**Why:** The four photos were saved at high JPEG quality, so the site downloaded about 2 MB of images. Re-saving at quality 72, at the same 1920px size, halves that with no visible difference at the sizes they're shown.

| Image | Used on | Before | After |
|---|---|---|---|
| `customer-1.jpg` | Customers | 555 KB | 246 KB |
| `customer-2.jpg` | Customers | 425 KB | 205 KB |
| `freedom.jpg` | Home hero | 347 KB | 130 KB |
| `plans-background.jpg` | Packages | 721 KB | 369 KB |
| **Total** | | **2,048 KB** | **950 KB** |

### 7. Deployed to GitHub Pages

**Why:** The site had never been published, so the only way to see it was to clone the repo. GitHub Pages now serves it from the `master` branch at the link at the top of this README.

### 8. Rewrote this README

**Before:** the repo name, "Academind CSS course project", and a course link.
**After:** this document.

---

## Known limitations

These are intentional for a course project and were left as they are:

- **The sign-up form doesn't send anything.** It has no backend, and the inputs have no `name` attributes, so submitting just reloads the page. Nothing you type is stored or sent anywhere.
- **Footer links ("Support", "Terms of Use") and the package cards link to `#`.** They're placeholders.
- **The company, prices and testimonials are fictional.**
- **The Anonymous Pro font** is declared with `@font-face` in `packages.css` as a lesson example but isn't applied to any text, so browsers never download it.

## Project structure

```
index.html                  home page
main.css                    home page styles (hero, plans, features, modal)
shared.css                  styles used by every page (header, nav, footer, buttons, backdrop)
shared.js                   mobile menu + plan modal
favicon.png
images/                     photos and uHost logo
packages/
  index.html, packages.css  Packages page
  anonymousPro-*.ttf        @font-face example font
customers/
  index.html, customers.css Customers page
start-hosting/
  index.html, start-hosting.css  Sign-up page
```
