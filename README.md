# Concertina Fingering

**Find the optimal fingering for any tune on a 30-button Anglo C/G concertina.**

Paste a tune in ABC notation, click Analyse, and the tool tells you which button to press for each note — and whether to push or pull the bellows.

→ **[Try it online](https://jeroenrijsdijk.github.io/concertina/concertina.html)**

---

## What it does

An Anglo concertina is bisonoric: each button produces a different note on push and pull. Many notes can be played on more than one button, so for a full tune there are often thousands of possible fingering combinations.

This tool searches through all combinations and picks the route that is easiest to play, minimising awkward jumps, finger hops, and unnecessary bellows reversals.

Results are shown as:
- a **table** — hand, row, direction (push/pull), and button number for each note
- a **button diagram** — all 60 buttons visualised per note, colour-coded push (blue) / pull (red)
- a **downloadable PNG** — the same diagram laid out on A4, ready to print

## Files

| File | Description |
|---|---|
| `concertina.html` | The web tool — self-contained, no dependencies |
| `help.html` | Help page and manual |
| `concertina.rb` | Original Ruby implementation with PNG export via `chunky_png` |

## Using the web tool

Just open `concertina.html` in a browser. No installation, no server needed.

The Kesh Jig is pre-loaded as an example — click **Analyse** straight away.

To use your own tune, paste any ABC notation into the text area. Thousands of tunes in ABC format are available at [thesession.org](https://thesession.org) and [abcnotation.com](https://abcnotation.com).

## Using the Ruby script

Requires the `chunky_png` gem for PNG output.

```bash
gem install chunky_png
ruby concertina.rb mytune.abc
ruby concertina.rb mytune.abc ./output
```

Output: a printed fingering table in the terminal and one or more A4 PNG files named after the tune.

## How the algorithm works

The search is a **beam search**:

1. For each note in the tune, all buttons that produce that note are looked up
2. All combinations are built step by step
3. Each transition between two buttons is scored (same hand, push-pull balance, preferred rows, finger hops)
4. When routes exceed 4,096, the worst-scoring ones are pruned
5. The route with the lowest total score is returned

Scoring favours:
- Staying on the same hand
- Push-pull alternation on the same button (natural bellows reversal)
- The c and g rows over the t row
- Buttons 1 and 2 over outer buttons

## Button layout

This tool is tuned for a standard **30-button C/G Anglo concertina**. The full note layout is documented in `help.html`. If your instrument differs, the `KEYS_DATA` object in `concertina.html` (and `KEYS_DATA` constant in `concertina.rb`) can be adapted.

A **Download template** button exports the current layout as JSON. Edit it and upload it with **Upload layout** to use your own instrument. The active layout name is shown in the UI.

## ABC notation

The tool reads standard ABC headers:

```
X:1
T:The Kesh Jig
R:jig
M:6/8
L:1/8
K:Gmaj
|:GAG GAB|ABA ABd|egd edB|dBA ABd:|
```

Key signatures are handled automatically. Explicit accidentals (`^c`, `_B`) are not yet supported.

## Known limitations

- 30-button C/G Anglo only (adaptable via `KEYS_DATA`)
- Notes outside the instrument's range are silently skipped
- Explicit accidentals in the note line are not yet parsed
- Chords are not supported

## Roadmap

See [`todo.md`](todo.md) for the current wish list.

## History

Originally written in Ruby in 2015–2018. Refactored and ported to a web tool in 2026, with a lot of help from Claude AI.

## License

MIT License — Copyright (c) 2024 jeroenrijsdijk

