# Maintainer: graysky <graysky AT archlinux DOT us>

pkgname=nct677x-ck-git
pkgver=20130329
pkgrel=1
pkgdesc="Nuvoton lm-sensor module for Z77-based motherboards.  Supports NCT6775F, NCT6776F, and NCT6779D."
arch=('i686' 'x86_64')
url="https://github.com/groeck/nct6775"
license=('GPLv2')
depends=('linux-ck>=3.8' 'linux-ck<3.9')
makedepends=('git' 'linux-ck-headers')
provides=('nct677x-ck')
install=nct677x.install

_extramodules=extramodules-3.8-ck
_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
_gitroot=https://github.com/groeck/nct6775
_gitname=nct6775

build() {
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

  # fix up Makefile for Arch Linux
  # headers location and remove shell dependency on running kernel to figure kern ver
  sed -i -e '/^KERNEL_BUILD/ s,headers-,,' -i -e "s/\$(shell uname -r)/$_kernver/" Makefile
  make
}

package() {
  cd "$_gitname-build"
  
  # makepkg v4.0.3 does not auto gzip modules
  gzip -9 nct6775.ko
  install -Dm644 nct6775.ko.gz "$pkgdir"/usr/lib/modules/$_kernver/kernel/drivers/hwmon/nct6775.ko.gz
}

# vim:set ts=2 sw=2 et:
