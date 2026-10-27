# The prompt

Paste this into [Claude Code](https://claude.com/claude-code) or another coding agent. The original took three rounds of feedback, and this prompt is what those rounds settled on, so it starts you close to the end. Every run comes out a little different.

```text
Build "Beryl", a one-file product page (HTML, CSS, raw WebGL2, Web Audio, no libraries, no images) for a glass reading stone: the polished dome of glass people laid on a page to read before spectacles. The stone on the page is the product's live try-on.

1. The idea and the layout
- The whole viewport is a sheet of printed paper. A large serif headline "Bring every word / a little closer." (Merriweather 300, second line in italic, about 9vw) sits left, and a glass hemisphere rests on the word "closer." and magnifies it.
- Header: three text links left (Stones, Care, Journal), the wordmark "Beryl" in Merriweather italic centred, "Sound on" and "Bag 0" right.
- Right column (about 290px): "The reading stone" in italic with "1.5x magnification" beside it, a printed hairline, a Glass picker with six small glass beads, a one-line caption, a Size picker (60 / 80 / 100 mm), an "Add to bag" button with the price in it, a delivery line, and a line of 6 point type: "Every stone is checked against a line of 6 point type like this one. If you can read it through the glass, it ships."
- Body and UI in Atkinson Hyperlegible Next. Use Merriweather for every number, because Atkinson's zero is slashed.

2. The glass (the part that has to be believable)
- Trace every pixel under the stone analytically, in a fragment shader. Camera ray from a far eye above the page, hit the sphere, Fresnel reflection to a procedural room, refract in, then up to five events inside the glass: the flat base (exit through a thin air gap to the paper, or total internal reflection back up when the angle is too steep) or the dome from inside (exit to the room, or reflect back in).
- Run it once per colour channel with refractive indices 1.548, 1.553 and 1.563 for faint fringes, with per-channel absorption so the glass carries a tint.
- The room the glass reflects: warm walls, a dark ceiling over the camera, a four-pane window with crossbars and the sun in it upper left, and a white card low on the right. The crossbars in the reflection are what sell it as glass.
- Filter the magnified ink with textureGrad and derivatives taken in uniform control flow (trace the whole disc, blend the rim by coverage), or the edge will sparkle.

3. Light on the paper
- At load, in plain JS, trace a regular grid of rays from the sun (one layer per colour), the window (soft) and uniform room light through a unit dome onto the paper and bin where they land. Normalise open paper to exactly 1. Blend into a sunny and an overcast half-float texture. This gives the caustic, the soft shadow and the dark band under the rim.
- The same four-pane window also falls across the page as soft light with crossbar shadows. Each dome's caustic is only sharp where that light reaches it.
- Every 16 to 30 seconds a cloud passes: the caustic softens, the shadow fades, the window's reflection dims. Otherwise the render loop sleeps.

4. The page is printed
- Keep all the real text in the DOM, transparent, for selection and screen readers. Measure every word with a Range and draw it into a canvas texture at the same spot: red channel for ink, green for the button colour, blue for soft ink. Scale each word to its DOM width so the two can never drift.
- Use a blurred copy of that texture as a height map, so inked areas look pressed into the paper like letterpress, raked by the window light. Hover presses a button a little deeper, a click deeper still.

5. Six glasses, each with a true one-line fact
- Ruby: gold, in specks far too small to see. Amber: iron and sulfur, the recipe behind brown bottles. Emerald: chromium, the metal that greens a real emerald. Cobalt: so strong a colourant that a trace turns a whole melt blue. Amethyst: manganese, the reason old clear bottles turn violet in the sun. Clear: low in iron, so it stays clear where window glass turns green.
- Each has its own paper stock, ink and button colour (rose, apricot, mint, powder blue, lavender, chalk). Start on Amber.
- Picking a bead spreads the new colour across the page from that bead as a soft circular wave over about a second, and the stone's glass changes when the wave reaches it. Allow two waves in flight so fast clicks blend.
- The beads are tiny glass domes traced with the same optics. The big stone collides with them and slides round them, with a small clink.

6. Motion
- Entrance: the stone is lowered onto the page (its shadow pulls in as it lands), then slides along the line onto "closer.".
- Drag the stone directly. A click anywhere on the page glides it there with two critically damped springs in a row, so it starts with no jolt and settles with no overshoot.
- A press sends travelling rings across the dome surface (up to four at once), tilting the surface by about 7 degrees. More looks like water.

7. Sound, all synthesised on the first gesture
- A tap: a soft knock of glass on paper, crystalline partials and a quiet tonal bed, pitched by where on the page you tap. Sliding: band-passed noise following the stone's speed. Buttons: a dry paper tap. A sound toggle in the header.

8. Rules
- Arrow keys move the stone word by word, Enter taps it. The glass and size pickers are radio groups with arrow keys.
- Honour prefers-reduced-motion. Keep a readable DOM fallback when WebGL2 is missing.
- Nothing cropped at any window size.
```

Back to the [README](README.md) · [Live demo](https://sam1983aing.github.io/beryl/)
