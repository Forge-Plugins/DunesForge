# Release notes

Every entry below is drawn from the development journal written at the time,
not reconstructed afterwards. Where a version shipped a measurement, the number
quoted is the one that was measured — including the cases where the measurement
said a feature was not worth shipping and it was taken back out.

> **On version numbers.** 0.85.0 is the first public beta and starts the
> public numbering; the 1.3.x entries below it are the internal build history.
> Nothing in this list changes an existing scene unless it says so: every
> physical feature is an option that is OFF at its default, and at that default
> the solver runs the same arithmetic it ran before the feature existed.

---

## 0.85.5 — PDF manuals, Blender 5.1 minimum, English everywhere (2026-10-19)

**The manuals ship as PDF only.** The add-on package no longer carries the
Markdown and HTML copies of the guides. The help buttons, the **?** icon on
each panel and **Explain this DunesForge option** open the PDF at the right
chapter or option in Adobe Acrobat or Reader, SumatraPDF and the PDF viewers
of web browsers; other viewers open the guide at its first page. Links from
one guide to another also open the right page now, not the first one. The
guides open in English by default (**EN** / **PL** next to **Guides**).

**Blender 5.1 is the supported minimum.** DunesForge still installs on
Blender 4.2 to 5.0 and may work there, but those versions have not been
thoroughly tested; the panel says so on them.

**All messages are in English.** The Diagnose panel, the wind regime readout
and two hints were still in Polish.

**Diagnose now warns when 3D Wind runs without the GPU.** The warning checked
for a wind model that does not exist, so it never appeared.

**Beta Tester Notes:** where to look first and where to ask (the manuals, then
Discord); how to stop slopes from piling up too steep with **Settle Slopes**
(every step, 10 to 20 rounds), the cheaper alternative to **Avalanche
Trigger**; and what the wind solver modes do -- **Flow 2D** for the most
realistic wind, **Flow 3D** for even more accuracy at a much higher cost.

**The guides were checked against the add-on**: button and option names now
match the panel, demos are called by the names in the demo list, experimental
water demos are marked as such, and outdated statements were corrected.

## 0.85.4 — No more terraces on thick sand with the default settings (2026-10-19)

**Thick sand forms real dunes again with the default settings.** Since 1.3.12
a thick starting layer (about 1 m and more) tended to break into flat steps
running along the wind instead of dunes. Running the same scene on twenty
older versions found the change: in 1.3.12 several physics options started
switched on, and two of them together -- **Crest Shear Ceiling** and
**Saltation Feedback (Owen)** -- caused it. Neither does it alone, which is
why turning only one off never helped. Both, and **Slope Affects Threshold**,
which adds to it, are now off by default (0), as their own tooltips always
said. Measured on the reported scene: the along-wind banding drops from 3.50
to 1.25 (1.0 means none) and the dunes are about 70 % taller, as they were
before 1.3.12.

**The tooltips of every option that can make terraces now say so**, and the
red hint at the sand layer appears only when both options are switched on.
CROSS-SLOPE TRANSPORT stays available but is no longer needed with the
defaults. Scenes saved before 0.85.4 keep the values they were saved with.

## 0.85.3 — Demo terrain and water survive Reset; shallow water marked Experimental (2026-10-19)

**Reset Simulation brings back a demo's own landscape.** Twelve demos shape
their ground in code after Initialize -- the water scenes (pond, waterfall,
wadi, sand dam, puddles, sluice), the four avalanche slopes and the two
gravity walls. Reset used to bring back the flat sand from before the
sculpting, and Initialize generated it again. The sculpted ground, with its
rock floor, is now the scene's start: Reset and Initialize give it back.

**Reset clears the flowing water, and Initialize puts the demo's starting
water back.** The shallow-water field used to survive Reset, so a scene
initialised again kept the previous run's water.

**Building a water demo a second time no longer fails** with "Height array
size ... does not match vertex count" when Render Subdiv was left above 1.

**Reset with Render Subdiv above 1 no longer loses what was painted before
Initialize.** Rebuilding the terrain to the simulation grid dropped the
masks painted before Initialize, the committed start and their copies; they
are now carried across.

**Shallow water is marked Experimental** -- the Shallow-Water Simulation
switch, its tooltips and the eleven demos that use it, with a note in the
panel. It needs deeper integration and will probably arrive in a version
after the first release (1.0). The simple water level, rain and moisture are
not affected.

**Wind Sweep now defaults to Exact Bilinear** (the wind at its true angle,
no snapping to 8 directions), and every demo uses it. On the GPU it costs the
same; on the CPU the add-on still falls back to the 8-direction sweep above
its work limit. Scenes that never changed this setting use the new default
when opened.

## 0.85.2 — GPU install for Windows user names with Polish letters (2026-10-18)

**The GPU backend now installs when the Windows user name contains letters
such as ł, ó or ż.** Testing the installer on a brand-new profile under such
a folder showed every library downloading and installing, and then NVIDIA's
compiler stopping with "cannot open source file cupy/complex.cuh": it cannot
read files under a path with those letters. The earlier workaround (the
short 8.3 name Windows can give a folder) only helps where Windows creates
those names, which is not everywhere. The installer now falls back to a
folder link with a plain name in `C:\ProgramData\DunesForge\gpu-links`; the
libraries stay in your user folder. The same happens for the temporary folder
and CuPy's kernel cache, both of which also live in the user folder: the
compiler opens each kernel from there. The same files through those links
pass the whole GPU check. Found on Blender 5.2 (CuPy 14) and 4.4 (CuPy 13).

**A failed GPU check now names its cause** instead of the compiler's last
line ("Compilation terminated."), and the full compiler message is written
to the installation log that the error points to.

No solver arithmetic, no physical defaults and no scene contents changed.

## 0.85.1 — Activity log, beta notes, add-on conflicts (2026-10-18)

**The log now records what you did.** tydunes.log lists every DunesForge
button you press, the settings you changed before pressing it, how it ended
and how long it took, and the full error when one fails. Caught errors during
playback, which used to appear only in the system console, are written there
too (a failure repeating every frame is written once per 30 seconds, with a
count). Nothing is written per frame or per redraw; a logged button press
costs about 7 ms (measured), most of it making sure the line is on disk.

**Beta buttons at the very top of the panel:** **Beta Tester Notes** (what to
try, known limitations, what the log contains) and **Report a Bug** (the
GitHub issue forms). Under **Open Log Folder** the panel now asks to attach
tydunes.log to every report.

**Troubleshooting** gained a section on other add-ons: add-ons that bring
their own CUDA libraries (a known case is BlendLuxCore), a NumPy changed by
another add-on, and old copies of DunesForge3D. Also new: what to do when
Install from Disk fails and when the GPU download is blocked by a firewall.
Getting Started gives the real size of the GPU download (about 1 to 2 GB).

**The rest of the add-on now says DunesForge too:** the add-on conflict
message, the painting and wind-gizmo status bar text, GPU messages and the
log header.

**Tester packages are numbered:** every build also leaves
`DunesForge_<version>_beta.zip` and its SHA-256 next to it.

No solver arithmetic, no physical defaults and no scene contents changed.

## 0.85.0 — First public beta (2026-10-18)

**Version numbers restart for the beta.** 0.85.0 is the first build handed to
beta testers and follows internal build 1.3.221; everything below this entry
is the internal history that led to it. The add-on's identifier does not
change, so installed copies update in place and saved scenes open as before.

