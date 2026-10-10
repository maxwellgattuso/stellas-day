# Stella's Day

A short pixel-art game about one day in the life of Stella, a very good dog.

The site is just these files:

| File | What it is |
|---|---|
| `index.html` | The game |
| `outro.json` | Settings for the photo slideshow at the end |
| `photos/` | The photos and video clips the slideshow can use |

---

## Changing the end-of-game slideshow

Everything about the slideshow lives in **`outro.json`**. You can edit it right on GitHub:
open the file, click the **pencil** icon, make your change, then click **Commit changes**.
The site updates about a minute later (refresh the page; on desktop use Ctrl+F5).

### How many photos play

```json
"howMany": 18,
```

- Any number, like `10` or `25`.
- `"all"` (with quotes) plays every photo in the list (videos still follow `maxVideos` below).
- `0` skips the slideshow and goes straight to the goodbye card.

Each play draws a fresh random set, in the order of her day.

### Timing

```json
"secondsPerPhoto": 4.2,
"secondsPerVideo": 7,
"maxVideos": 3,
```

`maxVideos` caps how many video clips appear in one slideshow.

### Chapters (the order and the mix)

```json
"chapters": [
  { "name": "puppy", "share": 1, ... },
  { "name": "sun",   "share": 3, ... },
  { "name": "beach", "share": 5, ... },
  ...
]
```

Chapters play in the order listed. `share` is how much of the slideshow each chapter gets,
relative to the others. With the defaults, beach gets about 5 out of every 18 picks.
Set a share to `0` to leave a chapter out. You can add your own chapter by giving photos a
new chapter name; it plays after the listed ones.

### The photo list

Each photo is one line:

```json
{ "file": "2023-05-14_lying-in-the-backyard-grass_01.jpg", "chapter": "sun", "caption": "Warm spot. Mine." },
```

- **Change a caption:** edit the text in `"caption"`.
- **Move a photo to another chapter:** change `"chapter"`.
- **Always include a photo:** add `"pin": true`
  `{ "file": "...", "chapter": "beach", "caption": "...", "pin": true },`
- **Hide a photo without deleting it:** add `"skip": true`
- **Remove a photo:** delete its whole line.
- **No caption:** use `"caption": ""`

### Adding a new photo or video

1. In the repo, open the **`photos`** folder, then **Add file → Upload files**, and drop the picture in.
   JPG works best. Photos straight off a phone are fine, but smaller files load faster;
   around 1 MB or less is ideal. Short MP4 clips (under ~15 seconds) work too and play silently.
2. Open `outro.json` and add a line to the list with the exact file name:
   `{ "file": "my-new-photo.jpg", "chapter": "home", "caption": "New thought here." },`
3. Commit.

### Avoiding typos

`outro.json` is picky about punctuation:

- Every line in the list ends with a comma **except the last one**.
- Text goes in straight double quotes `"like this"`.
- `true`, `false` and numbers don't get quotes.

If something's off, the game still works but skips the slideshow and goes straight to the
goodbye card. To see exactly what's wrong, open the game and press the **`** key (top-left of
the keyboard) to show the debug bar; the first line tells you whether `outro.json` loaded and,
if not, roughly where the typo is.

---

## Debug bar

Press **`** (backtick) to show or hide it. It lets you jump to any chapter, preview the
slideshow, and check that `outro.json` loaded. It stays hidden for everyone else.

---

## Shared memories and the play count

The end screen lets anyone share a memory of Stella, and shows how many people have
"spent a day with Stella" (also on the title screen). These live in a free
[Supabase](https://supabase.com/dashboard) project called **stellas-day**.

- **Nothing shows up until you approve it.** New memories wait in the `memories` table with
  `approved` set to `false`. The person who wrote one still sees it on their own screen.
- **To approve:** open the Supabase dashboard → stellas-day → **Table Editor** → `memories`,
  and tick `approved` on the ones you want shown. Un-tick to hide one again, or delete the row.
- **The count** goes up once per browser, the first time someone starts the game.
- **Built-in limits:** 300 characters, no links, a few memories per person per day.
- **Keeping it awake:** free Supabase projects pause after a quiet week. The
  `Keep memories awake` workflow (in the repo's Actions tab) checks in once a day so that
  doesn't happen. If the project ever pauses, the game still works; the memories and count
  just hide until you click **Resume project** in the dashboard.
