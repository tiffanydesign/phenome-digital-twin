# Phenome · Digital Twin

A single-file design study for a health-scan portal. `digitaltwin.html` carries the design system and two interactive WebGL pages side by side: a screen-space halftone figure for the home page, and a stepped organ view for the body page.

**Live: https://tiffanydesign.github.io/phenome-digital-twin/**

- Type: Contralto in four optical sizes, New Science Mono for the 80px display numeral only, Helvetica Neue for everything else
- 3D: three.js r160 with a minimal GLB reader of its own, so no GLTFLoader
- The figure is a static composition — no rotation, no orbit, key light behind the subject, no cast shadow

## The three pages

| Tab | What it is |
| :--- | :--- |
| **Home page** | The scan summary. A halftone figure whose legs dissolve into the page rather than stopping at the frame. |
| **Body page** | Pick an organ, the camera flies to it, the body turns to glass around it. |
| **Design system** | Tokens, type scale, colour, chart types, eleven report patterns, glass surfaces. |

## Running it locally

**It has to be served over HTTP.** The page fetches `GLB/*.glb` by relative path, and under `file://` the same-origin policy blocks that: you get `Failed to fetch` and the page falls back to the 2D procedural figure.

```bash
python -m http.server 8777
```

Then open `http://localhost:8777/` — the root redirects to `digitaltwin.html`.

Double-clicking the HTML file will **not** load the 3D models.

## Layout

```
index.html                 redirect to digitaltwin.html, for the Pages root
digitaltwin.html           design system + the home and body pages

organ-material-lab.html    material bench — drop in any .glb to swap the specimen;
                           the source of truth for material values
home-hero-lab.html         hero preset sheet: male/female x full/upper body
organ-body-full.html       full organ view, and the reference this page's organ
                           placement was measured against
organ-body-stage1.html     an earlier organ view, kept as a record of the work

vendor/
  three.module.js          three.js r160 (local copy; the code falls back to
                           jsDelivr then unpkg)
GLB/
  Male.glb                 body model
  Female.glb               body model
  organ/
    pink_brain.glb                   Brain
    realistic_human_heart.glb        Heart
    realistic_human_lungs.glb        Lungs
    small_and_large_intestine.glb    Intestines
    human_liver_and_gallbladder.glb  Liver & Gallbladder
    human_kidney.glb                 Kidneys
    realistic_stomach.glb            Stomach — organ-body-full.html only
    lungs.glb                        no page loads this any more; the body page
                                     moved to realistic_human_lungs.glb, whose
                                     proportions the placement was measured on
```

The four lab pages share the same root-relative paths (`vendor/`, `GLB/`) as `digitaltwin.html`, so they belong at the repo root and run off the same server — `http://localhost:8777/home-hero-lab.html` and so on.

`_local/` is an untracked directory (see `.gitignore`) holding spare models no page references, the source document for the model links, and a third-party screenshot kept only as a design reference. **It is never committed, so it is not backed up** — archive anything you need to keep.

## Browser support

A current browser with WebGL2 and ES modules. three.js is loaded from local `vendor/` first, then jsDelivr, then unpkg; if all three fail the page keeps its 2D figure rather than going blank.

## Fonts

Contralto and New Science Mono are licensed faces. They are **not distributed with this repo** and not embedded in the page. Without them installed the page falls back to a system serif and mono — the proportions still hold, the letterforms differ.

## Model sources and licence

Every model under `GLB/` comes from Sketchfab under [CC Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which **permits commercial use and requires attribution**.

| File | Model | Author | Source |
| :--- | :--- | :--- | :--- |
| `Male.glb` `Female.glb` | Male & Female Base Mesh Pack | FormForge3D (`aleenasani841`) | [Sketchfab](https://sketchfab.com/3d-models/male-female-base-mesh-pack-ec3041da6a214c1c995f3d47dc7d04c1) |
| `pink_brain.glb` | pink brain | msurovik | [Sketchfab](https://sketchfab.com/3d-models/pink-brain-b032ee889d844af9b4acd4a2c1ccbba5) |
| `realistic_human_heart.glb` | Realistic Human Heart | neshallads | [Sketchfab](https://sketchfab.com/3d-models/realistic-human-heart-3f8072336ce94d18b3d0d055a1ece089) |
| `realistic_human_lungs.glb` | Realistic Human Lungs | neshallads | [Sketchfab](https://sketchfab.com/3d-models/realistic-human-lungs-ce09f4099a68467880f46e61eb9a3531) |
| `lungs.glb` | lungs | reynosa2000 | [Sketchfab](https://sketchfab.com/3d-models/lungs-981d026657984895a90422d5e99e7ac2) |
| `small_and_large_intestine.glb` | Small and large intestine | antonia.sundberg | [Sketchfab](https://sketchfab.com/3d-models/small-and-large-intestine-8a1ca8e3ca224cdeb9264674416bde38) |
| `human_liver_and_gallbladder.glb` | Human liver and gallbladder | ElliotSS | [Sketchfab](https://sketchfab.com/3d-models/human-liver-and-gallbladder-6c4e9bd0d49f4828b804259330c0c6c4) |
| `human_kidney.glb` | Human Kidney | neshallads | [Sketchfab](https://sketchfab.com/3d-models/human-kidney-e1476ceb1e3b4412af5418eee9c5ed08) |
| `realistic_stomach.glb` | Realistic Stomach | Brain Diagno (`Brain_Diagno`) | [Sketchfab](https://sketchfab.com/3d-models/realistic-stomach-07859d72489d4f818e508b3738ab7449) |

### Attribution in the interface

The models are modified (retopology, materials replaced), so CC BY asks for the change to be stated alongside the credit. The design system specifies how, as **pattern 11, Provenance mark**: one ⓘ ring per anatomy field opens a panel that names the licence and the modification once, then lists each work and its author.

It is live on both figure pages, bottom-left above the tab bar. The home page shows one borrowed model so its list is one row. The body page's list is **built from the same `CATS` array the page loads its files from**, so the credit cannot name a model the page does not use, or miss one it does — and when an organ is open, its row takes the dot.

The credit line reads:

> "Realistic Human Heart" by neshallads is licensed under CC BY 4.0. Modified by Phenome Longevity

## License

No licence is set for the code. The 3D models are CC BY 4.0 as listed above; the fonts remain the property of their foundries.
