# Kristian's Portfolio — standalone website

Everything in this folder is the website. Upload the whole folder as it is.

```
index.html      the page (your content lives inside it, see "Editing your content")
car.glb         the black Octavia 3D model
vendor/         3D engine files (three.js r128 + model loader)
airports.json   airport locations for the flight map (OurAirports, public domain)
land.json       the dotted world map (Natural Earth, public domain)
_redirects      price-feed setup for Netlify
vercel.json     price-feed setup for Vercel
```

## Put it online (free)

### Option A: Netlify (easiest, recommended)
1. Go to https://app.netlify.com/drop and sign up or log in.
2. Unzip this folder, then drag the whole folder onto the page.
3. After a few seconds you get a live address like `something.netlify.app`.
4. Optional: in Site settings → Domain management you can rename it or connect your own domain.

The `_redirects` file makes Netlify pass price requests through your own site,
so the live prices work reliably.

### Option B: Vercel
1. Sign up at https://vercel.com and choose "Add New → Project".
2. Upload the folder (or push it to a GitHub repository and import it).
3. Framework preset: "Other". No build command needed.

`vercel.json` does the same price-feed setup as on Netlify.

### Option C: GitHub Pages
Works too, but it has no price-feed setup, so the page asks Crypto.com directly
and falls back to CoinGecko if that is blocked. Put the files in a repository and
turn on Settings → Pages.

## How live prices work
Every 30 seconds the page asks the Crypto.com Exchange for each coin's USD price
(trend lines every 5 minutes). It tries, in order:
1. your own site's `/api/cdc/` path (Netlify / Vercel),
2. Crypto.com directly,
3. CoinGecko (prices and 24h change only, no trend lines).

If nothing answers, it shows the prices saved in the page and says so in the
status line under the heading. The status line also says which source is live.

## Editing your content
Open `index.html` in any text editor and search for:

```
<script type="application/json" id="state">
```

That block holds everything the page shows: name, role, intro, about text,
email, skills, links, the contact heading and its hidden text, work experience,
and your coin holdings. Change the text between the quotes, keep the commas and
brackets as they are, save, and upload again.

Useful switches inside that block:
- `"showAmounts": false` hides coin amounts and the total value (visitors see
  share % and 24h change). Set it to `true` to show them.
- Holdings are listed as `{"sym":"BTC","amount":0.5,"cost":null}`. Use the
  ticker as listed on Crypto.com. Put your average buy price in `cost` to show
  profit and loss.

Your flights are in the same block under `"flights"`. To update them, the
easiest way is to upload a new Flighty export in the claude.ai version
(Edit → Flights) and ask Claude for a fresh folder. Set `"showFlights": false`
to hide the flight map.

The Edit button from the claude.ai version is not available here. You can also
ask Claude to make changes and send you a new folder.

## Trying it on your own computer
Opening `index.html` by double-clicking will not load the car, because browsers
block local files. Instead open a terminal in this folder and run:

```
python3 -m http.server 8000
```

then visit http://localhost:8000
