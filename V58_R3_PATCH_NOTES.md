# V5.8-R3 Patch — Creative Director Review Fixes

Built from the deployed R2 file after reviewing the first five R2 sequences against the final presentation deck. No database changes needed — deploy is a single index.html swap. All R2 behavior preserved (verified: orientation lock, flat-lay gating, bedding 10-shot, foreground, Portra grade).

## What the review found

R2 wins confirmed: foreground styling present on every Shot 1, walkers upright on the high-angle top view, components spread and folded stack at presentation grade, bedside context correct on both bassinets, Portra warmth visible throughout.

Four gaps against the deck, all prompt-layer:

1. World lock broken between shots (green wainscot to cream room on the comforter, shiplap to plaster on the green bassinet, vanishing mural on the pink walker). Root cause: Shots 2+ only said "same environment as Shot 1" without ever restating the wall, lighting, or materials — each generation is independent and cannot match a room it was never described.
2. Overpowering background on bassinet pink Shot 1 (fully-dressed bookshelf competing with the hero).
3. No depth separation — everything deep focus front to back; the deck look is product tack-sharp against a softly defocused background.
4. Detail hallucination — invented "MAREY" brand tag on the green bassinet fabric close-up.

## Changes in this patch

Continuity spec. Locked shots (2-11) now rebuild the room from an explicit spec: the exact wall descriptor, lighting descriptor, and aesthetic materials chosen for the sequence are restated in every shot, with a hard prohibition on introducing murals, wallpaper, paneling, shiplap, shelving, or any wall treatment not in the spec.

Depth plan (Shot 1). Three named depth planes: foreground rug and props sharp, product tack-sharp (always the sharpest object in frame), background progressively and gently defocused with natural optical falloff — explicitly not heavy blur or artificial bokeh. Shots 8 and 9 camera lines get matching depth-separation language.

Background discipline (Shot 1). The wall zone behind and beside the product stays quiet: at most one secondary background element, off to one side, partially cropped, defocused. Fully-dressed bookshelves, storage units, and gallery walls behind the product are banned. Clean-wall breathing space declared desirable.

Detail fidelity (Shots 4-5). Macro shots must draw every stitch, print motif, and piece of hardware from the reference; inventing brand tags, woven labels, badges, embroidery, or printed text is forbidden. A matching anti-tag line was added to the universal negative prompt for all categories and all shots.

## Deploy

Replace index.html in the repo with this file (same rename flow as R2), wait for Pages, hard-refresh, confirm the tab title reads V5.8-R3. Then re-run the same three test SKUs — judge continuity by flipping shots 1-2-3 in sequence: the room must hold.

## Known limit worth deciding on

The continuity spec narrows room drift substantially but text alone cannot guarantee pixel-identical rooms across independent generations. The definitive fix is pipeline-level: in n8n, pass Shot 1's generated output as a second reference image on shots 2-9 (the Nano Banana Pro edit endpoint accepts multiple input images) with an instruction that it defines the room, not the product. Recommend piloting on one SKU after R3 validates.
