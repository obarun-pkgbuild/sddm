# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/sddm
# 						Maintainer: Felix Yan <felixonmars@archlinux.org>
# 						Contributor: Andrea Scarpino <andrea@archlinux.org>

pkgname=sddm
pkgver=0.16.0
pkgrel=2
pkgdesc='QML based X11 and Wayland display manager'
arch=(x86_64)
url='http://github.com/sddm/sddm'
license=('GPL')
depends=('qt5-declarative' 'xorg-xauth' 'xorg-server')
makedepends=('extra-cmake-modules' 'python-docutils' 'qt5-tools' 'libdbus')
optdepends=('sddm-s6serv: sddm s6 service'
			'sddm-s6rcserv: sddm s6-rc service'
			'sddm-runitserv: sddm runit service')
provides=('display-manager')
install="${pkgname}.install"
backup=('usr/share/sddm/scripts/Xsetup'
        'etc/pam.d/sddm'
        'etc/sddm.conf')
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/${pkgname}/${pkgname}/archive/v${pkgver}.tar.gz"
		"launch_consolekit_in_priority_if_exist.patch"
		'sddm.conf'
		'sddm.sysusers'
		'sddm.tmpfiles')
sha256sums=('83a34a5f7a5af5e62f0c1bb8a9ce79c7c0ee55b3e0a9bb7ca437dc42f8ce4292'
            '6866671b41d6390534659b7da97ebf374b9be2b04cfc8464aa2f681a18ca8007'
            '5136b2bdc43a1e79b14c2cd44c90b25b0e9d164ceb1608cb7558668457f3a533'
            '421d6d137a32b7a749427f4ab770e5adeef7dac66b138ab6e216ddc0cf4e2cb6'
            'db625f2a3649d6d203e1e1b187a054d5c6263cadf7edd824774d8ace52219677')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare() {
  mkdir -p build
  cd "${srcdir}"/${pkgname}-${pkgver}
  patch -Np1 -i ../launch_consolekit_in_priority_if_exist.patch
}

build() {
  cd build
  cmake ../${pkgname}-${pkgver} \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_LIBEXECDIR=/usr/lib/sddm \
        -DDBUS_CONFIG_FILENAME=sddm_org.freedesktop.DisplayManager.conf \
        -DBUILD_MAN_PAGES=ON \
		-DENABLE_JOURNALD=OFF
  make
}

package() {
  cd build
  make DESTDIR="${pkgdir}" install
  
  install -Dm0644 "${srcdir}"/sddm.conf "${pkgdir}"/etc/sddm.conf

  install -Dm644 "$srcdir"/sddm.sysusers "$pkgdir"/usr/lib/sysusers.d/sddm.conf
  install -Dm644 "$srcdir"/sddm.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/sddm.conf
}
