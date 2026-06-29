# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux72

pkgname="${_linuxprefix}-nvidia-open"
pkgver=610.43.02
pkgrel=0.1
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
source=('drm_atomic_state-to-commit.patch'
         'strncpy_removed.patch')
sha256sums=('3473f2f2b1648423dcf81779f2ba7ad8dd92acb07017d97a36e56db9cfa1a79a'
            'fdd60d8beb4a102e3522e6bcfc6ba55b66962f5de5e763903ae9a2ea28c92752')

prepare() {
  mkdir -p nvidia/${pkgver}/source_patched
  cp -av /usr/src/nvidia-${pkgver}/* nvidia/${pkgver}/source_patched
  cd nvidia/${pkgver}/source_patched/kernel-open
  # https://github.com/babiulep/my-kernel-patches/blob/main/NVIDIA/7.1/610.43.02
  patch -p1 -i $srcdir/drm_atomic_state-to-commit.patch
  patch -p1 -i $srcdir/strncpy_removed.patch
  cd ../..
  ln -sfv source_patched source
}

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
