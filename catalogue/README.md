# Catalogue Site

This directory is the source for the GitHub Pages website.

## Contributing content

The catalogue entries are maintained in [`catalogue/catalogue.md`](./catalogue.md), not in `index.html`.

## Local preview

Use a static server (recommended) so markdown loading works:

```bash
cd catalogue
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Deployment

A GitHub Actions workflow in `.github/workflows/pages.yml` deploys this directory to GitHub Pages on pushes to `main`.
