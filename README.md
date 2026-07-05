# rootstock
Rootstock is a single-page web app that lets a farmer photograph a crop (with their phone camera or from their gallery), instantly get a plain-language health check, a likely disease/deficiency name, a matching biofertilizer recommendation with dosage.
Built as a learning/portfolio project exploring client-side image analysis, browser camera APIs, and simple heuristic diagnostics for agriculture.

---

## ✨ Features

- **📷 Live camera capture** — opens the phone's rear camera directly in the browser (with a front/back switch), so a farmer can snap a leaf photo without ever leaving the page
- **🖼️ Gallery upload** — or choose an existing photo instead
- **🩺 Instant health check** — analyzes the image's pixel colors on-device to estimate:
  - % healthy green tissue
  - % yellowing (chlorosis)
  - % brown / dry / necrotic tissue
  - % dark spotting (possible lesions)
- **🎯 A 0–100 health score** with a simple, plain-language badge (e.g. "Healthy crop," "Needs nitrogen," "Needs attention")
- **🦠 Likely disease / deficiency name** — matched from the detected pattern and the crop you select (e.g. "Likely Rice Blast or Bacterial Leaf Blight," "Nitrogen Deficiency")
- **🧪 Biofertilizer recommendation** — specific product type, application rate, and frequency
- **📈 6-week forecast graph** — projects crop condition if the advice is followed vs. left untreated
- **💾 Saved, editable history** — every check is saved automatically; edit the crop/notes or delete entries later
- **🔁 Before/after comparison** — select any two saved checks to compare recovery, with images side by side and a score delta
- **📱 Mobile-first, high-contrast UI** — large tap targets and readable text designed for outdoor phone use, not a desktop dashboard shrunk down

---

## 🛠️ How it works (tech notes)

This is intentionally a **zero-backend, single HTML file** app:

| Piece | How it's done |
|---|---|
| Camera capture | `navigator.mediaDevices.getUserMedia()` |
| Image analysis | Drawn to an off-screen `<canvas>`, then read pixel-by-pixel via `getImageData()` and classified by RGB/luminance thresholds |
| Diagnosis logic | Rule-based thresholds on the % of green / yellow / brown / dark pixels, cross-referenced with the selected crop |
| Forecast graph | Hand-rolled inline SVG line chart, no charting library |
| History storage | Simple key-value persistent storage, saved per check with a compressed thumbnail |

**No frameworks, no build step, no dependencies.** Open `index.html` in a browser and it works.

---

## ⚠️ Important limitations — please read

This is a **heuristic, portfolio-grade prototype**, not a validated agronomic tool:

- The disease/deficiency name is a **best-guess pattern match** on leaf color, not the output of a trained image-classification model (like a CNN trained on a labeled crop-disease dataset). It can be wrong, especially with poor lighting, mixed backgrounds, or diseases that don't show a strong color signature.
- The 6-week forecast is a **simple projection formula**, not a yield model based on real agronomic or weather data.
- **Camera access requires HTTPS** (or `localhost`). If you deploy this over plain HTTP, the browser will block camera access and the app will fall back to gallery upload.
- Saved history currently depends on the storage environment it's running in — if you lift this file out and self-host it as a static site, you'll need to swap in `localStorage`, IndexedDB, or a real backend for persistence to survive across sessions.

**Always recommend confirming with a local agriculture officer or Krishi Vigyan Kendra before spending money on treatment** — the app says this in the UI too.

---

## 🚀 Getting started

No build tools needed.

```bash
git clone https://github.com/<your-username>/rootstock.git
cd rootstock
```

Then either:
- Double-click `index.html` to open it locally (camera capture may be blocked without HTTPS on some browsers — gallery upload will still work), or
- Serve it with any static host for full camera support:

```bash
# Quick local HTTPS-friendly option: Python's simple server (camera works on localhost)
python3 -m http.server 8000
# then open http://localhost:8000
```

For camera access on a real phone (not just your dev machine), deploy to any static host with HTTPS:
- [GitHub Pages](https://pages.github.com/)
- [Vercel](https://vercel.com/)
- [Netlify](https://netlify.com/)

---

## 🗺️ Possible next steps

- [ ] Swap the pixel-heuristic classifier for a trained model (e.g. a PlantVillage-dataset CNN) served via a lightweight backend
- [ ] Multi-language support for regional farmer accessibility
- [ ] Offline support (PWA / service worker) for low-connectivity areas
- [ ] Export a check or comparison as a shareable PDF/image
- [ ] Weather-aware forecast adjustments using a live weather API

---

## 📄 License

MIT — free to use, modify, and build on.

---

*Rootstock is a prototype built for learning purposes and does not replace professional agronomic advice.*

