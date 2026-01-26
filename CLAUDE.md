# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

web-pic-merge is a single-page image card generator web application for creating promotional cards with:
- Main image + optional QR code composition
- Gradient backgrounds with preset color schemes (WeChat-style, warm, cool, etc.)
- Customizable text titles with font/color/size controls
- Watermark support (tiled or positioned)
- Export to PNG and clipboard copy

## Architecture

This is a pure static HTML application with no build system. All code is contained in [index.html](index.html):
- **CSS**: Embedded in `<style>` block (lines 16-486) - handles layout, theming, responsive design
- **HTML**: Structure with settings panel (left) and preview grid (right)
- **JavaScript**: Embedded in `<script>` block (lines 810-2170) - all canvas rendering and UI logic

### Key Canvas Functions
- `generateCard()` - Main async function that creates card canvases (line 1008)
- `drawBackground()` - Renders gradient backgrounds (line 1261)
- `drawCardArea()` - Draws rounded rectangle with shadow (line 1338)
- `drawWatermark()` - Tiled or positioned watermarks (line 2002)
- `extractDominantColors()` - Auto-extracts vibrant colors from uploaded images (line 1658)

### UI Interaction Flow
1. User uploads main images and optional QR codes
2. Adjusts style/text/watermark settings in left panel
3. Clicks "生成卡片" (Generate) to create canvases
4. Preview grid shows results with per-item download/copy buttons

## Development

No build required. Open `index.html` directly in a browser for development. All changes are immediately visible on refresh.

## Card Dimensions

Canvas size: 900×1200 pixels (set in `generateCard()` at lines 1036-1039)
