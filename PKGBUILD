pkgname=firefox-git
pkgver=20120811
pkgrel=1
pkgdesc="Standalone web browser from mozilla.org, latest development version"
url="http://www.mozilla.org/projects/firefox/"
arch=(i686 x86_64)
license=(MPL GPL LGPL)
depends=(gtk2 gcc-libs mozilla-common libxt libxrender hunspell
         startup-notification mime-types dbus-glib alsa-lib libevent
         libnotify desktop-file-utils libvpx lcms libpng cairo libffi5 gstreamer0.10)
makedepends=(unzip zip pkg-config diffutils python2 wireless_tools
             yasm mesa autoconf2.13 gconf xorg-server-xvfb git)
source=(mozconfig
        firefox-git.desktop)

_gitroot=https://github.com/mozilla/mozilla-central.git
_gitname=mozilla-central

build(){
  cd $srcdir
  if [[ -d "$_gitname" ]]; then
    cd "$_gitname"
    git reset --hard
    git clean -dxf
    git pull 
  else
    git clone "$_gitroot" "$_gitname"
    cd "$_gitname"
  fi

  cat "$srcdir/mozconfig" > .mozconfig
  #export LDFLAGS="$LDFLAGS -Wl,-rpath,/usr/lib/firefox-git"
  export PYTHON="/usr/bin/python2"
  make -f client.mk
}
package(){
  cd "$srcdir/$_gitname"
  make -f client.mk -j1 DESTDIR="$pkgdir" install
  rm -r "$pkgdir"/usr/{include,lib/firefox-devel-*,share/idl,bin/firefox}
  mv "$pkgdir"/usr/lib/firefox-* "$pkgdir/usr/lib/firefox-git"
  rm $pkgdir/usr/lib/firefox-git/{removed-files,firefox-bin}
  ln -s "/usr/lib/firefox-git/firefox" "$pkgdir/usr/bin/firefox-git"
  install -Dm644 "$srcdir/firefox-git.desktop" "$pkgdir/usr/share/applications/firefox-git.desktop"


  rm -rf "$pkgdir"/usr/lib/firefox-git/dictionaries
  ln -sf /usr/share/hunspell "$pkgdir/usr/lib/firefox-git/dictionaries"
  #rm -rf "$pkgdir"/usr/lib/firefox-git/hyphenation
  #ln -sf /usr/share/hyphen "$pkgdir/usr/lib/firefox-git/hyphenation"
}

md5sums=('3bb93cc45255781f0115a58fb8db1430'
         '5b16c87eed1658d84b3e03ffd7a3b48d')
