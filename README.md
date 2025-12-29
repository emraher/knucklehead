## Knucklehead

[![Build](https://github.com/emraher/knucklehead/actions/workflows/build.yml/badge.svg)](https://github.com/emraher/knucklehead/actions/workflows/build.yml)

Knucklehead is a mnemonic, macOS-optimized layout for 42-key split ergonomic keyboards. It uses the [Colemak-DH](https://colemakmods.github.io/mod-dh/) alpha layout and is specifically designed for data science workflows involving R, LaTeX, Markdown, and terminal work on macOS.

The layout prioritizes ergonomics and intuitiveness by keeping keys in consistent, memorable positions across layers. Rather than requiring you to learn arbitrary key placements, Knucklehead uses associative mnemonics—keys are placed where they "make sense" based on their relationship to other keys or their function.

### Layout

<img src="./img/corne.svg" alt="Knucklehead keymap layout" width="125%" />

## Legend

| Symbol | Key Name                                            | Symbol | Key Name                                                  |
| :----: | --------------------------------------------------- | :----: | --------------------------------------------------------- |
| 🆆    | [Smart 🆆ord behavior](#smart-behaviors)              | 🆇      | [E🆇it smart 🆆ord behavior](#exiting-smart-behaviors)      |
| ⌃      | Control                                             | ⇥      | Tab                                                       |
| ⌥      | Option                                              | ␣      | Space                                                     |
| ⌘      | Command                                             | ⇡      | Page Up                                                   |
| ▲      | Meh (⌃&nbsp;+&nbsp;⌥&nbsp;+&nbsp;⇧)                 | ⇣      | Page Down                                                 |
| ✦      | Hyper (⌃&nbsp;+&nbsp;⌥&nbsp;+&nbsp;⌘&nbsp;+&nbsp;⇧) | ⟲      | Firmware reset (hold: bootloader mode)                    |
| ⇧      | Shift                                               | ⌫      | Backspace                                                 |
| ⌦      | Delete                                              | ⏎      | Return                                                    |
| `L1`   | Layer 1                                             | `L2`   | Layer 2                                                   |
| `Fn`   | Function Layer                                      |        |                                                           |

The **Ref** layer in the keymap image shows key position numbers (0-41) for reference when reading this guide.

---

### Design Philosophy

#### Static, Associative Key Placement

This layout keeps keys in the same physical position across layers whenever possible. When a key must be replaced on an upper layer, its replacement uses an associative mnemonic to help you remember where it is.

For example, function keys F1-F5 on the Fn layer occupy the same positions as numbers 1-5 on L2. The semicolon sits next to comma and period, forming a natural punctuation cluster. Arrow keys use traditional VIM HJKL positions. These associations leverage existing muscle memory and logical groupings rather than forcing you to memorize arbitrary placements.

On upper layers, unused keys are "transparent"—they pass through to the base layer. This means if you accidentally press a letter key while on L2, you still get that letter rather than nothing. Combined with the single base layer design, this makes the keyboard's behavior predictable: you always know what layer you'll return to.

#### Single Base Layer with Non-Stacking Upper Layers

L1 is the one true base layer. There is no way to permanently activate another layer—only momentary, sticky, and smart behaviors are available. This eliminates the confusion of ZMK's layer stacking model where you might accidentally end up with multiple layers active.

Layer-switching macros (`&csl`, `&cmo`) send a cancel event before activating the new layer, ensuring you always return to L1 first. From your perspective, you simply "swap" between upper layers rather than stacking them. This keeps behavior predictable and prevents the disorientation of being "lost" in a layer maze.

#### Mnemonic Affordances

Keys are positioned using memorable associations. 


<!-- &nbsp;s force column width and prevent unwanted breaks -->

| Key&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Cue&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Mnemonic&nbsp;Affordance(s)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                        |
| --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **⇥** (Tab)                                                                                                           | **␣** (Space)                                                                                                                           | Tab as a space multiplier; proximity.                                                                                                                                |
| `` ` ~ ``                                                                                                             | `H`, **⇥** (Tab)                                                                                                                        | `~` a.k.a. "`H`ome" directory on 'nix systems; proximity. Same position, opposite hand as **⇥** (Tab), typically adjacent on Apple keyboards.                        |
| `- _`                                                                                                                 | `N + L`                                                                                                                                 | `N`egative, `L`ow; adjacent to `= +`                                                                                                                                 |
| `= +`                                                                                                                 | `E + U`                                                                                                                                 | `E`quals, `U`p (`+`); adjacent to `- _`                                                                                                                              |
| `[ {`                                                                                                                 | `N + H`                                                                                                                                 | Proximity; used to define a `N`ew `H`ash table/map on many programming languages; adjacent to `] }`                                                                  |
| `] }`                                                                                                                 | `E + ,`                                                                                                                                 | Proximity; used to `E`nd hash tables/maps on many programming languages; `,` is also typically used to delimit items within hash tables/maps; adjacent to `[ {`      |
| `-_  =+`<br/>`[{  ]}  \|\`                                                                                            | Apple ANSI position                                                                                                                     | This key cluster retains their order/position relative to each other as on Apple keyboards, but moved to vertical combos more easily accessible to stronger fingers. |
| `/ ?`                                                                                                                 | `Y + I` or `\| \`                                                                                                                       | Shape similarity, proximity, symmetry; same column as `\| \`; "wh`y`?"; `i`nterrogation s`y`mbol.                                                                    |
| `\| \`                                                                                                                | `I + .` or `/ ?`                                                                                                                        | Shape similarity, proximity, symmetry; logical `OR` — same position, opposite hand as `&` (logical `AND`); same column as `/ ?`.                                     |
| `&`                                                                                                                   | `R + X`                                                                                                                                 | Shape similarity; logical `AND` — same position, opposite hand as `\|` (logical `OR`)                                                                                |
| `*`                                                                                                                   | `S + C`                                                                                                                                 | `S`tar, wild `C`ard                                                                                                                                                  |
| `! @ # $ %`<br/>`^ & * ( )`                                                                                           | `1 2 3 4 5`<br/>`6 7 8 9 0`                                                                                                             | Symbols maintain their standard ANSI association with numbers as laid-out on `L2`, replicated as combos on `L1` and `L2`                                             |
| `Fn`                                                                                                                  | Apple ANSI position                                                                                                                     | `Fn` keys retains their familiar lower left corner position, mirrored on the right.                                                                                  |



The bracket cluster (`- _ = + [ { ] } \ |`) maintains its relative spatial arrangement from Apple keyboards but moves to vertical combos accessible to stronger fingers.

---

### Layers

#### L1 — Base Layer (Colemak-DH)

The base layer provides your primary typing surface with several enhancements over a standard layout.

**Backspace/Delete** occupies position 12 (left pinky home row). Tap for backspace, hold shift and tap for delete. This is more ergonomic than reaching to the top-right corner.

**Quote** sits at position 23 (right pinky home row) rather than the traditional top row, making string delimiters easier to reach during coding.

**Escape** is at position 0 (top-left) and **Cancel** at position 11 (top-right). The cancel key exits any smart word behavior, functioning as an "escape from smart mode."

**Home row modifiers** are available on the inner columns: hold R for Control, S for Option, T for Command, G for Meh (⌃+⌥+⇧) on the left hand, and the mirror positions on the right. These activate only when you press a key on the opposite hand, so normal typing is unaffected.

#### L2 — Numbers, Navigation, and Media

Access L2 via the inner thumb keys (positions 38 and 39). This layer provides numbers, arrows, and media controls.

**Numbers** follow a logical split: 1-5 on the top row (positions 1-5) and 6-0 on the home row (positions 13-17). This keeps all numbers on the left hand and places the most-used programming symbols in more accessible positions via their shifted variants.

**Arrows** use VIM-style HJKL positioning on the right home row (positions 18-21), with home row mods still active for easy selection shortcuts like ⌘+Shift+Arrow.

**Navigation shortcuts** on the bottom right row provide macOS text navigation: ⌘+Left/Right for line start/end (positions 30, 33), ⌥+Left/Right for word jumping (positions 31, 32), and ⌘+Up/Down for document start/end (positions 22, 34).

**Media controls** occupy the bottom left row: Mute (24), Volume Down (25), Volume Up (26), Previous Track (27), Next Track (28), and Play/Pause (29). Volume aligns with the `-/+` concept (down/up), and track controls align with left/right arrow concepts.

**Window management** shortcuts for [AeroSpace](https://github.com/nikitabobko/AeroSpace) occupy the top right row (positions 6-11): ⌥+HJKL for window focus and ⌥+Minus/Equal for resize.

#### Fn — Function Keys and System

Access the Fn layer via the outer pinky keys (positions 24, 35) or by holding the outer thumb keys (positions 36, 41).

**Function keys** F1-F15 align with their corresponding number positions from L2. F1-F5 on the top row (positions 1-5), F6-F10 on the home row (positions 13-17), and F11-F15 on the bottom row (positions 25-29).

**R language operators** occupy the right home row (positions 18-22): `%in%`, `<-`, `|>`, `` ```{r} ``, and Run (⌘+⇧+↵). This places the most frequently used R operations under your strongest fingers when holding Fn.

**Screenshot shortcuts** occupy the top right row (positions 6-11): Full screen to file (⌘+⇧+3), Region to file (⌘+⇧+4), Screenshot toolbar (⌘+⇧+5), Window to file (⌘+⇧+4+Space), Full screen to clipboard (⌃+⌘+⇧+3), and Region to clipboard (⌃+⌘+⇧+4).

**System controls** include Reset/Bootloader at positions 12 and 23 (tap to reset, hold for bootloader) and Output toggle at position 0 (switch between USB and Bluetooth).

---

### Smart Behaviors

Smart behaviors let you tap a key to enter a special mode rather than holding it. You remain in that mode until you press a "break" key (like space) or explicitly exit.

#### Smart L2 Layer

The inner thumb keys (positions 38 and 39) provide three ways to access L2:

**Tap** activates a sticky layer—L2 becomes active for exactly one keypress, then you return to L1. This is ideal for typing a single number or making one arrow movement.

**Double-tap** activates num-word mode—L2 stays active while you type numbers, arrows, operators (`. , / - _ + = *`), backspace, or delete. Press any other key to exit back to L1. This is perfect for entering longer numbers, equations, or navigating through code.

**Hold** activates a momentary layer—L2 is active only while you hold the key, exactly like a traditional layer switch.

#### Smart Enter

The right middle thumb (position 40) combines Enter with Shift and Caps Word:

**Tap** sends Enter. Moving Enter to the thumb reduces pinky strain during long coding sessions.

**Double-tap** activates Caps Word, which shifts all letters until you press a non-continuing character. The continue list is customized for R and LaTeX workflows: it includes underscore, numbers 0-9, minus, curly braces, and caret. This lets you type `MAX_VALUE_123`, `\label{FIG_MAIN}`, or `X^{MAX}` without manually holding shift.

**Hold** activates Shift, providing normal shift behavior for capitalization and symbols.

#### Exiting Smart Behaviors

Press the Cancel key (position 11, top-right on L1) to exit any smart behavior immediately. This functions as an "escape" for smart modes.

On L2, the same thumb keys that activated the smart layer can exit it: tap to exit and return to L1, or hold to exit the smart behavior but remain on L2 momentarily.

---

### Timer-less Home Row Mods

Home row mods let you access modifiers (Control, Option, Command, Meh) by holding keys in the home row rather than reaching to corner positions. This layout uses [@urob's timer-less implementation](https://github.com/urob/zmk-config), which solves the classic problem of home row mods interfering with fast typing.

The key insight is that modifiers only activate when you press a key on the **opposite** hand. If you're typing normally and roll from T to H quickly, the T won't accidentally trigger Command because H is on the same hand. But if you hold T and press J (opposite hand), you get Command+J.

**Left hand modifiers** (trigger with right hand keys):
- R → Control
- S → Option
- T → Command
- G → Meh (⌃+⌥+⇧)

**Right hand modifiers** (trigger with left hand keys):
- I → Control
- E → Option
- N → Command
- M → Meh (⌃+⌥+⇧)

To hold-repeat a home row key (for example, holding R to repeat the letter R), tap it twice quickly then hold on the second tap.

---

### Combos

Combos let you press two keys simultaneously to produce a third character. This layout uses vertical combos (pressing a key and the one directly below it) for symbols and special functions.

#### Symbol Combos (L1 and L2)

Left hand vertical combos produce the symbols normally accessed via Shift+Number:

| Position | Keys | Symbol |
|----------|------|--------|
| 1+13 | Q+A | ! |
| 2+14 | W+R | @ |
| 3+15 | F+S | # |
| 4+16 | P+T | $ |
| 5+17 | B+G | % |
| 13+25 | A+Z | ^ |
| 14+26 | R+X | & |
| 15+27 | S+C | * |
| 16+28 | T+D | ( |
| 17+29 | G+V | ) |

Right hand vertical combos produce brackets, operators, and navigation:

| Position | Keys | Symbol |
|----------|------|--------|
| 7+19 | L+N | - (minus) |
| 8+20 | U+E | = (equal) |
| 9+21 | Y+I | / (slash) |
| 19+31 | N+H | [ (left bracket) |
| 20+32 | E+, | ] (right bracket) |
| 21+33 | I+. | \ (backslash) |
| 6+18 | J+M | Page Up |
| 18+30 | M+K | Page Down |

#### Bluetooth Combos (Fn layer only)

The same vertical combo positions on the Fn layer control Bluetooth:

| Position | Keys | Function |
|----------|------|----------|
| 1+13 | F1+F6 | Select profile 1 |
| 2+14 | F2+F7 | Select profile 2 |
| 3+15 | F3+F8 | Select profile 3 |
| 4+16 | F4+F9 | Select profile 4 |
| 5+17 | F5+F10 | Select profile 5 |
| 1+2+3+4 | F1+F2+F3+F4 | Clear bonds |

---

### R and LaTeX Workflow

The Fn layer includes dedicated keys for R programming, eliminating the need to type multi-character operators manually.

| Position | Key | Output | Purpose |
|----------|-----|--------|---------|
| 18 | M | ` %in% ` | Membership test operator (with surrounding spaces) |
| 19 | N | `<-` | Assignment operator |
| 20 | E | `\|>` | Native pipe operator |
| 21 | I | `` ```{r}``` `` | RMarkdown/Quarto code chunk with cursor positioned inside |
| 22 | O | ⌘+⇧+↵ | Run current line/selection in RStudio or VS Code |

Access these by holding Fn (positions 24, 35, 36, or 41) and pressing the corresponding home row key.

The Caps Word behavior is customized to continue on characters common in R and LaTeX: underscore for `snake_case` variables, numbers for names like `VAR_123`, minus for some function names, curly braces for `\label{FIG_MAIN}`, and caret for superscripts like `X^{MAX}`.

---

### macOS Integration

#### Screenshot Shortcuts (Fn layer, positions 6-11)

| Position | Shortcut | Action |
|----------|----------|--------|
| 6 | ⌘+⇧+3 | Full screen to file |
| 7 | ⌘+⇧+4 | Region selection to file |
| 8 | ⌘+⇧+5 | Screenshot/recording toolbar |
| 9 | ⌘+⇧+4+Space | Window capture to file |
| 10 | ⌃+⌘+⇧+3 | Full screen to clipboard |
| 11 | ⌃+⌘+⇧+4 | Region selection to clipboard |

#### Output Toggle (Fn layer, position 0)

Toggle between USB and Bluetooth output. Useful when the keyboard is connected via USB but you want to use it with a Bluetooth-paired device.

---

### Bluetooth

The keyboard supports five Bluetooth device profiles, allowing you to pair with multiple computers and switch between them.

**Switching profiles**: On the Fn layer, use vertical combos at F-key positions 1-5 (same positions as the symbol combos, but on Fn layer).

**Clearing bonds**: Press F1+F2+F3+F4 simultaneously on the Fn layer, or uncomment `CONFIG_ZMK_BLE_CLEAR_BONDS_ON_START=y` in `config/corne.conf`, flash the firmware, pair your devices, then re-comment the line and flash again.

**Sleep**: The keyboard enters sleep mode after 30 minutes of inactivity to conserve battery. Press any key to wake it.

---

### OLED Displays

The keyboard uses [zmk-nice-oled](https://github.com/mctechnology17/zmk-nice-oled) for custom OLED widgets.

**Left display (central half)** shows:
- Battery percentage for both halves
- Active modifier indicators (⌃ ⌥ ⌘ ⇧)
- Current layer name (L1, L2, Fn)

**Right display (peripheral half)** shows:
- Battery percentage

Note: WPM and Bongo Cat features are currently disabled due to API incompatibility with ZMK v0.3.0.

---

### Building and Flashing

#### Automatic Building

Push changes to GitHub and the firmware builds automatically via GitHub Actions. The workflow monitors `build.yaml`, `config/` files, and `knucklehead/` files.

#### Downloading Firmware

1. Go to the [Actions tab](../../actions/workflows/build.yml)
2. Click the latest successful build (green checkmark)
3. Download `firmware.zip` from the Artifacts section
4. Extract to find `corne_left-*.uf2` and `corne_right-*.uf2`

#### Flashing

For each keyboard half:

1. Connect via USB-C
2. Double-tap the reset button on the nice!nano controller
3. The board appears as a USB drive named `NICENANO`
4. Drag the appropriate `.uf2` file onto the drive
5. The drive disconnects automatically when flashing completes

#### First Time Setup

1. Flash both halves
2. Disconnect USB from both halves
3. Power on both halves and wait 10-15 seconds for them to pair with each other
4. The keyboard appears as "Corne" in your Mac's Bluetooth settings
5. Pair and enter the passkey if prompted