**The add-on is now called DunesForge everywhere you see it** -- the name in
Blender's add-on list, the sidebar tab, panel headings, tooltips, messages
and every manual. Internal names stay as they were: objects, materials and
node groups created by the add-on keep their `TyDunes_...` / `TyDunes ...`
names, so scenes saved before this version keep working.

**New manuals for testers:** a short **Getting Started** (ten minutes from
install to your first dune field) and **Troubleshooting** (the most common
problems and what to do about each), in English and Polish. The QuickStart
and Installation guides lost their old per-version notes and now start with
the current beta.

No solver arithmetic, no physical defaults and no scene contents changed.

## 1.3.221 — Every demo scene file rebuilt (2026-10-14)

All 71 saved demo scenes are rebuilt with the current version. Eleven of the
shipped files dated from August and were missing switches added since (dust
on snow, Water Wets The Sand, splashes, tide following, bodies of water and
others), and 25 demos had no saved file at all; every one now opens with the
features its card describes.

Three Grand Scenes and the "What Objects Tell The Sand" showcase could not be
saved before: the check that guards which demos wear the surface shader did
not know that a Grand Scene wears what its base scene wears, nor that the
ground-rules showcase deliberately uses its own flat material. Both rules
are now stated where the check reads them.

No solver arithmetic, no physical defaults and no scene contents changed.


## 1.3.220 — Five new shallow-water demos (2026-10-14)

New in the Demo Scene list, all built on the shallow-water field:

- **Water: Puddles After Rain** — wet sand after a shower, with still,
  mirror-flat puddles in every hollow. Each hollow is found exactly and
  filled part-way to where it would overflow, so every puddle has a level
  surface and a dry rim.
- **Water: A Pond Among Dunes** — the dunes grow first, then the lowest
  ground between them holds a pond with an irregular, wet shore. Press Play
  and the dunes keep migrating around (and into) it.
- **Water: Waterfall From A Lake** — a lake on a rock plateau spills through
  a notch over a cliff into a plunge pool and runs off downstream.
- **Water: Flash Flood In A Wadi** — a wall of brown water released at the
  head of a dry, winding canyon; the flood runs down the sand bed and cuts a
  channel that stays after the water has gone.
- **Water: A Sand Dam Breaks** — a pond brims just over the lowest point of
  its sand dam; the overflow scours a breach and the pond drains through it.

The add-on has no water source, so each scene places its water at the start
and leaves the rest to the physics: no scripted motion anywhere. Water volume
is conserved in every scene until it leaves through the open downstream edge.

**Fixed: water erosion cut through the bedrock.** Erosion by flowing water
did not know where the loose sand ends: it kept lowering the bed for as long
as the flow could pick grains up, rock included, and with Erosion Target set
to The Dune Terrain it wrote that into the dunes. Found on the sand-dam
scene, where 30 seconds of water dug 10.8 m into the rock under the dam. The
bed now goes down to the bedrock and no further; deposition is unchanged, and
wherever the flow never reaches the rock the result is exactly as before. The
dune physics without water erosion is untouched (physics fingerprint
identical).

## 1.3.219 — Simulation no longer stays frozen after the Airborne demo (2026-10-13)

**Fixed: after the Airborne (dust) demo, Play did nothing.** That demo freezes
the dunes after its warm-up on purpose — the dunes hold still while the dust,
dust devils and the wind keep moving — but nothing turned the two freezes off
again. Measured: after it, a new terrain + Initialize + 20 frames of Play ran
0 solver steps and moved nothing (without the demo: 100 steps); loading
another demo afterwards cleared Freeze Dunes but left Freeze Terrain on, so
that demo was frozen too.

- every demo now starts unfrozen; the Airborne demo still freezes itself
  after its warm-up, so it looks and plays exactly as before;
- Create Terrain turns both freezes off for the new terrain and says so;
- whenever either freeze is on, a red-tinted **DUNES FROZEN** box sits right
  under Play with both switches, so a frozen scene can no longer look like a
  broken one.

No solver arithmetic and no physical defaults changed.


## 1.3.218 — CROSS-SLOPE TRANSPORT: easy to find, checked on every demo (2026-10-12)

The anti-terrace option from 1.3.217 stays **off by default**, but it can no
longer be missed:

- its name is now in capitals, **CROSS-SLOPE TRANSPORT**;
- in the Physics panel it sits in its own box, tinted red while it is off,
  with a one-line "when to use it";
- in the starting-sand section, a thick layer (Even sand layer 1 m or more,
  or a terrain that is itself loose sand) shows a tinted hint with the
  slider right there.
- it is shown in every UI mode, including Simple (what a new scene opens
  in) -- it used to be hidden there.

Why not on by default: it was run on all 66 demos at 0, 0.1 and 0.2. No
errors anywhere and the wind-parallel steps never increase, but it also
softens small, young dunes and fine detail (Surface Detail, Dune Field demos)
and lines up the grid noise behind obstacles into cross-wind combs (Desert
demo). On a thick layer at 512 the terraces are even stronger than at 128
(9.1 % of the area) and 0.1 removes them there too (2.2 %).

Also: the term now reads the sand flux averaged over the neighbouring cells
(a saltation hop is longer than a cell), so the flux's own grid ripple is
not printed into the bed. Only affects scenes with the option on.

## 1.3.217 — Cross-slope transport: no more terraces on thick sand (option) (2026-10-12)

**New: Cross-slope Transport** (Physics, under Cross-wind Shear Smooth).
Reported: on a fresh terrain with a thick starting sand layer (4.5 m) the field
grew "plateaus instead of dunes". Measured first that nothing had regressed --
the same scene is bit-identical in every release back to 1.3.100, on GPU and
CPU alike. The cause is a missing term: the transport sweep moves sand along
the wind one streamline at a time, so neighbouring grid rows grow their crests
in different places and the field breaks into TERRACES, steps running parallel
to the wind with planar ramps between them. The thicker the layer, the worse
(cross-wind jaggedness 0.91 / 1.44 / 2.84 / 3.50 at 0 / 1 / 2 / 4.5 m).

The option adds what real saltating sand does: grains landing on a cross-slope
bounce down it, so part of the flux runs downhill across the wind. It acts
only where sand is moving and only across the wind, conserves mass exactly and
never digs below bedrock. Measured on the reported scene (600 frames):
wind-parallel walls drop from 5.1 % of the area to 2.5 % at 0.1 and 0.6 % at
0.2, with dunes 10-13 % lower; the result is continuous, sinuous transverse
dunes. 0.1-0.2 keeps the crests wavy, 0.5 and up makes them long and straight.
On a GPU it is one fused pass, about 0.5 ms per substep at 1024.

Off by default: every existing scene simulates exactly as before.

## 1.3.216 — Windows GPU paths (2026-10-11)

Use Windows short directory names when available for private CuPy/CUDA loading. GPU verification includes larger indexing and CUDA/CUB header compilation. Reproduced an actual first-step compilation failure in a long path on Blender 5.3; the corrected backend completed terrain, water and checkpoint checks. Documentation synchronized with the current controls.

## 1.3.215 — Smooth bedrock edges (option) (2026-10-09)

**New: Rock Edge Smoothing** (Surface Shader, under Bedrock Threshold). Where
bare bedrock meets sand, the outline used to follow the triangles of the
terrain mesh and showed as a pixel staircase up close: the sand mask flips
from sand to rock within a single cell. With smoothing above 0 the rock/sand
decision is worked out per vertex, smoothed over that many cells and drawn
through the middle, so the outline curves instead of stepping. Measured on a
round patch of bare rock: the rock outline is about 5× smoother at 1.0 and the
pale thin-sand band around it about 2.5×, with the edge staying in place.

