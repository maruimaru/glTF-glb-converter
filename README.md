# glTF → GLB Converter

> Developed to bridge the gap between RealityScan and Nomad Sculpt.  
> Fully client-side — **no data is ever uploaded**.

A single-file web application that converts a glTF folder (`.gltf` + `.bin` + textures) into a
self-contained `.glb` file with optional 3D optimisation features designed for **Nomad Sculpt**
and **VR streaming** pipelines.

![App screenshot](https://github.com/user-attachments/assets/9383c028-c021-4150-97ad-e85c6082ae59)

---

## Features

| Feature | Description |
|---|---|
| **Core conversion** | Faithfully packs a glTF folder into a single binary GLB (spec v2.0). All buffers, accessors, sparse accessors, and extensions are preserved. |
| **Texture resize** | Optional downscaling of textures to a configurable maximum (512 px / 1 K / **2 K** / 4 K). Uses the browser Canvas API — no server required. |
| **Draco mesh compression** | Optional `KHR_draco_mesh_compression` support via the Google Draco encoder (loaded from CDN). Reduces mesh geometry by 60–80 %. |
| **Metadata embedding** | Embeds conversion timestamp, MIT license, and generator tag into `asset` / `extras`. |
| **3D preview** | In-browser Three.js viewer (OrbitControls) renders the model before conversion, including support for already-Draco-compressed inputs via DRACOLoader. |

## Usage

1. Open `index.html` in any modern browser (Chrome 89+, Firefox 108+, Safari 16.4+).
2. Drag & drop — or click **Choose Files** — and select all files from the glTF export folder
   (`.gltf`, `.bin`, and all texture images).
3. Configure the optimisation options:
   - **Resize Textures** — toggle on/off; choose max size from the dropdown.
   - **Draco Mesh Compression** — requires an internet connection to load the Draco encoder WASM.
   - **Embed Metadata** — adds conversion timestamp, license, and generator fields.
4. Click **👁 Preview** to render the model in the browser.
5. Click **⚡ Convert to GLB** and then **⬇ Download GLB** to save the result.

## Technical details

- **Zero dependencies** at runtime — Three.js and the Draco encoder are loaded via CDN
  (`cdn.jsdelivr.net` / `gstatic.com`).
- **No build step** — the entire application is a single `index.html` file using
  [import maps](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap)
  for ES module resolution.
- GLB binary layout follows the
  [glTF 2.0 specification](https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html#glb-file-format-specification)
  exactly (4-byte aligned chunks, magic `0x46546C67`, JSON chunk `0x4E4F534A`, BIN chunk
  `0x004E4942`).

## License

MIT © nakamura tadashi
