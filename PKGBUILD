# Maintainer: Chilledheart <hukeyue@hotmail.com>
# Contributor: Chilledheart <hukeyue@hotmail.com>

pkgname=yass-proxy
pkgver=1.4.6
pkgrel=1
_pkgver=1.4.6
_pkgrel=1
pkgdesc="lightweight http/socks proxy"
arch=(x86_64)
url="https://github.com/Chilledheart/yass"
license=(GPL2)
depends=(gcc-libs glibc gtk4 zlib libnghttp2 c-ares)
optdepends=(gtk-update-icon-cache)
makedepends=(git ninja perl pkg-config cmake gtk4 gettext curl go clang lld llvm)
checkdepends=(curl)
provides=(yass-proxy)
conflicts=(yass-proxy-git)
source=("https://github.com/Chilledheart/yass/releases/download/${_pkgver}/yass-${_pkgver}.tar.gz")
sha256sums=('1e2deb84781fefcd9aa0575bc7ec79ba270a27911eb86493d82d578c07d9ece6')

prepare() {
  SRC_DIR="${srcdir}/yass-${_pkgver}"
  pushd $SRC_DIR
  cd tools
  go build
  cd ..
  popd
}

build(){
  SRC_DIR="${srcdir}/yass-${_pkgver}"
  pushd $SRC_DIR
  export CC=clang
  export CXX=clang++
  mkdir build-linux-amd64
  cd build-linux-amd64
  cmake .. -DGUI=ON -DCMAKE_BUILD_TYPE=Release -G Ninja -DBUILD_TESTS=on \
    -DUSE_SYSTEM_ZLIB=on -DUSE_SYSTEM_CARES=on -DUSE_SYSTEM_NGHTTP2=on \
    -DCMAKE_INSTALL_PREFIX=/usr -DCLI=off -DSERVER=off \
    -DUSE_LIBCXX=on -DENABLE_LTO=on
  ninja yass yass_test
  llvm-objcopy --strip-debug yass
  cd ..

  popd
}

check() {
  SRC_DIR="${srcdir}/yass-${_pkgver}"
  pushd $SRC_DIR
  ./build-linux-amd64/yass_test
  popd
}

package(){
  SRC_DIR="${srcdir}/yass-${_pkgver}"
  pushd $SRC_DIR

  install -Dm644 ./build-linux-amd64/LICENSE ${pkgdir}/usr/share/licenses/yass/LICENSE
  DESTDIR=${pkgdir} ninja -C build-linux-amd64 install
  rm -rf ${pkgdir}/usr/share/doc

  popd
}
