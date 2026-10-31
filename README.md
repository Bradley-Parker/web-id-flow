# ID Flow

A collection of browser-only HTML tools for creating professional ID photos — no server, no uploads, no dependencies to install.

## Files

| File | Purpose |
|------|---------|
| `index.html` / `autoid.html` | **Full auto flow** — guided capture → AI background removal → auto-crop to 4:5 |
| `takephoto.html` | Standalone guided camera capture with alignment overlay |
| `bgremoval.html` | Standalone background removal tool (drag-and-drop or file upload) |

## Auto ID Flow (`index.html`)

A single-page, three-step tool:

### Step 1 — Guided Capture
- Opens the device camera with a **4:5 framed viewport**
- SVG overlay guides: head ellipse, eye-line, shoulder curve, corner marks, centre axis
- Snap → preview → **Retake** or **Use this photo**

### Step 2 — AI Background Removal
- Runs entirely in the browser using **ONNX Runtime Web** + the [U2Net rembg model](https://huggingface.co/tomjackson2023/rembg)
- Live download progress bar (model is ~5 MB; cached after first use)
- Default output: **transparent background**
- Advanced ⚙ panel (hidden by default): switch to High Quality model (43 MB), or choose a white / custom-colour fill

### Step 3 — Auto-crop to 4:5
- Scans the transparent bounding box of the subject
- Adds 3% padding, then expands to hit the exact **4:5 aspect ratio**
- Renders to a standard **800 × 1000 px** PNG — ready to download

## Running Locally

No build step needed. Open any file directly in a browser:

```bash
# Python quick server (recommended — camera requires a secure context)
python -m http.server 8080
```

Then visit `http://localhost:8080`.

> **Note:** `getUserMedia` requires either `localhost` or an HTTPS origin. Opening `file://` directly will block camera access in most browsers.

## Privacy

All processing happens **client-side**. No images or data are sent to any server.

## Browser Support

| Feature | Requirement |
|---------|------------|
| Camera capture | `getUserMedia` (all modern browsers) |
| Background removal | WebAssembly + ONNX Runtime Web (Chrome, Edge, Firefox, Safari 16.4+) |
| Transparent PNG export | `canvas.toBlob` (all modern browsers) |
