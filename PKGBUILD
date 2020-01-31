# Modified from original minidlna PKGBUILD from Arch Linux.
# Original maintainers/contributers below:
#
#  Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
#  Maintainer:Biginoz < biginoz AT free point fr>
#  Contributor: Ignacio Galmarino <igalmarino@gmail.com>
#  Contributor: Matthias Sobczyk <matthias.sobczyk@googlemail.com>
#  Contributor: Kamil Kaminski <kyle@kkaminsk.com>

pkgname=minidlna-multi
pkgver=1.2.1
pkgrel=1
pkgdesc="A DLNA/UPnP-AV Media server (aka ReadyDLNA) with multi instance support"
arch=('x86_64')
url="https://sourceforge.net/projects/minidlna/"
license=('GPL')
depends=('libexif' 'libjpeg' 'libid3tag' 'flac' 'libvorbis' 'ffmpeg' 'sqlite')
conflicts=('minidlna')
makedepends=('git')
backup=('etc/minidlna.conf')
source=("minidlna::git+https://git.code.sf.net/p/minidlna/git#tag=v${pkgver//./_}"
    minidlna@.service
    minidlna.tmpfiles
    minidlna.sysusers
    minidlna-multi-uuid.patch)
_topdir=minidlna

sha512sums=('SKIP'
            '35def12f2eb932863bdc18de64ca10b80089e0397c0b8c6e386147968055c82962b05eac5b1376020d25b94abc1433740265fd4b5cb8ea852158d75fefdbd2d8'
            'c58631c20416997c538be6258ef9c13b9304d5906b19f063157df70f672b7153b452ffb9612be267a90942bd880af8d665ebe3c53a2926ffa99acc943d875d97'
            'e3e6c46faac768b283134a47013b77c4152840c61d3503f51fbe154bf25fe8a0e585ed9a40950212254b4a844b007f674875e4d25f55af51914694213fecc420'
            '1b45b5d140f45b6f4f96958c550e92e964a74cb1a1fa1edd2abc5f3af03d4722f2939aa20a918a3da2d7550405abcaabb7d1e9e72799f59c506b72ccdd7af035')
prepare() {
  cd "$srcdir/$_topdir"
  sed -i 's|#user=.*|user=minidlna|g' minidlna.conf
  patch -p1 < ../minidlna-multi-uuid.patch
}

build() {
  cd "$srcdir/$_topdir"
  [ -x configure ] || ./autogen.sh
  ./configure --prefix=/usr --sbindir=/usr/bin
  make
}

package() {
  cd "$srcdir/$_topdir"
  DESTDIR="$pkgdir" make install
  install -Dm644 minidlna.conf "$pkgdir"/etc/minidlna.conf
  install -Dm0644 "$srcdir"/minidlna.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/minidlna.conf
  install -Dm0644 "$srcdir"/minidlna.sysusers "$pkgdir"/usr/lib/sysusers.d/minidlna.conf
  install -Dm0644 "$srcdir"/minidlna@.service "$pkgdir"/usr/lib/systemd/system/minidlna@.service
  install -Dm644 "$srcdir"/$_topdir/minidlna.conf.5 "$pkgdir"/usr/share/man/man5/minidlna.conf.5
  install -Dm644 "$srcdir"/$_topdir/minidlnad.8 "$pkgdir"/usr/share/man/man8/minidlnad.8
}
