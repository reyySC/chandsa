# WAPH — Web Application Programming and Hacking

**Instructor:** Dr. Phu Phung

## Individual Project 1 — Front-end Web Development with a Professional Profile Website on github.io

**Student:** Shreya Chandra
**Email:** shreyachandra3102@gmail.com

<img src="headshot-150.jpg" width="150" height="150" alt="Shreya Chandra">

- **Deployed website:** https://reyysc.github.io
- **Course page:** https://reyysc.github.io/waph.html
- **Website source repository:** https://github.com/reyySC/reyySC.github.io
- **Project folder:** https://github.com/reyySC/chandsa/tree/main/individual-project1

---

## Overview

In this project I designed and deployed a professional profile website on the GitHub Pages cloud
service. The site presents my resume for a potential employer and is built on the Bootstrap 5 CSS
framework, with client-side behaviour implemented in jQuery and Vue 3. It integrates two public web
APIs, uses JavaScript cookies to recognise returning visitors, and includes a Flag Counter page
tracker.

Outcomes I took from the assignment:

- Deploying a static site to GitHub Pages, and understanding why a repository must be named
  `username.github.io` to be published at the root of that domain.
- Using an open-source responsive CSS framework instead of hand-written CSS, and customising it with
  CSS variables so the same markup supports a light and a dark theme.
- Writing DOM manipulation and timer-driven code in jQuery, and contrasting it with the reactive
  data-binding model of Vue 3 in the skills filter.
- Consuming third-party REST APIs from the browser, including handling request failures and CORS
  restrictions.
- Reading and writing cookies from JavaScript with an explicit expiry and path, and understanding
  that the same-origin policy limits which scripts can read them.

---

## Task 1 — General requirements: profile website and `waph.html`

`index.html` is the main page of the site. It contains my headshot, name, location, contact
information behind a show/hide control, education at the University of Cincinnati, four professional
roles, selected projects, leadership positions, and a filterable skills list. The page is linked to
`waph.html`, a second page that introduces this course, its topic areas, and every hands-on exercise
in it. Both pages are committed to the `reyySC.github.io` repository and published automatically by
GitHub Pages.

*Screenshot 1: the live site at https://reyysc.github.io.*

*Screenshot 2: the live `waph.html` course page.*

## Task 2 — Non-technical requirements: CSS framework and page tracker

The site uses **Bootstrap 5.3**, loaded from the jsDelivr CDN, for its grid, spacing, and form
components. On top of Bootstrap I defined a small custom layer of CSS variables (ink, paper, pine,
line) so that the colour scheme, typography, and dark mode are consistent across both pages. The
layout collapses from a multi-column desktop grid to a single column on mobile through Bootstrap's
responsive column classes.

The page is written as an employer-facing document: the headline states what I build and my
availability, each role leads with what shipped rather than what was assigned, and the résumé PDF and
GitHub profile are linked directly from the header.

The footer carries the page tracker. A cookie-backed visit counter reports `Visit #n from this
browser`, and a **Flag Counter** widget sits beside it recording visits by country.

*Screenshot 3: the footer showing the page tracker.*

## Task 3 — Basic JavaScript with jQuery and a second framework

| Feature | Implemented with |
|---|---|
| Digital clock | jQuery, `setInterval` at 1 s |
| Analog clock | HTML5 Canvas, redrawn every second |
| Show / hide email | jQuery `.toggle()` |
| Extra feature: dark mode toggle | jQuery + Bootstrap `data-bs-theme`, remembered in a cookie |
| Extra feature: visit counter | jQuery, backed by the same cookie helpers |
| Skills filter | Vue 3 reactive `computed` property |

The digital clock zero-pads each component and rewrites the status bar text every second. The analog
clock is drawn on a canvas: the face and hour numerals are rendered first, then three hands are drawn
from angles derived from the current time, with the hour hand offset by the elapsed minutes so it
moves continuously. The clock reads its stroke colours from the same CSS variables as the rest of the
page, so it redraws correctly when the theme changes.

The email address is hidden by default and revealed by a jQuery `.toggle()` call, with the button
label switching between "Show email" and "Hide email". The extra feature of my choice is a dark mode
toggle, which flips Bootstrap's `data-bs-theme` attribute and stores the choice in a cookie so the
preference survives a reload.

Vue 3 is the second framework. The skills section is mounted as a Vue application in which a text
input is bound with `v-model` and a `computed` property filters the skills array; the chips and the
"n/m shown" counter re-render automatically as I type, with no manual DOM manipulation.

*Screenshot 4: the header showing both clocks running and the email revealed.*

*Screenshot 5: the skills filter narrowing the list as text is typed.*

## Task 4 — Two public web API integrations

1. **JokeAPI** — `https://v2.jokeapi.dev/joke/Any` is requested with `$.getJSON` on page load and
   again every 60 seconds via `setInterval`. The response can be either a single-line joke or a
   setup/delivery pair, so the handler checks `data.type` and formats accordingly. A failure handler
   displays a fallback message rather than leaving the panel empty.
2. **NASA Astronomy Picture of the Day** — `https://api.nasa.gov/planetary/apod` returns a JSON
   object containing an image URL, which is written into an `<img>` element along with its title and
   date. Because the demo API key is rate limited, a `.fail()` handler falls back to the Dog CEO
   image API so the section always renders a graphic.

I originally tried the xkcd API suggested in the lecture slides, but `xkcd.com/info.0.json` does not
send an `Access-Control-Allow-Origin` header, so the browser blocks the response under the
same-origin policy. This is why I selected APIs that explicitly support cross-origin requests.

A disclaimer below both widgets states that the content is generated by third-party services and
that I am not responsible for it.

*Screenshot 6: the API section showing a joke and an image, with the disclaimer visible.*

## Task 5 — Cookies to remember the client

On load the page reads a `lastVisit` cookie. If it is absent the status bar displays
`Welcome to my homepage for the first time!`; if present it displays
`Welcome back! Your last visit was <date/time>`. In both cases the cookie is then rewritten with the
current timestamp and a one-year expiry, so the stored value updates on every visit. Values are
URL-encoded before being written, because a raw `toLocaleString()` result contains commas that would
otherwise terminate the cookie value.

*Screenshot 7: first-visit message, in a private browsing window.*

*Screenshot 8: return-visit message showing the previous timestamp.*

*Screenshot 9: DevTools → Application → Cookies showing the `lastVisit` cookie and its expiry.*

---

## Conclusion

The hardest part of this project was not the JavaScript but the third-party dependencies: the CORS
failure on xkcd and the rate limit on NASA's demo key both fail silently in the browser, and finding
them meant reading the console rather than the page. Adding fallback handlers to every external
request was the fix. Given more time I would replace the CDN links with locally vendored copies so
the site does not depend on four external hosts to render.

This project covers only the client side, where every value the page holds is visible and editable by
the user. That limitation is the bridge to the rest of the course: Individual Project 2 moves the
same concerns — identity, sessions, stored data — onto a server where they can actually be enforced.