It is off by default and leaves every existing render exactly as it was. Use
it for large areas of exposed rock; on finely speckled rock (specks one or two
cells across) or one-cell cliff walls it fades the specks and can draw a
sawtooth on the walls, so leave it at 0 there.

Also: the Bedrock Threshold description no longer says the sand mask is
normalised to the field's range — it is sand depth divided by Sand Cover Depth.

No solver arithmetic and no physical defaults changed.


## 1.3.214 — Water demo review (2026-10-09)

- Added existing sand-bed friction to Wind On The Lake and Dynamic Coast; field resolution and global solver defaults unchanged.
- Ice Pack uses partial floe rigidity so fractures and leads can form.
- Corrected five demo-list descriptions and removed the stale Water Flow scene-card override.
- Updated PL/EN manuals and runtime control index; rendered and measured water demos with controls.

## 1.3.213 — GPU installer robustness (2026-10-08)

GPU installer: repair damaged settings with backup, respect Blender Online Access, cancel verification, recover from worker/UI errors. PL/EN manuals synchronized.

## 1.3.212 — Review fixes and isolated GPU installation (2026-10-08)

GPU installation now selects CuPy compatible with Blender's bundled NumPy,
downloads CUDA component wheels and installs into a private user directory.
Blender's NumPy and bundled packages are never replaced. A generation becomes
active only after array, NVRTC kernel and FFT checks. Downloading runs in a
cancellable worker; missing NVIDIA support or a failed download leaves CPU usable.

Additional mask collections refresh after selection and object movement, including
the final throttled drag. Cancelled starting-sand painting restores previous flags.
Start / Transport Variation accurately describes both initialization and continuing
transport, and is visible for all terrain start modes. Numeric checkpoints restore
Flow2D and Flow3D state; legacy checkpoints restart those fields. Failed solver steps
restore temporary time scale and warm-up scope. Two CPU-only residual reads now use
the common host-array helper. Numerical formulas and physical defaults are unchanged.

Offline guides and their complete control indexes are synchronized in MD, HTML and
PDF, including the expanded English QuickStart and current Simple/Standard/Advanced UI.

## 1.3.211 — Avalanche demos build faster; same result to the bit (2026-10-07)

**The avalanche demos were slow to build, not to render.** Measured: the
Avalanche Valley demo took 17 minutes after Create Demo and 6 seconds to
render, and 91 % of the build was the slab-flow solve — 46,515 small time
steps, the largest avalanche alone 12,000. Three changes cut the cost of each
step without changing a single number in the result:

- a zero shift of a grid no longer copies the whole grid;
- the terrain's slope for the avalanche friction is worked out once per
  terrain instead of at every step;
- each step reads its time-step limit back from the GPU once instead of three
  times.

The deposit of a test avalanche is bit-for-bit identical to 1.3.210 on both
the CPU and the GPU, and each step is about 1.25× faster. The demos are still
long to build (Avalanche Valley ≈ 14 min, Avalanche and Avalanche Gates ≈ 3–4
min); a larger speed-up needs the avalanche solver itself to be rebuilt and is
planned separately.

No solver arithmetic, no physical defaults and no scene contents changed.


## 1.3.210 — Protection against add-ons that use DunesForge's internal names (2026-10-01)

**An older DunesForge3D can no longer silently take DunesForge over.** Up to
2.56.4, DunesForge3D stored its settings under the same internal name as
DunesForge (`Scene.tydunes`) and used the same ids for 19 panels and 111
operators. Blender does not refuse a second add-on with the same names — it
quietly replaces the first — so with both enabled, whichever started last took
the other's settings, panels and buttons. Current DunesForge3D builds use names
of their own and work side by side with DunesForge; this release protects users
who still have an older copy installed.

- **Older DunesForge3D enabled first:** DunesForge now refuses to start and says
  why — update DunesForge3D, or disable it. Nothing is half-installed, and the
  other add-on keeps working.
- **Older DunesForge3D enabled after DunesForge:** a warning appears once, in a
  popup and in the system console. (In background renders it goes to the
  console only — a popup would crash Blender there.)
- **Disabling DunesForge after such a takeover** no longer stops halfway, leaves
  the other add-on's settings alone, and DunesForge can be enabled again in the
  same session.

No solver arithmetic, no physical defaults and no scene contents changed.


## 1.3.209 — Code review: presets carry settings only; Standard shows the phenomena (2026-10-01)

**A user preset could delete your terrain — fixed.** Save User Preset wrote
the list of objects that Remove Demo Scene is allowed to delete. A preset saved
in a demo scene and loaded into your own scene switched Remove Demo Scene on,
and the button then deleted your objects whose names matched the demo's —
including `TyDunes_Terrain`, the name Create Terrain gives every terrain.
Measured on 1.3.208: 3 of 3 such objects deleted. Load User Preset also accepted
`is_initialized`, `is_playing` and the step counter from a hand-edited file,
leaving the panel claiming a simulation that did not exist.

Presets now carry settings only, in both directions: one table of scene-state
keys is skipped when saving AND ignored when loading, so presets saved by older
versions are safe too. The report says how many such keys were ignored. The
Simple / Standard / Advanced choice is no longer stored in a preset either —
like the preview grid, it is your own preference, not part of a look.

**Standard mode now shows Extra Phenomena and Multi-Fraction.** 52 controls —
tides, snow layers, slab avalanches, playa, desert pavement, the two-grain
model — were declared as Standard controls but sat in panels drawn only in
Advanced, so in Standard they could not be reached, and the "active hidden
features" notice did not mention them either. Seven demo scenes rely on them.
Both panels are now part of Standard. Thirteen performance and debug controls
had the opposite mismatch and are now marked Advanced, matching their panels;
what you see in each mode does not change for them.

A new check fails if a control is ever declared at a lower level than every
panel that draws it, and a new gate (`test_preset_scene_state.py`) runs the
demo-preset scenario end to end, including a control that must see the
deletion.

No solver arithmetic, no physical defaults and no scene contents changed.


## 1.3.208 — Framing for the two flat Grand Scenes (2026-09-30)

**Living Oasis** and **Dynamic Coast** now have their own camera framing, and
the other two Grand Scenes are untouched.

The four 400 m scenes shared one composition, and it suits the two that have
relief: in Desert Expedition and Winter Storm the dunes and drifts close the
distance by themselves. A rain-fed basin and a shoreline are flat, so from the
same height — 0.30 of the scene size, 120 m up — they read as coloured patches
with the far rim of the plate across the top of the frame. The camera now comes
down to 0.11 and moves in, on a 50 mm lens instead of 43 mm.

Two measurements were taken before this change and BOTH ruled out the obvious
explanations, which is why they are recorded here rather than quietly dropped:

- **Background area is not the problem.** The scenes that read correctly carry
  MORE of it: Desert 40.0% and Terrain: Mesa 40.4% of frame, against 26.1% for
  Oasis and 25.3% for Coast. Sky above a horizon costs nothing.
- **Neither is the visible edge of the domain.** Projecting the terrain
  boundary into the frame puts 37 of 84 sampled points inside the image in
  EVERY scene measured, good-looking or not.

What was actually wrong is not something either number captures: from directly
above, a flat scene has no form to show. The fix is to frame what these two
scenes do have — the waterline, the shallows and the shore — which is a
question for the eye, and was settled by rendering three candidate framings and
looking at them.

No solver arithmetic, no physical defaults and no other scene changed. The
shared framing function is untouched; this is an additional branch for the two
scenes, exactly as Winter Storm already had one.

## 1.3.207 — Complete Simple workflows (2026-09-28)

- Group starting terrain, starting sand and appearance in workflow order.
- Keep fallback sand depth editable when the map has no image or painted field.
- Add a dedicated starting-sand brush action; keep physics and existing values unchanged.
- Make textures, own presets, wind model, collections, water presets and basic water appearance available in Simple.
- Add compact Simple export; move dune checkpoints from Debug into Export for all modes.
- Promote independent water bed/drainage to Standard; expand hidden-feature notices.
- Correct Random Start Height wording to match the generator, not the old coverage tooltip.

# DunesForge — Changelog

What changed, version by version, in the words of what it means for your
scene. Every entry is drawn from the development journals written at the time,
not reconstructed afterwards; where a version shipped a measurement, the number
is the one that was measured.

**How to read the version numbers.** The third number is a shipped build:
tests, audit and a bit-for-bit fingerprint of the physics all pass before the
number changes. A build that only adds documentation says so.

**Nothing in this list changes an existing scene unless it says so.** Every
physical feature is an option that is OFF at its default, and at that default
the solver runs the same arithmetic it ran before the feature existed.

---


## 1.3.206 — Shallow Water integration and demos (2026-09-28)

- Water advances per actual simulation step; refreshing the display never advances it.
- Recreating a water plane preserves the existing simulated surface.
- Dry obstacles no longer become water walls. Dry shoreline corners extend the adjacent water surface for rendering only; simulated wet heights and volume are unchanged.
- External water beds respect world transforms; Height Scale remains a visual exaggeration of model heights instead of adding water to the field.
- Step Water refreshes obstacle masks, including a manually moved sluice gate.
- The sluice demo uses a bottom outlet and a continuous descending chute, with the sculpted bed published before gate-mask generation.
- Flowing Pools starts with a volume-preserving level imbalance. Ice Pack includes water/ice warmup. Grand Coast enables sediment erosion and rain input to the water field.
- Demo instructions describe the actual forcing and water clock instead of stale measurements.

## 1.3.205 — Review fixes (2026-09-28)

- Protect existing terrain and painted masks when a heightmap fails to load.
- Clear Bake Files now matches numbered frame filenames only; unrelated names are preserved.
- Image brushes use a private temporary image, preserving images used by materials.
- Undo/Redo restores flow speed, flow direction, snow sources, avalanche and slab-weakness paint, including empty masks.
- Checkpoints preserve the missing painted/source input fields, sand maps and obstacle porosity, and validate vector fields before changing the solver. Older numeric checkpoints remain readable.
- Repeated preview refreshes no longer advance physical ripples; actual solver steps advance them.
- Recording masks uses the scene being recorded. Evolution Check reports actual CPU changes.
- User presets restore Image and Collection references as well as Object references.
- Absorb Mesh and mask refresh handle Proxy resolution. Reset to Commit accepts an all-zero terrain.
- Reapplying a shader variant preserves the original procedural settings for restoration.
- Fast Surface builds with vegetation. Splat Surface combines a custom normal map with the set normal. Hex scan tiling applies per-set Tint.

Validation: 232 test programs passed; all 15 review reproductions checked in Blender 4.2.3 LTS and 5.3 Alpha; 12 GPU terrain fingerprints unchanged from 1.3.204. The alpha check is not a stable-version support declaration.

## 1.3.204 - Complete Simple workflows and offline publication guides (2026-09-22)

- Corrected obsolete shader-variant help: rain and displacement are not categorically incompatible; retained scene/version-dependent limits.

- Simple exposes heightmap import, scale/offset, edge clamping and the core brush controls, including conditional stamp inputs. Terrain and starting-sand preparation precede initialization. Standard exposes sand-map blending and preview choices; solver internals remain in Advanced.
- Removed an obsolete brush guard that rejected pre-initialization sand painting despite the existing staging path. A real Blender probe enters painting, writes a 2 m layer and reads 2 m after Initialize; disabling its reader gives 0 m.
- Added offline QuickStart with eight tutorials, feature applications and a complete runtime control/action dictionary, installation, product copy and a demonstration kit. Each document is available as Markdown, PDF and linked HTML. Polish practical guides retain the original English tooltip dictionary alongside the full English edition.
- QuickStart and All Features appear at the top. Panel help opens the matching chapter; right-click help explains the exact DunesForge setting. PL/EN guide selection does not change simulation values.
- Documentation uses actual rendered demo images and states the limits of the heightfield model and tested compatibility; no external store publication is performed.

## 1.3.203 — State safety, complete UI workflows and large demonstrations (2026-09-20)

- Checkpoints use numeric RNG data. Legacy RNG arrays are imported through a restricted reader; arbitrary pickle callables are rejected. Invalid field layouts and values are checked before live state is written.
- Checkpoint restore and DunesForge Undo/Redo rebuild composed masks. Deposit masks survive checkpoints.
- File load and add-on disable release all solver, water and undo state. Scene renames migrate runtime ownership; demo removal preserves objects in other scenes and shared objects.
- Simple and Standard keep actions with their required inputs. Sand-map input and placement controls are available with their switches; preset/variant choices and map export folders are available in Standard. Important hidden active systems are listed near the interface selector.
- Demos and presets have a start block near the top. Panels follow the authoring workflow; advanced numerical tuning stays out of Simple. The existing top-level reset and experimental controls are retained.
- Reframed Dune Field, Ground Rules, Airborne and Water Flow demonstrations, shortened their cards and improved material/particle presentation. Snow vortices receive explicit visual warmup and snow colours. Dust demos raise Cycles transparent-ray depth to avoid black overlap artifacts; removing a demo restores the previous depth. Ground Rules identifies each simulated mask footprint with an outline.
- Four 400 m multi-system scenes: Desert Expedition, Living Oasis, Winter Storm and Dynamic Coast. They combine compatible systems rather than enabling mutually exclusive models, file operations or withdrawn experiments. Their explicit feature manifests are in `ui/demo_showcases.py`.
- No solver arithmetic or physical defaults changed. Existing scenes keep their stored parameter values when changing interface level.


## 1.3.202 — Five of the last six features get a scene

Five features that had no demo now have one, and every one of them went into a
scene chosen because it MEETS THE FEATURE'S CONDITION, not because the name
sounded right. Coverage: 157 of 215 to 162.

**Terrain: Plateau** gained **Edge Flat** — a border band pinned to the starting
terrain, so a simulated tile blends into the landscape around it instead of
ending at its own edge. It is measured in GRID CELLS, not metres or percent.
This scene has the thickest sand of the rock terrains (1.2), so the edge really
does move and there is something for the pin to hold. MEASURED over 200 steps:
max|dH| inside the 24-cell band is 0.00000000 with the option against 1.292094
without it, while the middle of the scene moves by 1.377433 either way.

**Terrain: Mountains** gained **Strict Sand Budget**. The feature matters ONLY
for thin sand over bedrock, and this is the one scene where both halves of that
condition hold: rock underneath, 0.35 of sand on top, and a RIDGED relief, so
there are crests for the wind to blow along. Without it the flux scan and the
upwind sampling disagree off-axis and the solver CREATES sand. MEASURED over 200
steps: the sand mass grows by +2.979% with the budget open and by +0.325% with
it closed — nine times less phantom sand. It is a large reduction, not a cure,
and it costs about 3.5x per solver step, which is why it stays an option.

**Terrain: Dune Field** gained **Terrain = Sand**. The scene starts from
DUNE_RIDGES — a dune field used as the STARTING RELIEF. Until now that relief
was bedrock: the wind dusted fresh sand over it, but the shapes themselves stood
still forever. With this on they are loose sand and do what dunes do. MEASURED
over 800 steps: 19.83% of the surface drops BELOW the bedrock floor, against
0.00% without the option, which is impossible by definition — and the control's
deepest point is exactly −1.200000 m, the thickness of its own sand layer. The
deepest cut goes from −1.200 m to −2.996 m.

