# rywmcc.github.io

Personal site, served by GitHub Pages at **https://rywmcc.github.io**.

Plain static HTML and CSS — no build step, no dependencies. Edit a file, commit, push;
Pages redeploys in a minute or so.

## Layout

```
index.html              Home
countdown/index.html    Countdown project page (description, controls, screenshots)
countdown/play/         The Godot 4 web export — the playable build
resume/index.html       Résumé as HTML
assets/*.pdf            Résumé PDF (kept in sync by hand — replace both when it changes)
assets/css/site.css     All styling
assets/img/             Screenshots and favicon
.nojekyll               Serve files as-is; skip Jekyll processing
```

## Updating the playable build

The game lives in a separate private repo. To publish a new build, export the **Web**
preset from the Godot editor (or headless, below), then copy the output over
`countdown/play/` and commit.

```powershell
& "$env:USERPROFILE\Downloads\Godot_v4.7.1-stable_win64.exe\Godot_v4.7.1-stable_win64_console.exe" `
    --headless --path "$env:USERPROFILE\gamejam-july-2026" `
    --export-release "Web" "$env:USERPROFILE\rywmcc.github.io\countdown\play\index.html"
```

Two things that matter for the build to run on Pages:

- **Keep `thread_support` off** in the Web export preset. Threads require the
  `Cross-Origin-Opener-Policy` / `Cross-Origin-Embedder-Policy` headers, and GitHub Pages
  cannot set custom headers. Without threads the build needs no special headers at all.
- **Never put the build behind Git LFS.** Pages serves LFS pointer files as-is rather than
  resolving them, so the game would fail to load. The `.wasm` is ~39 MB, well under
  GitHub's 100 MB per-file limit, so committing it directly is fine.

## Local preview

```powershell
python -m http.server 8000
```

Then open <http://localhost:8000>. Opening `index.html` directly from the filesystem will
not work for the game — the WebAssembly build has to be fetched over HTTP.
