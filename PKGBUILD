# Maintainer: Martin Reboredo <yakoyoku@gmail.com>

pkgname=libmongocrypt
pkgver=1.12.0
pkgrel=1
pkgdesc='C library for client side and queryable encryption in MongoDB'
arch=('x86_64')
url='https://github.com/mongodb/libmongocrypt'
license=('Apache' 'BSD')
provides=(
  libkms_message.so
  libmongocrypt.so
)
depends=(libbson)
makedepends=(cmake git)
source=(
  ${pkgname}-${pkgver}.tar.gz::$url/archive/refs/tags/$pkgver.tar.gz
  shared-libbson.patch::$url/pull/681.patch
)
sha256sums=('6f4d5eca873e36492e4150cce23f84311fc0fcbfeffaad64971d2b2f34996d3c'
            '9064949bcd1b0496b7949f6646ab97d2e97b49255f9548ada909be228a5889a2')

prepare() {
  cd "$srcdir"/$pkgbase-$pkgver

  patch -Np1 -i ../shared-libbson.patch
  # sed -i 's/\(libbson_for_\)static/\1shared/' CMakeLists.txt
}

build() {
  cd "$srcdir"/$pkgbase-$pkgver

  cmake -B build \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_LIBDIR=lib \
    -DBUILD_VERSION="$pkgver" \
    -DBUILD_TESTING=OFF \
    -DENABLE_ONLINE_TESTS=OFF \
    -DENABLE_STATIC=OFF \
    -DMONGOCRYPT_MONGOC_DIR="USE-SYSTEM" \
    -DUSE_SHARED_LIBBSON=ON
  cmake --build build
}

package() {
  cd "$srcdir"/$pkgbase-$pkgver

  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}
