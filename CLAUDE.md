# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK keyboard firmware configuration repository for Corne-style 42-key split ergonomic keyboards. It implements "Knucklehead" - a mnemonic, macOS-optimized layout using a modular architecture that separates hardware-specific configs from reusable keyboard logic.

## Build System

### Automated Building (Recommended)
- **Trigger**: Push changes to any file in `build.yaml`, `config/*.{keymap,dtsi,conf}`, `config/west.yml`, or `knucklehead/*.dtsi`
- **Workflow**: `.github/workflows/build.yml` automatically compiles firmware using ZMK's official build workflow
- **Output**: UF2 firmware files available in GitHub Actions artifacts
- **Status**: Check build badge in README.md

### Build Configuration
- **File**: `build.yaml` - Defines hardware targets (board, shield, display)
- **Current default**: nice!nano v2 board with Corne keyboard and nice_oled display
- Uncomment/comment lines to change hardware targets

### Dependencies
- **File**: `config/west.yml` manages ZMK dependencies via West tool
- **External modules**:
  - `zmkfirmware/zmk` - Core firmware
  - `urob/zmk-auto-layer` - Timer-less home row mods
  - `urob/zmk-tri-state` - Tri-state behaviors
  - `mctechnology17/zmk-nice-oled` - OLED customizations

## Architecture

### Modular Include System

The architecture uses a highly modular approach with hardware-specific entry points that include shared logic:

```
config/corne.keymap ──┐
                      ├──> knucklehead/base.dtsi (orchestrator)
config/corneish_zen.keymap ─┘
                             │
                             ├── behaviors.dtsi (custom ZMK behaviors)
                             ├── macros.dtsi (keyboard macros)
                             ├── combos.dtsi (key combinations)
                             ├── macos-shortcuts.dtsi (screenshot shortcuts)
                             ├── L1_colemak-dh.dtsi (base layer - selectable)
                             ├── L2.dtsi (numbers/nav/media)
                             └── Fn.dtsi (function keys/system)
```

**Hardware configs** (`config/*.keymap`) are thin wrappers - they only include `knucklehead/base.dtsi`

**All keyboard logic** lives in `knucklehead/*.dtsi` files for maximum reusability

### Layer System

**3 layers defined in `base.dtsi`**:
- `L1` (0): Base alpha layer - Default is Colemak-DH
- `L2` (1): Numbers (1-5 top row, 6-0 home row), VIM-style arrows (HJKL), media controls
- `Fn` (2): Function keys (F1-F15), system controls, Bluetooth, macOS screenshots

**Key positions** (42-key split):
```
╭────────────────────────╮  ╭────────────────────────╮
│ 0   1   2   3   4   5  │  │ 6   7   8   9   10  11 │
│ 12  13  14  15  16  17 │  │ 18  19  20  21  22  23 │
│ 24  25  26  27  28  29 │  │ 30  31  32  33  34  35 │
╰───────────╮ 36  37  38 │  │ 39  40  41 ╭───────────╯
            ╰────────────╯  ╰────────────╯
```

### Important Constants (base.dtsi)

```c
// Layers
#define L1 0
#define L2 1
#define Fn 2

// Home row mod timing
#define TAPPING_TERM_MS 280
#define QUICK_TAP_MS 175
#define REQUIRE_PRIOR_IDLE_MS 150

// Combo timing
#define COMBO_TERM_DEFAULT 30       // Very fast recognition
#define COMBO_QUICK_TAP_MS 100

// Key position groups
#define KEYS_L 0 1 2 3 4 5 12 13 14 15 16 17 24 25 26 27 28 29
#define KEYS_R 6 7 8 9 10 11 18 19 20 21 22 23 30 31 32 33 34 35
#define THUMBS 36 37 38 39 40 41
```

## Key Configuration Patterns

### Home Row Mods (Timer-less)

Uses @urob's timer-less approach for reliable home row modifiers without timing issues:

- **Left hand** (`&hrml`): Hold-trigger on KEYS_R + THUMBS only
  - Example: `&hrml LCTRL R` = Tap:R, Hold:Control
- **Right hand** (`&hrmr`): Hold-trigger on KEYS_L + THUMBS only
  - Example: `&hrmr LCMD N` = Tap:N, Hold:Command