**Terrain: Mesa** gained **Combine with OR**, and the placement filters it needs
to mean anything — no other scene uses them. A mesa is the right host because it
has three different answers to "is there sand here": low ground, flat shelves on
top, and steep walls between. AND means low AND flat, so sand collects only at
the foot. OR means low OR flat, so the shelves get sand too and only the walls —
steep AND high at once — stay bare, which is what you see in the field.
MEASURED: sand coverage 0.1473 with AND against 0.9617 with OR, 6.53x.

**Not shipped: Cloud Roughness (Owen)** was fitted to Showcase: Grain Systems
and then taken back out, and the reason is not that it failed. It worked:
MEASURED at 3.0 over 200 steps, terrain movement 0.314147 against 0.274531 in
the control, +14.4%. It comes out because the project holds a rule that no demo
and no preset touches the realistic defaults, and this parameter sits on that
list as the one entry deliberately left at zero. The owner's reason is itself a
measurement: the earlier Owen hypothesis is largely subsumed by the Saltation
Feedback that is already on (relief 8.32 with it against 8.26 without), and a
second knob reproducing the first is duplication rather than realism. The two
measurements do not contradict each other — they are different quantities — so
the rule stands and the demo does not work around it.

**Snow: Drifts + Fence** gained **Needs Fresh Snow**, and the dust devils it
needs to show it. The vortex normally draws its strength from loose SAND, which
a snow scene does not have; with this it reads FRESH snow depth instead, so it
kicks up where new snow has drifted and dies over wind-packed crust. MEASURED
over 300 steps, sampling the fresh-snow depth UNDER the devils: 0.000293 without
the option against 0.004820 with it — 16.5x. The control is not random, and that
is the physics of the scene rather than a flaw in the measurement: fresh snow
banks up BEHIND the fence where the wind drops, while devils travel where the
wind is strong, so without the coupling they are ANTI-correlated with the snow
(0.06x of the domain average). The option removes that avoidance rather than
hunting for the deepest drift.

Nothing here changes an existing scene's physics. The fingerprint is unchanged:
108 leaves, 0 differences.

---

## 1.3.201 — Four more features get a scene, and two get withdrawn

**Showcase: Ground Control** gained **Bedrock Smoothing**. The bedrock slab was
stamped into the terrain as a vertical wall, and the simulation then had to live
with that step — the same flaw Sand Source Smoothing fixes on the supply side
and Wind Gain Smoothing on the wind side. MEASURED: the rim gradient falls to
0.228121 with the feather against 1.600000 without, so the edge is 85.7%
gentler. The flat top of the slab is untouched; the feather applies to the rim
slope alone and adds nothing.

**Terrain: Canyon** gained **Canyon Channelling**, and it is the only scene
where that term means anything. The feature is NOT a flow solver — it is an
analytical squeeze that needs walls on BOTH sides of the wind, and a single
cliff face speeds nothing up. Terrain: Canyon leaves Wind Model at its
analytical default and has a canyon with two walls. Flow: Canyon does not
qualify despite the name: it stands on FLOW2D, where the acceleration through a
narrows already comes out of the solver itself.

**Showcase: Moving Air** gained **Freeze Dunes** and **Freeze Terrain (haze
flies)**, set AFTER the warm-up, and the ordering is what makes the scene exist.
Freeze Dunes reaches the solver, so set in the branch it would have frozen the
warm-up too and the scene would open flat, without the dunes it is meant to
show. MEASURED: max|dH| over 40 steps with both freezes is 0.00000000, against
0.80208252 for the same scene without them. Both are invisible in a still frame
on purpose — you see them when you press Play: the dunes hold while the dust,
the devils and the wind clock keep running.

**Pools That Flow** gained **Water Wets the Sand**. Until now that conversation
went one way: the sand told the water where the ground was, and the water told
the sand nothing, because wet sand came from ONE GLOBAL LEVEL. A pool the rain
had cut into a ridge did not exist as far as the wind was concerned. MEASURED:
the wet-sand field the option builds covers 70.1% of the grid and varies by
0.4553 across it; with the option off the field is not built at all.

**Withdrawn before shipping: Fast Ripple Solver.** It was fitted to Showcase:
Grain Sizes and then taken back out. The reasoning was sound — the feature
promises 4.4x where the ripple wavelength varies across the grid, and that scene
has both sources of variation at once. The measurement disagreed. Over 40 steps,
WITH a warm-up and with the two arms run in both orders, the option gave 4.13 s
against 4.09 s without it: 0.99x, a hair SLOWER. An earlier figure of 5.55 s
against 4.13 s was itself wrong — measured without a warm-up, so whichever arm
ran first paid the one-off cost. A demo shows what you can see; an option that
promises a speed-up and does not deliver one in the scene has nothing to show.

**Withdrawn before shipping: Water Leaves at Y Edges** — and this one is a
verdict about the SCENE, not about the feature. A reservoir behind a sluice gate
is the one demo where water has somewhere to go, but it goes along X, which
Water Leaves at Open X already handles there; the Y edges are the sides of the
channel. MEASURED at the demo's opening frame: 0.372203 of water in the domain
and 13.30% of cells wet, with depth at the Y edges of exactly 0.000000. The
water sits in the basin, whose ellipse spans 0.20 to 0.80 of the domain in Y and
never touches the edge. What the state looks like after the gate lifts was NOT
measured and nothing here claims otherwise: the gate is keyframed, keyframes are
read by frame playback, and in background Blender frame stepping does not
advance the simulation.

Nothing in this version changes an existing scene's physics. The fingerprint is
unchanged: 108 leaves, 0 differences.

---

## 1.3.200 — The last four object slots

The remaining object slots now have a demo, which completes the set of seven.

**Showcase: Object Masks** gained three of them, which suits a scene whose job
is to be a catalogue of object-driven masks:

- **Use Wind Gain Curve** — the same wind channel as the gain volume already in
  that scene, but steered by a curve, and the two MULTIPLY rather than compete.
  MEASURED: 49 164 cells above gain 1 with the curve against 27 234 without,
  so the corridor adds 21 930.
- **Objects Leave Tracks** — a log pressed into the surface. This is the only
  mask in the scene that reads an object's SHAPE rather than its footprint from
  above: it stamps just what touches the ground. MEASURED: the ground under the
  log sits 0.0108 below its surroundings against 0.0010 without the option —
  eleven times deeper.
- **Use Wind Object** — and this one ships as an option with ZERO IDENTITY on
  purpose. It TAKES OVER the wind heading from the number in the panel, so set
  at any angle it would quietly restyle a scene someone had already signed off.
  The empty is oriented so it returns exactly the heading the panel already
  had: measured 0.0000 against a Wind Direction of 0.00. You get a handle to
  rotate or keyframe, and nothing changes until you do.

**Terrain: Islands + Water** gained **Water Bodies (several levels)**. That
scene already has a sea, so the slot adds to an existing phenomenon instead of
changing what the scene is about: three ponds sit at their own heights on the
islands, above the single Water Level that every uncovered cell still uses.

**Two things the scene had to learn about the imprint**, both recorded in its
comments. The log was first placed at a fixed height and ended up BURIED — its
underside half a metre below a surface averaging 0.429, with a Contact Gap of
0.15, so nothing touched anything. Terrain height is not knowable before the
warm-up, so the log is now seated on the solver's own heightfield afterwards.
And the live imprint runs in the frame handler — it marks while the scene
PLAYS — whereas a demo's warm-up calls the solver directly. The scene therefore
stamps once explicitly, with the same operator the panel offers, and takes
twelve settling steps.

