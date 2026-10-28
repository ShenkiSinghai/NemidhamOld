# Nemidham mandir website — content guide

Shri 1008 Neminath Digambar Jain Mandir, Nemidham. This file is about one thing only: **what's on the site, and how to change, add, remove or hide it.**

**The one file you'll use almost every time: `js/data.js`.** Everything on the site — address, timings, announcements, karyakram, team, daan details, gallery, all of it — lives there, written in plain commented blocks. Open it on GitHub, click the pencil icon top-right, make your change, scroll down, hit "Commit changes." The live site has it within a minute or two. Nearly everything below is "go find this block in `data.js` and edit it."

A few universal rules, true almost everywhere in this file:
- **`hidden: true`** on any block hides it from the site without deleting it — the safest way to "remove" something you might want back later.
- **`pinned: true`** on a yojana, a karyakram or an announcement puts it on the **Home page** as well (see "How to pin" just below). Without it, the item shows only on its own page.
- Content is written as **`{ hi: "...", en: "..." }`** — fill both the Hindi and English versions when you add something new. Leaving one blank means that language shows nothing there.
- To add a new entry in a list (a new yojana, karyakram, team member…), copy one whole existing block, paste it right below, and edit the copy. To remove one, delete its block entirely (or use `hidden: true` if you're not sure yet).

---

## Home page (`index.html`)

Built automatically from other sections — there's nothing to edit specifically "on the home page." It pulls: today's timings (from `SITE.timings`), the **pinned** yojanayen, the **pinned** announcements, the next 3 **pinned** upcoming karyakram, and the photo slideshow.

### How to pin (what shows on Home)

Yojanayen, karyakram and announcements work the same way: **only items with `pinned: true` appear on the Home page.** Everything else still appears on its own full page (Yojanayen / Karyakram / Announcements).

- **To pin:** open `js/data.js`, find the item's `{ ... }` block and add the line `pinned: true,` inside it (next to its other lines).
- **To unpin:** change it to `pinned: false,` or delete that line. The item stays on its own page.
- If *nothing* in a group is pinned, that whole section (heading and "see all" link too) is simply left out of the Home page. A karyakram that is pinned drops off Home by itself once its date has passed.
- Right now all 5 yojanayen, the "Welcome" announcement and the 9 November karyakram are pinned. Unpin the ones you don't want on Home.

## Announcements (`announcements.html`, highlights on Home)

`window.ANNOUNCEMENTS` — an array of `{ date, pinned, title, text }`. Every announcement (pinned or not) always shows on the full **Announcements** page (in the menu under Karyakram). Only the **pinned** ones show on Home, so Home doesn't fill up with routine notices — set `pinned: true` on whichever one or two you want visitors to see right away. If nothing is pinned, the announcements section is left out of Home. `date` is optional. Add a new announcement by copying a block; remove by deleting it, or `hidden: true` to park it (hidden from both places).

## Karyakram (`karyakram.html`)

`window.EVENTS` — each one is `{ date, endDate, time, place, title, type, text, pinned }` (`pinned: true` also shows an upcoming karyakram on Home — see "How to pin"). `endDate` is only needed for multi-day events. New karyakram: copy a block, change date/time/title. Old ones don't need manual cleanup — anything more than 30 days past its date quietly drops off the "past" tab by itself (change `SITE.karyakramPastDays` in `data.js` for a longer or shorter window; `-1` keeps them all). To remove one immediately: `hidden: true`, or delete the block.

## Jain Calendar (`calendar.html`)

`window.CALENDAR` — same shape as karyakram (`date`, `type`, `title`, `note`), for kalyanak/parv entries. Same add/remove/hide rules.

## Yojanayein (`yojana.html`)

`window.YOJANA` — each entry: `title`, `short` (one-line, used on Home), `status` (`"active"`, `"soon"`, etc. — controls the little pill label), `details` (a list of bullet points), `summary` (longer text for the Yojanayen page), `target` / `raised` (optional amounts), and an optional `link: { href, text }` button. `pinned: true` also shows it on the Home page (see "How to pin"); `hidden: true` removes it everywhere.

## Gallery (`gallery.html`)

`window.GALLERY = { photos: [...], videos: [...] }`. Both are lists of "occasion albums" (Diwali, Vedi Pratishtha, etc.).

**Photo albums**, two ways to list the photos in one:
- Name them yourself: `files: ["1.jpg", "2.jpg"]` inside the album, pointing at files you've uploaded to `images/gallery/<album-name>/`. Always works, no surprises.
- Skip `files` and the site reads the folder on its own, picking up whatever's there (needs `SITE.github.user`/`repo` filled in, which they already are).

Don't want to hand-write a new album block? Open `gallery-helper.html`, fill in the form, and paste the code it gives you into `GALLERY` in `data.js`.

**Video albums**: each item is `{ youtube: "the link", title: {...} }` — works for a single video, a playlist, or an entire channel link, and plays right on the page, no visitor ever leaves the site. For a small clip you've uploaded yourself instead of YouTube: `{ file: "clip.mp4", title: {...} }`.

**Removing or hiding**: `hidden: true` on any album or any single photo/video item.

### The Home page photo strip

A horizontally scrolling strip of photos near the top of Home, controlled by `SITE.slideshow`:
- `enabled: false` turns the whole strip off.
- `source: "all"` pulls from every visible photo album; set it to one album's id (e.g. `"vedi-pratishtha"`) to only ever show that album.
- `shuffle: false` (the default) keeps the same order every time, so it doesn't look random to a returning visitor; set `true` if you'd rather it mix photos up differently on each visit.
- `count` — how many photos go into the strip. `seconds` — how often it auto-scrolls. Visitors can also drag it or use the arrow buttons.

Right now `images/gallery/vedi-pratishtha/` has 5 placeholder photos (`sample-1.jpg`–`sample-5.jpg`) just so the strip has something to show. To put your real photos in: upload them into that same folder, then in `data.js` find the `vedi-pratishtha` album and either swap the `sample-*.jpg` names in its `files: [...]` line for your real filenames, or delete that `files:` line entirely so the folder is read automatically. Delete the old sample files once you're done with them.

## Team (`team.html`)

`window.TEAM` — a list of groups (Trustees, Working committee, Pujari…), each with a `group` name and a `members` list. Each member: `{ name, role, phone, photo }`. `phone` shown as a tap-to-call link; leave `""` to hide it. `photo` points at a file in `images/team/`; leave `""` and their initials show instead. Add a person by copying a member line inside the right group; add a whole new group by copying a `{ group: {...}, members: [...] }` block. `hidden: true` on a group (like Trustees currently) hides that whole group until you're ready to show it.

## About page (`about.html`)

`window.ABOUT` — a list of sections, each with an `id`, a `title`, `paragraphs` (as many as you like), and an optional `list` for bullet points (used for the "visiting etiquette" rules). Add a new section the same way as everywhere else: copy a block, give it a unique `id`, write your own title/paragraphs.

## How to reach (`visit.html`)

`window.HOWTO` — a list of `{ title, text }` blocks (By road, By rail, By air, Parking…). The Parking block is currently `hidden: true`; remove that line whenever it's ready to show. Address, phone, the Google Map and the "open in Maps" link are all set separately in `SITE` (see Links & Map below).

## Daan (`daan.html`)

Several pieces, all inside or near `window.DAAN`:

- **`DAAN.enabled` / `intro`** — the page's on/off switch and intro line.
- **`upiId`** and **`qrImage`** — the UPI ID shown with a Copy button, and the path to the QR code picture (`images/upi-qr.png`).
- **`bank`** — a list of `{ label, value, copy }` rows for the bank-transfer card; `copy: true` adds a Copy button to that row (used for account number and IFSC).
- **`inPerson`** — the text for the "at the mandir" card.
- Leave any of the three (`upiId`/`qrImage`, `bank`, `inPerson`) blank/empty and that whole card just doesn't show.

- **`DAAN.disclaimer`** — the yellow caution line just above "Niyamit daan aur seva" (by default: confirm with a mandir karyakarta before paying, and check the account holder's name). Edit the Hindi/English text, or make both `""` to hide it.

- **`window.RECURRING`** — the "Niyamit daan aur seva" cards (Masik Dravya Daan, Saptahik/Masik Shantidhara, ...). **You can add as many cards as you like, remove them, or hide them — no coding.** Each card is one block:
  ```
  {
    title: { hi: "...", en: "..." },   // big heading of the card
    when:  { hi: "हर महीने", en: "Every month" },   // small tag on top ("Every week", "Every Purnima", ...)
    text:  { hi: "...", en: "..." },   // the main description
    how:   { hi: "...", en: "..." }    // small grey line at the bottom (how to join / who to contact)
  },
  ```
  - **Add a card:** copy one whole `{ ... },` block, paste it after the last card (make sure there is a comma between blocks), and change the text. New cards appear in the same order as in the file, three in a row on a computer; all cards in a row are the same height.
  - **Remove a card:** delete its whole `{ ... },` block.
  - **Hide a card for now:** add the line `hidden: true,` inside it; delete that line to show it again.
  - **Spare cards:** the file already has three hidden spare cards named "Daan 1", "Daan 2", "Daan 3". To use one, delete its `hidden: true,` line and replace the Hindi/English text. Cards you don't use can stay hidden or be deleted.

- **`window.PUNYARJAK`** — the Shantidhara punyarjak table: one row per name per date, `{ date, kind, name }` where `kind` is `"saptahik"` or `"masik"`. More than one person can share a date — just add one row per person. By default the table only shows the **current month** (`SITE.punyarjakCurrentMonthOnly: true`) so it doesn't turn into one long scroll of old names; set that to `false` if you'd rather every month stayed visible.

- **`window.DRAVYA`** — the Masik Dravya Daan donor table, same idea: `{ date, name }` rows. Empty list, and the table simply doesn't appear.

- **`window.TRUST`** — the trust/80G details card. `enabled: false` hides the whole section. Each `items` row with a blank `value` just doesn't show (so it's fine to leave most of these blank until you actually have the registration numbers).

## Links, map, top banner, logo

- **`window.LINKS`** — WhatsApp group, Facebook, Instagram, YouTube. The social icons in the footer always show, even for ones left blank — they just sit greyed out until a real link is added.
- **`SITE.mapEmbed`** — paste Google Maps' own "Share → Embed a map" code here (just the `src="..."` part). **`SITE.mapLink`** is the plain "open in Google Maps" link for the directions button. **`SITE.phone`** is the contact number shown around the site.
- **`window.BANNER`** — the thin gold strip across the very top of every page. Leave its text blank to hide the strip entirely.
- **`SITE.logo`** — points at `images/mandir-logo.png`, used in the header and the browser tab icon. Swap the image file (same name, or change the path here) to change it. The header shows the picture exactly as it is, with no cropping, so use a square image where the round badge (including its outer ring) fills the picture with only a thin margin and transparent corners (a PNG); a logo with a lot of empty space around it will look small. The browser-tab icons (`images/favicon-32.png`, `images/apple-touch-icon.png`, `images/icon-512.png`) are separate files — replace those too if the logo changes.
- **`SITE.name`** / **`SITE.place`** — the mandir's name as it appears in the header and throughout the site. If the name ever changes, also update the `<title>` line at the top of each `.html` file — those are separate from `data.js` on purpose (search engines and browser tabs read them directly).

## Languages

Hindi opens first; there's an English toggle in the menu. Every piece of real content in `data.js` is the `{ hi: "...", en: "..." }` pair described above. Menu words, button labels and other fixed interface text (not content) live separately in `js/i18n.js`, in the same `hi`/`en` shape.

## Light/dark mode and accessibility

There's a small "sliders" shaped Appearance button in the header (next to the English/हिंदी button). It opens a little menu with:
- **Theme** — Light or Dark. Opens in Light by default; whichever a visitor picks is remembered on their own device.
- **Contrast** — Normal or Increase contrast, for visitors who want stronger borders and higher-contrast text (similar to the same setting on GitHub).

Nothing to configure here — it's there for every visitor automatically.

## Visitor count and analytics

Two separate, unrelated numbers:
- **`SITE.analyticsId`** — your Google Analytics ID. Private, detailed stats (where visitors come from, which pages, how long they stay) visible only to you at analytics.google.com.
- **The number in the footer** — a public running total via GoatCounter. Sign up free at goatcounter.com, turn on "Allow adding visitor counts on your website" in its settings, and put your site code into `SITE.goatcounter`. It's a genuine count of real visits, not a placeholder — on a brand-new site, a small number is exactly what you'd expect; it climbs as more people actually visit. It can take a few hours to reflect the latest visit, since GoatCounter caches the number.

## The small "Website by" credit

Bottom-right of the footer. Change the name or link through `SITE.credit`, or set `credit: null` to remove it entirely.

## The two helper/tool pages

- **`check.html`** — open this whenever the site looks wrong, blank, or half-missing after an edit. It reads through `data.js` and tells you in plain words what's broken or still marked SAMPLE.
- **`gallery-helper.html`** — fills in the punctuation-correct code for a new gallery album, so you can paste it into `data.js` without typing it by hand.

Neither is in the site menu or searchable by visitors on purpose.

## Before you actually publish

Go through `js/data.js` and replace everything still marked **SAMPLE** — team names, placeholder karyakram, and especially anything in the daan section. Get the bank/UPI details checked by the trustees before publishing; that's the one place where a typo actually costs someone money.
