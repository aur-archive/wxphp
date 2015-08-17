# Maintainer: Andrew Rose <hello@andrewrose.co.uk>
# Contributor: Andrew Rose <hello@andrewrose.co.uk>

pkgname=wxphp
pkgdesc='Custom wxPHP PHP build with ZTS enabled'
pkgver=5.5.10
pkgrel=1

arch=('x86_64' 'i686')
license=('PHP')
url='http://www.php.net'

makedepends=('sqlite' 'unixodbc' 'net-snmp' 'libzip' 'enchant' 'file' 'freetds'
             'libmcrypt' 'tidyhtml' 'aspell' 'libltdl' 'libpng' 'libjpeg' 'icu'
             'curl' 'libxslt' 'openssl' 'bzip2' 'db' 'gmp' 'freetype2' 'systemd')
source=("http://www.php.net/distributions/php-${pkgver}.tar.bz2")
md5sums=('d608230c7890b6a0cc5b92e66e866226')
depends=('pcre' 'libxml2' 'bzip2' 'curl' 'libxslt' 'tidyhtml' 'sqlite' 'net-snmp'
        'aspell' 'postgresql-libs' 'unixodbc' 'freetds' 'libmcrypt' 'libltdl' 'libldap'
        'libpng' 'libjpeg' 'freetype2' 'libvpx' 'enchant' 'icu')

backup=('etc/wxphp/php.ini' 'etc/wxphp/pear.conf')

build() {

	phpconfig="--srcdir=../php-${pkgver} \
		 --config-cache \
		 --prefix=/opt/wxphp \
		 --bindir=/opt/wxphp/bin \
		 --includedir=/opt/wxphp/include \
		 --libdir=/opt/wxphp/lib \
		 --sysconfdir=/etc/wxphp \
		 --localstatedir=/var \
		 --with-layout=GNU \
		 --with-config-file-path=/etc/wxphp \
		 --with-config-file-scan-dir=/etc/wxphp/conf.d \
		 --disable-rpath \
		 --mandir=/opt/wxphp/man \
		 --without-pear \
		 --enable-maintainer-zts \
		 --enable-inline-optimization
	"

	phpextensions="--enable-bcmath=shared \
		--enable-calendar=shared \
		--enable-dba=shared \
		--enable-exif=shared \
		--enable-ftp=shared \
		--enable-gd-native-ttf \
		--enable-intl=shared \
		--enable-mbstring \
		--enable-phar=shared \
		--enable-posix=shared \
		--enable-shmop=shared \
		--enable-soap=shared \
		--enable-sockets=shared \
		--enable-sysvmsg=shared \
		--enable-sysvsem=shared \
		--enable-sysvshm=shared \
		--enable-zip=shared \
		--with-bz2=shared \
		--with-curl=shared \
		--with-db4=/usr \
		--with-enchant=shared,/usr \
		--with-freetype-dir=/usr \
		--with-gd=shared \
		--with-gdbm \
		--with-gettext=shared \
		--with-gmp=shared \
		--with-iconv=shared \
		--with-icu-dir=/usr \
		--with-imap-ssl \
		--with-imap=shared \
		--with-jpeg-dir=/usr \
		--with-vpx-dir=/usr \
		--with-ldap=shared \
		--with-ldap-sasl \
		--with-mcrypt=shared \
		--with-mhash \
		--with-mssql=shared \
		--with-mysql-sock=/var/run/mysqld/mysqld.sock \
		--with-mysql=shared,mysqlnd \
		--with-mysqli=shared,mysqlnd \
		--with-openssl=shared \
		--with-pcre-regex=/usr \
		--with-pdo-mysql=shared,mysqlnd \
		--with-pdo-odbc=shared,unixODBC,/usr \
		--with-pdo-pgsql=shared \
		--with-pdo-sqlite=shared,/usr \
		--with-pgsql=shared \
		--with-png-dir=/usr \
		--with-pspell=shared \
		--with-snmp=shared \
		--with-sqlite3=shared,/usr \
		--with-tidy=shared \
		--with-unixODBC=shared,/usr \
		--with-xmlrpc=shared \
		--with-xsl=shared \
		--with-zlib \
	"

        EXTENSION_DIR=/opt/wxphp/lib/modules
        export EXTENSION_DIR
        PEAR_INSTALLDIR=/opt/wxphp/pear
        export PEAR_INSTALLDIR

        cd ${srcdir}/php-${pkgver}

        # php
        mkdir ${srcdir}/build-php
        cd ${srcdir}/build-php
        ln -s ../php-${pkgver}/configure
        ./configure ${phpconfig} \
                --disable-cgi \
                --with-readline \
                --enable-pcntl \
                ${phpextensions}
        make
	make -j1 INSTALL_ROOT=${pkgdir} install

        install -d -m755 ${pkgdir}/opt/wxphp/pear
        install -d -m755 ${pkgdir}/usr/bin
        # install php.ini
        install -D -m644 ${srcdir}/php-${pkgver}/php.ini-production ${pkgdir}/etc/wxphp/php.ini
        install -d -m755 ${pkgdir}/etc/wxphp/conf.d/

	ln -s /opt/wxphp/bin/php ${pkgdir}/usr/bin/wxphp

        mv ${pkgdir}/opt/wxphp/man/man1/php.1 ${pkgdir}/opt/wxphp/man/man1/wxphp.1.gz
        mv ${pkgdir}/opt/wxphp/man/man1/php-config.1 ${pkgdir}/opt/wxphp/man/man1/wxphp-config.1.gz
        mv ${pkgdir}/opt/wxphp/man/man1/phpize.1 ${pkgdir}/opt/wxphp/man/man1/wxphp-phpize.1.gz
        mv ${pkgdir}/opt/wxphp/man/man1/phar.1 ${pkgdir}/opt/wxphp/man/man1/wxphp-har.1.gz
        mv ${pkgdir}/opt/wxphp/man/man1/phar.phar.1 ${pkgdir}/opt/wxphp/man/man1/wxphp-phar_phar.1.gz
}
