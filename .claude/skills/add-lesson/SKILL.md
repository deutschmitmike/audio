---
name: add-lesson
description: Add a new "vier Stimmen" audio lesson to the deutschmitmike/audio repo. Takes 4 MP3 files (one per speaker), drops them into a new lesson folder under the right level (b1/b2/...), and generates a styled HTML page matching the existing lesson template. Use whenever the user says "add lesson", "new lesson", "add audio", or hands over a set of MP3 files to publish.
---

# add-lesson

Add a new lesson to this repo. A lesson is **one HTML page + one folder of 4 MP3s**, following the established convention.

## Repo convention

```
{level}/{slug}.html              ← the page students open
{level}/{slug}/1-{name1}.mp3     ← four MP3s, numbered 1-4
{level}/{slug}/2-{name2}.mp3
{level}/{slug}/3-{name3}.mp3
{level}/{slug}/4-{name4}.mp3
{level}/{slug}/.gitkeep
```

- `level` is the CEFR level folder (currently `b2`; create `b1`, `c1`, etc. as needed)
- `slug` is lowercase, no spaces, German topic word (e.g. `pflege`, `mehrsprachigkeit`)
- Each speaker gets a numbered MP3 named `{n}-{firstname-lowercase}.mp3` (hyphenate compound names like `wei-hsuan`)

## HTML template

Each lesson page has:
- `<title>{Topic} — Vier Stimmen | Deutsch mit Mike</title>`
- An `<h1>` with the topic and a German subtitle
- Four `.person` cards with a fixed color rotation (blue → green → pink → purple):
  - Person 1: `#2E5C8A` (blue)
  - Person 2: `#3A7D44` (green)
  - Person 3: `#B53471` (pink)
  - Person 4: `#6B4FA0` (purple)
- The CSS class on each card is the speaker's firstname lowercased, no hyphens (so `wei-hsuan` becomes class `weihsuan`)
- Audio: `<audio controls preload="none">` with `<source src="{slug}/{n}-{name}.mp3" type="audio/mpeg">`

Copy `b2/pflege.html` as the canonical template — it has all 4 colors wired up correctly.

## Steps

1. **Gather inputs** (ask the user if not given):
   - Which level (`b2` default)
   - Lesson slug (lowercase German word)
   - The 4 source MP3 paths and the speaker name + order for each
   - Optional: page title (defaults to capitalized slug) and subtitle (defaults to "Vier Personen sprechen über ihre Situation" — tailor it to the topic when sensible, e.g. "ihre Sprachen" for a multilingualism lesson)

2. **Create the lesson folder**: `mkdir -p {level}/{slug}` and `touch {level}/{slug}/.gitkeep`

3. **Copy and rename the MP3s** into the folder as `{n}-{firstname}.mp3`

4. **Generate the HTML** at `{level}/{slug}.html` by copying `{level}/pflege.html` (or any existing lesson) and substituting the topic, subtitle, and four speaker blocks. Keep the four color classes in the same order.

5. **Commit and push**:
   ```
   git add {level}/{slug}.html {level}/{slug}/
   git commit -m "Add {slug} lesson"
   git push
   ```

6. **Report the live URL** to the user: `https://deutschmitmike.github.io/audio/{level}/{slug}.html` (GitHub Pages auto-deploys in ~30 seconds)

## Notes

- The site is GitHub Pages serving from the `main` branch of `deutschmitmike/audio`. No build step.
- Don't add MP3s above ~5MB without checking — GitHub's soft limit is 50MB per file but Pages can be slow with large files.
- If the user asks for a different layout (more than 4 speakers, video, transcripts), don't force this template — fall back to writing custom HTML.
