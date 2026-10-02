# Cloud Cafe ☁️🧁

A playful 3D cupcake-decorating toy for a five-year-old, made with **Three.js** and
**Vite**. The whole game is one file, `index.html`: models, textures, icons (inline
SVG) and sounds (Web Audio synth) are all generated in code. There are no image,
model or audio files.

A cozy kitchen floats in the sky on marshmallow clouds. It has a rainbow countertop,
bouncing utensils with little faces, bunting and a rainbow. Animal customers wait on
a cloud for their cupcakes.

## Run it

Requires Node 18+.

```bash
npm install
npm run dev        # opens on http://localhost:5173 (also on your LAN, for tablets)
```

Production build:

```bash
npm run build      # outputs dist/
npm run preview    # serves dist/ locally
```

No-install option: `index.html` has an import map that points `three` at the jsDelivr
CDN. You can open the file directly in a browser, or serve it from any static host,
as long as you're online. Under Vite, `three` comes from `node_modules` instead.

## How to play (no reading needed)

1. Press the big **Play** button. Gentle music and sounds start here, not before.
2. **Pick a cupcake shape.** Tap one of three big pictures: round, heart or flower.
3. **Pick a frosting.** Tap one of three pictures: soft swirl, puffy dollops or shiny
   drippy glaze. The pictures show the frosting on the shape you chose.
4. **Decorate.** Tap a decoration (fruit stars, googly eyes, rainbow sprinkles or
   biscuit ears), then tap the cupcake to stick it on. You can decorate as much as you
   like.
   - ↩️ **Undo arrow:** takes back the last thing you did. Keep tapping to step back
     to frosting and then to shape.
   - 🎲 **Dice:** makes a random silly cupcake. One undo removes all of it.
5. Ring the **serving bell**. A customer hops over, takes two bites and does a happy
   dance with hearts. The cupcake shrinks and floats to the parade cloud. Every design
   is a success.
6. After three customers, the animals carry their mini cupcakes on their heads in a
   **cupcake parade** with soft confetti. Tap the big pink button to bake a **new
   batch**. It comes with new colours and new customers.

There are no scores, timers, recipes or words to read. If nothing is tapped for a few
seconds, a cartoon **hand** shows what to tap next. The 🔈 button mutes all sound, and
the game remembers that setting. The utensils, animals, clouds and marshmallows also
wiggle or giggle when tapped, just for fun.

## Design notes

- **Fixed camera.** The camera always looks at the work surface. It is only re-fitted
  when the window changes shape (landscape or portrait).
- **Decorations stay on the top surface.** The frosting is a heightfield over the
  cupcake's outline. When the child taps, the tap point is mapped to that surface.
  Taps on the paper liner snap up to the top, and points near the rim are pulled
  inward. Each piece is then placed with a downward ray against the frosting, so it
  always lands on the highest point and is pushed slightly outward along the surface
  normal. Pieces can't end up inside or under the cake. Positions are stored in
  cupcake space, so undo, the random design and wobble animations keep them in place.
- **Gentle motion.** Fades and slides last at least a quarter of a second. Nothing
  flashes or strobes, and confetti falls slowly. The game also respects the
  reduced-motion setting for its CSS animations.
- **Large touch targets.** Choice cards are about 88–168 px and buttons about 68–104
  px. The bell is bigger still.

## Debug hooks

`window.__cloudCafe` exposes read-only state (mode, step, decorations, served count,
batch, palette) and a `report()` that checks every decoration piece against the
analytic frosting surface. The automated browser checks use these hooks.
