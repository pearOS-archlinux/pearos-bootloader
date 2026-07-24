# pearos-bootloader

pearOS's boot manager, based on [rEFInd](https://www.rodsbooks.com/refind/) (upstream install/usage docs are in `README.txt`). This fork hardcodes pearOS-specific branding and behavior; end-user theme configuration via `refind.conf` is intentionally disabled.

## Boot behavior

By default, the boot picker is never shown. On every boot, pearOS boots straight into the first detected OS with no screen shown at all. To reach the picker or a special boot mode, hold the corresponding key combination while the machine powers on.

| Combo | Boots into |
|---|---|
| *(nothing held)* | First detected OS, silently, no boot screen shown |
| `Alt` | The pearOS boot picker (menu) |
| `Alt` + `Meta` (Cmd/Win) + `R` | Partition labeled `pearos_internet_recovery` |
| `Shift` + `Alt` + `Meta` + `R` | Partition labeled `pearos_factory_recovery` |
| `Meta` + `R` | Partition labeled `pearos_local_recovery` |
| `Alt` + `D` | Partition labeled `pearos_hardware_test` |

This mirrors macOS's Option-key / Recovery-key boot conventions. Key state is read via `EFI_SIMPLE_TEXT_INPUT_EX_PROTOCOL`; on firmware that doesn't expose it, boot always proceeds silently into the first detected OS.

If a key combo is held but no partition with the matching label is found, boot falls through to the normal interactive picker instead of failing silently.

## Other pearOS customizations

- **Hardcoded theme** — icons, background, selection art, and font are baked into the `theme/` directory and loaded unconditionally; `refind.conf` directives that would normally control these (`icons_dir`, `banner`, `selection_big`/`selection_small`, `hideui`, `showtools`, `font`) are ignored.
- **Proportional bitmap font** — `libeg/text.c` auto-detects each glyph's ink width from the font atlas's alpha channel, so a proportional display font baked into the fixed-width atlas grid doesn't render with monospace-style gaps.
- **Boot chime** — `PlayBootSound()` in `refind/main.c` plays `bootsound.wav` via `EFI_AUDIO_IO_PROTOCOL`, provided by the bundled `refind/drivers_x64/AudioDxe.efi` driver (from OpenCorePkg, BSD-3-Clause). No-op on firmware without a supported HDA codec.
- **Short on-screen labels** — the selected entry's label under the icon row shows a short name (e.g. "Microsoft EFI boot") instead of rEFInd's verbose default title.
