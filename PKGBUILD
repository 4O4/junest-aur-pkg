# Maintainer: Filippo Squillace <feel dot sqoox at gmail dot com>
# More details on how to change this file:
# https://wiki.archlinux.org/index.php/PKGBUILD
# https://wiki.archlinux.org/index.php/Creating_packages

pkgname=junest-git
pkgver=r221.3609954
pkgrel=1
pkgdesc="The Arch Linux based distro that runs upon any Linux distros without root access"
arch=('any')
url="http://fsquillace.github.io/junest-site/"
license=('GPL')
groups=()
depends=()
makedepends=('git')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=()
options=()
install=junest.install
# Please refer to the 'USING VCS SOURCES' section of the PKGBUILD man page for
# a description of each element in the source array.
source=('junest::git+https://github.com/fsquillace/junest.git#branch=master')
noextract=()
md5sums=('SKIP')


pkgver() {
	cd "$srcdir/${pkgname%-git}"

# The examples below are not absolute and need to be adapted to each repo. The
# primary goal is to generate version numbers that will increase according to
# pacman's version comparisons with later commits to the repo. The format
# VERSION='VER_NUM.rREV_NUM.HASH', or a relevant subset in case VER_NUM or HASH
# are not available, is recommended.

# Git, tags available
#	printf "%s" "$(git describe --long | sed 's/\([^-]*-\)g/r\1/;s/-/./g')"

# Git, no tags available
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"

}

package() {
    cd "$srcdir/${pkgname%-git}"

    mkdir -p "${pkgdir}/opt/"
    cp -R "${srcdir}/${pkgname%-git}" "${pkgdir}/opt/${pkgname%-git}"

    mkdir -p "${pkgdir}/usr/bin"
    ln -s ../../opt/${pkgname%-git}/bin/${pkgname%-git} ${pkgdir}/usr/bin/${pkgname%-git}
}

# vim:set ts=2 sw=2 et:
