# V5.8-R2 Rebuild — Change Summary

Build date: 2 July 2026. Source: live GitHub Pages index.html (V5.8). All changes syntax-checked with Node and smoke-tested against Walker, Bassinet, Comforter Set, Crib, and Pillow scenarios.

## Root causes found in yesterday's batch

1. Missing foreground elements: the Supabase tables `foreground_floor_props_t1` and `foreground_floor_surfaces_t1` exist but are empty. When a table returns zero rows, gen() passed an empty placeholder and makePrompt silently skipped the entire mandatory foreground block. The code path was correct; the data was never seeded.
2. Products lying down in top shots: Shot 6 applied a true 90-degree top-down camera to every category. For dimensional products (walker, bassinet), the model obeys by tipping the product flat.
3. Comforter Set produced 5 images: it sat in the simple flat-textile tier by design. The 10-image presentation standard was never encoded.

## Changes in index_v58_r2.html

Foreground fix. gen() now falls back to the bundled _FP/_FS arrays whenever the Supabase pools are empty, so the Shot 1 foreground block can never silently disappear again. The Supabase pools should still be seeded (see below) so the team can edit them via the Resources tab.

Orientation system. New FLAT_LAY_SAFE gate (bedding, blankets, pillows, mattress, mats, sleeping bags/pods, nest bags, feeding pillows). Every category outside it now receives: a PRODUCT ORIENTATION — LOCKED UPRIGHT block on every shot; a rewritten Shot 6 that forbids 90-degree top-down and substitutes an elevated 35-45 degree high angle with the product standing; and new negative-prompt lines against tipped or lying products. Flat textiles keep the true 90-degree flat-lay.

Bedding uplift. Comforter Set, Cradle Bedding Set, Bassinet Bedding, and Bolster Set move from 5 shots to a 10-shot presentation-grade sequence: 1 hero, 2 straight-on, 3 clean crop, 4 texture macro, 5 construction detail, 6 overhead flat-lay, 7 Set Components Spread (every included piece laid out and countable, component list taken only from the reference image), 8 model lifestyle, 9 layered luxe, 10 Folded Stack (retail-ready). CSV headers remain shot-1 through shot-10, so n8n column mapping is unaffected.

Bassinet bedside shot. Bassinet gains Shot 7 as a no-model Bedside Context shot: bassinet beside a partially cropped adult bed in a co-sleep configuration, same aesthetic world as Shot 1. Bassinet goes from 8 to 9 shots. Shot 8 keeps its existing parent-bedroom lifestyle_context.

Aesthetic quality uplift. A Kodak Portra 400 color-grade block (warm midtones, cream highlights, gently lifted blacks, never cool or clinical) is appended to every shot. New MENA warm-lane pool entries added to both the local fallback arrays and the Supabase seeds: Desert Elegance Nursery (T1), Warm Plaster Home and Soft Sand Home (T2), Tone-on-Tone Greige Wainscoting (T1 wall), Warm Limewash Plaster (T2 wall), Gulf Golden-Hour Window and Brass Sconce Wall Wash (T1 lighting), Soft Gulf Afternoon Light (T2 lighting), linen-weave catchlight and brass patina micro cues, and the modest-luxe MENA caregiver wardrobe lane. Warm Plaster, Soft Sand, and Desert Elegance are whitelisted as mechanism-safe aesthetics. The dust-mote cue from the playbook was deliberately excluded: the standard negative prompt forbids dust particles.

Deferred per plan: Kashida Heritage (pending legal), Shot 0 ultra-wide banner.

## Deployment order

1. Seed Supabase first: open the app, Resources tab, Import, paste each JSON payload from supabase_import/ in numbered order using Append mode. Files 01 and 02 are required for foreground styling; 03-11 are the MENA additions. Alternatively run v58_r2_supabase_seed.sql (uses the description column; verify the tones column type before running).
2. Commit index_v58_r2.html to the repo as index.html and push to GitHub Pages.
3. Individual-generation test pass (per your gated process): one Walker, one Bassinet, one Comforter Set. Confirm: foreground rug plus 1-2 toys visible on every Shot 1; Walker/Bassinet Shot 6 standing upright; Comforter Set returns 10 prompts with the components spread and folded stack; warmer Portra-graded output overall.
4. Both data sources are now in sync (fallback arrays updated to match seeds). Any future taxonomy edit must touch both.
