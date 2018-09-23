# Minor patches: Marina Miyaoka <miyaokamarina@gmail.com>
# Maintainer: Xiao-Long Chen <chenxiaolong@cxl.epac.to>
# Original Maintainer: György Balló <ballogy@freestart.hu>
# Contributor: thn81 <root@scrat>

# vercheck-pkgbuild: auto
# vercheck-ubuntu: name=${pkgname}, repo=yakkety
# vercheck-launchpad: name=${pkgname}

pkgname=libunity
_actual_ver=7.1.4
_extra_ver=+16.10.20160516
_ubuntu_rel=0ubuntu1
pkgver=${_actual_ver}${_extra_ver/+/.}
pkgrel=1
pkgdesc="A library for instrumenting and integrating with all aspects of the Unity shell"
arch=(i686 x86_64)
url="https://launchpad.net/libunity"
license=(LGPL)
depends=(dee libdbusmenu-glib gtk3)
makedepends=(intltool vala python2 gobject-introspection)
groups=(unity)
source=("https://launchpad.net/ubuntu/+archive/primary/+files/libunity_${_actual_ver}${_extra_ver}.orig.tar.gz"
        "https://launchpad.net/ubuntu/+archive/primary/+files/libunity_${_actual_ver}${_extra_ver}-${_ubuntu_rel}.diff.gz"
        0001_autoconf.patch)
sha512sums=('f204da3f5bf8d8a321fb8daa4e8f42b8fec520b71a7917d213f3c443ffb8b313dfe6adbad68e7208b84b8bc93e956a30c37ea2533bdfc26c745dbd4504ea4f9a'
            'd3b3b7c616b0e4dd930857a3a18c9b033698725077ce9b32deed14134bf1c034d0bf6a95e7e63a44d6d5827f732a528003ba7406dd8a94deff5c749c5c2c73b1'
            '1570c01f4c8ac53bc7806fad78691844a12625b7142270b5a215d203618acc76ba258c4aa969a120186ba9224b726f20a2a9efad05940ffdaa258be49844fa4b')

prepare() {
    find -type f -name '*.py' -exec sed -i 's|^\(#!.*python$\)|\12|g' {} \+

    patch -p1 -i 0001_autoconf.patch

    # Apply Ubuntu patches
    patch -p1 -i "${pkgname}_${_actual_ver}${_extra_ver}-${_ubuntu_rel}.diff"

    for i in $(grep -v '#' debian/patches/series); do
        msg "Applying ${i} ..."
        patch -p1 -i "debian/patches/${i}"
    done
}

build() {
    intltoolize -f
    autoreconf -vfi
    ./configure --prefix=/usr --disable-static --enable-headless-tests PYTHON=python2
    make -j1
}

package() {
    make DESTDIR="${pkgdir}/" install
}
