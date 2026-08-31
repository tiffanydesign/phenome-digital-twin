# Phenome · Digital Twin

A single-file design study for a health-scan portal. `digitaltwin.html` carries the design system and four interactive WebGL pages side by side: a screen-space halftone figure for the home page, a stepped organ view for the body page, and two single-organ reports — heart rhythm and kidneys.

**Live: https://tiffanydesign.github.io/phenome-digital-twin/**

- Type: Contralto in four optical sizes, New Science Mono for the 80px display numeral only, Helvetica Neue for everything else
- 3D: three.js r160 with a minimal GLB reader of its own, so no GLTFLoader
- The figure is a static composition — no rotation, no orbit, key light behind the subject, no cast shadow

## The five pages

| Tab | What it is |
| :--- | :--- |
| **Home page** | The scan summary. A halftone figure whose legs dissolve into the page rather than stopping at the frame. |
| **Body page** | Pick an organ, the camera flies to it, the body turns to glass around it. |
| **Heart rhythm** | One organ close-up beside one data rail. The composition the design system was drawn from, put back together out of its own parts: split canvas, pinned readouts on hairline leaders, a waveform strip and three zoned range tracks, all on glass over the page's own ground. |
| **Kidneys** | The same composition, a second time, with a cut-away model and a different chart. It exists to test whether the heart page was a drawing or a pattern: same split, same rail, same glass, and not one component of its own — section 04's dot meter, section 06's card, pattern 02's pinned readouts, pattern 11's provenance mark. |
| **Design system** | Tokens, type scale, colour, chart types, eleven report patterns, glass surfaces. |

The two single-organ pages are drawn by one renderer. `organView(cfg)` builds a
complete tone-and-halftone pass per call — its own WebGL context, materials and
render target, so nothing crosses a renderer boundary — and both pages differ
only in a model file, a yaw and two framing numbers.

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
organ-body-full.html       full organ view, and the reference this page's organ
                           placement was measured against. Its panel exposes
                           every placement value including pitch, so a model's
                           seating can be dialled in by hand before the numbers
                           are carried across.
glass-liquid-lab.html      surface bench — Apple-style Liquid Glass against
                           section 06's doctrine, on four of the body page's
                           real chrome components over the live 3D scene. A
                           draggable wipe puts both treatments on the same
                           specimen at the same screen position. Chromium only:
                           backdrop-filter ignores SVG filters elsewhere.

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
    realistic_stomach.glb            Stomach
    small_and_large_intestine.glb    Intestines
    human_liver_and_gallbladder.glb  Liver & Gallbladder
    human_kidney.glb                 Kidneys, inside the body
    kidney_with_labels.glb           Kidneys, the cut-away on its own page
    bodyparts3d_thyroid.glb          Thyroid, pose lab only
  skeletal/
    lumbar-hip.glb                   L1-L5, lumbar discs, sacrum, hips & femurs
    NOTICE.md                        skeletal asset attribution and licence
```

The three lab pages share the same root-relative paths (`vendor/`, `GLB/`) as `digitaltwin.html`, so they belong at the repo root and run off the same server — `http://localhost:8777/organ-material-lab.html` and so on.

Two others were removed, and git history is a better record of the work than an idle file at the repo root. Recover either with `git show <rev>:<file>`.

  organ-body-stage1.html   an earlier organ view. Described here as "kept as a
                           record of the work", superseded by
                           organ-body-full.html, and linked by no page.
  home-hero-lab.html       the hero preset sheet, male/female x full/upper body.
                           Its presets had already been carried across into the
                           home page, and no comment in digitaltwin.html cited
                           it as the authority for a value — which is what
                           separates it from the two material and placement
                           benches that stay.

`_local/` is an untracked directory (see `.gitignore`) holding spare models no page references, the source document for the model links, and a third-party screenshot kept only as a design reference. **It is never committed, so it is not backed up** — archive anything you need to keep.

## Browser support

A current browser with WebGL2 and ES modules. three.js is loaded from local `vendor/` first, then jsDelivr, then unpkg; if all three fail the page keeps its 2D figure rather than going blank.

## Fonts

Contralto and New Science Mono are licensed faces. They are **not distributed with this repo** and not embedded in the page. Without them installed the page falls back to a system serif and mono — the proportions still hold, the letterforms differ.

## Model sources and licence

The base body and the organ models under `GLB/organ/` come from Sketchfab under [CC Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which **permits commercial use and requires attribution**. The two assets the pose lab adds — the thyroid and the lumbar/hip extract — are **share-alike** instead, and are listed separately below.

| File | Model | Author | Source |
| :--- | :--- | :--- | :--- |
| `Male.glb` `Female.glb` | Male & Female Base Mesh Pack | FormForge3D (`aleenasani841`) | [Sketchfab](https://sketchfab.com/3d-models/male-female-base-mesh-pack-ec3041da6a214c1c995f3d47dc7d04c1) |
| `pink_brain.glb` | pink brain | msurovik | [Sketchfab](https://sketchfab.com/3d-models/pink-brain-b032ee889d844af9b4acd4a2c1ccbba5) |
| `realistic_human_heart.glb` | Realistic Human Heart | neshallads | [Sketchfab](https://sketchfab.com/3d-models/realistic-human-heart-3f8072336ce94d18b3d0d055a1ece089) |
| `realistic_human_lungs.glb` | Realistic Human Lungs | neshallads | [Sketchfab](https://sketchfab.com/3d-models/realistic-human-lungs-ce09f4099a68467880f46e61eb9a3531) |
| `small_and_large_intestine.glb` | Small and large intestine | antonia.sundberg | [Sketchfab](https://sketchfab.com/3d-models/small-and-large-intestine-8a1ca8e3ca224cdeb9264674416bde38) |
| `human_liver_and_gallbladder.glb` | Human liver and gallbladder | ElliotSS | [Sketchfab](https://sketchfab.com/3d-models/human-liver-and-gallbladder-6c4e9bd0d49f4828b804259330c0c6c4) |
| `human_kidney.glb` | Human Kidney | neshallads | [Sketchfab](https://sketchfab.com/3d-models/human-kidney-e1476ceb1e3b4412af5418eee9c5ed08) |
| `kidney_with_labels.glb` | Kidney_with_labels | ikon97840 | [Sketchfab](https://sketchfab.com/3d-models/kidney-with-labels-68a030f7dad444b0b6b4a9ce6a572625) |
| `realistic_stomach.glb` | Realistic Stomach | Brain Diagno (`Brain_Diagno`) | [Sketchfab](https://sketchfab.com/3d-models/realistic-stomach-07859d72489d4f818e508b3738ab7449) |

### The pose lab's two share-alike assets

`bodyparts3d_thyroid.glb` combines the BodyParts3D v4.3 thyroid isthmus (FJ3670), left lobe (FJ3671), and right lobe (FJ3672), published by the Database Center for Life Science (DBCLS). It is licensed under [CC BY-SA 2.1 Japan](https://creativecommons.org/licenses/by-sa/2.1/jp/deed.en) and was combined, reoriented, centered, converted to GLB, and given replacement materials by Phenome Longevity. See the [BodyParts3D download page](https://dbarchive.biosciencedbc.jp/en/bodyparts3d/download.html).

`skeletal/lumbar-hip.glb` contains the bilateral femurs and hip bones, sacrum, L1-L5 vertebrae, and L1-S1 discs extracted from the Anatria3D male skeletal atlas. The attribution chain is BodyParts3D → Z-Anatomy → Anatria3D; the extract remains under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Phenome Longevity selected the named structures, decoded Draco for the local loader, and removed unreferenced data without polygon reduction. See [`GLB/skeletal/NOTICE.md`](GLB/skeletal/NOTICE.md).

Both are loaded by the pose lab alone. The home, body and detail pages never fetch them, so nothing outside that tab inherits a share-alike term.

### Attribution in the interface

The models are modified (retopology, materials replaced), so CC BY asks for the change to be stated alongside the credit. The design system specifies how, as **pattern 11, Provenance mark**: one ⓘ ring per anatomy field opens a panel that names the licence and the modification once, then lists each work and its author.

It is live on all four figure pages, bottom-left above the tab bar. The home page and the two single-organ pages each show one borrowed model, so their list is one row. The body page's list is **built from the same `CATS` array the page loads its files from**, so the credit cannot name a model the page does not use, or miss one it does — and when an organ is open, its row takes the dot.

The credit line reads:

> "Realistic Human Heart" by neshallads is licensed under CC BY 4.0. Modified by Phenome Longevity

## License

No licence is set for the code. The 3D models are CC BY 4.0 as listed above, except the pose lab's thyroid (CC BY-SA 2.1 Japan) and lumbar/hip extract (CC BY-SA 4.0); the fonts remain the property of their foundries.
