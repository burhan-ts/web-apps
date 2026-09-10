# 🗞️ News Globe

An interactive 3D globe that lets you click on any of 55 major world cities to pull up the latest news headlines for that location.

![Status](https://img.shields.io/badge/status-live-success)
![Backend](https://img.shields.io/badge/backend-Django-092E20)
![License](https://img.shields.io/badge/license-MIT-green)

## Features

- 🌐 **Interactive 3D Globe** — Rotating Earth rendered with Three.js
- 📍 **55 major cities** — Pre-loaded across every continent, sized by relative importance
- 🖱️ **Click a city** — Camera flies in and a news panel slides out
- 📰 **Live headlines** — Pulled from Google News RSS, with a Wikipedia current-events fallback
- ✨ **Auto-rotate & starfield** — Ambient motion and atmosphere glow until you interact

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Django + Django REST Framework |
| News data | Google News RSS (primary), Wikipedia current events API (fallback) |
| Frontend | Three.js (vanilla JS, no build step) |
| Country outlines | Natural Earth 110m GeoJSON (bundled) |

## Quick Start

### Prerequisites
- Python 3.10+

### Setup

```bash
# 1. Create a virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the server
python manage.py runserver 0.0.0.0:8000
```

### Open in browser
```
http://localhost:8000
```

## How It Works

1. **Globe** — A Three.js scene renders a textured sphere with a starfield background and atmospheric glow.
2. **Cities** — A fixed list of 55 cities (name, country, lat/lng, marker size) is served from the backend and plotted onto the globe.
3. **Click** — Clicking a city marker flies the camera to it and opens a news panel.
4. **News fetch** — The backend queries Google News RSS for that city/country; if no results come back, it falls back to Wikipedia's current events feed.

## API Endpoints

| Endpoint | Description |
|---|---|
| `GET /` | Main globe page |
| `GET /api/cities/` | List of the 55 cities with coordinates |
| `GET /api/news/<city>/<country>/` | Latest news articles for a city |
| `GET /api/borders/` | Reserved endpoint (borders are currently loaded client-side) |

## Project Structure

```
news-globe/
├── manage.py                      # Django entry point
├── requirements.txt                # Python dependencies
├── .env.example                    # Template for production environment variables
├── .gitignore
├── LICENSE
├── news_globe/                     # Django project config
│   ├── settings.py                 # App settings
│   ├── urls.py                     # Root URL config
│   ├── templates/core/index.html   # Main page template
│   └── static/
│       ├── js/app.js               # Globe rendering + interaction logic
│       ├── js/three.min.js         # Three.js library
│       ├── img/earth.jpg           # Globe texture
│       └── json/countries-110m.json# Country boundary data
└── news_app/                       # News app (backend)
    ├── views.py                    # API views (cities, news fetch)
    └── urls.py                     # API route definitions
```

## Configuration

Local development works with zero setup. For deployment, copy `.env.example` to `.env` (or set these as real environment variables) and fill them in:

| Variable | Purpose | Default |
|---|---|---|
| `DJANGO_SECRET_KEY` | Django's cryptographic signing key | insecure dev key |
| `DJANGO_DEBUG` | Enables/disables debug mode | `True` |
| `DJANGO_ALLOWED_HOSTS` | Comma-separated list of allowed hostnames | `localhost,127.0.0.1` |

**Before deploying:** set `DJANGO_SECRET_KEY` to a real random value, set `DJANGO_DEBUG=False`, and set `DJANGO_ALLOWED_HOSTS` to your actual domain.

## Notes

- No API key is required; news comes from public RSS/JSON feeds, so availability and rate limits depend on those services.

## License

MIT — free to use, modify, and share. Built with:
- [Three.js](https://threejs.org/)
- [Google News RSS](https://news.google.com/)
- [Natural Earth](https://www.naturalearthdata.com/)
