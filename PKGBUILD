# Maintainer: GordonGR <gordongr@freemail.gr>
# Contributor: Splex

pkgname=teapotviewer
pkgver=0.2.6.0
_pngver=1.5.1
pkgrel=5
pkgdesc="An Open Source third party viewers for Second Life® (secondlife) and OpenSim (opensimulator) grids."
url="http://bitbucket.org/ArminW/teapot/"
license=('LGPL')
#install=teapot.install
arch=('i686' 'x86_64')
depends=('apr-util' 'gtk2' 'libgl' 'libidn' 'libjpeg-turbo' 'mesa' 'nss' 'sdl' 'glu' 'pangox-compat')
optdepends=('libpulse: for PulseAudio support' 'alsa-lib: for ALSA support' 'nvidia-utils: for NVIDIA support' 'flashplugin: for inworld Flash support' 'gstreamer0.10: for video support, may need good, bad and ugly plugins' 'lib32-freealut: for OpenAL support')

if [ "$CARCH" = "i686" ]; then
#"http://bitbucket.org/ArminW/teapot/downloads/Teapot-${pkgver}-Linux-${_arch}.tar.bz2"
	_arch=$CARCH
	source=(
	"http://downloads.sourceforge.net/sourceforge/libpng/libpng-${_pngver}.tar.xz" \
	'teapot.desktop' \
	'teapot.launcher'
	'donotregister.patch')
md5sums=('35455234375f1adff8083f408a099e3a'
         '6b960a3107f657d61d95d6f1b14c1d78'
         'd2c092d1595a5a0135efba868447872a'
         '1b4db712c2db43c0ae4f69b6b5410c43')



elif [ "$CARCH" = "x86_64" ]; then
#"http://bitbucket.org/ArminW/teapot/downloads/Teapot-${pkgver}-Linux-${_arch}.tar.bz2"
	_arch=$CARCH
	source=(
	"http://downloads.sourceforge.net/sourceforge/libpng/libpng-${_pngver}.tar.xz" \
	'teapot.desktop' \
	'teapot.launcher'
	'donotregister.patch')
md5sums=('35455234375f1adff8083f408a099e3a'
         '6b960a3107f657d61d95d6f1b14c1d78'
         'd2c092d1595a5a0135efba868447872a'
         '1b4db712c2db43c0ae4f69b6b5410c43')

fi



build(){
if [ "$CARCH" = "i686" ]; then
	export CC="gcc"
	export CXX="g++"
	export PKG_CONFIG_PATH="/usr/lib/pkgconfig"

elif [ "$CARCH" = "x86_64" ]; then
	export CC="gcc -m32"
	export CXX="g++ -m32"
	export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

fi

cd "${srcdir}/libpng-${_pngver}"
./configure
make
}


package() {
  #Download AND rename source directory
  cd $srcdir

  wget http://bitbucket.org/ArminW/teapot/downloads/Teapot-${pkgver}-Linux-${CARCH}.tar.bz2
  tar -xjvf Teapot-${pkgver}-Linux-${CARCH}.tar.bz2
  mv Teapot-${pkgver}-Linux-${CARCH} teapot

  
  # Do not register the application (saves from badly-installed desktop files and icons)
  
  patch teapot/teapot donotregister.patch
  rm teapot/etc/refresh_desktop_app_entry.sh

  # Install Desktop File
  install -D -m644 $srcdir/teapot.desktop \
    $pkgdir/usr/share/applications/teapot.desktop
  
  # Install Icon File
  install -D -m644 $srcdir/teapot/teapot_icon.png \
    $pkgdir/usr/share/pixmaps/teapot.png
  
  # Install Launcher
  install -D -m755 $srcdir/teapot.launcher \
    $pkgdir/usr/bin/teapot

  # Move Data to Destination Directory
  install -d $pkgdir/opt
  mv teapot $pkgdir/opt/
  
  # Change Permissions of files to root:games
  chown -R root:games $pkgdir/opt/teapot
  chmod -R g+rw $pkgdir/opt/teapot
  # Make Binary Group-Executable
  chmod g+x $pkgdir/opt/teapot/teapot
  

  # Install libpng we built

if [ "$CARCH" = "i686" ]; then
	install -D -m755 $srcdir/libpng-${_pngver}/.libs/libpng15.so.15.1.0 \
	$pkgdir/opt/teapot/lib
	cd $pkgdir/opt/teapot/lib
	ln -s libpng15.so.15.1.0 libpng15.so.15
	ln -s libpng15.so.15.1.0 libpng15.so

elif [ "$CARCH" = "x86_64" ]; then
	install -D -m755 $srcdir/libpng-${_pngver}/.libs/libpng15.so.15.1.0 \
	$pkgdir/opt/teapot/lib32
	cd $pkgdir/opt/teapot/lib32
	ln -s libpng15.so.15.1.0 libpng15.so.15
	ln -s libpng15.so.15.1.0 libpng15.so

fi
}
