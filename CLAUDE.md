# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Knucklehead is a ZMK keyboard firmware configuration for a 42-key Corne split ergonomic keyboard. It uses Colemak-DH alphas and is optimized for macOS data science workflows (R, LaTeX, Markdown).

**Hardware**: Corne keyboard with nice!nano v2 controllers and nice!oled displays.

## Build System

Firmware builds automatically via GitHub Actions on push. There is no local build system.

**Workflow**: Push to repo → GitHub Actions builds → Download `firmware.zip` from Actions artifacts → Flash `.uf2` files to each keyboard half.

**Build triggers**: Changes to `build.yaml`, `config/*.keymap`, `config/*.dtsi`, `config/*.conf`, `config/west.yml`, or `knucklehead/*.dtsi`.

**Generate keymap SVG**: `./scripts/draw.zsh` (requires `keymap` CLI tool).

## Architecture

```
config/
├── corne.keymap          # Main keymap entry point (includes knucklehead/base.dtsi)
├── corne.conf            # Shared firmware settings (BLE, sleep, debounce)
├── corne_left.conf       # Left (central) OLED config
├── corne_right.conf      # Right (peripheral) OLED config
└── west.yml              # ZMK dependencies (ZMK v0.3.0, zmk-auto-layer, zmk-nice-oled)

knucklehead/              # Core firmware implementation
├── base.dtsi             # Layer definitions, includes all other files
├── behaviors.dtsi        # Custom behaviors (home row mods, smart shift, tap dances)
├── macros.dtsi           # R operators (<-, |>, %in%), layer switching macros
├── combos.dtsi           # Vertical combos for symbols, Bluetooth profiles
├── L1.dtsi               # Base layer (Colemak-DH)
├── L2.dtsi               # Numbers, navigation, media
├── Fn.dtsi               # Function keys, R operators, screenshots
└── macos-shortcuts.dtsi  # Screenshot shortcut macros
```

## Key Concepts

**Layer model**: L1 is the only true base layer. L2 and Fn are accessed via momentary holds, sticky taps, or smart behaviors. Layer macros (`&csl`, `&cmo`) cancel previous layers before activating to prevent stacking.

**Home row mods**: Timer-less implementation from @urob. Modifiers only activate when pressing opposite-hand keys (R/S/T/G on left → Ctrl/Opt/Cmd/Meh; N/E/I/M on right mirror).

**Smart behaviors**:
- `&smart_num` (thumb keys): tap = sticky L2, double-tap = num-word mode, hold = momentary L2
- `&smart_enter`: tap = enter, double-tap = caps_word, hold = shift

**Combos**: Vertical key pairs produce symbols (Q+A=!, W+R=@, etc.) and brackets (N+H=[, E+,=], I+.=\).

## ZMK-Specific Patterns

- Behaviors are defined in `behaviors.dtsi` using ZMK's behavior DSL (hold-tap, mod-morph, tap-dance, caps-word)
- Macros use `&macro_tap`, `&macro_press`, `&macro_release` sequences
- Combos specify key positions (0-41) and timeout in `combos.dtsi`
- Layer bindings use `&kp`, `&mo`, `&sl`, `&trans` and custom behaviors
- The `caps_word` continue-list is customized for R/LaTeX (underscore, numbers, minus, braces, caret)

## Dependencies

Defined in `config/west.yml`:
- ZMK v0.3.0 (zmkfirmware/zmk)
- zmk-auto-layer (urob/zmk-auto-layer) - provides `&num_word` behavior
- zmk-nice-oled (mctechnology17/zmk-nice-oled) - OLED widget library

## OLED Configuration

Uses 128×32 OLED screens with [zmk-nice-oled](https://github.com/mctechnology17/zmk-nice-oled). Split into shield-specific configs because some features (like WPM) only work on the central half.

**Left (central)** - `corne_left.conf`:
- Battery, BT profile, Luna animation, Modifiers (macOS symbols), Layer

**Right (peripheral)** - `corne_right.conf`:
- Battery, Cat animation
