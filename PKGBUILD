pkgname=nodejs-lts-iron
pkgver=20.20.1
pkgrel=1
pkgdesc="Evented I/O for V8 javascript (LTS release: Iron)"
arch=('x86_64')
url="https://nodejs.org/"
license=('MIT')
depends=(
    'brotli'
    'c-ares'
    'icu'
    'libuv'
    'nghttp2'
    'openssl'
    'zlib'
)
makedepends=(
    'python'
    'procps-ng'
)
provides=("nodejs=${pkgver}")
conflicts=('nodejs')
options=('!lto')
source=(https://nodejs.org/dist/v${pkgver}/node-v${pkgver}.tar.xz
    update-icu-tests.patch)
sha256sums=(e540efdd6750f838e867daf9ab9d90ea195423f915613d05d87105f4d2ecd186
    43da0fb7469e34a239b2711876475f303a4012151e44f72e636a5a7fcf21bff8)

prepare() {
    cd node-v${pkgver}

    # Update ICU tests https://github.com/nodejs/node/pull/60523
    patch -Np1 -i ${srcdir}/update-icu-tests.patch
}

build() {
    cd node-v${pkgver}

    local configure_args=(
        --prefix=/usr
        --libdir=/usr/lib64
        --with-intl=system-icu
        --without-corepack
        --without-npm
        --shared-openssl
        --shared-zlib
        --shared-libuv
        --experimental-http-parser
        --shared-brotli
        --shared-cares
        --shared-nghttp2
    )

    # /usr/lib64/libnode.so uses malloc_usable_size, which is incompatible with fortification level 3
    CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
    CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

    CC="${CHOST}-gcc"
    CXX="${CHOST}-g++"

    ./configure "${configure_args[@]}"

    make
}

package() {
    cd node-v${pkgver}

    make DESTDIR=${pkgdir} install
}
