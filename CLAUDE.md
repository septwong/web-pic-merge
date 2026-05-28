# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

web-pic-merge is a single-page image card generator web application for creating promotional cards with:
- Main image + optional QR code composition
- Gradient backgrounds with preset color schemes (WeChat-style, warm, cool, etc.)
- Customizable text titles with font/color/size controls
- Watermark support (tiled or positioned)
- iPhone device frame overlay feature (iPhone套壳)
- Export to PNG and clipboard copy

## Architecture

This is a pure static HTML application with no build system. All code is contained in [index.html](index.html):
- **CSS**: Embedded in `<style>` block (lines 16-930) - handles layout, theming, responsive design
- **HTML**: Structure with settings panel (left) and preview grid (right)
- **JavaScript**: Embedded in `<script>` block (lines 1316-3200+) - all canvas rendering and UI logic

### Key Canvas Functions
- `generateCard()` - Main async function that creates card canvases (line 1994)
- `generateIPhoneCards()` - Creates iPhone device frame overlays (line 1534)
- `drawBackground()` - Renders gradient backgrounds (line 2262)
- `drawCardArea()` - Draws rounded rectangle with shadow (line 2339)
- `drawWatermark()` - Tiled or positioned watermarks (line 3138)
- `extractDominantColors()` - Auto-extracts vibrant colors from uploaded images (line 2794)

### UI Interaction Flow
1. User uploads main images and optional QR codes
2. Adjusts style/text/watermark settings in left panel
3. Clicks "生成卡片" (Generate) to create canvases
4. Preview grid shows results with per-item download/copy buttons

## Development

No build required. Open `index.html` directly in a browser for development. All changes are immediately visible on refresh.

## Card Dimensions

- Card generation canvas: 900×1200 pixels (set in `generateCard()` at lines 2022-2025)
- iPhone frame canvas: 1400×2820 pixels (set in `generateIPhoneCards()` at lines 1557-1558)

## iPhone Frame Assets

iPhone border images are stored in `images/`:
- `iphone-border-15promax.png` - iPhone 15 Pro Max frame
- `iphone-border-16promax.png` - iPhone 16 Pro Max frame (default)