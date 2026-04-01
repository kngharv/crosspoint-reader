# Vertical Text Layout (Traditional Chinese EPUB)

## Status

This branch contains an experimental but working implementation of vertical EPUB text flow for Traditional Chinese reading on the X4.

Current checkpoint commit:
- `8ef2167` — add text flow toggle and cache-safe vertical layout plumbing

## What this branch adds

### 1. In-reader Text Flow toggle
A new **Text Flow** option is available in the reader menu while actively reading.

Supported values:
- **Horizontal**
- **Vertical**

Notes:
- This is **not** in `Settings -> Reader`
- It currently lives in the **reader menu** opened while reading
- It works in all 4 screen orientations

### 2. Cache-safe layout switching
Text flow is now part of EPUB section cache compatibility.

That means:
- switching between horizontal and vertical flow triggers correct re-layout
- stale cached page layouts are less likely to be reused incorrectly
- reading position is preserved across flow-triggered re-layout

### 3. Stray dash fix
A bug was fixed where a mysterious dash could appear at the end of a row/column even when reader hyphenation was turned off.

Observed behavior before the fix:
- dash appeared at line end in horizontal mode
- dash appeared at column end in vertical mode
- this was especially visible with Chinese text

Current behavior:
- the stray dash is gone
- fallback word splitting still works
- visible hyphen insertion now depends on hyphenation actually being enabled

## Current known-good behavior

Verified on this branch:
- vertical text flow works
- horizontal/vertical toggle works
- toggle works in all 4 orientations
- reading position remains approximately preserved after re-layout
- stray end-of-line / end-of-column dash bug is fixed

## Current known limitations

This branch is still experimental.

Known issues not yet fully solved:
1. **Vertical punctuation / full-width punctuation**
   - punctuation rendering is still incomplete
   - some marks may render in the wrong form or wrong position

2. **Blank-space issue associated with vertical/full-width punctuation**
   - not addressed yet in this checkpoint

3. **Occasional overflow into the bottom status bar**
   - still happens occasionally
   - not yet investigated fully

4. **Mostly text-first vertical layout PoC**
   - current work is focused on text rendering and pagination
   - image-heavy EPUB behavior may still need additional work

## Implementation summary

### Text flow control
The reader now uses a persisted `textFlow` setting with values:
- `HORIZONTAL`
- `VERTICAL`

The actual layout path is selected in the reader activity and passed into section caching/layout code.

### Section cache compatibility
Section cache compatibility now includes vertical layout state.

Implication:
- horizontal and vertical layouts are treated as distinct cache variants
- cache rebuild happens when needed instead of silently reusing an incompatible layout

### Reader menu plumbing
The reader menu now returns:
- orientation
- page-turn option
- text flow

The reader applies the chosen flow mode immediately and re-renders accordingly.

### Dash bug root cause
The stray dash issue came from fallback splitting logic for oversized words/tokens.

Root cause:
- fallback split logic could still append a visible hyphen even when hyphenation was disabled

Fix:
- fallback splitting is still allowed
- visible hyphen insertion is now suppressed unless `hyphenationEnabled` is actually on

## Files touched in this milestone

Main files involved in this checkpoint include:

- `lib/Epub/Epub/ParsedText.cpp`
- `lib/Epub/Epub/ParsedText.h`
- `lib/Epub/Epub/Section.cpp`
- `lib/Epub/Epub/Section.h`
- `lib/Epub/Epub/blocks/BlockStyle.h`
- `lib/Epub/Epub/blocks/TextBlock.cpp`
- `lib/Epub/Epub/blocks/TextBlock.h`
- `lib/Epub/Epub/parsers/ChapterHtmlSlimParser.cpp`
- `lib/Epub/Epub/parsers/ChapterHtmlSlimParser.h`
- `src/CrossPointSettings.h`
- `src/JsonSettingsIO.cpp`
- `src/activities/ActivityResult.h`
- `src/activities/reader/EpubReaderActivity.cpp`
- `src/activities/reader/EpubReaderActivity.h`
- `src/activities/reader/EpubReaderMenuActivity.cpp`
- `src/activities/reader/EpubReaderMenuActivity.h`

## Recommended next work items

Priority order:

1. fix vertical/full-width punctuation rendering
2. investigate blank-space issue associated with vertical punctuation
3. investigate occasional bottom overflow into the status bar
4. decide later whether Text Flow should also be surfaced in `Settings -> Reader`

## Branch note

This branch is intended as a focused working branch for vertical Traditional Chinese EPUB support.

It is not yet a finished upstream-ready implementation, but it now has:
- a usable text flow toggle
- safe cache behavior for flow switching
- one major wrap-related bug fixed

