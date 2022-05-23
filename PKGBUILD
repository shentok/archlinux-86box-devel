# Maintainer: oscareczek <oscareczek at gmail dot com>
pkgname=86box
_pkgname=86Box
pkgver=3.5
pkgrel=1
pkgdesc='An emulator for classic IBM PC clones'
arch=('pentium4' 'x86_64' 'arm7h' 'aarch64')
url='https://86box.net/'
license=('GPL2')
depends=('alsa-lib' 'faudio' 'freetype2' 'libpng' 'libslirp' 'openal' 'qt6-base' 'rtmidi' 'sdl2')
makedepends=('cmake>=3.21' 'ninja' 'qt6-tools')
optdepends=(
    '86box-roms: ROM files'
    'discord-game-sdk: Discord Rich Presence'
    'libpcap: Networking not limited to TCP/IP'
)
options=('!buildflags')
source=(
    "https://github.com/${_pkgname}/${_pkgname}/archive/refs/tags/v${pkgver}.tar.gz"
)
sha512sums=('8f40d918e0fbae388a4aa4f037adaa7fa1920cafd5c0493f1f052ee59c677a898226ab7958b8f22899e3983664c2e63c9bcbaf82e27b42efebfb22bdcf4717dc')

build() {
    case "$CARCH" in
        pentium4) _PRESET=regular;    _TOOLCHAIN=cmake/flags-gcc-i686.cmake ;;
        x86_64)   _PRESET=regular;    _TOOLCHAIN=cmake/flags-gcc-x86_64.cmake ;;
        arm7h)    _PRESET=regularndr; _TOOLCHAIN=cmake/flags-gcc-armv7.cmake ;;
        aarch64)  _PRESET=regularndr; _TOOLCHAIN=cmake/flags-gcc-aarch64.cmake ;;
    esac
    cmake -S"$_pkgname-$pkgver" -Bbuild --preset "$_PRESET" --toolchain "$_TOOLCHAIN" -DCMAKE_INSTALL_PREFIX=/usr -DUSE_QT6=on -DSLIRP_EXTERNAL=on
    cmake --build build
}

package() {
    DESTDIR="${pkgdir}" cmake --build "${srcdir}/build" --target install
    for i in 48x48 64x64 72x72 96x96 128x128 192x192 256x256 512x512; do
        install -Dm644 "$srcdir/$_pkgname-$pkgver/src/unix/assets/$i/net.86box.86Box.png" -t "$pkgdir/usr/share/icons/hicolor/$i/apps"
    done
    mkdir "$pkgdir/usr/share/applications"
    sed 's/^Exec.*/Exec=86Box -P .local\/share\/86Box/' "$srcdir/$_pkgname-$pkgver/src/unix/assets/net.86box.86Box.desktop" > "$pkgdir/usr/share/applications/net.86box.86Box.desktop"
}