**To hold-repeat** a home row key: tap twice and hold

### Smart Behaviors

**Smart Shift** (`&smart_shift`):
- Tap: Sticky shift (next key only)
- Double-tap: `caps_word` (until non-letter)
- Hold: Normal shift

**Smart Enter** (`&smart_enter RSHFT`):
- Tap: Enter
- Double-tap: `caps_word`
- Hold: Shift

**Smart L2 Layer** (`&smart_num L2 0`):
- Tap: Sticky layer (one key, return to L1)
- Double-tap: `num_word` (stays on L2 while typing numbers/arrows/operators)
- Hold: Momentary layer

**Layer Canceling Macros** (prevents stacking):
- `&csl <layer>` - Clear active layers + sticky layer
- `&cmo <layer>` - Clear active layers + momentary layer

### Caps Word Customization

The `caps_word` behavior is customized for R/LaTeX workflows (behaviors.dtsi:23-25):
- Continues on **underscore** (`_`) for snake_case variables in R
- Continues on **numbers** (0-9) for variable names like `VAR_123`
- Continues on **minus** (`-`) for some R function names
- Continues on **curly braces** (`{` `}`) for LaTeX labels and references
- Continues on **caret** (`^`) for LaTeX superscripts
- Default behavior: converts letters to uppercase, exits on space or other symbols

### Combos

**File**: `knucklehead/combos.dtsi`

**Macro syntax**:
```c
COMBO(name, &binding, key_positions, layers, timeout, quick_tap)
```

**Categories**:
1. Symbol combos - Vertical key pairs for special characters
2. Bluetooth combos - Device switching on Fn layer
3. R language operators - `<-`, `|>`, ` ```{r} `
4. Layer access - Quick Fn layer access via thumb combo

**Add visual representation** in `keymap-drawer/combos.yaml` for any new combos

### Using &trans vs &none in Layers

**Critical distinction**:
- `&trans` (transparent): Falls through to base layer - pressing Z on L2 would type "z"
- `&none`: Does nothing - pressing Z on L2 has no effect

**Convention**: Use `&none` for unused keys on upper layers to prevent accidental character input. Only use `&trans` for keys you explicitly want to pass through (like Enter, Backspace).

## Common Development Tasks

### Switch Base Layout (Colemak-DH → QWERTY/Dvorak/Colemak)

Edit `knucklehead/base.dtsi`:
```diff
// Alpha layer: uncomment desired, comment the others
-#include "L1_colemak-dh.dtsi"
+// #include "L1_colemak-dh.dtsi"
// #include "L1_colemak.dtsi"
// #include "L1_dvorak.dtsi"
-// #include "L1_qwerty.dtsi"
+#include "L1_qwerty.dtsi"
```

### Add a New Combo

1. Add to `knucklehead/combos.dtsi`:
```c
COMBO(name, &kp KEY, 14 27, L1, COMBO_TERM_DEFAULT, COMBO_QUICK_TAP_MS)
```

2. Add visual to `keymap-drawer/combos.yaml`:
```yaml
- p: [14, 27]
  k: { t: "symbol" }
  l: [L1]
```

### Add a New Macro

Add to `knucklehead/macros.dtsi`:
```c
/omit-if-no-ref/ macro_name: macro_name {
  wait-ms = <0>;
  tap-ms = <0>;
  compatible = "zmk,behavior-macro";
  #binding-cells = <0>;
  bindings = <&kp KEY1 &kp KEY2>;
};
```

Reference in layers: `&macro_name`

### Add a New Behavior

1. Define in `knucklehead/behaviors.dtsi` using ZMK behavior syntax
2. Add visual mapping in `keymap-drawer/config.yaml` under `raw_binding_map`

### Modify Layers

- **L1 (base)**: Edit active layout file (e.g., `L1_colemak-dh.dtsi`)
- **L2 (numbers/nav)**: Edit `knucklehead/L2.dtsi`
- **Fn (function/system)**: Edit `knucklehead/Fn.dtsi`

Maintain the 42-key grid structure with ASCII diagrams for readability.

## Keymap Visualization

### Automated (Recommended)
- **Workflow**: `.github/workflows/draw.yml` auto-generates SVG on keymap changes
- **Output**: `img/corneish_zen.svg`
- **Auto-commits** with message: "chore(draw): keymap"

### Local Generation
```zsh
./scripts/draw.zsh
```

### Configuration Files
- `keymap-drawer/config.yaml` - Maps ZMK bindings to visual symbols, styling
- `keymap-drawer/combos.yaml` - Simplified combo definitions for visualization

## Design Philosophy

### Single Base Layer
L1 is the only permanent layer. All upper layers use momentary (`&mo`), sticky (`&sl`), or smart behaviors. To change layouts, edit at compile time.

### Non-Stacking Upper Layers
Layer-switching macros (`&csl`, `&cmo`) cancel active layers before switching to prevent stacking. This ensures transparent keys always fall through to L1, making layer behavior predictable.

### Mnemonic Key Placement
Keys maintain consistent positions across layers. When replaced on upper layers, associated keys use mnemonics (e.g., Fn keys align with number positions on L2).

### macOS Optimization
- Uses macOS modifier symbols (⌃ ⌥ ⌘ ⇧)
- Built-in screenshot shortcuts (`&ss_full`, `&ss_sel`, `&ss_bar`, `&ss_win`, `&ss_full_clip`, `&ss_sel_clip`)
- Mnemonic placement matches Apple keyboard conventions

**macOS Brightness Keys (Important):**
- macOS doesn't properly support HID consumer brightness codes from external keyboards
- Instead, use `SLCK` (Scroll Lock) for brightness down and `PAUSE_BREAK` for brightness up
- These get treated as F14/F15 on macOS which control brightness
- Reference: [ZMK Issue #1045](https://github.com/zmkfirmware/zmk/issues/1045)

## Hardware Configuration

### Change Board/Shield
Edit `build.yaml`:
```yaml
include:
  - board: nice_nano_v2
    shield: corne_left nice_view_adapter nice_view
  - board: nice_nano_v2
    shield: corne_right nice_view_adapter nice_view
```

### Firmware Settings
Edit `.conf` files in `config/`:
- `corne.conf` / `corneish_zen.conf` - Bluetooth power, sleep timeout, debouncing, display settings

## OLED Display Customization

The repository uses the `zmk-nice-oled` module (from mctechnology17) which provides custom widgets for nice!OLED displays.

### Current Configuration (Productivity Focus)

**Left Display (Central):**
- Layer indicator (L1, L2, Fn)
- WPM graph - tracks typing speed over time
- Battery levels (both halves)

**Right Display (Peripheral):**
- Bongo Cat animation - responds to typing
- Bluetooth profile and connection status
- HID indicators (Caps Lock, Num Lock, Scroll Lock)

### Available Widgets

Edit `config/corne.conf` to enable/disable widgets:

**WPM Displays:**
- `CONFIG_NICE_OLED_WIDGET_WPM_NUMBER` - Numeric WPM
- `CONFIG_NICE_OLED_WIDGET_WPM_SPEEDOMETER` - Speedometer gauge
- `CONFIG_NICE_OLED_WIDGET_WPM_GRAPH` - Graph over time
- `CONFIG_NICE_OLED_WIDGET_WPM_LUNA` - Animated Luna character
- `CONFIG_NICE_OLED_WIDGET_WPM_BONGO_CAT` - Animated Bongo Cat

**Animations:**
- `CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_CAT` - Cat animation
- `CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_GEM` - Gem animation
- `CONFIG_NICE_OLED_WIDGET_ANIMATION_PERIPHERAL_POKEMON` - Pokemon character

**Status Indicators:**
- `CONFIG_NICE_OLED_WIDGET_HID_INDICATORS` - CapsLock/NumLock/ScrollLock
- `CONFIG_NICE_OLED_WIDGET_MODIFIERS_INDICATORS` - Ctrl/Shift/Alt/Cmd state

**RAW HID Features** (requires zmk-hid-host app):
- `CONFIG_NICE_OLED_WIDGET_RAW_HID_TIME` - System time
- `CONFIG_NICE_OLED_WIDGET_RAW_HID_VOLUME` - Audio volume
- `CONFIG_NICE_OLED_WIDGET_RAW_HID_WEATHER` - Weather info (macOS)
- `CONFIG_NICE_OLED_WIDGET_RAW_HID_MEDIA_PLAYER` - Spotify now playing (macOS)
