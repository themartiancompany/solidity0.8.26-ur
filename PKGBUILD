#!/usr/bin/env bash
# shellcheck disable=SC2034
# shellcheck disable=SC2154
# The PKGBUILD for Solidity.
# Maintainer: Matheus <matheusgwdl@protonmail.com>
# Contributor: Matheus <matheusgwdl@protonmail.com>
# Contributor: Serge K <arch@phnx47.net>
# Contributor: Nicola Squartini <tensor5@gmail.com>

pkgname="solidity"
pkgver="0.8.25"
pkgrel="1"
pkgdesc="Contract-Oriented Programming Language"
arch=("x86_64")
url="https://github.com/ethereum/${pkgname}"
license=("GPL-3.0-or-later")
depends=("boost")
optdepends=("cvc4: SMT checker"
    "z3: SMT checker")
makedepends=("cmake")
conflicts=("solidity-bin" "solidity-git")
source=("${pkgname}-v${pkgver}.tar.gz::${url}/releases/download/v${pkgver}/${pkgname}_${pkgver}.tar.gz")
sha512sums=("03d47be7f7356cf6e0763fa96a008076e2baec849064bea74135e8c59b50c7734c3757f9e748b410a2a9fc6c8d05a712ed8783f4457de9f3e6e8f32317b6f538")

_compile()
{
    cmake -B "${srcdir}"/"${pkgname}"_"${pkgver}"/build/ \
        -D CMAKE_BUILD_TYPE=None \
        -D CMAKE_INSTALL_PREFIX=/usr/ \
        -D ONLY_BUILD_SOLIDITY_LIBRARIES=OFF \
        -D PEDANTIC=ON \
        -D PROFILE_OPTIMIZER_STEPS=OFF \
        -D SOLC_LINK_STATIC=OFF \
        -D SOLC_STATIC_STDLIBS=OFF \
        -D STRICT_NLOHMANN_JSON_VERSION=OFF \
        -D STRICT_Z3_VERSION=OFF \
        -D TESTS="$1" \
        -D USE_SYSTEM_LIBRARIES=OFF \
        -D USE_Z3=OFF \
        -S "${srcdir}"/"${pkgname}"_"${pkgver}"/ \
        -Wno-dev
    cmake --build "${srcdir}"/"${pkgname}"_"${pkgver}"/build/
}

build()
{
    for build_tests in "OFF" "ON"; do
        _compile "${build_tests}"
    done
}

check()
{
    _compile "ON"
    ctest --output-on-failure --test-dir "${srcdir}"/"${pkgname}"_"${pkgver}"/build/
    _compile "OFF"
}

package()
{
    # Assure that the directories exist.
    mkdir -p "${pkgdir}"/usr/share/doc/"${pkgname}"/

    # Install the software.
    DESTDIR="${pkgdir}"/ cmake --install "${srcdir}"/"${pkgname}"_"${pkgver}"/build/

    # Install the documentation.
    install -Dm644 "${srcdir}"/"${pkgname}"_"${pkgver}"/README.md "${pkgdir}"/usr/share/doc/"${pkgname}"/
    cp -r "${srcdir}"/"${pkgname}"_"${pkgver}"/docs/* "${pkgdir}"/usr/share/doc/"${pkgname}"/

    find "${pkgdir}"/usr/share/doc/"${pkgname}"/ -type d -exec chmod 755 {} +
    find "${pkgdir}"/usr/share/doc/"${pkgname}"/ -type f -exec chmod 644 {} +
}
