pkgname=mingw-w64-isl
pkgver=0.11.2
pkgrel=2
pkgdesc="Library for manipulating sets and relations of integer points bounded by linear constraints (mingw-w64)"
arch=(any)
url="http://www.kotnet.org/~skimo/isl"
license=("MIT")
makedepends=(mingw-w64-gcc mingw-w64-pkg-config)
depends=(mingw-w64-crt mingw-w64-gmp)
options=(!libtool !strip !buildflags)
source=("$url/${pkgname#mingw-w64-}-$pkgver.tar.bz2")
md5sums=('c40daa17d2995d1c98a0c1aca607541f')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    unset CPPFLAGS LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    ${srcdir}/${pkgname#mingw-w64-}-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch}
    make libisl_la_LDFLAGS="-no-undefined -version-info 11:2:1"
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' -o -name '*.bat' -o -name '*.def' -o -name '*.exp' -o -name '*.py' \
			| xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip --strip-unneeded
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
  done
}
