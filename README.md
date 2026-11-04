# Diwali Advent Calendar

A 21-day interactive countdown to Diwali. Each day a clay lamp opens on a mandala. Lighting it reveals a short story, a film, a picture, or a small hidden game.

The live calendar is at **[diwali.snapnana.com](https://diwali.snapnana.com/)**.

Days 1–16 lead up to the festival. Days 17–21 are the five principal observances: Dhanteras, Naraka Chaturdashi (Choti Diwali), the main Diwali night, Govardhan Puja / Annakut, and Bhai Dooj. Names and customs differ by region and family. The main Diwali lamp, 8 November 2026, sits at the centre of the mandala.

## Run it locally

The site is static. No build step and no package install.

```bash
python3 -m http.server 8080
```

Then open [http://127.0.0.1:8080/](http://127.0.0.1:8080/). `index.html` can also be opened directly as a file, because the calendar config is embedded in JavaScript rather than fetched as JSON.

## How a day unlocks

Each lamp has an `unlockDate` of `YYYY-MM-DD`. “Today” is the calendar date on the visitor’s device. With the lock on, a lamp stays closed until that date begins.

The switch is `dateLockEnabled` in [`assets/diwali_days.json`](assets/diwali_days.json):

- `true` enforces the schedule.
- `false` opens every lamp and leaves the dates in place, so the lock can be turned back on later.

After any edit to that JSON file, regenerate the copy the page actually loads:

```bash
python3 tools/sync_diwali_days_js.py
```

That writes [`assets/diwali-days-data.js`](assets/diwali-days-data.js). Do not edit the generated file by hand.

## Change what a lamp shows

Two files share the work:

| File | What it controls |
| --- | --- |
| [`assets/diwali_days.json`](assets/diwali_days.json) | Dates, lamp positions, themes, and the cultural note stored on the lamp |
| [`assets/diwali-experiences.js`](assets/diwali-experiences.js) | The card, film, picture, or hidden game that opens when the lamp is lit |

A lamp can be an info card, an event card, a bonus film, or a popup image. A card may also carry `cardVideo`, which plays after the card fades, or an `easterEgg`, which stays on the card as a small game. Experience types are keyed by lamp id, from `1` to `21`.

When you change `style.css`, `sketch.js`, or another script the browser may already have cached, bump the `?v=` query on that file in [`index.html`](index.html).

## Schedule for 2026

| Day | Date | Theme |
| --- | --- | --- |
| 1 | 21 Oct | Rangoli |
| 2 | 22 Oct | Cleaning the Home |
| 3 | 23 Oct | Rangoli in Motion |
| 4 | 24 Oct | Festival Sweets |
| 5 | 25 Oct | Traditional Dress |
| 6 | 26 Oct | The Diya |
| 7 | 27 Oct | Fireworks |
| 8 | 28 Oct | Marigold Garlands |
| 9 | 29 Oct | Music & Dance |
| 10 | 30 Oct | Diwali Around the World |
| 11 | 31 Oct | Share the Light |
| 12 | 1 Nov | Auspicious Beginnings |
| 13 | 2 Nov | A Warm Welcome |
| 14 | 3 Nov | Savour Something Sweet |
| 15 | 4 Nov | A Mindful Celebration |
| 16 | 5 Nov | Auspicious Beginnings |
| 17 | 6 Nov | Dhanteras |
| 18 | 7 Nov | Naraka Chaturdashi (Choti Diwali) |
| 19 | 8 Nov | Happy Diwali |
| 20 | 9 Nov | Govardhan Puja / Annakut |
| 21 | 10 Nov | Bhai Dooj |

## Project layout

```
index.html                  Page shell
sketch.js                   Scene, cards, films, and the date lock
diya.js                     Lamp drawing and lighting
scroll.js                   Top banner
style.css                   Layout and motion
contributors.html           Credits page
assets/diwali_days.json     Schedule and lamp layout (source of truth)
assets/diwali-days-data.js  Generated copy of that schedule
assets/diwali-experiences.js  What each lamp opens
assets/images/              Backgrounds, cards, and lamp art
assets/videos/              Card and bonus films
vendor/p5.min.js            Drawing library, served with the site
tools/                      Layout, sprite, and schedule helpers
docs/                       Notes for artwork, films, and audio
```

`tools/sync_diwali_days_js.py` is the helper you need for content edits. The other scripts in `tools/` and `scripts/` were used to place lamps, tune the mandala, and prepare artwork.

## Credits

The people named on the [contributors page](contributors.html) are Sim Tze Hin, Shahril Ibrahim, Amit Agrawal, and Syed Mahbub. Edit that HTML file to update the list.

The scene is drawn with [p5.js](https://p5js.org/), included at `vendor/p5.min.js`. Cinzel and Poppins are loaded from Google Fonts. The lamp-lighting sound is documented in [`docs/third-party-audio.md`](docs/third-party-audio.md).

## Publishing

GitHub Pages serves this repository from the root of `main`. The custom domain is set in [`CNAME`](CNAME).
