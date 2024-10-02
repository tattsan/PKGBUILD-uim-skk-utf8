#############################
_pkgname=uim
pkgname=uim-skk-utf8
pkgver=1.8.9
pkgrel=2
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
	"uim-skk-1.8.8-utf8-1.patch"
	"uim-skk-1.8.8-utf8-2.patch"
	"uim-skk-1.8.8-utf8-3.patch"
	"uim-skk-1.8.8-utf8-gai.patch"
	)
sha256sums=('dbbd983768bf748449551644f330dbebe859bfeb6f024fea6697ac75131c7aa4'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	'SKIP'
	)

provides=('uim')
conflicts=('uim')


prepare() {
	cd "${srcdir}/${_pkgname}-${pkgver}"/scm
	for f in japanese-act.scm japanese-azik.scm japanese-custom.scm japanese-kana.scm japanese-kzik.scm japanese.scm skk.scm skk-custom.scm
	  do
            new="$(echo $f | sed 's/\.scm$/-utf8.scm/')"
            iconv -f EUC-JP -t UTF-8 < "$f" > "$new"
        done
        cat "${srcdir}"/uim-skk-1.8.8-utf8-1.patch |  patch -p0 -b --follow-symlink
	mv skk.scm skk.scm.orig && mv skk-utf8.scm skk.scm
	mv skk-custom.scm skk-custom.scm.orig && mv skk-custom-utf8.scm skk-custom.scm
	rm -f japanese-custom-utf8.scm.orig japanese-utf8.scm.orig skk-utf8.scm.orig
	cd "${srcdir}/${_pkgname}-${pkgver}"/uim
        cat "${srcdir}"/uim-skk-1.8.8-numeric.patch |  patch -p0 -b --follow-symlink
        iconv -f EUC-JP -t UTF-8 < skk.c > skk-utf8.c
	mv skk.c skk.c.orig && mv skk-utf8.c skk.c
        cat "${srcdir}"/uim-skk-1.8.8-utf8-2.patch |  patch -p0 -b --follow-symlink
        cat "${srcdir}"/uim-skk-1.8.8-utf8-3.patch |  patch -p0 -b --follow-symlink
	cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -p1 -l < "${srcdir}"/uim-skk-1.8.8-utf8-gai.patch
}

build() {
	cd "${srcdir}/${_pkgname}-${pkgver}"

	CFLAGS+=' -fcommon' # https://wiki.gentoo.org/wiki/Gcc_10_porting_notes/fno_common

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
	cd scm
	for f in japanese-act-utf8.scm japanese-azik-utf8.scm japanese-custom-utf8.scm japanese-kana-utf8.scm japanese-kzik-utf8.scm japanese-utf8.scm
	  do
	    install -D -m644 $f "${pkgdir}/usr/share/uim/"
        done
}
