# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a GitHub Pages website for Rhode Island Food Fights (RIFF), a food event organization established in 2011. The site features an interactive home page with an image carousel and an event map showing participating locations.

## Architecture

### Core Pages

- **index.html**: Main landing page with carousel, navigation pills (WHAT/WHERE/WHEN/HOW), and popup for event passports
- **map.html**: Interactive Leaflet map showing event locations with toggleable transport/satellite views
- **style.css**: Shared styles across all pages with colorful brand palette (pink/yellow/blue)

### Data Management

**references.json** is the single source of truth for all dynamic content:
- `main_image`: Main promotional image displayed on index.html
- `carousel_images[]`: Array of image filenames for the rotating carousel
- `locations[]`: Array of event locations with structure:
  ```json
  {
    "name": "Business Name",
    "address": "Full Address",
    "instagram": "handle",
    "image": "filename.jpg",
    "lat": 41.xxxxx,
    "lng": -71.xxxxx
  }
  ```

### Image Organization

Images are organized in `/imgs/` with subdirectories:
- `carousel/`: Images for the home page rotating carousel
- `listpics/`: Thumbnail images for map location list
- `mainpic/`: Main promotional image shown at bottom of index.html
- `rifflogo.jpg`: RIFF logo used in header

### JavaScript Architecture

Both HTML files contain inline JavaScript with similar patterns:
- Async fetch from `references.json` on page load
- Error handling with CORS warnings (requires local server or GitHub Pages)
- Image path construction by prepending directory paths to JSON filenames

**index.html** features:
- Auto-rotating carousel (3s intervals) with manual navigation arrows
- Button selection system that updates text display
- Popup overlay for passport purchases

**map.html** features:
- Leaflet.js integration with custom markers
- Bidirectional synchronization: clicking map markers highlights list entries and vice versa
- Toggle between Thunderforest transport and Esri satellite tiles
- Custom marker icons (red pin / gold pin for selected state)

## Development

### Local Development Server

The site requires a local HTTP server due to CORS restrictions when loading `references.json`:

```bash
python3 -m http.server 8000
```

Then access at `http://localhost:8000`

### Adding New Event Locations

Edit `references.json` to add locations to the map:
1. Add location object to `locations[]` array
2. Include image file in `imgs/listpics/`
3. Get coordinates from Google Maps (right-click location → coordinates)
4. Instagram handle should include '@' or it will be added automatically

### Updating Images

**Carousel images**:
1. Add files to `imgs/carousel/`
2. Update `carousel_images[]` in `references.json`

**Main promotional image**:
1. Add file to `imgs/mainpic/`
2. Update `main_image` in `references.json`

### External Dependencies

- **Leaflet.js 1.9.4**: Map library (CDN loaded in map.html)
- **Thunderforest API**: Transport map tiles (API key embedded: `6170aad10dfd42a38d4d8c709a536f38`)
- **Esri/ArcGIS**: Satellite imagery tiles

## Deployment

Site is deployed via GitHub Pages pointing to the main branch root. The CNAME file configures the custom domain.
