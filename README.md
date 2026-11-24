# site-generator-speed

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/yang0128-max/site-generator-speed/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/yang0128-max/site-generator-speed/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A fast, dependency-free drop-in replacement for makefun-site 1.0.0. `solve.js` exports `run(input)`, which returns exactly the output tree makefun-site's `build` returns for a site — every page's `index.html`, the index and tag pages, `build-report.txt` and each static file, byte for byte — with the reference's own front matter, template dialect, Markdown subset, slug rule and report format.

The reference escapes HTML one character at a time (a regex replace per character), walks Markdown text character by character, re-tests every block regex against every line, splits each whole source to find its front matter, and re-renders every include on every page. This implementation keeps the semantics and removes the waste: HTML escaping short-circuits strings with nothing to escape; inline Markdown is scanned by a regex over just the characters that can change the output, with plain text taken as single slices; block lexing dispatches on a line's first character; front matter is found by scanning for the closing line; includes are expanded once per build (to the same depth cap the renderer enforces) so rendering is one flat walk that appends into a single parts array; and each post's card — including its reading-time word count — is built once. Non-ASCII slugs and text take the reference's own algorithms unchanged. On the competition's practice set (3 sites, 200–600 sources each) it runs about 3x faster than the reference in-process, verified byte-identical on the practice set, on 120 generated sites from the competition's own generator, and on a hand-written edge-case suite (include depth caps, broken templates, CRLF sources, reserved directories, shadowed static files).

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=<dir holding cases.json> MAKEFUN_OUTPUT_DIR=out node main.ts
```

Needs Node 22.18 or later (it runs the TypeScript driver as-is). It reads every case from `$MAKEFUN_INPUT_DIR/cases.json` (default `/input`), answers each with `run`, and writes `$MAKEFUN_OUTPUT_DIR/answers.json` (default `/output`).

---

<sub>Entered on [make.fun](https://make.fun/?ref=c5eeqz3e&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
