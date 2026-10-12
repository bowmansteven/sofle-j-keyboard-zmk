# Sofle

- [Chinese](README.md)
- [English](README_EN.md)

## Update List

- 2024/12/21
  1. Added support for zmk-studio (just refresh the left hand to use).
- 2024/10/24
  1. Modified power supply mode to reduce power consumption.
  2. Fixed the automatic shut-off feature for RGB power supply.
- 2025/8/22
  1. update the soft off.When you press the keys Q, S and Z simultaneously and hold them for 2 seconds, the keyboard will enter a deep sleep state and cannot be awakened by pressing the keys. This function can be used when carrying it outside. The activation method is to press the reset switch once.
  2. This month, I also updated the ultra-thin versions of the corne and sofle cases. The frame and base plate have been thickened, and the opening of the reset switch has been adjusted, so that the reset switch can be easily pressed. At present, we are still conceptualizing how to design the shell with an inclined bracket.If you have carefully examined a PCB, you will notice that there are reserved interfaces for expansion IO. I wonder if anyone has been able to utilize them,I will try it！
  3. The GIF animations on the right-hand keyboard screen have been removed, which will significantly reduce the power consumption of the right-hand keyboard.

> If your  sofle was updated before 2025/8/22, please update to the latest firmware.
>

## Contact Me

For 3D printed model files or any issues and malfunctions with the keyboard, please contact [380465425@qq.com](mailto:380465425@qq.com)

## Sofle Keymap

![Sofle键位图](keymap-drawer/eyelash_sofle.svg)

## Adjust: Bluetooth and output controls

Hold **Lower + Raise** (base layer left thumb at index **56**, right thumb at
**61**) to activate **Adjust (layer 3)**. Either press order works; releasing
either thumb exits Adjust. Indices are zero-based keymap binding positions
(including the central pointing keys), not switch numbers. Both thumb positions
remain transparent on layers 1, 2, and 3. Layer indices, encoder behavior, and
existing Lower/Raise controls are unchanged; other Adjust keys still fall through.

While holding both thumbs, tap these physical base-layer keys:

| Physical key | Index | Adjust binding | Action |
| --- | --- | --- | --- |
| 1 | 1 | `&bt BT_SEL 0` | Select user profile 1 |
| 2 | 2 | `&bt BT_SEL 1` | Select user profile 2 |
| 3 | 3 | `&bt BT_SEL 2` | Select user profile 3 |
| 4 | 4 | `&bt BT_SEL 3` | Select user profile 4 |
| 5 | 5 | `&bt BT_SEL 4` | Select user profile 5 |
| Q | 14 | `&bt BT_PRV` | Previous profile (wraps) |
| W | 15 | `&bt BT_NXT` | Next profile (wraps) |
| T | 18 | `&bt BT_CLR` | Clear pairing for the selected profile |
| A | 27 | `&out OUT_USB` | Prefer USB output |
| S | 28 | `&out OUT_BLE` | Prefer Bluetooth output |
| D | 29 | `&out OUT_TOG` | Toggle preferred USB/Bluetooth output |
| Enter (far-right outer key) | 51 | `&bt BT_CLR_ALL` | Clear pairings for all profiles |

Q/W are adjacent for cycling; A/S/D group output controls on the home row.
Clear-current is on T, separated from cycling; **clear-all is on the opposite
half's outer Enter key**, away from number-row selection. These are ordinary
press bindings, **not timed holds or confirmation prompts**. Clearing requires
re-pairing the affected hosts (also forget/remove the old keyboard pairing on
the host before pairing again). Do not test clear-all unless prepared to lose
every host pairing. Existing clear-all shortcuts on Lower (physical 1) and
Raise (physical W) remain available; this placement does not protect those.

