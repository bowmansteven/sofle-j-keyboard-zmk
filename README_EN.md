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

### Momentary Lower + Raise → Adjust

Firmware builds use `config/eyelash_sofle.keymap` via the workflow's
`ZMK_CONFIG` directory; the board fallback keymap is unchanged.
Layer indices remain Base (0), Lower (1), Raise (2), Adjust (3), and layer 4.

- Hold the left thumb at zero-based key position **56** (`&mo 1`) for Lower.
- Hold the right thumb at position **61** (`&mo 2`) for Raise. This deliberately
  replaces Base's Down arrow at the stock Raise thumb location. The other
  navigation bindings are unchanged; Down remains available on Lower.
- Holding both thumbs activates Adjust through native ZMK conditional layers,
  in either press order. Releasing either immediately disables Adjust, leaving
  only the other held layer active; releasing both returns to Base.

Both thumb positions remain transparent on Lower, Raise, and Adjust. Adjust
currently has only transparent key bindings, so it inherits from the active
layers below it (Raise, then Lower, then Base). Its scroll encoder binding is
unchanged; no new Adjust layout or Bluetooth/reset controls were added.
See [ZMK conditional layers](https://zmk.dev/docs/keymaps/conditional-layers).

Manual verification after flashing:

1. Hold/release Lower alone: layer 1 is active only while held, never Adjust.
2. Hold/release Raise alone: layer 2 is active only while held, never Adjust.
3. Hold Lower, then Raise: layers 1, 2, and 3 activate. Release Lower first:
   only Raise remains. Release Raise: return to Base.
4. Hold Raise, then Lower: layers 1, 2, and 3 activate. Release Raise first:
   only Lower remains. Release Lower: return to Base.
5. Repeat both press orders, releasing the second-pressed thumb first; only the
   first-held layer should remain until its thumb is released.

Because Adjust is transparent, its activation has no distinct key output.
Confirm layer 3 activation/deactivation with layer-state debug logging if
available; key output alone cannot distinguish it from Lower + Raise.

![Sofle键位图](keymap-drawer/eyelash_sofle.svg)
