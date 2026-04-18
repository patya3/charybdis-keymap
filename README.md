# Charybdis 4x6 ZMK Firmware

Personal ZMK firmware for a Charybdis 4x6 wireless split keyboard with PMW3610 trackball. This is an aliexpress clone running SuperMini nRF52840 controllers (nice!nano pin-compatible).

Forked from [nophramel/charybdis_zmk](https://github.com/nophramel/charybdis_zmk) with a custom shield definition to match this board's matrix wiring.

## Hardware

- **Controller**: SuperMini nRF52840 (nice!nano v2 compatible)
- **Sensor**: PMW3610 trackball (right half)
- **Connection**: Bluetooth (right half is central)
- **Layout**: 4x6 split + 5 thumb keys (3 left, 2 right) + 3 extra lower thumb keys

## Building & Flashing

Firmware builds automatically via GitHub Actions on every push. Download the latest artifacts from the [Actions tab](../../actions).

| Artifact | Description |
|----------|-------------|
| `firmware` | Contains `charybdis_left.uf2` and `charybdis_right.uf2` |
| `settings_reset` | Nuclear option for clearing BLE bonds |

To flash: double-tap reset on the SuperMini, drag the `.uf2` onto the `NICENANO` drive that appears. macOS will complain about an incomplete transfer -- that's normal, it's the board rebooting after a successful flash.

If the halves won't pair, flash `settings_reset.uf2` to both, then reflash the actual firmware (right first, then left).

## Layers

### 0: Base (QWERTY)

Standard QWERTY with [urob's timeless home row mods](https://github.com/urob/zmk-config) in CAGS order (Ctrl, Alt, GUI, Shift) for macOS. The trackball is **not active** on this layer to prevent accidental cursor movement while typing.

### 1: Gaming

Overlay that strips home row mods from ASDF/JKL; so they don't interfere with gaming inputs. Toggle from Base with the top-right key.

### 2: Symbols

Programming-oriented symbol layer with paired brackets, arrow macros (`->`, `=>`), elixir pipe (`|>`), and `!=`. Activated by holding the left-most upper thumb key.

### 3: FN/Numpad

F-keys on the left (F1-F12), numpad on the right. Page up/down on the left. Hold or tap-toggle via the left lower thumb key.

### 4: Nav

Navigation, media, bluetooth, and trackball layer. This is the only layer where the trackball moves the cursor.

- **Right hand**: Arrow keys (hjkl position), mouse buttons (LCLK / scroll hold / RCLK on bottom row)
- **Left hand**: Media controls (prev/play/next, volume), vim `:w` macro
- **Number row right**: BT profile select (0-3) and BT clear
- **Hold the key between LCLK and RCLK** to switch the trackball to scroll mode

### 5: Scroll

Transparent layer activated by holding the scroll key on the Nav layer. Converts trackball movement to scroll wheel input.

### 6: Lewd

You know what this is for.

## Trackball Configuration

- **CPI**: 2400 with a 1/2 scale divisor (effective 1200 DPI). High CPI + divisor smooths out jitter from small hand movements while keeping the cursor responsive.
- **Scroll**: Hold the middle key between LCLK/RCLK on the Nav layer to scroll with the trackball.
- The trackball is only active on the Nav layer -- no accidental cursor movement while typing.

## Macros

| Macro | Output | Layer |
|-------|--------|-------|
| `vim_write` | `:w<enter>` | Nav |
| `single_arrow` | `->` | Sym |
| `double_arrow` | `=>` | Sym |
| `elixir_pipe` | `\|>` | Sym |
| `not_equals` | `!=` | Sym |

## Editing the Keymap

The easiest way to modify bindings is through the [keymap editor](https://nickcoutsos.github.io/keymap-editor/). Point it at this repo, make changes, and it'll commit directly. The build pipeline picks it up from there.

For structural changes (new layers, behaviors, macros), edit `config/charybdis.keymap` directly.

## Credits

- [nophramel](https://github.com/nophramel/charybdis_zmk) - Base ZMK config for Charybdis 4x6
- [badjeff](https://github.com/badjeff) - PMW3610 driver, input behavior listener, split peripheral input relay
- [petejohanson](https://github.com/petejohanson) - ZMK pointers-with-input-processors branch
- [urob](https://github.com/urob/zmk-config) - Timeless home row mods
- [nickcoutsos](https://github.com/nickcoutsos/keymap-editor) - Keymap editor
