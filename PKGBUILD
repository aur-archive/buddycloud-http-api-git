# Maintainer: Thomas Jost <schnouki@schnouki.net>
pkgname=buddycloud-http-api-git
_pkgname=buddycloud-http-api
pkgver=20130210
pkgrel=1
pkgdesc="A fully distributed social network based on XMPP - HTTP API server"
arch=(i686 x86_64)
url="https://buddycloud.org/wiki/Main_Page"
license=('Apache')
depends=('nodejs' 'buddycloud-server' 'icu' 'expat' 'libxml2')
makedepends=('git')
optdepends=('logrotate: rotate log file automatically'
            "start-stop-daemon: daemon managment if you're not using systemd")
provides=('buddycloud-http-api')
conflicts=('buddycloud-http-api')
backup=('etc/buddycloud-http-api/config.js'
        'etc/logrotate.d/buddycloud-http-api')
install=buddycloud-http-api.install
source=('buddycloud-http-api.rc'
        'buddycloud-http-api.logrotate'
        'buddycloud-http-api.service')
md5sums=('7ec0b99a970b106f6e0a3427f3e05055'
         'd3f0d46554d95d89d6affb36f252f912'
         '3b3728f9ed52fcac32db43d8c1078d03')
sha256sums=('24c03107180eafd6c3d7048dd0046b054927e302777d1aa2edf400fc96261b1b'
            'f14a35f4716ad92f967ab6bc09a79ba4f03331665b2f569657fe8003aa60a647'
            '6ffedfe3a0de0405bbd152be1993e0fa07fae077ff030f1aa300ba8ed19aeccd')

_gitroot=git://github.com/buddycloud/buddycloud-http-api.git
_gitname=buddycloud-http-api

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

  npm install .
}

package() {
  install -d "$pkgdir/opt"
  cp -R "$srcdir/$_gitname-build" "$pkgdir/opt/$_pkgname"

  cd "$pkgdir/opt/$_pkgname"
  rm -rf .git

  install -Dm644 config.js.example "$pkgdir/etc/$_pkgname/config.js"
  ln -sf "../../etc/$_pkgname/config.js" config.js

  install -Dm755 "$srcdir/buddycloud-http-api.rc" "$pkgdir/etc/rc.d/buddycloud-http-api"
  install -Dm644 "$srcdir/buddycloud-http-api.service" "$pkgdir/usr/lib/systemd/system/buddycloud-http-api.service"
  install -Dm644 "$srcdir/buddycloud-http-api.logrotate" "$pkgdir/etc/logrotate.d/buddycloud-http-api"
}

# vim:set ts=2 sw=2 et:
