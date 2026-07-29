# Maintainer: pearOS-archlinux
# Based on the official Arch Linux 'refind' PKGBUILD, adapted to build
# pearOS's Ploader fork instead of upstream rEFInd.

pkgname=pearos-bootloader
pkgver=26.7
pkgrel=1
_commit=ae38d2236579324c94fd17a8160f8d09906a7ad4
pkgdesc="pearOS's Ploader boot manager (fork of rEFInd)"
arch=('x86_64')
url='https://github.com/pearOS-archlinux/pearos-bootloader'
license=('GPL3')
makedepends=('bash' 'dosfstools' 'efibootmgr' 'gnu-efi')
depends=('bash' 'dosfstools' 'efibootmgr')
optdepends=('gptfdisk: for finding non-vfat ESP with ploader-install'
            'openssl: for generating local certificates with ploader-install'
            'sbsigntools: for EFI binary signing with ploader-install'
            'sudo: for privilege elevation in ploader-install')
source=("git+$url.git#commit=$_commit"
        "ploader-install")
sha256sums=('SKIP' 'SKIP')

# NOTE: unlike the official Arch 'refind' PKGBUILD, no gnu-efi-4.x /
# --output-target compat patches are needed here — this fork already
# carries its own fixes (PearOSReallocatePool, AsciiStrLen guard, and
# Make.common already uses --output-target).

build() {
	cd "$srcdir/$pkgname"
	make
}

package() {
	cd "$srcdir/$pkgname"

	# main EFI binary + tools
	install -Dm644 ploader/ploader_x64.efi "$pkgdir/usr/share/$pkgname/ploader_x64.efi"
	install -Dm644 gptsync/gptsync_x64.efi "$pkgdir/usr/share/$pkgname/tools_x64/gptsync_x64.efi"

	# sample config + sbat
	install -Dm644 ploader.conf-sample "$pkgdir/usr/share/$pkgname/ploader.conf-sample"
	install -Dm644 ploader-sbat.csv "$pkgdir/usr/share/$pkgname/ploader-sbat.csv"
	install -Dm644 ploader-sbat-local.csv "$pkgdir/usr/share/$pkgname/ploader-sbat-local.csv"

	# hardcoded theme (icons, background, font, selection art) + boot chime
	cp -r theme "$pkgdir/usr/share/$pkgname/theme"
	install -Dm644 bootsound.wav "$pkgdir/usr/share/$pkgname/bootsound.wav"

	# install script (adapted from upstream refind-install)
	install -Dm755 "$srcdir/ploader-install" "$pkgdir/usr/bin/ploader-install"

	# docs + license
	install -Dm644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
	install -Dm644 README.txt "$pkgdir/usr/share/doc/$pkgname/README.txt"
	install -Dm644 BUILDING.txt "$pkgdir/usr/share/doc/$pkgname/BUILDING.txt"
	install -Dm644 CREDITS.txt "$pkgdir/usr/share/doc/$pkgname/CREDITS.txt"
	install -Dm644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
	install -Dm644 COPYING.txt "$pkgdir/usr/share/licenses/$pkgname/COPYING.txt"
}
