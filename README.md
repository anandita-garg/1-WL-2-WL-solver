# Weisfeiler–Leman Visualizer

A static web page for stepping through Weisfeiler–Leman color refinement on a graph given as an adjacency matrix. It runs three algorithms side by side, or one at a time:

- **1-WL** (color refinement): `c'(v) = (c(v), {{c(u) : u ∈ N(v)}})`
- **2-WL** (ordinary): `c'(u,v) = (c(u,v), {{c(w,v)}}_w, {{c(u,w)}}_w)`. It has the same distinguishing power as 1-WL.
- **2-FWL** (folklore): `c'(u,v) = (c(u,v), {{(c(u,w), c(w,v))}}_w)`. It has the same distinguishing power as 3-WL.

Each algorithm can be stepped on its own, or all of them together. A run stops once its partition stops getting finer.

## Usage

Open `index.html` in a browser. There is no build step and nothing else to install.

Paste a symmetric 0/1 adjacency matrix and click **Load / restart**, or click **Surprise me** for a random graph.

Tick **Compare with a second graph** to run the WL test on two graphs at once. Both graphs are refined together with one shared color map, and each algorithm then gives its own verdict:

- **Not isomorphic:** the color histograms differ at some iteration, which proves the graphs are different.
- **Cannot distinguish:** both runs are stable with identical histograms, so this algorithm can't tell the graphs apart.

C6 vs two triangles is a good pair to try: 1-WL and 2-WL cannot tell them apart, but 2-FWL can.

## Structure

```
index.html              page markup
styles.css              styles
js/app.js               input parsing, step controls, rendering
js/algorithms/
  common.js             registry + shared helpers (canonical relabeling, atomic pair types)
  wl1.js                1-WL
  wl2.js                ordinary 2-WL
  fwl2.js               folklore 2-WL
```

### Adding an algorithm

1. Create a new file in `js/algorithms/` that registers `ALGORITHMS.<key> = { name, hint, pairs, init(g), refine(g, c) }`.
2. Add a `<script>` tag for it in `index.html`.
3. Add an `<option>` for it to the algorithm menu.
