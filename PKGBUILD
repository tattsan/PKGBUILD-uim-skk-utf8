#############################
_pkgname=uim
pkgname=uim-skk-utf8
pkgver=1.9.1
#############################
pkgrel=1
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
	"CMakeLists.txt_qt5_applet.patch"
	)
sha256sums=('5050a9fbb09941258e3e4c3201b817966190b678d6675f0c4ec291f07dd666df'
	'454017d090a50b3b8f26e58ea57edcab321e757a3eb369720dc04d4bb0edeeef'
	'ab8fdf9e17c30f3267253b1e36b0d758f902495aab51df2ec152acec9cad936f'
	'SKIP'
	)

provides=('uim')
conflicts=('uim')

prepare() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -p1 -l < "${srcdir}"/uim-skk-utf8.patch
	patch -p1 -l < "${srcdir}"/uim-skk-utf8-gai.patch
	patch -p1 -l < "${srcdir}"/CMakeLists.txt_qt5_applet.patch
	./autogen.sh
}

build() {
	cd "${srcdir}/${_pkgname}-${pkgver}"

	CFLAGS+=' -fcommon' # https://wiki.gentoo.org/wiki/Gcc_10_porting_notes/fno_common
	CFLAGS+=' -Wno-implicit-function-declaration'

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
