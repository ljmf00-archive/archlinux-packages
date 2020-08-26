# Maintainer:
# Contributor: Felix Golatofski <contact@xdfr.de>
# Contributor: Tomas Ostasevicius (aquarix) <t dot ostasevicius at gmail dot com>

pkgname=gitter
pkgver=5.0.1
pkgrel=2
pkgdesc='Gitter Desktop Client - Where developers come to talk'
url='https://gitlab.com/gitlab-org/gitter/desktop'
license=('MIT')
arch=('x86_64')
makedepends=(git gulp npm jq)
source=("$pkgname::git+$url.git#tag=$pkgver"
        "npm-shrinkwrap.json")
install='gitter.install'
sha256sums=('SKIP'
            'a96541d17717256cb6a6c21a5372d62d4a56a32ea1f122c1f15450d988685818')

# Even though we don't officially support other archs, let's
# allow the user to use this PKGBUILD to compile the package
# for his architecture
case "$CARCH" in
  i686)
    _gitter_arch=32
    ;;
  x86_64)
    _gitter_arch=64
    ;;
  *)
    # Needed for mksrcinfo
    _gitter_arch=DUMMY
    ;;
esac

prepare() {
  cd "$srcdir/$pkgname"

  # Remove outdated npm dependency. This fix errors on postinstall script:
  # npm ERR! cb.apply is not a function
  cat <<< $(jq 'del(.devDependencies.npm)' package.json) > package.json

  # Another hack to fix npm errors: ReferenceError: primordials is not defined
  mv "$srcdir/npm-shrinkwrap.json" .

  # As they describe: Because of various errors, you may need to comment some
  # of these out depending on what platform you are building.
  sed -i "/^const SUPPORTED_PLATFORMS/s/\[.*\]/\['linux$_gitter_arch'\]/g" \
    gulpfile.js
}

build() {
  cd "$srcdir/$pkgname"

  npm install
  npm run gulp -- "pack:deb:$_gitter_arch"

}

package() {
  cd "$srcdir/$pkgname"
  ls -la

  # Non-deterministic race in npm gives 777 permissions to random directories.
  # See https://github.com/npm/npm/issues/9359 for details.
  chmod -R u=rwX,go=rX "$pkgdir"

  # npm installs package.json owned by build user
  # https://bugs.archlinux.org/task/63396
  chown -R root:root "$pkgdir"
}
