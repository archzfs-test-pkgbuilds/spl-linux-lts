# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
# ! WARNING !
#
# The archzfs packages are kernel modules, so these PKGBUILDS will only work with the kernel package they target. In this
# case, the archzfs-linux-lts packages will only work with the default linux-lts package! To have a single PKGBUILD target
# many kernels would make for a cluttered PKGBUILD!
#
# If you have a custom kernel, you will need to change things in the PKGBUILDS. If you would like to have AUR or archzfs repo
# packages for your favorite kernel package built using the archzfs build tools, submit a request in the Issue tracker on the
# archzfs github page.
#
pkgbase="spl-linux-lts"
pkgname=("spl-linux-lts" "spl-linux-lts-headers")
_splver="0.7.13"
_kernelver="4.19.41-1"
_extramodules="4.19.41-1-lts"

pkgver="${_splver}_$(echo ${_kernelver} | sed s/-/./g)"
pkgrel=1
makedepends=("linux-lts-headers=${_kernelver}")
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("https://github.com/zfsonlinux/zfs/releases/download/zfs-${_splver}/spl-${_splver}.tar.gz")
sha256sums=("6fd4445850ac67b228fdd82fff7997013426a1c2a8fa9017ced70cc9ad2a4338")
license=("GPL")
depends=("kmod" "linux-lts=${_kernelver}")

build() {
    cd "${srcdir}/spl-${_splver}"
    ./autogen.sh
    ./configure --prefix=/usr --libdir=/usr/lib --sbindir=/usr/bin \
                --with-linux=/usr/lib/modules/${_extramodules}/build \
                --with-linux-obj=/usr/lib/modules/${_extramodules}/build \
                --with-config=kernel
    make
}

package_spl-linux-lts() {
    pkgdesc="Solaris Porting Layer kernel modules."
    provides=("spl")
    groups=("archzfs-linux-lts")
    conflicts=("spl-dkms" "spl-dkms-git" 'spl-linux-lts-git')
    cd "${srcdir}/spl-${_splver}"
    make DESTDIR="${pkgdir}" install
    mv "${pkgdir}/lib" "${pkgdir}/usr/"
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_spl-linux-lts-headers() {
    pkgdesc="Solaris Porting Layer kernel headers."
    provides=("spl-headers")
    conflicts=("spl-dkms" "spl-dkms-git" "spl-dkms-rc" "spl-headers")
    cd "${srcdir}/spl-${_splver}"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/spl-*/${_extramodules}/Module.symvers
}
