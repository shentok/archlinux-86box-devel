# Maintainer: Lili1228 <aur at lili dot lgbt>
pkgname=86box
_pkgname=86Box
pkgver=4.1.1
pkgrel=1
pkgdesc='An emulator for classic IBM PC clones'
arch=('pentium4' 'x86_64' 'armv7h' 'aarch64')
url='https://86box.net/'
license=('GPL-2.0-or-later')
depends=('fluidsynth' 'hicolor-icon-theme' 'libslirp' 'openal' 'qt6-base' 'rtmidi' 'sdl2' # explicit
'freetype2' 'gcc-libs' 'glib2' 'glibc' 'libevdev' 'libglvnd' 'libpng' 'libx11' 'libxcb' 'libxext' 'libxi' 'libxkbcommon-x11' 'libxkbcommon' 'wayland' 'zlib') # implicit 
makedepends=('cmake>=3.21' 'extra-cmake-modules' 'ninja' 'qt6-tools' 'vde2') # vulkan-headers on qt5
optdepends=(
    '86box-roms: ROM files'
    'discord-game-sdk: Discord Rich Presence'
    'libpcap: Networking not limited to TCP/IP'
)
options=('!buildflags')
source=(
    "${pkgname}_$pkgver.tgz::https://github.com/${_pkgname}/${_pkgname}/archive/refs/tags/v${pkgver}.tar.gz"
)
sha512sums=('d1b0a1810f8712464ce8266942e9bfdc9721ab4ac70bbb242b3b06d6a7d6613b7bcb0ba730a2e458b2731c8d58e38c4b245b6f4afb5ee6c11ab4a2fb0dfd6d5e')

build() {
    case "$CARCH" in
        pentium4) _NDR=off; _TOOLCHAIN=cmake/flags-gcc-i686.cmake ;;
        x86_64)   _NDR=off; _TOOLCHAIN=cmake/flags-gcc-x86_64.cmake ;;
        armv7h)    _NDR=on;  _TOOLCHAIN=cmake/flags-gcc-armv7.cmake ;;
        aarch64)  _NDR=on;  _TOOLCHAIN=cmake/flags-gcc-aarch64.cmake ;;
    esac
    LDFLAGS='-z now' cmake -S"$_pkgname-$pkgver" -Bbuild --preset regular --toolchain "$_TOOLCHAIN" -DCMAKE_INSTALL_PREFIX=/usr -DUSE_QT6=on -DNEW_DYNAREC="$_NDR"
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
