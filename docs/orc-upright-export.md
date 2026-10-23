# Orc Upright animation and equipment export correction

Validated on 22�23 October 2026, Retail product `wow`, build **12.1.5.70391**.
Exporter revision: **`wmv-fbx-posture-2`**, recorded in new character/equipment
catalogs. The plugin's existing public version is 0.5; this revision field and
the installation SHA-256 manifest distinguish the correction. Work is on
`my-develop`, based on `b744b4283ca812d1f48a12d32ceaacdedc1067e3`, without a commit
or push for this correction.

## Cause and correction

| Body | Model FileDataID | Skeleton FileDataID | Relationship |
| --- | --- | --- | --- |
| Hunched, option 24 / choice 438 | 917116 | 1964411 | Parent/base |
| Upright, option 24 / choice 439 | 1968587 | 1969462 | SKPD parent 1964411, plus its own SKS1/SKB1/AFID |

The loader replaced the child's sequences and bones with the parent's whenever
SKPD existed. This discarded **118 authored Upright sequences**, their track
headers, external animation mappings, bind pivots and global clocks. The FBX
exporter therefore sampled Hunched movement on the Upright mesh. The integrated
Unity parser duplicated the same assumption.

Both now compose complete compatible rigs by AnimationID/SubAnimationID. Child
identities override the parent; missing identities inherit it. Track data is read
using the selected source's own indices, aliases, interpolation and global-loop
periods. Child bind hierarchy/pivots remain associated with its geometry. The
existing parent-only handling of partial skeletons with another bone-index space
is retained. Complete rigs with incompatible bone identities are rejected.

The 0–225 selection contains 151 clips on each body. Upright uses 73 child clips
and 78 inherited clips in that selection. In particular:

| Animation | Hunched | Upright | Upright source |
| --- | --- | --- | --- |
| Stand / 0 | 2467 ms | 2467 ms | 1969462, sequence 0 |
| Walk / 4 | 1066 ms | 1000 ms | 1969462, sequence 1 |
| Run / 5 | 667 ms | 667 ms | 1969462, sequence 2 |

The WMV Appearance conditional-model toggle itself remains unimplemented. Preview
validation loaded each actual model, including `character/orc/male/orcmaleupright.m2`,
rather than treating the toggle on the base model as a reference. Saved character
snapshots can now use the existing hidden `-unityipctest` path as well as `-mo`.

## Equipment and consumer contract

Selecting Orc male exports both postures for every selected piece and the combined
set rules. Each model is loaded separately, its posture choice is applied, and
items are resolved and bound again. No model ID is relabelled on another mesh.

Customization remains `wmv.customization` version 2 with
`conditional-body-models-v1`. Its alternate path remains exactly
`postures/choice-439/character.fbx.customization/catalog.json`.
The additive `animations` array records take/AnimationID/SubAnimationID/duration;
existing `animationTakes` and correction semantics remain intact.

Equipment indexes remain version 1, retaining one `variants` entry per race/sex
and the existing Hunched `catalog`. On the Orc male entry an additive
`bodyVariants` array identifies both model IDs and option/choice IDs. Upright
piece paths are `race-2/male/postures/choice-439/catalog.json`; combined rules use
`rules/race-2/male/postures/choice-439/catalog.json`. Paths are relative to the
index containing the entries.

The consumer must select `bodyVariants` by the active character's **modelFileId**,
for all pieces **and** combined rules. An older consumer can continue using the
base Hunched entry, but must keep rejecting it for Upright. Model validation is
retained. WowCharacterViewer was not modified.

## Validation and evidence

The independent C++ oracle reads original local client bytes and compares every
30 Hz baked frame after FBX SDK reimport. It does not call the production skeleton
resolver or its animation sampler for expected values.

| Check | Result |
| --- | --- |
| Supplied pre-fix Upright FBX | 525,728 pose/duration discrepancies detected |
| Corrected Hunched, 151 clips | 1,564,860 bone/time samples; 0 discrepancies |
| Corrected Upright, 151 clips | 1,568,600 bone/time samples; 0 discrepancies |
| Maximum translation/scale difference | 0 in these exported samples |
| Maximum quaternion orientation error metric | 4.79e-14 |
| Integrated parser, both bodies | 199,320 native-track comparisons passed |
| Facial correction catalogs | All 18 face/posture mappings match the client database; 9 bone sets per body |
| Oathclaw Wargarb | 7 pieces × 2 postures = 14 variants, plus both combined rules |
| Battlelords Plate | 8 pieces × 2 postures = 16 variants, plus both combined rules |
| Independent native FBX skin evaluation | 18 cases: body / each set × both postures × Stand/Walk/Run |
| Bind/attachment comparisons in those cases | 11,928; maximum matrix-component difference 0 |
| Posed geometry | 99,600 evaluated points across the 18 cases; finite positions and normalized weights |
| Hidden WMV runtime | Six complete body/outfit runs passed; 18 front captures plus six body captures |
| Night Elf female regression | 137 choices, 9 bone sets, 239 bones, 276 textures and 3 clips retained |

Geometry review samples are Stand at 0 ms, Walk at 250 ms and Run at 167 ms.
The animation oracle checks the complete selected clips; the geometry renders
are selected frames, not an exhaustive collision test for every animation and
every outfit combination. Native FBX SDK evaluation and WMV captures are the
visual evidence; the generic Blender draft harness is not an acceptance oracle.
The native render uses neutral materials and does not implement the full texture
compositor or facial correction consumer. Face ownership, resources and matrices
were checked separately against the client's authored tables.

Builds: Release x64 C++ and Unity 6000.3.25f1 passed. Parser synthetic tests and the
real Orc corpus passed. No new IPC protocol is required. No visual capture of the
running Blizzard client or test inside WowCharacterViewer is claimed; the source
comparison uses the original installed files of the exact requested build.

On Mauricio's machine, all new/extracted WoW resources and derived geometry stay
under `WowBattlegrounds/LocalAssets/Prepared/WoWModelViewer/upright-20261022/`:

- `raw/`: original read-only extraction, manifests and preview track evidence.
- `packages/`: validated generation, before updating the shared packages.
- `sdk-poses/` and `fbx-native/`: independent skin output and neutral renders.
- `previews/`: WMV captures from an isolated Windows desktop.
- `posture-comparison.png`: native FBX Hunched/Upright comparison.
- `equipment-posture-checks.png`: the 18 body/outfit/posture/clip cases.
- `original-packages.zip`: replaced original files and preserved `.meta` files.

Logs, source/installed hashes, numeric comparisons and update manifests are in
`C:/Users/mauri/WMVDev/upright-20261022/`. No WoW assets are added to Git. The package
update preserves other race/sex variants, original default selections/preferences,
matching default visibility and their fallback atlases. It does not delete any
`.meta` file. Installation configuration and caches are preserved.

The shared package update changed 102 files, preserved and independently compared
all 2,081 existing `.meta` files against the backup archive, and left 3,675 other
existing files unchanged. The daily installation replaced 9 files and verified
176 SHA-256 hashes, including the integrated Unity player. It was not opened on
the user's desktop after installation. The final shared FBX hashes match the
validated generated files; the copied catalogs were independently revalidated.
