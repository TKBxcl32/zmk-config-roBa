# roBa ZMK Config — Copilot Instructions

## What this repo is

ZMK firmware configuration for **roBa**, a custom wireless split keyboard. The shield definition lives in `boards/shields/roBa/`; the keymap and west manifest live in `config/`.

## Build

Firmware is built entirely via GitHub Actions — there is no local build command. Push to the repo (or trigger `workflow_dispatch`) to build. Artifacts are produced by the reusable workflow at `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3-branch`.

The build matrix is defined in `build.yaml`:
- `seeeduino_xiao_ble` + `roBa_R` (with `studio-rpc-usb-uart` snippet)
- `seeeduino_xiao_ble` + `roBa_L`
- `seeeduino_xiao_ble` + `settings_reset`

## Hardware architecture

| Half | Role | Notable hardware |
|------|------|-----------------|
| roBa_R | BLE central | PMW3610 trackball (SPI), ZMK Studio enabled |
| roBa_L | BLE peripheral | EC11 rotary encoder |

Both halves use the **Seeeduino XIAO BLE** (nRF52840). The right half is central: `ZMK_SPLIT_ROLE_CENTRAL=y` is set in `Kconfig.defconfig`.

The trackball driver is a custom fork: `kumamuk-git/zmk-pmw3610-driver` (pinned to `main` in `config/west.yml`).

## Key file roles

- `boards/shields/roBa/roBa.dtsi` — shared physical layout, matrix transform, kscan (rows only), encoder and trackball nodes (disabled by default)
- `boards/shields/roBa/roBa_L.overlay` — enables the encoder; defines left-half column GPIOs (6 columns)
- `boards/shields/roBa/roBa_R.overlay` — enables the trackball; configures SPI (PMW3610 at `spi0`); defines right-half column GPIOs (5 columns); sets `col-offset = <6>`
- `boards/shields/roBa/roBa_R.conf` — Kconfig for PMW3610 (CPI, orientation, scroll, automouse), ZMK Pointing, ZMK Studio
- `config/roBa.keymap` — all layers and behaviors
- `config/west.yml` — west manifest (ZMK v0.3-branch + pmw3610 driver)

## Layers

| Index | Name | Access |
|-------|------|--------|
| 0 | default | — |
| 1 | FUNCTION | hold right thumb enter |
| 2 | NUM | hold left thumb space |
| 3 | layer_6 (BT/bootloader) | hold left index (LANGUAGE_2) |
| 4 | MOUSE | auto-activated by trackball movement |
| 5 | SCROLL | hold `K` (home row) |
| 6 | SYMBOL | hold right index (LANGUAGE_1) |

> Note: the `#define` values at the top of `roBa.keymap` (`MOUSE 4`, `SCROLL 5`, `NUM 6`) are not used in the binding literals — the layers are referenced by their numeric index directly.

## Key conventions

- **Layer switching via `to_layer_0` macro**: a one-param macro that calls `&to 0` then sends a keycode. Used so thumb keys return to layer 0 after activating a feature (e.g., `&to_layer_0 LS(LG(S))`).
- **`mkp_exit_AML` macro**: mouse button press that also triggers `&sl MOUSE` on release, keeping the auto-mouse layer active for one more action.
- **`lt_to_layer_0` behavior**: hold-tap that holds `&mo` and taps `&to_layer_0`. Used on LANGUAGE keys.
- **`&mt` global override**: flavor set to `"balanced"`, `quick-tap-ms = <0>`.
- **`&sl` global override**: `release-after-ms = <250>` (required for double-click timing in MOUSE layer).
- **Encoder scroll**: left encoder uses `scroll_up_down` sensor behavior (SCRL_DOWN / SCRL_UP, `tap-ms = <20>`). SYMBOL layer overrides the encoder to `inc_dec_kp PAGE_UP PAGE_DOWN`.
- **Bluetooth combos**: BT profile selection uses simultaneous left+right key combos across both halves (positions 22–32). BT clear-all is a 4-key combo.
- **ZMK Studio**: enabled (`CONFIG_ZMK_STUDIO=y`), locking disabled. The `roBa_R` build includes the `studio-rpc-usb-uart` snippet for USB communication with Studio.

## Keymap visualization

The `draw.yml` workflow (manual trigger) uses [keymap-drawer](https://github.com/caksoylar/keymap-drawer) to generate `keymap-drawer/roBa.svg` from `config/roBa.keymap`. Output goes to `keymap-drawer/` as both SVG and YAML. The YAML representation is at `keymap-drawer/roBa.yaml`.

To update the SVG after editing the keymap, trigger the "Draw Keymap" workflow manually from GitHub Actions.

## PMW3610 trackball configuration (roBa_R.conf)

Key settings to be aware of when tuning:
- `CONFIG_PMW3610_CPI=400` / `CONFIG_PMW3610_CPI_DIVIDOR=1`
- `CONFIG_PMW3610_ORIENTATION_180=y` (sensor is mounted upside-down)
- `CONFIG_PMW3610_INVERT_SCROLL_X=y`
- `CONFIG_PMW3610_AUTOMOUSE_TIMEOUT_MS=700` (how long MOUSE layer stays active)
- `CONFIG_PMW3610_SMART_ALGORITHM=y`
- Snipe mode settings are commented out but available
