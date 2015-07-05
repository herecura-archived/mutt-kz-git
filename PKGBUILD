# Contributor: boyska <piuttosto@logorroici.org>

pkgname=mutt-kz-git
pkgver=20130111
pkgrel=1
pkgdesc="A small but very powerful text-based mail client + integration with notmuch"
url="https://github.com/karelzak/mutt-kz"
arch=('i686' 'x86_64')
license=('GPL')
depends=('openssl>=0.9.8e' 'gdbm' 'mime-types' 'zlib' 'libsasl' 'gpgme' 'ncurses' 'notmuch')
makedepends=('git' 'gnupg' 'libxslt')
conflicts=('mutt')
provides=('mutt')
options=('!strip')

_gitroot=git://github.com/karelzak/mutt-kz.git
_gitname=muttkz

build() {
	cd "$srcdir"
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

	sed -e 's/AM_CONFIG_HEADER/AC_CONFIG_HEADERS/g' -i configure.ac
	./prepare \
		--prefix=/usr \
		--sysconfdir=/etc \
		--enable-debug \
		--enable-gpgme \
		--enable-hcache \
		--enable-imap \
		--enable-notmuch \
		--enable-pgp \
		--enable-pop \
		--enable-smtp \
		--with-idn \
		--with-sasl \
		--with-ssl=/usr \
		--with-regex
	make
}

package() {
	cd "$srcdir/$_gitname-build"
	make DESTDIR="$pkgdir/" install
	rm -f ${pkgdir}/etc/mime.types*
	install -Dm644 contrib/gpg.rc ${pkgdir}/etc/Muttrc.gpg.dist
}
