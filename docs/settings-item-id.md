# Settings layout and item-ID selection

## Behavior

- General uses a vertically scrolling window. Its natural content size determines
  the initial floating size, bounded by the active display's work area.
- The Settings pane is resizable, the notebook fills it, and path/URL fields grow
  with the available width. A minimum width keeps labels and buttons readable.
  A restored old fixed-size pane is migrated to the new size and resize behavior.
- Item pickers search their stable ItemIDs as well as displayed labels. Showing
  IDs changes presentation only. Existing name/wildcard, slot and category filters
  still apply; a numeric match does not bypass a category or slot restriction.
- The catalog retains existing named items. Empty, whitespace or missing names
  are admitted when ItemModifiedAppearance resolves through ItemAppearance to a
  nonzero, existing ItemDisplayInfo. Duplicate modifiers do not duplicate items.
  Such items are labelled `Unnamed item`, optionally followed by `[ItemID]`.
- Standalone item loading accepts a valid model without an external replacement
  skin. This supports weapons whose M2 material definitions name their own textures.

## Repeatable hidden tests

Configure and build `scripts/tests/settings-item-id` with CMake and the same
wxWidgets and Qt installations used by WMV:

```text
cmake -S scripts/tests/settings-item-id -B <test-build> -A x64 -DWMV_ROOT=<checkout> -DQT_ROOT=<Qt-installation>
cmake --build <test-build> --config Release
<test-build>/Release/settings-item-id.exe <runtime>/wowdb.sqlite
```

Place wxWidgets and Qt DLLs on the test process's PATH. The SQLite database is
opened read-only. The harness compiles production layout, catalog query, item
labels, slot filtering, picker construction and dialog filtering/selection code.
Settings persistence/folder actions, network imports and the final equipment
receiver are stubbed. The picker's final Show and Move are suppressed. No window
is shown or focused. A separate in-memory fixture covers invalid appearance links,
missing/blank names, duplicate modifiers and non-equipment inventory types.

The native hidden AUI frame is checked for a resize border and sufficient initial
space including window decorations. Size events exercise the real layout and
scroll handlers. Tests shrink the panel, scroll to Apply, and enlarge it again.

## Verification — 10 October 2026

Release x64 compilation passed. Against Retail `12.1.0.69933`, schema 15:

- Initial Settings size on the test display: 440 × 687. IDs and Apply fit.
  Notebook and text fields expand; at height 350 the scrollbar appears and Apply
  is reachable. Expanding removes the scrollbar. The native frame is resizable.
- 137,570 assertions passed, including uniqueness checks over the 137,501-row
  catalog, both ID visibility settings, both hand pickers, category exclusion,
  head-slot rejection, case-insensitive name/wildcard search, empty results,
  correct selected ItemID, and the no-category picker with its existing None row.
- Existing transmog tests passed: 194 selector checks, catalog fixtures, four
  equipment and seven appearance application cases, plus an integration smoke
  pass with 101,876 assertions and 18 filtered set selections.
- Real game assets loaded in an isolated WMV runtime. Item `153575` resolved to
  display `185141` and model FileDataID `1717765`,
  `item/objectcomponents/weapon/staff_2h_jaina_d_01.m2`.
- Equipping the staff in Tyrande's right hand and exporting a verification FBX
  succeeded. An independent FBX reader found the staff's 1,072 triangles, three
  materials, texture references and attachment to Tyrande bone 222. The model's
  intrinsic textures loaded; the legacy equipment path logs a missing optional
  replacement texture 0, without preventing attachment/export.
- Standalone `-item 153575` also loaded the correct model and enabled its three
  component geosets after the optional-skin correction.

Evidence is local to `C:/Users/mauri/WMVDev/settings-item-id-20261010/`.
No installation files were replaced; the installed executable's SHA-256 was
unchanged. No visible application session was opened. Visual inspection of the
settings and rendered staff in Unity, alternate DPI/monitor configurations, and
animation of the equipped staff remain unverified by this task.

The earlier transmog changes were separately saved in local commit `e2f6b978`
at Mauricio's request. These Settings/item-ID fixes remain uncommitted, with no
push or installation update.

## Installation follow-up — 10 October 2026

After Mauricio authorized installation, the tested executable was copied to the
 daily installation. All seven C++ runtime binaries were checked by SHA-256;
only `wowmodelviewer.exe` differed and required replacement. Its previous version
and the prior build metadata were backed up beneath the evidence directory.
Installation metadata records the uncommitted fixes. WMV was not launched after
installation. The earlier no-install statement above describes the verification
phase, before this follow-up authorization.

## Confirmación del usuario — 10 de octubre de 2026

Mauricio probó la instalación actualizada y confirmó que los arreglos funcionaron
bien. Esta confirmación complementa las pruebas automáticas anteriores; las
referencias previas a revisión visual pendiente describen el estado antes de su
prueba. No se atribuyen a esa confirmación pruebas de otros DPI o de animaciones.

## Publication and daily integration — 10 October 2026

The independent contribution is commit `0df7dc58` on `codex/settings-item-id`,
PR https://github.com/wowmodelviewer/wowmodelviewer/pull/90 against `develop`,
based on `d3643c06`. Its Release x64 build and hidden tests passed with wxWidgets
3.3.3. That branch uses the current `Item.SheatheType` schema spelling; this daily
branch retains `Item.SheathType`. All eight production-file deltas were compared
and match apart from that schema spelling.

The already-installed daily changes are recorded together as the integration of
that contribution, with their original tests and local verification history.
Earlier statements about uncommitted changes describe the implementation phase.
