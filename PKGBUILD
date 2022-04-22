# Maintainer: EndeavourOS-Team <info@endeavouros.com>

pkgname=welcome-t2
pkgdesc="Welcome greeter for new EndeavourOS users."
pkgver=3.17.31
pkgrel=2
arch=('any')
license=('GPL')
depends=(
    systemd eos-bash-shared eos-translations
    eos-quickstart
)
provides=('welcome')
optdepends=(
  'translate-shell: for generating a language translation for the User Interface'
  'reflector-simple: for the Update Mirrors button'
  'yad: upstream yad provider'
  'yad-eos: alternative yad provider (includes fixes)'
  'kdiff3: alternative app for pacdiff to use'
  'meld: alternative app for pacdiff to use'
  'diffuse: alternative app for pacdiff to use'
  'vim: alternative app for pacdiff to use'
)

url=https://github.com/t2linux/EOS-calamares_config_t2/tree/welcome/
_url="https://raw.githubusercontent.com/t2linux/EOS-calamares_config_t2/welcome/"

source=(
  $_url/welcome
  $_url/welcome.desktop
  $_url/wallpaper-once
  $_url/wallpaper-once.desktop
  $_url/eos-kill-yad-zombies
  $_url/welcome-dnd
  $_url/eos-install-mode-run-calamares
)
sha512sums=('3bae40331ce6c10e97d8fcf776098f55994c600a9a855e95d55160b8f42332d159e46955378b0333974aa40c589f07c0b84dec6d75012bb57a19ddc220d74a84'
            'fc360cf63894772bbc75f41d5b5f312ac4e70afb43bc081b63e3d8db6b8c9a1f0e08fa5a3fdff840f8064c883dda1d3f16328bfc8aa5b4a18e8adf83b93174a0'
            '66977e3853538a25a7f1530ab34e91749ec4890ad03ac7d80e49fe1e0bdfca114ee0cff795d1a5fb4569c7c395d78b07aadc161a70e12c6546086a18f9e47db7'
            'a193a605d95d837db568dab2feb074c035bea12bc7b08a39a7be3b075dd063cd05fb46b4bde5c86f81a26456c0aab3b0b834cb1ffd88820095a14291b4e059a5'
            '90dfc1f08a428787305d72a2e439a377aee0bc095fcb0a2efb4245a15285d4b96b5e0ac644c02f27ab68aebbfc5c64e6ab7d733d9db8952d2999d589410d3b9e'
            '9f0cd5edabf93439656046c60dc6a29f439a9d8b7afab8ec03b9470321ba98fc53b927944e2f68b5b152c150cca76913490eaab4256a5699da5960f860d81e81'
            'SKIP')
package() {
  install -d $pkgdir/usr/share/endeavouros/scripts
  install -Dm755 welcome                 $pkgdir/usr/share/endeavouros/scripts/welcome
  install -Dm755 wallpaper-once           $pkgdir/usr/share/endeavouros/scripts/wallpaper-once

  install -d $pkgdir/usr/bin
  ln -s /usr/share/endeavouros/scripts/welcome $pkgdir/usr/bin/eos-welcome

  install -Dm755 eos-kill-yad-zombies           $pkgdir/usr/bin/eos-kill-yad-zombies
  install -Dm755 welcome-dnd                    $pkgdir/usr/bin/welcome-dnd
  install -Dm755 eos-install-mode-run-calamares $pkgdir/usr/bin/eos-install-mode-run-calamares

  install -d $pkgdir/etc/xdg/autostart
  install -Dm644 wallpaper-once.desktop   $pkgdir/etc/xdg/autostart/wallpaper-once.desktop
  install -Dm644 welcome.desktop         $pkgdir/etc/xdg/autostart/welcome.desktop      # no --once

  install -d $pkgdir/usr/share/applications
  cp -a welcome.desktop welcome.desktop-enable
  sed -i welcome.desktop-enable \
      -e 's|^\(Exec=.* --startdelay.*\)$|#\1|' \
      -e 's|^#\(Exec=.* --once.*\)$|\1|'
  install -Dm644 welcome.desktop-enable         $pkgdir/usr/share/applications/welcome.desktop      # has --once
}
