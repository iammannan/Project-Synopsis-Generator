# Project Synopsis Generator

A free, browser-based tool that generates a formatted **Project Synopsis / Project Report cover document (.docx)** for college engineering projects — no Microsoft Word required.

Fill in a form (college details, project title, guide, students, abstract, block diagram, estimated cost) and download a ready-to-print `.docx` file, generated entirely client-side in your browser.

**Live demo:** https://iammannan.github.io/Project-Synopsis-Generator/

## Features

- Choose document type: **Synopsis** or **Project**
- Institution details: college name, address, department, academic year
- Optional college logo upload (falls back to a default placeholder logo)
- Project title and guide name
- Dynamic student list (add/remove students with name & register number)
- Project abstract text area
- Optional block diagram image upload
- Estimated project amount
- One-click **Download DOCX** — the `.docx` is generated in the browser using [JSZip](https://stuk.github.io/jszip/) and downloaded directly, with no server or account needed
- Works offline once loaded (except for the CDN-hosted JSZip library)
- Light/dark theme support (follows system preference)

## Usage

1. Open the [live page](https://iammannan.github.io/Project-Synopsis-Generator/) (or `index.html` locally in a browser).
2. Fill in the institution, project, student, and abstract details.
3. Optionally upload a college logo and a block diagram image.
4. Click **Download DOCX** to generate and download the formatted document.
5. Open the downloaded `.docx` in Word, Google Docs, or LibreOffice Writer to review and print.

## Running locally

This is a single static HTML file with no build step.

```bash
git clone https://github.com/iammannan/Project-Synopsis-Generator.git
cd Project-Synopsis-Generator
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

Or simply open `index.html` directly in a browser.

## Tech stack

- Plain HTML, CSS, and JavaScript — no framework, no build tools
- [JSZip](https://stuk.github.io/jszip/) (via CDN) to assemble the `.docx` (OOXML/ZIP) file in-browser

## Deployment

This repository is deployed with **GitHub Pages** via GitHub Actions (see [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)). Every push to `main` redeploys the site automatically.

## License

Released under the [MIT License](LICENSE).
