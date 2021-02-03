# Maintainer: Ray Donnelly <mingw.android@gmail.com>
# Maintainer: Martell Malone <martellmalone@gmail.com>

_realname="crosstool-ng"
pkgname="$_realname-git"
replaces=("$_realname")
pkgver=1.25.0.r155.g1adc236e
pkgrel=1
pkgdesc="A cross-platform open-source toolchain system (git)"
arch=(i686 x86_64)
url="http://www.crosstool-ng.org/"
license=("MIT")
makedepends=("autoconf" "binutils" "bison" "flex" "git"
             "gcc" "gperf" "patch" "libtool" "make"
             "tar" "texinfo" "wget" "xz" "unzip"
             "help2man" "dos2unix")
depends=("ncurses" "rsync")
[ "$(uname -o)" == 'GNU/Linux' ] && {
makedepends+=("automake" "ncurses" "gettext")
depends+=("make" "unzip")
}
[ "$(uname -o)" == 'Msys' ] && {
makedepends+=("automake-wrapper" "ncurses-devel" "gettext-devel")
depends+=("libintl")
}
options=('staticlibs' 'strip')
source=('crosstool-ng::git+https://github.com/crosstool-ng/crosstool-ng.git#branch=master'
        "0002-Add-new-gmp-patches.patch"
        "0003-Add-new-gcc-patches.patch"
        "0004-Add-new-newlib-patches.patch")
sha256sums=('SKIP'
            '8ca11b022a033816c498695d2ccd551126b3cb66967e15c70de27dfe7d5453bf'
            'aac206bef981f37bcd9dedefa6eb7d384a575240a6ab32b0a0b939147ef2db45'
            'c8c6b814a2ca156e70ad6e654b77d2b2bad416b6fec5a68d2e2530a4f9ef9b20')

pkgver() {
  cd "${srcdir}/${_realname}"
  git describe --long --tags | sed 's/^crosstool-ng-//;s/-rc/rc/;s/-/.r/;s/-/./'
}

prepare() {
  cd "${srcdir}/${_realname}"
  
  patch -p1 -i "${srcdir}/0002-Add-new-gmp-patches.patch"
  patch -p1 -i "${srcdir}/0003-Add-new-gcc-patches.patch"
  patch -p1 -i "${srcdir}/0004-Add-new-newlib-patches.patch"
  find packages/ncurses -type f -name "*.patch" | xargs dos2unix
  
  ./bootstrap
}

build() {
  cd "${srcdir}/${_realname}"
  ./configure \
    --build=${CHOST} \
    --prefix=/usr

  make
}

package() {
  cd "${srcdir}/${_realname}"
  make DESTDIR=${pkgdir} install
}
