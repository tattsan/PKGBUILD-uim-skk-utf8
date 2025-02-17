#############################
_pkgname=uim
pkgname=uim-skk-utf8
pkgver=1.8.9
#############################
pkgrel=3
#############################
pkgdesc='Multilingual input method library'
url='https://github.com/uim/uim'
license=('custom:BSD')
arch=('x86_64')
depends=('libxft' 'libedit' 'm17n-lib')
makedepends=('intltool' 'gettext' 'gtk2' 'gtk3' 'qt5-x11extras' 'anthy' 'plasma-framework5')
optdepends=('qt5-x11extras: immodule and helper applications'
            'gtk2: immodule and helper applications'
            'gtk3: immodule and helper applications'
            'skk-jisyo: input method'
            'anthy: input method')
source=("https://github.com/${_pkgname}/${_pkgname}/releases/download/${pkgver}/${_pkgname}-${pkgver}.tar.bz2"
	"uim-skk-1.8.8-numeric.patch"
	"uim-skk-1.8.9-utf8.patch"
	"uim-skk-1.8.9-utf8-gai.patch"
	)
sha256sums=('dbbd983768bf748449551644f330dbebe859bfeb6f024fea6697ac75131c7aa4'
	'SKIP'
	'SKIP'
	'SKIP'
	)

provides=('uim')
conflicts=('uim')


prepare() {
	cd "${srcdir}/${_pkgname}-${pkgver}"/uim
        patch -p0 -l < "${srcdir}"/uim-skk-1.8.8-numeric.patch
	cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -p1 -l < "${srcdir}"/uim-skk-1.8.9-utf8.patch
	patch -p1 -l < "${srcdir}"/uim-skk-1.8.9-utf8-gai.patch
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
		--with-anthy-utf8 \
		--with-skk \

	make
}

package() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install -j1 # FS#41112
	install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${_pkgname}/COPYING"
}
