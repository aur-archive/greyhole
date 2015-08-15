# Maintainer: Robert Holak <rholak@gmail.com>
# Maintainer: Christopher Krooß <didi2002@web.de>
pkgname=greyhole
pkgver=0.9.60
pkgrel=2
pkgdesc="Greyhole is an application that uses Samba to create a storage pool of all your available hard drives and allows you to create redundant copies of the files you store, in order to prevent data loss when part of your hardware fails."
url="http://greyhole.net"
arch=('x86_64' 'i686')
license=('GPLv3')
depends=('php' 'php-intl' 'samba' 'mysql' 'rsync' 'lsof')
backup=("etc/greyhole.conf")
install='greyhole.install'
source=("http://www.greyhole.net/releases/${pkgname}-${pkgver}.tar.gz"
	"greyhole.service")
sha256sums=('2e28d8a3a6cfa83965eb692d02bdf681bb80977d02e96e3596cd76f08ac5ee14'
            'f09cc153424934f2e7e0d8b66cdb47bd5f331640388202917533115d53962ef9')

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  mkdir -p $pkgdir/var/spool/greyhole
  chmod 777 $pkgdir/var/spool/greyhole

  install -m 0755 -D -p greyhole $pkgdir/usr/bin/greyhole
  install -m 0755 -D -p greyhole-dfree $pkgdir/usr/bin/greyhole-dfree
  install -m 0755 -D -p greyhole-dfree.php $pkgdir/usr/share/greyhole/greyhole-dfree.php
  install -m 0644 -D -p logrotate.greyhole $pkgdir/etc/logrotate.d/greyhole
  install -m 0644 -D -p greyhole.cron.d $pkgdir/etc/cron.d/greyhole
  install -m 0644 -D -p greyhole.example.conf $pkgdir/etc/greyhole.conf
  install -m 0755 -D -p greyhole.cron.weekly $pkgdir/etc/cron.weekly/greyhole
  install -m 0755 -D -p greyhole.cron.daily $pkgdir/etc/cron.daily/greyhole
  install -m 0644 -D -p docs/greyhole.1.gz $pkgdir/usr/share/man/man1/greyhole.1.gz
  install -m 0644 -D -p docs/greyhole-dfree.1.gz $pkgdir/usr/share/man/man1/greyhole-dfree.1.gz
  install -m 0644 -D -p docs/greyhole.conf.5.gz $pkgdir/usr/share/man/man5/greyhole.conf.5.gz
  install -m 0644 -D -p schema-mysql.sql $pkgdir/usr/share/greyhole/schema-mysql.sql
  install -m 0644 -D -p USAGE $pkgdir/usr/share/greyhole/USAGE
  install -m 0644 -D -p $startdir/greyhole.service $pkgdir/usr/lib/systemd/system/greyhole.service


  #Choose correct samba module
  if [ "$CARCH" = "i686" ] ; then
    vfs_file=greyhole-i386.so
  else
    vfs_file=greyhole-x86_64.so
  fi

  mkdir -p "$pkgdir/usr/lib64/greyhole/"
  mkdir -p "$pkgdir/usr/lib64/samba/vfs/"
  install -m 644 "samba-module/bin/4.1/$vfs_file" "$pkgdir/usr/lib64/greyhole/greyhole-samba41.so"
  ln -s "/usr/lib64/greyhole/greyhole-samba41.so" "$pkgdir/usr/lib64/samba/vfs/greyhole.so"
}
