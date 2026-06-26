# ALMR Terrain Coverage Tiles

This repository hosts slippy-map terrain coverage tiles (XYZ format) for the Alaska Land Mobile Radio (ALMR) network, as well as a lightweight Leaflet viewer.

## Repository Contents

- **`index.html`**: A simple, standalone Leaflet-based map viewer to preview the hosted tiles locally or online.
- **`almr_tiles/`**: The directory containing generated coverage tiles structured as `{z}/{x}/{y}.png`.
- **`.nojekyll`**: Prevents GitHub Pages from ignoring directories or files (if any start with underscores or other Jekyll-restricted patterns).

## Tile Generation & Color Scale

These tiles are generated in the sister repository [almr-coverage](file:///home/clayauld/Custom%20Apps/almr-coverage) by parsing Pathloss 6 KMZ exports, vectorizing signal bands, and rendering them into slippy-map tiles.

The coverage layers map to the following signal strength thresholds:

| Color      | Hex       | Range                      | Signal Class / Quality                |
| :--------- | :-------- | :------------------------- | :------------------------------------ |
| **Red**    | `#e31a1c` | $\ge -50\text{ dBm}$       | Strongest (Excellent indoor/handheld) |
| **Orange** | `#ff7f00` | $-50$ to $-65\text{ dBm}$  | Strong                                |
| **Green**  | `#33a02c` | $-65$ to $-80\text{ dBm}$  | Good (Reliable portable coverage)     |
| **Blue**   | `#1f78b4` | $-80$ to $-95\text{ dBm}$  | Moderate (Reliable vehicle coverage)  |
| **Purple** | `#984ea3` | $-95$ to $-116\text{ dBm}$ | Fringe (Marginal voice coverage)      |

*Signal levels below $-116\text{ dBm}$ are considered out of range and are not rendered (transparent).*

---

## Local Development & Testing

You can spin up a local server to view the Leaflet interface and test tile loading:

```bash
python3 -m http.server 8000
```

Once running, visit:
- **Viewer**: `http://localhost:8000/index.html`
- **Tile Endpoint**: `http://localhost:8000/almr_tiles/{z}/{x}/{y}.png`

---

## Registering Tiles in CalTopo

Once hosted (e.g. via GitHub Pages), you can overlay this layer directly onto any CalTopo map:

1. Open your CalTopo map workspace.
2. In the left sidebar, click **Add** > **Custom Source**.
3. Configure the source:
   - **Name**: `ALMR Terrain Coverage`
   - **Type**: `Tile`
   - **URL Template**: `https://<your-username>.github.io/<your-repo>/almr_tiles/{z}/{x}/{y}.png` 
      - *(Note: Ensure the hosting server sends the `Access-Control-Allow-Origin: *` header for CORS, which GitHub Pages does by default).*
   - **Max Zoom**: `12`
4. Click **Save**. You can now toggle the ALMR coverage overlay on any map canvas.
