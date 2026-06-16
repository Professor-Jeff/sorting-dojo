# Sorting Dojo

An interactive drag-and-drop tool for physically performing sorting algorithms.
Built for **COMP 220 Algorithms & Data Structures** at College of Marin.

**Live:** https://professor-jeff.github.io/sorting-dojo/

---

## What it does

Students drag integer tiles to sort an array, demonstrating a sorting algorithm
step by step. The app records every move, analyzes the session against canonical
algorithm fingerprints, and generates a shareable URL the student submits as
their answer.

## Features

- **Seeded puzzles** — deterministic shuffles via seed; instructor and student
  share the same initial array
- **Algorithm declaration** — student declares which algorithm they are
  demonstrating before starting; locked after first move
- **Temp variable + Scratch area** — models the memory structures each algorithm
  uses; dimmed automatically when not relevant to the declared algorithm
- **Ghost slots** — copy-to-temp semantics made visible (source slot greys out)
- **Shift mode** — models insertion sort's shift-right operation
- **Counting sort counter model** — tally counters in scratch, two-phase
  (count → output), FIFO tag ordering for stability demonstration
- **Stability tags** — every card gets a letter superscript (A/B/C... per repeat
  occurrence of a value); with Duplicates on, reveals whether equal elements
  maintain their original relative order
- **Quick Sort model** — Hoare-style partition with first-element pivot; L/R
  sub-array boundaries auto-managed; pivot auto-locks green when swapped into
  place (or via **Next Partition** when already in place); auto-advances
  through sub-arrays the student already finished manually
- **Algorithm analysis** — 7 scorers (bubble, insertion, selection, merge,
  quick, counting, radix) with confidence scores penalised by sort correctness,
  effort relative to initial disorder, and — for inherently-stable algorithms —
  empirically broken stability
- **Worst Case / Already Sorted** puzzle generators per algorithm
- **Playback** — ⏮ ⏪ ▶ ⏩ ⏭ controls replay the session with animated arrows
- **Share URL** — full session encoded as base64 in `?session=` param; includes
  sort duration for academic honesty review
- **Undo + Mark Pass** — student annotations for pass boundaries

## Usage (instructor)

1. Assign a seed per question (e.g. "use seed 4521")
2. Students open the live URL, set the seed, declare their algorithm, sort
3. Student clicks **Share** and pastes the URL into the Canvas quiz answer box
4. Instructor opens the shared URL to replay and review the session

### URL parameters (instructor puzzle links)

Build a question-specific link instead of relying on students to set up the puzzle by hand.
Parameters mirror the top-level UI controls:

| Param | Mirrors | Example |
|---|---|---|
| `seed` | Seed field | `?seed=4521` |
| `n` | N field (2–20) | `?n=10` |
| `dup` (or `duplicates`) | Duplicates checkbox (`1`/`0`) | `?dup=1` |
| `range` | Range field — implies duplicates even without `dup=1` | `?range=5` |
| `tags` | Show stability tags checkbox (`1`/`0`) | `?tags=1` |
| `algo` | "I am demonstrating" — pre-declares and locks it | `?algo=quick` |
| `array` | Explicit array contents (comma-separated) — **supersedes** `seed`/`n`/`dup`/`range` | `?array=8,3,2,4,7,5,1,6` |
| `lock` | Locks the whole setup row (N, Duplicates, Range, Seed, New Puzzle, Reset) so students can't reconfigure the puzzle (`1`/`0`) | `?lock=1` |

`algo` accepts: `bubble`, `insertion`, `selection`, `merge`, `quick`, `counting`, `radix`.

Examples:
- `?seed=4521&algo=bubble` — seeded puzzle, Bubble Sort pre-declared
- `?n=10&dup=1&range=5&tags=1&algo=counting` — 10 elements, duplicates with range 5, tags visible, Counting Sort declared
- `?array=8,3,2,4,7,5,1,6&algo=quick` — exact array contents, Quick Sort declared (`seed`/`n`/`dup`/`range` are ignored if also present, since the array fixes them)
- `?seed=4521&algo=bubble&lock=1` — same as above, but the student can't change N/Duplicates/Range/Seed or click New Puzzle/Reset — use this for the link embedded in a quiz

Add `lock=1` for any link going into a quiz where the puzzle must stay exactly as assigned.
For casual practice (not a graded link), click the **Sorting Dojo** title to open a fresh,
unconfigured dojo in a new tab — it strips all URL params.

### Building a quiz link by hand (Capture Config button)

Rather than writing the query string yourself, just configure the puzzle in the UI (N,
Duplicates, Range, Seed or Worst Case/Already Sorted, declare an algorithm, set Show stability
tags) and click the small **🧰** button at the end of the setup row. It copies a `lock=1` URL
reproducing the *current setup* (not whatever sorting progress you've made) to your clipboard.
It uses `seed`/`n`/`dup`/`range` for a plain random puzzle, or falls back to `array=` with the
exact values if you used Worst Case / Already Sorted (those can't be reproduced from the seed
alone). The button is deliberately tiny and low-contrast — it's an instructor tool, not something
students need.

## Embedding in Canvas

Use a direct link (not an iframe) for quiz questions — students open the dojo
in a new tab, sort, then paste the share URL back as their answer. This avoids
Canvas's iframe sandbox restrictions and matches the workflow used for other
external tools (e.g. Programiz).

If embedding as a reference/demo: `<iframe src="https://professor-jeff.github.io/sorting-dojo/" style="width:100%;height:90vh;border:none;"></iframe>`

## Updating the live version

Edit `index.html` locally, then upload it to the
[GitHub repo](https://github.com/Professor-Jeff/sorting-dojo) via the web
interface. GitHub Pages redeploys within ~1 minute.

## Architecture

Single `index.html` — no build step, no dependencies, no server required.
Opens via `file://` or any static host.

See [DECISIONS.md](DECISIONS.md) for the full design decision log (D1–D36, U1–U6).
