EFI filesystem drivers, bundled so Ploader can read kernels/initrds off
non-FAT partitions (btrfs, ext2/3/4, ntfs, xfs, etc) — without them Ploader
can only see the FAT32 ESP itself and finds no OS entries on a typical
Arch/btrfs root layout.

Source: [efifs](https://github.com/pbatard/efifs) (GPL-3.0-or-later),
pulled from Arch's `extra/efifs` package (version 1.12-1), x64 build.

NOTE: this directory is intentionally named "efifs-drivers", not
"drivers_x64" — the project's own `make clean` target does
`rm -rf drivers_x64/*`, which would wipe these checked-in binaries every
time someone runs a clean build. PKGBUILD copies them into
`/usr/share/pearos-bootloader/drivers_x64/` at package time, and
`ploader-install` copies them onto the ESP under
`EFI/ploader/drivers_x64/` from there, matching what the compiled binary
actually expects at runtime.