Demo coverage of the feature set: **152 of 215**, from 148.

---

## 1.3.199 — Showcase: What Objects Tell The Sand

A new scene for three object slots that had no demo: **Use Bedrock**, **Use
Erode Objects** and **Use No-Deposit Objects**. Three slabs on one moving sand
sheet, each holding the ground to a different rule — and the middle pair is
the reason the scene exists, because the No-Deposit tooltip says outright that
people confuse it with Erode.

Every number in the card and the tooltip was measured on the scene:

- **BEDROCK** is a floor the sand cannot sink below. This one sits above the
  sheet, so it stands as a rock sill **1.43 m** proud of the field.
- **ERODE** is the quiet one, and not what the first draft of this card
  claimed. Its roughness is **0.0025 against 0.30** for the sheet beside it —
  a hundred and nineteen times flatter. Dunes travel past a patch that never
  changes.
- **NO-DEPOSIT** is the opposite: nothing new lands while whatever is there
  blows away, so the footprint **sinks 0.12 m after the warm-up and 0.38 m
  after 300 steps more**, deepening as it goes.

With the three slots switched off, the same three patches are ordinary ground:
the erode patch is 1.2x smoother than its surroundings rather than 119x.

**The card was rewritten after the measurement contradicted it.** The first
version promised a mound over the erode slab, following the tooltip's "+1.4543 m
over 200 steps". That figure describes a PEDESTAL, which is left behind as the
surroundings deflate — and this solver conserves sand mass, so the field does
not deflate as a whole and no mound can form. What the slab actually does is
hold still, which is a better demonstration anyway and is now what the scene
claims.

Demo coverage of the feature set: **148 of 215**, from 145.

---

## 1.3.198 — Four options moved into scenes that already had their conditions

No new scene this time: four options went to the scenes where their phenomenon
already exists.

- **Showcase: Object Masks** gained **Wind Gain Smoothing**. The scene has
  feathered the SAND SOURCE rim since 1.3.191, while the wind-gain stamp still
  ended in a vertical wall — the same defect on the other mask. MEASURED as the
  mean gradient along the stamp rim: **0.271 with it on against 1.900 without**,
  so the rim is about seven times gentler.
- **The avalanche family** gained **Crown Line**. The scar mask is a FOOTPRINT —
  every cell that let go reads 1 — so the material painted a solid patch where a
  mountain shows a sharp edge above bare bed. The option fades the interior and
  keeps the rim; the tooltip's own numbers say why it matters, since the rim is
  only 9.8 % of a wide slab.
- **Rain: Drops On Water** gained **Impact Squash**, which flattens a droplet
  along its own flight path. That scene is a close-up of the impact layer, so it
  is the only one where a single droplet's shape is visible at all.
- **Frost: Dawn** gained **Frost Binds The Surface** and **Patchy Thaw**.

**That last one is a correction to 1.3.191.** Patchy Thaw was added then to
*Frost: Hoar on Snow*, because the name matched. It does nothing there: `hoar`
and `frost` are two different things in this solver — hoar is the surface layer
that grows ON SNOW and becomes the weak layer a slab releases over, while the
frost FIELD only forms on dew. MEASURED across the three cold scenes: Frost:
Dawn carries frost over **62.5 %** of the field and Freeze/Lock over 61.2 %,
while **Hoar on Snow has no frost field at all**. Both options now sit in Dawn,
where there is frost to bind and to thaw, and Hoar on Snow carries a note saying
why they are not there.

Demo coverage of the feature set: **145 of 215**, from 141.

---

## 1.3.197 — Vegetation: Rain-Fed, a scene that is its own control

**A new scene where nothing grows by itself.** Veg Growth is set to zero, so
every plant you see was put there by rain. That is not a flourish: Growth From
Rain is a *multiplier* and cannot lift a single stem at zero growth, so **Rain
Seeds Growth** — which adds its own rate from the root zone — is the only term
in the scene that can. The viewer can check that with one slider, and the
measurement says the same thing: cover **0.1111 with the option off against
0.2758 with it on**, after 150 steps of steady rain either way.

**Spread sits at 1.0, and that is what keeps the ground bare.** Growth then
happens only where plants already are, so the clumps thicken at their own edges
and **58 % of the field stays bare sand**. That bare sand is the scene's second
subject: **Plants Shade The Ground** slows drying under cover, measured here as
moisture **0.181 under the plants against 0.091 on the open sand**, a difference
that disappears (−0.010) when the option is switched off.

**Two measurements changed the scene while it was being built**, and both are
recorded in its comments. The first draft seeded with Initial Cover plus the
patch-shape controls — but Initial Cover is uniform, and those controls state
they apply to Nabkha and Blowout, so they were setting nothing. The result was
an even film of cover with no clumps and no bare ground at all, which also
removed the only brake in the scene: with cover everywhere, "grow where plants
already are" means grow everywhere, and the field carpeted over. Seeding
through Nabkha fixed both. The second was the rate itself: a value picked on a
128-cell fixture gave 0.21 cover there and 0.53 in the real 512-cell scene,
because spread works on a cell neighbourhood — so the shipped value was chosen
on the grid the scene actually uses, and stops at the property's own soft
maximum rather than above it.

Demo coverage of the feature set: **141 of 215**, from 139.

---

## 1.3.196 — Seeds Ride The Wind: a strong bias, not a marching patch

**A correction to the previous version's journal, which recorded this option
as barely working.** The measurement was right and the verdict was wrong: it
measured how far the patch CENTRE moved, and the code does not carry cover —
it biases growth. Those are different quantities.

MEASURED on a fixture where the answer can be derived in advance — one square
patch in an empty field, wind along +X, every other biology term at zero — as
the cover gained in the empty column behind the patch against the one in
front: **1.00x at 0 (exactly symmetric), 1.49x, 2.19x and 5.10x at 0.25, 0.5
and 1.0.** The bias is strong. The patch centre, meanwhile, moves **0.045 of a
cell over 40 steps and 0.063 over 120**, because the upwind side still grows
and the bias fades as the patch edge softens.

The strongest check here is not the direction but the arithmetic: the growth
term was reproduced in numpy from the code's own formula, and the solver agrees
**bit for bit** (max difference 0.000e+00).

So the option does what it says, and the tooltip now says how much: expect a
lopsided patch rather than one that marches across the field. Nothing in the
solver changed.

---

## 1.3.195 — A growth window of its own, and an option that quietly expires

**Vegetation: Growth Windows** now gives GROWTH its own height window, set
NARROWER than the one the seeding uses. That is the opposite of the example in
the tooltip, and the reason is measured: widening the window at any setting
floods the scene, and a scene named for its windows stops showing them once
everything is green. As the share of cells above half cover: 0.747 without the
option, then 0.993, 1.000 and 1.000 for growth windows of 0.9, 1.2 and 1.8.
Narrowing behaves the other way and stays controllable — 0.585, 0.601 and 0.661
at 0.20, 0.35 and 0.50 — so the scene ships at 0.35, where cover drops from
0.7208 to 0.5785 and the windows are still visible. Physically it is a tree
line: the seeding reaches higher than the green can hold on.

