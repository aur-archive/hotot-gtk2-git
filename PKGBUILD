# Maintainer: speps <speps at aur dot archlinux dot org>

_uname=lyricat
_commit=e0c78bf
pkgname=hotot-gtk2-git
pkgver=20130303
pkgrel=1
pkgdesc="A lightweight & open source microblogging software (twitter identi.ca). Gtk2 frontend."
arch=('any')
url="http://www.hotot.org/"
license=('LGPL3')
depends=('hotot-data-git' 'pywebkitgtk' 'python2-notify' 'python2-pycurl'
         'python2-keybinder2' 'python2-dbus' 'desktop-file-utils')
makedepends=('git' 'cmake' 'intltool')
optdepends=('libappindicator: unity menubar integration')
provides=("${pkgname/-git}" 'hotot')
conflicts=("${pkgname/-git}" 'hotot')
install="$pkgname.install"

_gitroot="https://github.com/$_uname/Hotot.git"
_gitname="hotot"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #
  # BUILD HERE
  #

  cmake . -DCMAKE_INSTALL_PREFIX=/usr \
          -DWITH_GTK=ON \
          -DWITH_GTK2=ON \
          -DWITH_GTK3=OFF \
          -DWITH_QT=OFF \
          -DWITH_QT5=OFF \
          -DWITH_KDE=OFF \
          -DWITH_CHROME=OFF \
          -DPYTHON_EXECUTABLE=/usr/bin/python2
  make
}

package() {
  cd "$srcdir/$_gitname-build"
  DESTDIR="$pkgdir/" cmake -P hotot/cmake_install.cmake

  # bin
  install -Dm755 scripts/${pkgname/-git} \
    "$pkgdir/usr/bin/${pkgname/-git}"

  # desktop file
  install -Dm644 misc/${pkgname/-git}.desktop \
    "$pkgdir/usr/share/applications/${pkgname/-git}.desktop"
}

# vim:set ts=2 sw=2 et:
