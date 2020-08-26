# Maintainer: Luís Ferreira <luis at aurorafoss dot org>
# Contributor: Andrew Rabert <ar@nullsum.net>

pkgname=scrcpy
pkgver=1.16
pkgrel=2
pkgdesc='Display and control your Android device'
arch=(x86_64)
url='https://github.com/Genymobile/scrcpy'
license=('Apache')
depends=('android-tools' 'ffmpeg' 'sdl2')
makedepends=('gradle' 'meson' 'java-environment')
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/Genymobile/scrcpy/archive/v${pkgver}.tar.gz")
sha256sums=('94cbd59e26faa08ca25d5126d6c8d45e831b6a9e716ce05cd57bc4bcc751f742')

prepare() {
  cd "${pkgname}-${pkgver}"

  rm -rf gradle/wrapper/ # Remove prebuilt gradle-wrapper
}

build() {
  cd "${pkgname}-${pkgver}"

  # build the server from source instead of prebuilt it
  cd "server"
  ./build_without_gradle.sh

  meson \
    --prefix /usr \
    --buildtype release \
    --strip \
    -Db_lto=true \
    build

  ninja -C build
}

package() {
    cd "${pkgname}-${pkgver}"
    DESTDIR="${pkgdir}" ninja -C build install
    install -Dm644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname"
}