**Keep Off Sand Source now says how long its head start lasts.** The option
works exactly as advertised at the moment of seeding — measured as cover on the
source against cover off it, 0.00 with it on versus 0.86 with it off — but
growth is deliberately not gated, so ordinary spread closes the gap: 0.01 vs
0.15 after 20 steps, 0.05 vs 0.09 after 60, and 0.09 vs 0.10 after 150, by
which point the two are the same picture. The tooltip carries those numbers
now, and says to lower Veg Spread if you want the bare source to last.

**A demo for it was written and then withdrawn**, which is worth recording. A
sand source was added to the nabkha scene so the gate would have something to
act on; with that scene's 150-step warmup the difference had already closed, and
what remained of the bare patch came from burial rather than from the gate. A
scene that sets an option the viewer cannot see promises something it does not
deliver.

Demo coverage of the feature set: **139 of 215**, from 138.

---

## 1.3.194 — De-facet Crests (in sim): what it really costs, and why that depends on your grid

**A correction first.** The previous version's journal recorded a measurement
suggesting this option did the opposite of what it promises. It does not — the
measurement was wrong, and this one replaces it. The old index was a ratio of
derivative spreads over the whole field; the function smooths a sand crest
line along *its own local orientation*, taken from a structure tensor. Those
are different properties.

MEASURED properly, on a fixture where the answer is known in advance — a
straight ridge carrying a zigzag of a set amplitude, handed straight to the
solver's own routine: the zigzag along the crest drops 62.4 %, a smooth ridge
with nothing to cure is left **completely untouched**, sand mass is conserved
to four decimals, and at 0 the field is bit-for-bit identical. In a real
400-step simulation the crest zigzag drops **85.6 %**.

**The tooltip's height cost is right for one grid and misleading for the
rest.** The smoothing reaches a fixed number of *cells*, not a fixed number of
metres, so on a 200 m domain its reach is 0.63 m at 512, 1.26 m at 256 and
2.52 m at 128. The relief it costs follows: **12.7 %, 23.6 % and 27.6 %.** The
documented "~10-15 pct" is correct at 512 and roughly half of what a 128 grid
actually pays. The tooltip now carries all three numbers and the reason.

Nothing in the solver changed: this is a documentation fix backed by a
measurement, and the physics fingerprint is unchanged.

---

## 1.3.193 — A demo for surface detail, shot from close enough to see it

**Showcase: Surface Detail** is a new scene about the layer that lives *above*
the dune. It is the first bundled scene that runs **Add Micro-Detail Pass**
for you: the pass freezes the dunes as bedrock and runs transport again at a
finer scale over them, so the result is a hierarchy of scales rather than
noise sprinkled on a surface. Crispness, Crests and Slump Allowance are its
dials — and they do nothing until the pass is RUN, which is why no previous
scene could show them.

**The camera had to move, and that is the point of the scene.** This file's
own comment says it: *at the usual demo distances the ripple layer is
sub-pixel*. A scene about surface detail cannot stand where the detail is
smaller than a pixel, so this one drops to eye level with a longer lens.

It also carries **De-facet Crests**, which takes the straight grid zigzag out
of the crest lines that a one-direction wind leaves behind; **Edge Rounding**,
which takes the corner off a brink but leaves the flats and the broad dunes
alone because it is weighted by convexity rather than by curvature; and a
little **Detail Feeds Sim**, which hands some of the render detail back to the
solver so the ripples are sand the wind can rework rather than a decal.

MEASURED, with each control given a measurement that could refute it:
De-facet Crests moves the directional crest index by −9.7 % while an isotropic
blur moves it *up* 4.0 %, so it is treating the symptom rather than blurring
everything; on the solver height it changes nothing at all to three decimals,
confirming it really is render-only. Edge Rounding: −5.7 % on that index and
−6.7 % roughness for +0.5 % relief, so the brink softens and the dune stays.
Against the plain desert scene from the same camera, the finished scene has
**4.94x the density of separate bedforms**.

**A cascade was tried and rejected**, and the scene's comments record it so
nobody repeats the experiment: two and three octaves with a 20-degree wind
shift raise the bedform count by 37 % and 61 %, and both look wrong — the
surface breaks into flat polygons with hard edges, the "polyhedron of frozen
faces" the Slump Allowance tooltip warns about, because each octave freezes
the one before it. The count rose because it was counting those facets.

Demo coverage of the feature set: **138 of 215**, from 132.

---

## 1.3.192 — A demo for the surface shader, and a tick that did nothing

**Showcase: Surface Shader** is a new scene about how the texture is
*projected* rather than what it is. A flat top-down projection stretches the
grain down every slope, and a dune is mostly slope — the slip face stands at
34 degrees. The scene puts the scanned sand and the bedrock on a triplanar
projection, turns the scan onto the wind heading and onto the local downslope,
makes the procedural/scan balance a map instead of one number, and lets the
terrain sit in its own ambient shade.

**A finding worth your attention if you have ever ticked Triplanar Scanned
Sand and seen nothing change: it does nothing while Break-up Method is
Hex-Tiling**, which is the default. The two cannot share the sand — the hex
path computes one set of blend weights and every map reuses them, and three
lattices would not fit — so hex wins and the tick is ignored. That was always
the documented behaviour; what is new is that the panel's own demo now *shows*
it. MEASURED: three triplanar flags on with hex left in place builds the same
699-node graph as all three off. The scene therefore drops to Texture Bombing,
and its card tells you to put Hex back and watch the tick stop working.

**Rain: Drops On Water** gained **Impacts From Particles**. Without it the
shader invents drop sites from a hash of the surface, so the rings had nothing
to do with the droplets the scene was already throwing; now a ring appears
under a droplet you can watch fall. That scene is the only one with both a
splash particle system and rain, so it is the only one where the option means
anything.

**Three shader options deliberately have no demo, and the reasons are worth
stating.** *Triplanar Vegetation* has nothing to show: the add-on ships
sixteen scanned sets — fourteen sands, a gravel and a scree — and none of them
is a plant. *Triplanar Drops* carries a known Cycles defect that blackens
patches of the terrain, and a bundled scene must not turn that on. *Texture
Rotation (all)* turns every layer at once, including the maps the scene has
just turned onto the wind — with both non-zero you could not tell which one
moved the grain, so it stays at zero and the card says where to find it.

Demo coverage of the feature set: **132 of 215**, from 124.

---

## 1.3.191 — A demo for Mars, and eleven more features with a scene of their own

**Another Planet had no demo at all** — the one headline feature you could only
reach by reading about it. It has one now, built the way the snow-density scene
is built: *the same ORGANIC dune field, one switch different*, so you can turn
it off and watch the same ground rebuild itself at Earth scale.

Four existing scenes picked up options that belong to them and were sitting
unused:

- **Showcase: Object Masks** — Sand Source Smoothing, Source Hold, Echo
  Strength, Mask Blocks Deposit and Fill Obstacle Footprint. The scene is named
  for the masks; these five are masks.
- **Showcase: Airborne + Dressing** — dust devils now need loose, moving,
  dry ground to form (three switches that tie them to the surface instead of
  letting them spawn on bare rock), the wind gusts in strength, and the crests
  carry the backlight sheet.
- **Flow 2D: Canyon Jet / Recirculating Wake / Wind Off a Wall** — Air Layer,
  Wall Height and Viscosity. Without the first, a plan-view solve cannot speed
  the wind up over a crest at all.
- **Phenomena: Tidal Coast** — Water Follows Tide. Without it the tide moved
  the level while the water surface stood still, so a scene about tides showed
  a dry shore changing colour.

Measured across the add-on: of 215 controls that switch a phenomenon on,
**124 are now shown by at least one demo — up from 99 two builds ago.**

---

## 1.3.190 — Demos updated for the features added since they were built

