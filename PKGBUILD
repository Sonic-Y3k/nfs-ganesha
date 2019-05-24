# Maintainer: Wes Jackson <icebal dot 7 at gmail dot com>
# Contributor: Sonic-Y3k <sonic.y3k@googlemail.com>

pkgname=nfs-ganesha
pkgver=2.7.3
ntirpcver=1.7.3
pkgrel=1
pkgdesc="NFS-Ganesha supports both the NFS and 9P protocols in user mode."
arch=('any')
url="http://nfs-ganesha.github.io/"
license=('GPL3')
depends=('ceph-libs')
conflicts=('nfs-ganesha-git')
makedepends=('cmake' 'ceph-libs')
source=("https://github.com/nfs-ganesha/nfs-ganesha/archive/V${pkgver}.tar.gz"
        "https://github.com/nfs-ganesha/ntirpc/archive/v${ntirpcver}.tar.gz")
noextract=("v${ntirpcver}.tar.gz")
sha256sums=('6a538f10d6e950414bfc4ecad1d08590f6c13112cec41f101c595f5d8ed0a3cd'
            '8713ef095efc44df426bbd2b260ad457e5335bf3008fb97f01b0775c8042e54b')

backup=(etc/ganesha/ganesha.conf etc/sysconfig/ganesha)

prepare() {
    cd "${pkgname}-${pkgver}/src"

    # rm libntirpc
    rm -r "libntirpc"

    # extract ntirpc
    tar xfz "${srcdir}/v${ntirpcver}.tar.gz" "ntirpc-${ntirpcver}"

    # rename folder
    mv "ntirpc-${ntirpcver}" "libntirpc"
}

build() {
        cd "${pkgname}-${pkgver}"
        cmake src/ -DCMAKE_BUILD_TYPE=Release -DBUILD_CONFIG=everything -DLIB_INSTALL_DIR=/usr/lib
        make
}

package() {
        cd "${pkgname}-${pkgver}"
        make install DESTDIR="$pkgdir"
        mv "$pkgdir"/var/run "$pkgdir"/run
        rmdir "$pkgdir"/var
        install -d "$pkgdir"/usr/lib/systemd/system "$pkgdir"/etc/sysconfig "$pkgdir"/usr/libexec/ganesha "$pkgdir"/etc/dbus-1/system.d/ "$pkgdir"/etc/tmpfiles.d
        install src/scripts/ganeshactl/org.ganesha.nfsd.conf "$pkgdir"/etc/dbus-1/system.d/
        install src/scripts/systemd/nfs-ganesha.service.el7 "$pkgdir"/usr/lib/systemd/system/nfs-ganesha.service
        install src/scripts/systemd/nfs-ganesha-lock.service.el7 "$pkgdir"/usr/lib/systemd/system/nfs-ganesha-lock.service
        install src/scripts/systemd/nfs-ganesha-config.service-in.cmake "$pkgdir"/usr/lib/systemd/system/nfs-ganesha-config.service
        install src/scripts/systemd/sysconfig/nfs-ganesha "$pkgdir"/etc/sysconfig/ganesha
        install src/scripts/systemd/tmpfiles.d/ganesha.conf "$pkgdir"/etc/tmpfiles.d/ganesha.conf
        install src/scripts/nfs-ganesha-config.sh "$pkgdir"/usr/libexec/ganesha
}
