# Maintainer:
# Contributor: Serge K <arch@phnx47.net>
# Contributor: Nicola Squartini <tensor5@gmail.com>

pkgname=solidity
pkgver=0.8.26
pkgrel=1
pkgdesc='Contract-Oriented Programming Language'
arch=('x86_64')
url='https://github.com/ethereum/solidity'
license=('GPL3')
depends=('gcc-libs')
optdepends=('z3: SMT checker' 'cvc4: SMT checker')
makedepends=('boost' 'cmake')
source=("${pkgname}_${pkgver}.tar.gz::${url}/releases/download/v${pkgver}/${pkgname}_${pkgver}.tar.gz")
sha512sums=('ac799b64a792a857469e06d258225865acfca715b191d0e8df471429827725e0cbe631e57938f2bb7560e3d1629223ef0e24652480f385ed581ac96269fe2e29')

build() {
  cmake -B build -S "${pkgname}_${pkgver}" \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DTESTS=OFF \
    -DSTRICT_Z3_VERSION=OFF
  cmake --build build
}

package() {
  DESTDIR="${pkgdir}" cmake --install build
}
