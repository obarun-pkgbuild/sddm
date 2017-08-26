# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/sddm
# 						Maintainer: Felix Yan <felixonmars@archlinux.org>
# 						Contributor: Andrea Scarpino <andrea@archlinux.org>

pkgname=sddm
pkgver=0.14.0
pkgrel=5
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
		sddm-fix-avatars.patch::"https://github.com/sddm/sddm/commit/ecb903e4.patch"
		sddm-qt-im-module.patch::"https://github.com/sddm/sddm/commit/10352e08.patch")
sha256sums=('a551551a6ba324e9c384c89bc63e871de65fea3740eadbea2d63df86045f8205'
            '6866671b41d6390534659b7da97ebf374b9be2b04cfc8464aa2f681a18ca8007'
            '5136b2bdc43a1e79b14c2cd44c90b25b0e9d164ceb1608cb7558668457f3a533'
            'a2bcdc20bb8915ed4eae810832266ff3bd22a523813ef1674eb25a28800e4368'
			'3810d2e9360fd96760165e2f7cabaa0579f55d92fdd53579aafdd17c944353a2')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare() {
  mkdir -p build
  cd "${srcdir}"/${pkgname}-${pkgver}
  patch -Np1 -i ../launch_consolekit_in_priority_if_exist.patch
  # Fix displaying avatars
  cd "${srcdir}/${pkgname}-${pkgver}"
  patch -p1 -i ../sddm-fix-avatars.patch
  # Fix displaying virtual keyboard
  patch -p1 -i ../sddm-qt-im-module.patch
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
}
