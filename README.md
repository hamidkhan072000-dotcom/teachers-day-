# Teacher's Day 2026 — St. Mary's Inter College

A single static page (`index.html`), no build step, no dependencies.

## Deploy to Vercel

**Option A — Vercel CLI**
```
npm i -g vercel        # once, if you don't have it
cd teachers-day-2026
vercel                 # first deploy (follow the prompts)
vercel --prod          # promote to production
```

**Option B — Git-connected project (recommended for ongoing updates)**
1. Push this folder to a GitHub/GitLab/Bitbucket repo.
2. In the Vercel dashboard: **Add New… → Project → Import** that repo.
3. Framework Preset: leave as **Other** (no `package.json` on purpose — there is
   nothing to build, so there's nothing that can fail to build).
   Build Command / Output Directory: leave blank/default.
4. Deploy. Every future push redeploys automatically.

**Option C — Drag and drop**
On the Vercel dashboard, you can also drag this whole folder onto the
"Add New… → Project" screen.

There is no `npm install`, no bundler, and no build command — Vercel serves
the files in this folder exactly as they are, which means there's nothing
that can fail to "build."

## Folder structure

```
teachers-day-2026/
├── index.html
├── vercel.json
└── assets/
    ├── teachers/   ← Principal & individual teacher portraits
    ├── moments/    ← classroom photos, Teacher's Day photos, the 2 group photos
    └── video/      ← the Teacher's Day compilation video + poster
```

All paths in the page start with `/assets/...` (root-relative). That only
works correctly if `assets/` sits at the **root** of what gets deployed —
which it does here, since `index.html` and `assets/` are siblings at the
project root. Don't nest this folder inside another folder before deploying,
or the `/assets/...` paths will 404.

File names are case-sensitive on Vercel's servers (unlike Windows/older
macOS). Match the casing used in `index.html` exactly — everything here is
lowercase-with-hyphens.

## Adding real content

All content lives in one place: the `window.SITE_DATA` object at the very
top of `index.html`. Nothing else needs to change.

| To do this...                        | Edit this field                          |
|---------------------------------------|-------------------------------------------|
| Add a teacher's photo / moment photo  | that teacher's `image` in `teachers[]`     |
| Add a student note                    | that teacher's `note` in `teachers[]`      |
| Change a subject                      | that teacher's `subject` in `teachers[]`   |
| Change a class                        | that teacher's `klass` in `teachers[]`     |
| Add the two group photos              | `groupPhotos[0].image` / `[1].image`       |
| Add the Principal's photo/message     | `principal.photo` / `principal.message`    |
| Add the video                         | drop the file in `assets/video/`, matching `video.src` (no code change needed if the filename matches) |

Drop the actual image/video files into the matching `assets/` subfolder using
the exact filenames referenced in `SITE_DATA`, then redeploy (or just push —
a Git-connected project redeploys automatically).

## Notes

- No external fonts, CDNs, or scripts are used — nothing else can fail to
  load at runtime due to network/CORS issues.
- Missing images/video fail gracefully to an on-page placeholder (this is
  intentional, so an empty `assets/` folder still deploys and browses fine
  before real photos are added).
- Comments/Feedback are stored in the visitor's own browser (`localStorage`)
  only — see the note in the Feedback section of the page itself.