These controls are supported by the pinned **ZMK v0.3.0**
([Bluetooth definitions](https://github.com/zmkfirmware/zmk/blob/v0.3.0/app/include/dt-bindings/zmk/bt.h),
[output definitions](https://github.com/zmkfirmware/zmk/blob/v0.3.0/app/include/dt-bindings/zmk/outputs.h)).
See the official [Bluetooth](https://zmk.dev/docs/keymaps/behaviors/bluetooth)
and [output selection](https://zmk.dev/docs/keymaps/behaviors/outputs) behavior docs.
Output selection changes the preferred transport; verify with a connected USB
host and a paired Bluetooth host, not just the charging cable.

## Flashing and ZMK Studio migration

1. Download the firmware artifact from the **Build ZMK firmware** run for the
   desired commit. Flash `eyelash_sofle_studio_left.uf2` to the left half and
   `eyelash_sofle_right.uf2` to the right half using each half's UF2 bootloader.
   Use the matching pair from the same run.
2. **Do not flash `settings_reset` for this update.** The matrix still offers
   that separate recovery artifact, but nothing here resets settings
   automatically at build time or runtime. Ordinary firmware flashing preserves
   settings, including Bluetooth pairings and saved Studio keymaps.
3. Connect the left half over USB and open [ZMK Studio](https://zmk.studio/).
   The active left build uses `studio-rpc-usb-uart`, enables Studio, and disables
   Studio locking in `build.yaml`.

**Flashing alone does not guarantee the new bindings appear.**
As documented in [ZMK Studio](https://zmk.dev/docs/features/studio), saved Studio
keymap changes override future `.keymap` changes until **Restore Stock Settings**.
The [settings documentation](https://zmk.dev/docs/config/settings) confirms that
ordinary flashes preserve settings.

- **Use the new compiled defaults:** first record/back up every current binding
  and layer (for example, screenshots and a written binding list). Then choose
  **Restore Stock Settings** in Studio. This replaces your Studio edits with the
  compiled defaults; do not do it before preserving customizations you want.
  It is not a request to flash the separate settings-reset firmware.
- **Keep the saved keymap:** manually apply the table above to layer 3 in Studio
  if that layer is available. On base layer 0, set position 56 to **Momentary
  Layer 1** and position 61 to **Momentary Layer 2**, not toggle/to-layer.
  Keep **both positions transparent on layers 1, 2, and 3**, then save changes
  to the keyboard. The native conditional rule `1 + 2 -> 3` is compiled into
  firmware; these instructions do not assume Studio can edit that rule.

If Studio shows only layers 0–2, check that the left half actually runs the new
Studio-left artifact, reconnect Studio, and compare against the compiled defaults
after backing up any edits. Old firmware or a persisted three-layer keymap are
diagnostic possibilities, **not proven causes**. In
[pinned ZMK's keymap implementation](https://github.com/zmkfirmware/zmk/blob/v0.3.0/app/src/keymap.c)
and [Studio layer enumeration](https://github.com/zmkfirmware/zmk/blob/v0.3.0/app/src/studio/keymap_subsystem.c),
compiled keymap child nodes define layers, Studio reports the runtime layer list,
and saved layer metadata can restore a different layer count. This repository
compiles five layers (0–4); layers are not hidden merely because they lack a
direct `&mo` reference. Do not add artificial `&mo 3`/`&mo 4` bindings to address
visibility. If layer 3 is unavailable, preserve your edits before considering
Restore Stock Settings, then reapply desired customizations.

`config/eyelash_sofle.json` is a **physical layout description**, not a Studio
export. Saving in Studio writes device settings, not repository files. To make
custom bindings part of future stock firmware, edit
`config/eyelash_sofle.keymap` and regenerate the diagram.

## Verification

The existing `.github/workflows/draw.yml` parses the keymap with keymap-drawer
and regenerates `keymap-drawer/eyelash_sofle.yaml` and `.svg`. The existing
`.github/workflows/build.yml` builds both halves against ZMK v0.3.0. Parser and
binding-count checks are not substitutes for a successful firmware build;
the earlier run [38191909977](https://github.com/bowmansteven/sofle-j-keyboard-zmk/actions/runs/38191909977)
passed for the pre-change keymap, not these Adjust bindings.

Local validation: keymap-drawer 0.23.0 parsed and rendered the updated keymap;
all five layers have 64 bindings, the exact control positions and transparent
thumbs were checked, and other layers/macros/encoder bindings were unchanged.
A local firmware build was unavailable because west and the Zephyr SDK were
not installed; use the firmware workflow result for this change's commit.

Hardware verification is pending. After flashing and resolving saved Studio
overrides:

- Check Lower alone and Raise alone retain their original bindings.
- Press Lower then Raise, and Raise then Lower. For **each press order**, test
  releasing Lower first and releasing Raise first: Adjust must exit immediately,
  the remaining held layer must still work, and neither thumb may get stuck.
  Use Q/W or the display/profile indicator to distinguish Adjust from Raise,
  since Raise already has the same number-row profile selection.
- Select each of profiles 1–5; verify the intended paired host receives keys.
  Test previous/next, including wraparound between profiles 1 and 5.
- With USB and a paired BLE host connected, verify A routes to USB, S to BLE,
  and D toggles the preferred output in both directions.
- On a disposable/test pairing, verify T clears only the selected profile and
  other profiles still work; forget the old host entry and re-pair. Only when
  ready to re-pair all hosts, verify Enter clears all profiles.
- Confirm encoder scrolling and transparent keys retain their prior behavior;
  reconnect Studio/reboot and repeat activation/output tests after saving.
