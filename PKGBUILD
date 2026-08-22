# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Bernhard Landauer <bernhard@manjaro.org>
# Contributor: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux72

pkgname="${_linuxprefix}-nvidia-open"
pkgver=610.57.04
pkgrel=2
pkgdesc="NVIDIA open kernel modules for ${_linuxprefix}"
arch=('x86_64')
url="https://www.nvidia.com/"
license=('MIT AND GPL-2.0-only')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}" "nvidia-utils=${pkgver}" 'libglvnd')
makedepends=("${_linuxprefix}-headers" "nvidia-open-dkms=$pkgver")
provides=("nvidia=${pkgver}" 'NVIDIA-MODULE')
conflicts=("${_linuxprefix}-nvidia")
options=('!strip' '!debug')

build() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    fakeroot dkms build --dkmstree "${srcdir}" -m nvidia/${pkgver} -k ${_kernver}
}

package() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    install -Dt "${pkgdir}/usr/lib/modules/${_kernver}/extramodules" -m644 nvidia/${pkgver}/${_kernver}/${CARCH}/module/*.ko.zst

    # compress each module individually
    find "$pkgdir" -name '*.ko' -exec zstd --rm -19 {} +

    install -Dm644 /usr/share/licenses/nvidia-open-dkms/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
