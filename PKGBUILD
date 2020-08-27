# Maintainer: Olivier Biesmans <olivier at biesmans dot fr>

pkgname="ruby-insist"
pkgver=1.0.0
pkgrel=4
pkgdesc="A simple block-driven assertion library for both testing and for production code"
arch=('any')
url="https://github.com/jordansissel/ruby-insist"
license=('Apache')
depends=('ruby')
makedepends=('rubygems')
source=("https://rubygems.org/downloads/${pkgname#*-}-${pkgver}.gem")
noextract=("${pkgname#*-}-${pkgver}.gem")
sha256sums=(6f6759eee583dc4e00a6cc3f713cfa7c570572958ba1f5d65595046d795b832f)
options=(!emptydirs)

package() {
  local _gemdir
  _gemdir="$(ruby -e'puts Gem.default_dir')"

  gem install --ignore-dependencies --no-user-install -i "$pkgdir/$_gemdir" \
    -n "$pkgdir/usr/bin" "${pkgname#*-}-$pkgver.gem"
  find "${pkgdir}" -type f -name '*.gem' -delete

  install -dm 755 "${pkgdir}"/usr/share/licenses/${pkgname}
  ln -s "${_gemdir}/gems/${pkgname#*-}-${pkgver}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/"
}
