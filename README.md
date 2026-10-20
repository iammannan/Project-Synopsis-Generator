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

## View counter

The page shows a **Today** and **Total Views** counter stored in a Firebase Realtime Database. Each browser is counted once per session per day (days follow Indian Standard Time). The counter talks to Firebase through its REST API, so no Firebase SDK or API key is needed — only the database URL.

The database URL is **not committed** to this repository. `index.html` contains the placeholder `__FIREBASE_DB_URL__`, which the deploy workflow replaces with a GitHub Actions secret at build time. If the secret is missing, the counter simply stays hidden.

> Anything a browser uses is visible in the published page, so the URL itself is not truly secret. What protects the data is the database rules below: visitors can read the counts and add exactly 1, but can never reset, lower, or overwrite them.

### Setup

1. In the [Firebase console](https://console.firebase.google.com/), create a project and add a **Realtime Database**.
2. In **Realtime Database → Rules**, paste and publish:

   ```json
   {
     "rules": {
       ".read": false,
       ".write": false,
       "views": {
         "total": {
           ".read": true,
           ".write": "newData.isNumber() && newData.val() === (data.exists() ? data.val() : 0) + 1"
         },
         "daily": {
           "$day": {
             ".read": true,
             ".write": "$day.matches(/^[0-9]{4}-[0-9]{2}-[0-9]{2}$/) && newData.isNumber() && newData.val() === (data.exists() ? data.val() : 0) + 1"
           }
         }
       }
     }
   }
   ```

3. Copy the database URL (e.g. `https://your-project-default-rtdb.firebaseio.com`).
4. In this repository, go to **Settings → Secrets and variables → Actions → New repository secret**, name it `FIREBASE_DB_URL`, and paste the URL.
5. In **Settings → Pages → Build and deployment**, set **Source** to **GitHub Actions** so the workflow (which injects the secret) is what publishes the site.

## Deployment

This repository is deployed with **GitHub Pages** via GitHub Actions (see [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)). Every push to `main` redeploys the site automatically. Pages must be set to the **GitHub Actions** source (step 5 above).

## License

Released under the [MIT License](LICENSE).
