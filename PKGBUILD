pkgname=lib32-mingw-w64-gtkmm
pkgver=2.24.2
pkgrel=2
pkgdesc="C++ bindings for gtk2 (mingw-w64, 32-bit)"
arch=(any)
url="http://gtkmm.sourceforge.net"
license=("LGPL")
makedepends=(mingw-w64-gcc mingw-w64-pkg-config)
depends=(mingw-w64-crt
lib32-mingw-w64-atkmm
lib32-mingw-w64-pangomm
lib32-mingw-w64-gtk2)
options=(!libtool !strip !buildflags)
source=("http://ftp.gnome.org/pub/GNOME/sources/gtkmm/${pkgver%.*}/gtkmm-${pkgver}.tar.xz")
md5sums=('388a63ffc40cc8e208df9a1732a67d2d')

_architectures="i686-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    export CFLAGS="-O2 -pipe"
    export CXXFLAGS="$CFLAGS"
    export CPPFLAGS="$CPPFLAGS -D_REENTRANT"
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    ${srcdir}/gtkmm-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch} \
      --enable-static \
      --enable-shared \
      --disable-documentation
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' -o -name '*.bat' -o -name '*.def' -o -name '*.exp' | xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip -x
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
  done
}