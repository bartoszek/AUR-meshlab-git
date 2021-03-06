#!/usr/hint/bash
# Maintainer : bartus <arch-user-repoᘓbartus.33mail.com>
# shellcheck disable=SC2034,SC2154 # allow unused/uninitialized variables.

_name="meshlab"
pkgname="$_name-git"
pkgver=2021.05.r315.ga941ca30d
pkgrel=1
pkgdesc="System for processing and editing of unstructured 3D models arising in 3D scanning (qt5 version)"
arch=('i686' 'x86_64')
url="https://www.meshlab.net"
conflicts=('meshlab')
provides=('meshlab')
license=('GPL2')
depends=('bzip2' 'glew' 'glu' 'openssl-1.0' 'qt5-base' 'qt5-declarative' 'qt5-script' 'qt5-xmlpatterns' 'xerces-c')
makedepends=('cmake' 'eigen' 'ninja' 'git' 'muparser' 'levmar' 'lib3ds' 'mpir')
optdepends=('u3d: for U3D and IDTF file support'
            'lib3ds: for Autodesk`s 3D-Studio r3 and r4 .3DS file support'
            'levmar: for isoparametrization and mutualcorrs plugins'
            'muparser: for filer_func plugins'
            'mpir: for Constructive Solid Geometry operation filters')
#also create openctm(aur) jhead-lib structuresynth-lib to handle last dep
source=("$_name::git+https://github.com/cnr-isti-vclab/meshlab.git"
        )
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

prepare() {
  prepare_submodule
  #fix gcc:11 headers regression
  sed -i '1 i\#include <limits>' -i "${srcdir}"/${_name}/src/external/e57/src/E57XmlParser.cpp
}

pkgver() {
  git -C "${srcdir}/${_name}" describe --long --tags --match "Meshlab-[0-9.]*"| sed 's/Meshlab-//g;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  local cmake_flags=( '-DALLOW_SYSTEM_QHULL=OFF'
                      '-DCMAKE_INSTALL_PREFIX=/usr'
                    )
  cmake "${cmake_flags[@]}" -G Ninja -B "${srcdir}/build" -S "${srcdir}/meshlab/src"
# shellcheck disable=SC2086 # allow MAKEFLAGS to split when passing multiple flags.
  ninja ${MAKEFLAGS:--j1} -C "${srcdir}/build"
}

package() {
  DESTDIR="$pkgdir" ninja -C "${srcdir}/build" install
}

# Generated with git_submodule_PKGBUILD_conf.sh ( https://gist.github.com/bartoszek/41a3bfb707f1b258de061f75b109042b )
# Call prepare_submodule in prepare() function

prepare_submodule() {
  git -C "$srcdir/meshlab" config submodule.src/vcglib.url "$srcdir/vcglib"
  git -C "$srcdir/meshlab" config submodule.src/external/nexus.url "$srcdir/nexus"
  git -C "$srcdir/meshlab" submodule update --init
  git -C "$srcdir/meshlab/src/external/nexus" config submodule.src/corto.url "$srcdir/corto"
  git -C "$srcdir/meshlab/src/external/nexus" submodule update --init
}
source+=(
  "vcglib::git+https://github.com/cnr-isti-vclab/vcglib.git"
  "nexus::git+https://github.com/cnr-isti-vclab/nexus.git"
  "corto::git+https://github.com/cnr-isti-vclab/corto.git"
)

# vim:set ts=2 sw=2 et:
