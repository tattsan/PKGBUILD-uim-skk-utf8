#############################
_pkgname=uim
pkgname=uim-skk-utf8
pkgver=1.9.3
#############################
pkgrel=3
#############################
pkgdesc='Multilingual input method library'
url='https://github.com/uim/uim'
license=('custom:BSD')
arch=('x86_64')
depends=('gcc-libs' 'glibc' 'libxext' 'ncurses' 'sqlite' 'libxft' 'libedit' 'm17n-lib')
makedepends=('extra-cmake-modules' 'intltool' 'gettext' 'gtk2' 'gtk3' 'qt5-x11extras' 'qt6-base' 'anthy' 'plasma-framework5')
optdepends=('qt5-x11extras: immodule and helper applications'
	    'qt5-declarative: immodule and helper applications'
            'gtk2: immodule and helper applications'
            'gtk3: immodule and helper applications'
            'skk-jisyo: input method'
            'anthy: input method')
source=("https://github.com/${_pkgname}/${_pkgname}/releases/download/${pkgver}/${_pkgname}-${pkgver}.tar.bz2"
	"uim-skk-utf8.patch"
	"uim-skk-utf8-gai.patch"
	)
sha256sums=('aa2febae77f675272e3d1d26922ae08f37d97837b6992a429fc4ea78e981a8e0'
	'454017d090a50b3b8f26e58ea57edcab321e757a3eb369720dc04d4bb0edeeef'
	'ab8fdf9e17c30f3267253b1e36b0d758f902495aab51df2ec152acec9cad936f'
	)

provides=('uim')
conflicts=('uim')

prepare() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -p1 -l < "${srcdir}"/uim-skk-utf8.patch
	patch -p1 -l < "${srcdir}"/uim-skk-utf8-gai.patch
	./autogen.sh
}

build() {
	cd "${srcdir}/${_pkgname}-${pkgver}"

	# CFLAGS+=' -Wno-implicit-function-declaration'
	CFLAGS+=' -Wno-incompatible-pointer-types'
	CFLAGS+=' -std=gnu17'

	./configure \
		--prefix=/usr \
		--libexecdir=/usr/lib/uim \
		--with-qt5-immodule \
		--with-qt5 \
		--with-qt6-immodule \
		--with-qt6 \
		--with-anthy-utf8 \
		--with-skk \

	make
}

package() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install -j1 # FS#41112
	install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${_pkgname}/COPYING"
}
