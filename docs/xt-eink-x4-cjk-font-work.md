# XT EINK X4 Chinese Font Work

This document describes the Chinese font work in this fork for XT EINK X4.

## Goals

- Improve Traditional Chinese reading support
- Improve Chinese filename and UI rendering
- Keep runtime memory usage stable on the device
- Preserve a practical path for future vertical Traditional Chinese layout work

## Summary of changes

This fork adds a stable Chinese font setup for XT EINK X4 on the CrossPoint `1.1.1` version line.

### Reader font

The current reader font is based on **Source Han Serif TC Regular**.

Why:
- better reading appearance for long-form Traditional Chinese text
- more literary serif style than earlier sans-serif experiments
- stable in the current grouped/compressed font pipeline

### UI font

The current UI font is based on **Chiron Hei HK Text-R**.

Why:
- clearer small-size appearance for filenames and interface text
- better suited than reusing the reader font for UI elements
- keeps UI and reader typography separated

## Key technical decision: separate UI font from reader font

A major lesson from this work is that UI font and reader font should be treated separately.

- UI text on a small e-ink display benefits from a compact sans-style font
- reading text can use a different font optimized for long-form reading comfort

## Key technical fix: safer CJK grouping

The original Chinese EPUB crash was not an EPUB parsing problem.
The main issue was runtime font bitmap decompression allocation behavior.

The successful fix was to make CJK grouping much finer in:

- `lib/EpdFont/scripts/fontconvert.py`

The current working approach uses smaller grouped ranges for CJK, which reduces large decompression allocations during rendering.

## Current stable outcome

This snapshot currently provides:

- stable Chinese EPUB rendering
- improved Traditional Chinese reading appearance
- working Chinese filename and UI rendering
- coverage for many common Simplified Chinese glyphs

## Current limitations

This snapshot does not yet provide:

- dedicated Simplified-Chinese-specific font tuning
- vertical Traditional Chinese layout
- full typography work for vertical punctuation forms

## Files primarily involved

- `lib/EpdFont/scripts/fontconvert.py`
- `lib/EpdFont/builtinFonts/all.h`
- `src/main.cpp`
- `platformio.ini`
- `lib/EpdFont/builtinFonts/chironheihk_textr_ui_10_regular.h`
- `lib/EpdFont/builtinFonts/sourcehanseriftc_regular_grouped_14_regular.h`

## Notes for future work

Planned future work may include:

- vertical layout for Traditional Chinese ebooks
- calibre conversion guidance for Traditional Chinese ebooks
- further Simplified Chinese font evaluation
- release assets for firmware downloads
