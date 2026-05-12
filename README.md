# Pexels Image Search Skill

A reusable skill for fetching high-quality, royalty-free stock photos from [Pexels](https://www.pexels.com) to use as backgrounds in Instagram carousels, blog headers, presentations, or any visual content.

This skill is designed to integrate with the **TravelMint Instagram Carousel Configuration** (v3.3), automatically selecting the right layout variant, chapter markers, and photo search strategy based on topic classification.

---

## Quick Start

### 1. Download the Skill

Download the latest release:

```bash
curl -L -o pexels-image-search.zip https://github.com/YOUR_USERNAME/pexels-image-search/releases/latest/download/pexels-image-search.zip
```

Or clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/pexels-image-search.git
cd pexels-image-search
```

### 2. Extract & Edit

```bash
unzip pexels-image-search.zip -d pexels-image-search/
cd pexels-image-search
```

Edit the example scripts to add your Pexels API key:

```bash
# Edit examples
nano examples/search-examples.py
nano examples/download-batch.py
```

Replace `YOUR_API_KEY` with your actual key from [pexels.com/api](https://www.pexels.com/api/).

### 3. Re-zip (if you modified files)

```bash
zip -r pexels-image-search.zip pexels-image-search/
```

---

## Skill Structure

```
pexels-image-search/
├── SKILL.md                          # Main skill definition & documentation
├── examples/
│   ├── search-examples.py            # Common search patterns (portrait, landscape, thumbnails)
│   └── download-batch.py            # Batch download for multiple topics
└── references/
    └── api-reference.md              # Pexels API endpoint docs, auth, params
```

### `SKILL.md`

The core skill file that defines:

- **Topic Classification** — How to classify topics as `location_based`, `functional`, or `hybrid`
- **Layout Variants** — Three CSS layouts: `gradient_centered`, `photo_overlay_bottom_left`, `photo_overlay_center`
- **Chapter Markers** — Narrative chapter titles (replaces progress counters)
- **Typography Emphasis** — Bold + italic word highlighting rules
- **Feed Alignment** — Zero-tolerance baseline locks for Instagram feed consistency
- **Pexels Search Strategy** — Which keywords to search per slide position
- **Config-Aware Client** — `TravelMintPexelsClient` class that reads your JSON config

### `examples/search-examples.py`

Demonstrates common search patterns:

| Pattern | Use Case | Orientation |
|---------|----------|-------------|
| Portrait single | Instagram carousel cover | `portrait` |
| Landscape single | Blog header | `landscape` |
| Thumbnail grid | Gallery preview | `medium` size |
| Dark moody | Dramatic background | `portrait` |

### `examples/download-batch.py`

Batch download script for multiple travel topics:

```python
topics = [
    "amritsar golden temple",
    "goa beach sunset",
    "manali mountains"
]
```

Automatically creates dated output folders and saves photos with photographer credits.

### `references/api-reference.md`

Quick reference for Pexels API:

- Authentication header format
- Search/curated/photo-by-id endpoints
- Parameter values (`orientation`, `size`, `color`)
- Response field descriptions

---

## TravelMint Config Integration

This skill is designed to read your `travelmint_config_v3.3.json` and automatically apply its rules:

```python
from pexels_client import TravelMintPexelsClient

client = TravelMintPexelsClient(
    api_key="YOUR_PEXELS_KEY",
    config_path="travelmint_config_v3.3.json"
)

# Automatically decides:
# - Which slides need photos (based on topic_classification)
# - Which layout variant to use (based on layout_variants)
# - Which keywords to search (based on location extraction)
# - Chapter markers per slide (based on chapter_markers)

images = client.get_photos_for_carousel("Golden Temple Amritsar", Path("output"))
```

### Topic Type → Photo Strategy

| Topic Type | Slides with Photos | Layout | Pexels Search |
|-----------|-------------------|--------|---------------|
| `location_based` | Slide 1, Slide 7 | `photo_overlay_bottom_left` | Location name + sunset |
| `functional` | None | `gradient_centered` | Skip Pexels |
| `hybrid` | Slide 1 only | Mixed | Location name only |

### Layout Variants

#### `gradient_centered`
Dark gradient background, centered text. No photo needed.

#### `photo_overlay_bottom_left`
Full-bleed photo, text anchored bottom-left with gradient overlay.

#### `photo_overlay_center`
Full-bleed photo, centered text with heavy overlay for dramatic effect.

---

## Requirements

- Python 3.8+
- `requests` library (`pip install requests`)
- Free Pexels API key ([get one here](https://www.pexels.com/api/))
- TravelMint config JSON (optional, for auto-configuration)

---

## Pexels API Limits

| Tier | Requests/Hour | Requests/Month | Price |
|------|--------------|----------------|-------|
| Free | 200 | 20,000 | $0 |
| Premium | 1,000 | 100,000 | Contact Pexels |

---

## Customization

### Adding More Locations

Edit the location list in `TravelMintPexelsClient._extract_location()`:

```python
locations = [
    "amritsar", "jaipur", "goa", "kerala", "manali",
    "rishikesh", "varanasi", "udaipur", "spiti", "ladakh",
    # Add your locations here
]
```

### Changing Search Queries

Modify the query templates in `get_photos_for_carousel()`:

```python
cover_query = f"{location} sunrise"      # Instead of just location
cta_query = f"{location} night"          # Instead of sunset
```

### Custom CSS

Edit the CSS templates in `SKILL.md` or create your own stylesheet:

```css
.chapter-marker {
    /* Your custom styling */
}
```

---

## License

- **Skill code**: MIT License
- **Pexels photos**: [Pexels License](https://www.pexels.com/license/) — free for commercial use without attribution (attribution appreciated)

---

## Contributing

1. Fork the repository
2. Edit `examples/search-examples.py` or `examples/download-batch.py`
3. Test your changes
4. Re-zip the skill package
5. Submit a pull request

---

## Support

- Pexels API docs: [pexels.com/api/documentation](https://www.pexels.com/api/documentation/)
- TravelMint config reference: See `references/config-mapping.md`
- Issues: [github.com/YOUR_USERNAME/pexels-image-search/issues](https://github.com/YOUR_USERNAME/pexels-image-search/issues)

---

## Changelog

### v1.0.0 (2026-05-12)
- Initial release
- TravelMint config v3.3 integration
- Three layout variants: gradient_centered, photo_overlay_bottom_left, photo_overlay_center
- Chapter markers replacing progress indicators
- Typography emphasis with bold + italic support
- Zero-tolerance feed alignment
