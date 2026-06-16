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

See [DECISIONS.md](DECISIONS.md) for the full design decision log (D1–D33, U1–U6).
