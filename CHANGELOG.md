# Changelog

All notable changes for this fork will be documented in this file.

## [v1.1.1-slimlog-x4-cjk1] - 2026-03-29

### Added
- Stable Chinese font support for XT EINK X4
- Traditional Chinese reading font based on Source Han Serif TC
- Chinese UI font based on Chiron Hei HK Text-R
- Coverage for many common Simplified Chinese glyphs

### Changed
- Separated UI font choice from reader font choice
- Updated CJK font grouping in `fontconvert.py` for safer runtime decompression
- Kept this snapshot on the CrossPoint `1.1.1` version line

### Fixed
- Chinese EPUB rendering crash caused by overly large grouped CJK font bitmap decompression
- Chinese filename and UI text rendering on the device

### Notes
- This snapshot focuses on stable Chinese reading and UI support
- Dedicated Simplified-Chinese-specific font tuning is not yet done
- Vertical Traditional Chinese layout support is not yet implemented
