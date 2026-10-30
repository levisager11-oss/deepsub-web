# DEEPSUB web deployment

This separate deploy repo contains the generated Godot Web export for Vercel,
alongside the itch release. Vercel serves the repo root as static files; there
is no framework, install, or build step.

Generate it from the game source repo (Bash, Python 3, Git, Godot and the preset's
local Web templates required):

```sh
DEPLOY_DIR=/absolute/path/to/deepsub-web ./tools/publish_web.sh
# Optional: GODOT_BIN=/path/to/godot
```

The script imports and exports the `Web` release, checks the 95 MB per-file
limit, replaces the deploy clone's contents except `.git`, and records the
project version and source commit in `version.txt`. It prints Git commands for
you to review and run; it never commits or pushes. Local uncommitted source
changes are included in the export and marked in `version.txt`.

Do not edit generated files here by hand. Change the game source or
`tools/web/vercel/`, then regenerate. Binary filenames stay as Godot exports
them: `executable` also controls companion file lookups, so this publisher
avoids renaming or rewriting engine configuration. WASM/PCK use a one-hour
cache with revalidation, never immutable caching; HTML/JS always revalidate.
