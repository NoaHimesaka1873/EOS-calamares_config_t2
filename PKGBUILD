# t2linux Maintainer: Noa Himesaka <himesaka@noa.codes>
# Original Maintainer: Portergos Linux <portergoslinux@gmail.com>
# EndeavourOS Maintainer : EndeavourOS <info@endeavouros.com>
# Calamares installer configured for EndeavourOS
#
# Configurations are in extra packages:
# calamares_config_default - default settings
# calamares_config_ce - Community Edition settings

pkgname=calamares_current_t2
_eos_changes=EndeavourOS-calamares
_reponame=calamares
_calamares_ver=3.2.54
pkgver=22.04.1.4
pkgrel=1
pkgdesc="Calamares installer for EndeavourOS for t2linux-provided ISOs"
arch=('any')
url="https://github.com/endeavouros-team"
license=('GPL3')
optdepends=('mkinitcpio-openswap' 'calamares_config_default' 'calamares_config_ce' 'ckbcomp')
makedepends=('git' 'cmake' 'boost-libs' 'extra-cmake-modules' 'kpmcore' 'boost' 'python-jsonschema' 'python-pyaml' 'python-unidecode')
conflicts=('calamares_offline' 'calamares_netinstall_test' 'calamares_netinstall' 'calamares_current' 'calamares_test')
depends=( 'qt5-svg' 'qt5-webengine' 'yaml-cpp' 'networkmanager' 'upower' 'kcoreaddons' 'kconfig' 'ki18n' 'kservice' \
'kwidgetsaddons' 'kpmcore' 'squashfs-tools' 'rsync' 'cryptsetup' 'qt5-xmlpatterns' 'doxygen' 'dmidecode' \
'gptfdisk' 'hwinfo' 'kparts' 'polkit-qt5' 'python' 'qt5ct' 'solid' 'qt5-tools')
provides=("${pkgname}")
options=(!strip !emptydirs)
source=(
  "https://github.com/endeavouros-team/$_eos_changes/archive/refs/tags/${pkgver}.tar.gz"
  "https://github.com/calamares/calamares/releases/download/v$_calamares_ver/$_reponame-$_calamares_ver.tar.gz"
)

sha256sums=('e0aa5cf1f707fb4051673e8922807ebce5c378fbcf4975267171095c09ad4acb'
            '4df74b6c3b238cf0bff01a04566e265de3f143c297fbd8e0c0aa9447595cc8e9')

prepare() {
    mv "$_reponame-$_calamares_ver"            "$_reponame"
    # remove upstream modules packagechooser and netinstall for replacing with testing ones
    rm -r "$_reponame/src/modules/"{fstab,mount,packages}
    
    mv "${_eos_changes}-${pkgver}" "${_eos_changes}"
    
    # move extra modules from external repo inside calamares sources
    rsync -va "$_eos_changes/calamares-extra-modules/"*       "$_reponame/"
    rm -rf "$_eos_changes"

    mkdir -p "$_reponame/build/$pkgname"

    # remove default branding // keep package small
    rm -r "$_reponame/src/branding/default"

    # change some files on the go - distro-specific
    sed -i "s?configuration files\" OFF?configuration files\" ON?g" "$_reponame/CMakeLists.txt"

    # remove hardcoded quiet from grubcfg
    sed -i "s?    kernel_params = \[\"quiet\"\]?    kernel_params = \[\]?" "$_reponame/src/modules/grubcfg/main.py"
}

build() {
    cd "$_reponame/build"
    cmake .. \
    -DCMAKE_BUILD_TYPE=Debug  \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DBoost_NO_BOOST_CMAKE=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DINSTALL_CONFIG=OFF \
    -DWEBVIEW_FORCE_WEBKIT=OFF \
    -DWITH_PYTHONQT=OFF \
    -DWITH_KF5DBus=OFF \
    -DWITH_APPSTREAM=OFF \
    cmake .. \
    -DCMAKE_BUILD_TYPE=Debug \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DBoost_NO_BOOST_CMAKE=ON \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DINSTALL_CONFIG=OFF \
    -DSKIP_MODULES="dracut dracutlukscfg \
    dummycpp dummyprocess dummypython dummypythonqt \
    finishedq keyboardq license localeq notesqml oemid \
    openrcdmcryptcfg packagechooserq \
    plymouthcfg plasmalnf services-openrc \
    summaryq tracking usersq webview welcomeq"
    export DESTDIR="$srcdir/$_reponame/build/$pkgname"
    make -j4 install
}

package() {
    local destdir="/usr"

    # Commom install -D doesn't work
    cp -r "${srcdir}/${_reponame}/build/$pkgname/"* "${pkgdir}${destdir}"
    
}