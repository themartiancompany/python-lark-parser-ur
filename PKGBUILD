# Maintainer:
# Contributor: Felix Yan <felixonmars@archlinux.org>

pkgname=python-lark-parser
pkgver=1.1.7
pkgrel=1
pkgdesc='A modern parsing library'
arch=('any')
url='https://github.com/lark-parser/lark'
license=('MIT')
depends=(
  'python'
  'python-typing_extensions'
)
makedepends=(
  'git'
  'python-build'
  'python-installer'
  'python-setuptools'
  'python-wheel'
)
checkdepends=(
  'python-js2py'
  'python-tzlocal'
  'python-atomicwrites'
  'python-regex'
)
optdepends=(
  'python-atomicwrites: for atomic_cache'
  # 'python-interegular: for interegular support'  # TODO: package
  'python-regex: for regex support'
  'python-js2py: for nearley support'
)
provides=('python-lark')
_commit='e795810d9849a5ee517effadc7693f7a4ea2f076'
source=(
  "$pkgname::git+$url#commit=$_commit"
  'github.com-Hardmath123-nearley::git+https://github.com/Hardmath123/nearley'
)
b2sums=('SKIP' 'SKIP')

pkgver() {
  cd "$pkgname"

  git describe --tags | sed 's/^v//'
}

prepare() {
  cd "$pkgname"

  # setup git submodules
  git submodule init
  git config submodule.tests/test_nearley/nearley.url "$srcdir/github.com-Hardmath123-nearley"
  git -c protocol.file.allow=always submodule update
}

build() {
  cd "$pkgname"

  python -m build --wheel --no-isolation
}

check() {
  cd "$pkgname"

  python -m tests
}

package() {
  cd "$pkgname"

  python -m installer --destdir="$pkgdir" dist/*.whl

  # symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir/usr/share/licenses/$pkgname"
  ln -s "$site_packages/lark-$pkgver.dist-info/LICENSE" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
