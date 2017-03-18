# Maintainer: Filippo Squillace <feel dot sqoox at gmail dot com>
# More details on how to change this file:
# https://wiki.archlinux.org/index.php/PKGBUILD
# https://wiki.archlinux.org/index.php/Creating_packages
# https://wiki.archlinux.org/index.php/Arch_User_Repository#Submitting_packages

ARCH_LIST=('x86_64' 'x86' 'arm')
HOST_ARCH=$(uname -m)
if [ $HOST_ARCH == "i686" ] || [ $HOST_ARCH == "i386" ]
then
    NORM_ARCH="x86"
elif [ $HOST_ARCH == "x86_64" ]
then
    NORM_ARCH="x86_64"
elif [[ $HOST_ARCH =~ .*(arm).* ]]
then
    NORM_ARCH="arm"
fi

pkgname=junest-git
pkgver=5.6.11
pkgrel=1
pkgdesc="The Arch Linux based distro that runs upon any Linux distros without root access"
arch=('any')
url="http://fsquillace.github.io/junest-site/"
license=('GPL')
groups=()
depends=('yaourt' 'pacman')
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
md5sums=('SKIP')
noextract=()

PROOT_LINK=http://static.proot.me/
MAIN_REPO=https://s3-eu-west-1.amazonaws.com/${pkgname%-git}-repo
QEMU_LINK=$MAIN_REPO/qemu/$NORM_ARCH

declare -A proot_md5sums
proot_md5sums["x86_64"]='14080705dd45a6bafa20e909a68072cb'
proot_md5sums["x86"]='b1c08236b56d121e04e9e29b197d0eeb'
proot_md5sums["arm"]='8218c5f00e77e2e6e04c372ced27c7e7'
declare -A qemu_md5sums
if [ "$NORM_ARCH" == "x86_64" ]
then
    qemu_md5sums["x86"]='8a706d734f8c790743a8114dda4c344a'
    qemu_md5sums["arm"]='3ced729c95d2514f35d4899e944a4582'
elif [ "$NORM_ARCH" == "x86" ]
then
    qemu_md5sums["x86_64"]='c28d5049193dbce75efa0c8655d71427'
    qemu_md5sums["arm"]='f75fd15722fcc2914e3de0b0a46eb982'
elif [ "$NORM_ARCH" == "arm" ]
then
    qemu_md5sums["x86_64"]='bd9de1927aae4eb26dc0e5615159a616'
    qemu_md5sums["x86"]='a7c2b6ca53fa166f0c06ec76cc5edd7d'
fi

for archh in ${ARCH_LIST[@]}
do
    source+=("$PROOT_LINK/proot-$archh")
    md5sums+=("${proot_md5sums[$archh]}")

    if [ "$archh" != "$NORM_ARCH" ]
    then
        source+=("${QEMU_LINK}/qemu-${NORM_ARCH}-static-$archh")
        md5sums+=("${qemu_md5sums[$archh]}")
    fi
done


pkgver() {
	cd "$srcdir/${pkgname%-git}"

# The examples below are not absolute and need to be adapted to each repo. The
# primary goal is to generate version numbers that will increase according to
# pacman's version comparisons with later commits to the repo. The format
# VERSION='VER_NUM.rREV_NUM.HASH', or a relevant subset in case VER_NUM or HASH
# are not available, is recommended.

# Git, tags available
	printf "%s" "$(git describe --tags | sed 's/\([^-]*-\)g/r\1/;s/-/./g')"

# Git, no tags available
#	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"

}

package() {
    cd "$srcdir/${pkgname%-git}"

    install -d -m 755 "${pkgdir}/opt/"
    cp -R "${srcdir}/${pkgname%-git}" "${pkgdir}/opt/${pkgname%-git}"

    install -d -m 755 "${pkgdir}/usr/bin"
    ln -s ../../opt/${pkgname%-git}/bin/${pkgname%-git} ${pkgdir}/usr/bin/${pkgname%-git}

    echo "Generating the metadata info"
    install -d -m 755 "${pkgdir}/etc/${pkgname%-git}"
    echo "JUNEST_ARCH=$NORM_ARCH" > ${pkgdir}/etc/${pkgname%-git}/info

    echo "Installing proot static binaries"
    cd "$srcdir"
    install -d -m 755 "${pkgdir}/opt/proot"
    for archh in ${ARCH_LIST[@]}
    do
        install -m 755 proot-$archh ${pkgdir}/opt/proot/proot-$archh
    done

    echo "Installing qemu static binaries"
    install -d -m 755 "${pkgdir}/opt/qemu"
    for arch in ${ARCH_LIST[@]}
    do
        if [ "$arch" != "$NORM_ARCH" ]
        then
            install -m 755 qemu-${NORM_ARCH}-static-$arch ${pkgdir}/opt/qemu
        fi
    done

    echo "Applying patches for yaourt and makepkg"
    install -d -m 755 "${pkgdir}/opt/yaourt/bin/"
    install -m 755 /usr/bin/yaourt ${pkgdir}/opt/yaourt/bin/
    sed -i -e 's/"--asroot"//' ${pkgdir}/opt/yaourt/bin/yaourt
    sed -i -e 's@#!/bin/bash@#!/bin/bash\nPATH=/opt/yaourt/bin:$PATH@' ${pkgdir}/opt/yaourt/bin/yaourt
    install -m 755 /usr/bin/makepkg ${pkgdir}/opt/yaourt/bin/
    sed -i -e 's/EUID\s==\s0/false/' ${pkgdir}/opt/yaourt/bin/makepkg
    ln -s ../../opt/yaourt/bin/yaourt ${pkgdir}/usr/bin/yogurt
}

# vim:set ts=2 sw=2 et:
