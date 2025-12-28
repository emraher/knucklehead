# Layout Improvements Changelog

This document summarizes all changes made in the `layout-improvements` branch.

## Summary

This branch includes comprehensive improvements to optimize the Knucklehead layout for R, LaTeX, Markdown, and Bash workflows on macOS. Key improvements include better ergonomics, workflow-specific shortcuts, enhanced navigation, and proper macOS integration.

## 1. Layer Access Optimization

### Fn Layer Quick Access
- **Added**: Fn layer combo via both middle thumb keys (Space + smart_num, positions 37+40)
- **Benefit**: Faster access to function keys and system controls without holding outer pinky keys
- **File**: `knucklehead/combos.dtsi`

## 2. R/LaTeX/Markdown/Bash Workflow Optimizations

### R Language Support
**New Macros** (`knucklehead/macros.dtsi`):
- `r_assign` - `<-` assignment operator
- `r_pipe` - `|>` native pipe operator
- `r_chunk` - ` ```{r}``` ` code chunk insertion for RMarkdown/Quarto

**New Combos** (`knucklehead/combos.dtsi`):
- `<-` → H+. (positions 31+33)
- `|>` → K+H (positions 30+31)
- ` ```{r}``` ` → R+C (positions 14+27)

### LaTeX & General Symbol Support
**New Symbol Combos** (`knucklehead/combos.dtsi`):
- `{` `}` → C+D, D+V (positions 27+28, 28+29)
- `:` → .+; (positions 33+34)
- `|` → H+, (positions 31+32)
- `_` → ,+. (positions 32+33)
- `` ` `` → '+O (positions 10+22)
- `~` → O+; (positions 22+34)

## 3. Layer Behavior Improvements

### Trans → None Conversion
- **Changed**: `&trans` → `&none` for unused keys on L2 and Fn layers
- **Benefit**: Prevents accidental character input on upper layers (e.g., pressing Z on L2 no longer types "z")
- **Files**: `knucklehead/L2.dtsi`, `knucklehead/Fn.dtsi`

## 4. Navigation Enhancements (L2 Layer)

### Word & Document Navigation
**Added to L2 bottom row** (`knucklehead/L2.dtsi`):
- Position 26: Cmd+Down (jump to document start)
- Position 27: Cmd+Up (jump to document end)
- Position 30: Cmd+Left (jump word backward)
- Position 31: HOME (line start)
- Position 32: END (line end)
- Position 33: Cmd+Right (jump word forward)

**Benefits**: Essential for quick navigation in R scripts, bash commands, markdown documents

## 5. Thumb Layout Optimization

### Smart Enter Behavior
**Created new behavior** (`knucklehead/behaviors.dtsi`):
- `smart_enter` - Combines Enter functionality with smart shift
  - Tap: Enter
  - Hold: Shift
  - Double-tap: caps_word

**Layout Changes** (`knucklehead/L1_colemak-dh.dtsi`):
- Position 40: `&smart_shift` → `&smart_enter RSHFT`
- Position 23: ENTER → ' (quote) - moved to home row pinky
- Position 10: ' (quote) → ? (question mark)

**Benefits**:
- Enter on thumb more ergonomic than pinky reach
- Quote on home row better for R strings
- Question mark useful for R help system (`?function`)

## 6. Caps Word Customization

### R/LaTeX Workflow Support
**Modified** (`knucklehead/behaviors.dtsi`):
- Continue-list expanded to include: `_`, `0-9`, `-`, `{`, `}`, `^`
- Allows seamless typing of:
  - R variables: `MY_VARIABLE_123`
  - LaTeX labels: `\label{FIG_MAIN_RESULT}`
  - LaTeX superscripts: `X^{MAX_VALUE}`

## 7. Media Controls Optimization

### Improved Layout (L2.dtsi)
**Reorganized top row right** (positions 6-11):
- Play/Pause → Position 6 (most-used, easiest access)
- Volume Down → Position 7
- Volume Up → Position 8
- Previous Track → Position 9
- Next Track → Position 10
- Mute → Position 11 (end, avoid accidental press)

## 8. Screenshot Shortcuts Expansion

### New macOS Screenshot Macros
**Added** (`knucklehead/macos-shortcuts.dtsi`):
- `ss_win` - Window capture (Cmd+Shift+4, Space)
- `ss_full_clip` - Full screen to clipboard (Ctrl+Cmd+Shift+3)
- `ss_sel_clip` - Selection to clipboard (Ctrl+Cmd+Shift+4)

**Fn Layer Layout** (`knucklehead/Fn.dtsi`):
- Position 6: Full screen
- Position 7: Select region
- Position 8: Screenshot toolbar
- Position 9: Window capture (NEW)
- Position 10: Full to clipboard (NEW)
- Position 20: Selection to clipboard (NEW)

## 9. macOS Brightness Fix

### Fixed Non-Working Brightness Controls
**Problem**: macOS doesn't support standard HID brightness codes from external keyboards

**Solution** (`knucklehead/Fn.dtsi`):
- Position 36: `C_BRI_DN` → `SLCK` (Scroll Lock = F14 = Brightness Down)
- Position 37: `C_BRI_UP` → `PAUSE_BREAK` (= F15 = Brightness Up)
- Position 11: Removed non-functional `C_POWER`

**Reference**: [ZMK Issue #1045](https://github.com/zmkfirmware/zmk/issues/1045)

## 10. OLED Display Configuration

### Productivity Focus Setup
**Configured** (`config/corne.conf`):

**Left Display (Central)**:
- Layer indicator
- WPM graph
- Battery levels

**Right Display (Peripheral)**:
- Bongo Cat animation
- Bluetooth status
- HID indicators

**Widgets Enabled**:
- `CONFIG_NICE_OLED_WIDGET_WPM=y`
- `CONFIG_NICE_OLED_WIDGET_WPM_GRAPH=y`
- `CONFIG_NICE_OLED_WIDGET_WPM_BONGO_CAT=y`
- `CONFIG_NICE_OLED_WIDGET_HID_INDICATORS=y`

## 11. Documentation

### Updated Files
- **README.md**: Added comprehensive sections for:
  - R/LaTeX/Markdown/Bash workflow optimizations
  - Smart Enter behavior documentation
  - OLED display features
  - macOS-specific optimizations
  - Navigation enhancements

- **CLAUDE.md**: Complete technical reference including:
  - Architecture overview
  - Smart behaviors (including smart_enter)
  - Caps word customization
  - OLED display customization
  - macOS brightness key workaround
  - All common development tasks

- **CHANGELOG_layout-improvements.md**: This file - comprehensive change summary

## Files Modified

### Core Functionality
- `knucklehead/behaviors.dtsi` - Added smart_enter, enter_dance; customized caps_word
- `knucklehead/macros.dtsi` - Added r_assign, r_pipe, r_chunk
- `knucklehead/combos.dtsi` - Added R operators, symbols, Fn layer access
- `knucklehead/macos-shortcuts.dtsi` - Added window and clipboard screenshot macros
- `knucklehead/L1_colemak-dh.dtsi` - Updated thumb layout, moved quote key
- `knucklehead/L2.dtsi` - Added navigation keys, optimized media controls, changed trans to none
- `knucklehead/Fn.dtsi` - Fixed brightness keys, added screenshot shortcuts, changed trans to none

### Configuration
- `config/corne.conf` - Added OLED widget configuration

### Documentation
- `README.md` - Added workflow optimization sections, updated features
- `CLAUDE.md` - Comprehensive technical documentation
- `keymap-drawer/config.yaml` - Added visualizations for new shortcuts
- `keymap-drawer/combos.yaml` - Added R operators and layer access combos

## Testing Recommendations

1. **Build & Flash**: Verify firmware compiles without errors
2. **Brightness**: Test Fn + thumbs for brightness control on macOS
3. **R Operators**: Test `<-` and `|>` combos
4. **Navigation**: Test word jumping and HOME/END on L2
5. **Smart Enter**: Test tap (Enter), hold (Shift), double-tap (caps_word)
6. **Caps Word**: Test with R variables (`MY_VAR_123`) and LaTeX labels
7. **Screenshots**: Test all 6 screenshot variants
8. **OLED**: Verify displays show correct widgets and animations

## Benefits Summary

- **Ergonomics**: Enter on thumb, reduced pinky strain
- **R Workflow**: Dedicated operators and chunk insertion
- **LaTeX**: Customized caps_word for labels and commands
- **Navigation**: Quick document/word jumping
- **macOS**: Proper brightness control and comprehensive screenshots
- **Visual Feedback**: OLED displays with useful info and fun animations
- **Cleaner Layers**: No accidental character input on upper layers
