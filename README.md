# Keyball44 ZMK Configuration

ZMK firmware configuration for the Keyball44 split keyboard with integrated
trackball.

## Hardware

- **Keyboard**: Keyball44 (44-key split ergonomic)
- **Controllers**: nice!nano v2 (both halves)
- **Trackball**: PMW3610 optical sensor (right half)
- **Display**: SSD1306 OLED (128x32)

## Layers

| # | Name    | Description                                      |
|---|---------|--------------------------------------------------|
| 0 | DEFAULT | QWERTY base layer with home row mods             |
| 1 | NUM     | Numbers and symbols (top row: `!@#$%^&*()`)      |
| 2 | SYM     | Programming symbols, brackets, quotes, tmux      |
| 3 | ACC     | Lowercase accented characters (é, è, à, etc.)    |
| 4 | ACCS    | Uppercase accented characters (É, È, À, etc.)    |
| 5 | MOVE    | Arrow keys, F-keys, navigation (Home/End/PgUp)   |
| 6 | MOUSE   | Mouse buttons (auto-activated on trackball move) |
| 7 | SCROLL  | Trackball outputs scroll instead of cursor       |
| 8 | SNIPE   | Reduced trackball sensitivity for precision      |
| 9 | SYS     | Bluetooth profile selection and management       |

## Trackball Features

The PMW3610 trackball has three special modes configured in
`boards/shields/keyball_nano/keyball44_right.overlay`:

- **automouse-layer (6)**: Automatically activates the MOUSE layer when the
  trackball moves, providing quick access to click buttons without holding a
  layer key
- **scroll-layers (7)**: When layer 7 (SCROLL) is active, trackball movement
  outputs scroll events instead of cursor movement
- **snipe-layers (8)**: When layer 8 (SNIPE) is active, trackball sensitivity
  is reduced for precise cursor positioning

## Accents

French accented characters are available via macOS dead key sequences.

**Lowercase accents** (layer 3 - ACC, accessed via S or L hold):

| Key | Character | Description    |
|-----|-----------|----------------|
| W   | è         | grave e        |
| E   | é         | acute e        |
| R   | ê         | circumflex e   |
| U   | ù         | grave u        |
| I   | î         | circumflex i   |
| O   | ô         | circumflex o   |
| A   | à         | grave a        |
| S   | â         | circumflex a   |
| C   | ç         | cedilla c      |

**Uppercase accents** (layer 4 - ACCS, accessed via A+D or ;+L combo):

Same key positions as lowercase, but produces uppercase variants (É, È, À, etc.)

- **A + D**: Momentarily activates ACCS layer (left hand)
- **; + L**: Momentarily activates ACCS layer (right hand)

## Home Row Mods

The default layer uses home row modifiers for ergonomic access to modifiers:

**Left hand:**
- `A` → Left Shift (hold)
- `S` → Layer 3/ACC (hold)
- `D` → Left Alt (hold)
- `F` → Layer 2/SYM (hold)

**Right hand:**
- `J` → Layer 2/SYM (hold)
- `K` → Right Alt (hold)
- `L` → Layer 3/ACC (hold)
- `;` → Right Shift (hold)

## Key Behaviors

- **Layer-tap timing**: 240ms tapping term, balanced flavor
- **Mod-tap timing**: 200ms tapping term, tap-preferred flavor
- **Quick-tap**: 150ms for both (allows rapid repeat taps)
- **Caps word**: Continues with underscore and minus keys

## Building

Firmware builds automatically via GitHub Actions on push. Download the
artifacts from the Actions tab.

### Build outputs

- `keyball44_left-nice_nano.uf2` - Left half firmware
- `keyball44_right-nice_nano.uf2` - Right half firmware (includes ZMK Studio
  support)
- `settings_reset-nice_nano.uf2` - Settings reset utility

### Flashing

1. Connect the nice!nano via USB
2. Double-tap the reset button to enter bootloader mode
3. Copy the appropriate `.uf2` file to the mounted drive

## File Structure

```
├── boards/shields/keyball_nano/   # Shield definition files
│   ├── keyball44.dtsi             # Physical layout and matrix
│   ├── keyball44_left.overlay     # Left half pin configuration
│   ├── keyball44_right.overlay    # Right half with trackball config
│   └── *.conf                     # Shield-specific settings
├── config/
│   ├── keyball44.keymap           # Keymap and layer definitions
│   ├── keyball44.conf             # Firmware configuration
│   ├── keyball44.json             # Layout for keymap editors
│   └── west.yml                   # ZMK and driver dependencies
└── build.yaml                     # GitHub Actions build matrix
```

## Dependencies

Defined in `config/west.yml`:

- [ZMK Firmware](https://github.com/zmkfirmware/zmk) (main branch, Zephyr 4.x)
- [PMW3610 Driver](https://github.com/AntoineGS/zmk-pmw3610-driver) (for
  trackball support with improved deep sleep battery consumption)

## Configuration Options

Key settings in `config/keyball44.conf`:

| Setting                          | Value | Description                   |
|----------------------------------|-------|-------------------------------|
| `CONFIG_ZMK_DISPLAY`             | y     | Enable OLED display           |
| `CONFIG_ZMK_EXT_POWER`           | y     | External power control        |
| `CONFIG_BT_CTLR_TX_PWR_PLUS_8`   | y     | Max Bluetooth TX power        |
| `CONFIG_ZMK_BEHAVIORS_QUEUE_SIZE`| 512   | Larger queue for complex mods |