Six demo scenes were showing an older version of their own subject, because
the option that belongs to them arrived after the scene did. Measured across
the whole add-on: of 215 controls that switch a phenomenon on, **109 are now
shown by at least one demo, up from 99.**

- **Snow: Slab Avalanches Held Back By Gates** now runs the moving debris with
  **Runout Sees Obstacles** instead of falling back to the routing rule. The
  scene is about barriers, and until 1.3.175 only the routing could see them —
  the momentum solver pushed 85 % of the mass straight through.
- **Ice: Pack, Ridges and Leads** turns on **Floes Move As Bodies**, **Dark
  Floe Bias** and **Ice Insulates**. Without the first, a floe the eye reads as
  one lump is carried by a velocity field that pulls it apart.
- **Rain: Drops On Water** now emits the real **Splash Particles** and
  **Impact Crown**, not only the painted rings on the surface. The camera sits
  a few metres above the water — the one distance where a single splash reads.
- **Temperature: Cold Front** makes the temperature a **field** over the
  terrain with **Sunlit Slope Warming**, so snow no longer melts as fast on a
  ridge as in a shaded hollow.
- **Temperature: Spring Thaw** gains a **Day/Night Swing**: at a constant −2 °C
  nothing melts at all, while a 6 K daily swing carries the afternoon above
  zero and the night below it, so the pack goes in steps.
- **Frost: Hoar on a Snowfield** uses **Patchy Thaw**, so the frost burns off
  where the wind reaches first and survives in the lee, instead of the whole
  field fading evenly.

---

## 1.3.189 — Demo cards and this file

- Every one of the **57 demo scenes now has a card** in the Presets panel
  saying what to look at and which controls to try. The last four —
  *Plants: two species side by side*, *Snow: fresh, packed and drifted*,
  *Vegetation: nabkha hummocks* and *Vegetation: blowout hollows* — were
  running without one.
- **This changelog exists**, and the release audit now refuses a build whose
  version has no line in it.

---

## 1.3.184 – 1.3.188 — Three interface modes

The add-on draws 925 controls across 21 panels. That is the whole product and
none of it has been removed — but it is not what a first scene needs.

- **1.3.188** — Release review. Fixed a class of defect the gates had not been
  looking for: a control visible in a mode while the switch that enables it was
  hidden, so the slider looked permanently dead (1 case in Simple, 16 in
  Standard). The rule "a control may not be more visible than its own switch"
  is now a permanent test.
- **1.3.187** — The three tiers were re-cut **by measurement instead of
  judgement**: the 57 bundled demo scenes are a statement of which controls
  make a look, so every control they set most often is now in Simple.
  **Show Render Detail** alone is set by 38 of the 57 demos and had been left
  out. Five earlier decisions were reversed by the same measurement.
- **1.3.186** — Simple gained Objects / Masks, Water / Moisture, Vegetation and
  Physics: you can now put a rock in the sand, pour water, grow plants and
  change the angle of repose without leaving it.
- **1.3.185** — **Simple is what a fresh scene opens in.** A .blend saved
  before 1.3.184 has no value stored for the setting, so it opens in Simple
  too: the scene is unchanged, but panels you had open look emptier until you
  switch. The switch is the first row of the DunesForge panel.
- **1.3.184** — Simple / Standard / Advanced added. **Nothing is disabled, only
  hidden**: a control you cannot see keeps its value and the solver keeps
  reading it, so changing mode cannot change a number in your scene. Advanced
  is the interface exactly as it was before.

Today: **Simple 92 controls in 11 panels, Standard 403 in 16, Advanced 912
in 20.**

---

## 1.3.181 – 1.3.183 — Documentation, and what writing it found

- **1.3.183** — Objects / Masks documented (46 controls). Fixed two tooltips
  that read `the RIM of the the emitter footprint`.
- **1.3.182** — Wind (51) and Physics (36) documented. Fixed four strings that
  showed `%%` to the user — two tooltips and two panel labels.
- **1.3.181** — **The panel now tells you when the water clock is being set by
  a film of water.** Measured across the water demos, the cell that dictates
  the time step is 1–5 mm deep in some scenes and deeper than the median in
  others, so raising the floor is free speed in one and lost accuracy in
  another. A note appears only where it would help.

By the end of this run every one of the 925 controls had its own entry in
`REFERENCE.md`, and six audit rules keep it that way.

---

## 1.3.175 – 1.3.180 — Water, snow and speed

- **1.3.180** — Water can leave through the **Y** edges as well as X.
- **1.3.179** — Surface detail is back in the gravity demos.
- **1.3.178** — **Depth hoar**: every Nth buried snow layer can be marked as a
  layer that never hardens, so a slab has something to fail on. Without it the
  reported slab depth could never exceed the new-layer threshold, and releases
  died out entirely after about a hundred steps.
- **1.3.177** — **Frame time 10.25 s → 4.29 s** on a scene with several object
  masks: the terrain lookup that `Max Float Gap` needs is now built once per
  refresh instead of once per object slot.
- **1.3.176** — The avalanche-gates demo was retuned onto a 48° face, above the
  repose angle of snow, so the gates actually release.
- **1.3.175** — **Obstacles stop the moving debris.** The relaxation and the
  routing had treated obstacles as walls for a long time; the momentum solver
  behind *Slab Runs Out* had not, and debris went straight through a fence.

---

## 1.3.140 – 1.3.174 — Ice, avalanche momentum, demos

- **1.3.168 – 1.3.173** — The slab avalanche gained a momentum stack: debris
  carries its speed (**Momentum Rides Along**), a limiter that stops the
  transport smearing it (**Sharper Transport**, run-up 55 % → 92 % of the
  frictionless head on a 15° slope), and the measurement that explained why a
  flowing avalanche stopped early — it was the step budget, not the friction.
- **1.3.160 – 1.3.166** — Ice floes: one velocity per floe, a darker half that
  reads as thicker ice, and the tide that finally moves the sheet.
- **1.3.153 – 1.3.157** — The water step got a fast path (preallocation and a
  CUDA graph) and the ice-contrast measure was rebuilt from the field.
- **1.3.158 – 1.3.163** — New demo scenes: avalanche entrainment, vegetation
  windows, a thaw that leaves one patch, flowing ripples.
- **1.3.161** — **Black material solved**: the shader did not fit in the Cycles
  node stack, which is why some scenes rendered black.

---

## 1.3.100 – 1.3.139 — Physics you can point at

Grain diameter, surface roughness and atmospheric stability became real inputs
instead of numbers folded into an Earth-only calibration; Owen's saltation
feedback closed the only negative loop in the solver; the planetary block
arrived (Mars, Titan, Venus, the Moon) with Earth exactly 1.0 so nothing moved;
snow gained a layer stack, density and slab release; water gained the
shallow-water solver, erosion, ice and rain that lands.

## 1.3.1 – 1.3.99 — The rewrite settles

Blender 5.2 compatibility, the GPU path and its self-check, the interactive
brush, undo for solver state, the rock-layer shader, hex tiling for scanned
sand, wind models (analytic, Flow 2D, the 3D experiment), object burial and
re-exposure, and the long series of measurements that fixed the sand budget
along a crest.

## 1.2.x — Before the current numbering

The first public series: demo scenes, presets, the surface shader, export and
bake, vegetation, dust devils and haze. Detailed notes live in the
`HANDOFF_1.2.*` journals shipped with the source.

---

## How this file is kept

The version in `blender_manifest.toml` must appear here — the release audit
fails otherwise (`NO-CHANGELOG`). A build that ships without a line in this
file cannot be built by mistake.
