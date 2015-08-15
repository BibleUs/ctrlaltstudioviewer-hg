# Maintainer: Zach Jaggi <feilen1000@gmail.com>

_basename=ctrlaltstudioviewer
pkgname=${_basename}-hg
pkgver=0.1
pkgrel=1
pkgdesc='Second Life is a 3-D virtual world entirely built and owned by its residents. Ctrl Alt Studio Viewer is alternative viewer for secondlife, with Oculus Rift support'
arch=('x86_64')
url="http://www.secondlife.com/"
license=('LGPL')
[ "$CARCH" = "x86_64" ] && depends=('lib32-freealut' 'lib32-gtk2' 'lib32-libidn' 'lib32-mesa' 'lib32-sdl' 'lib32-libxml2' 'lib32-glu' 'lib32-pangox-compat')
[ "$CARCH" = "x86_64"   ] && optdepends=('lib32-libpulse: for PulseAudio support' 'lib32-alsa-lib: for ALSA support' 'lib32-nvidia-utils: for NVIDIA support' 'lib32-flashplugin: for inworld Flash support')
makedepends=('cmake' 'mercurial' 'autobuild' 'oculus-rift-sdk-jherico-git')
optdepends=('oculus-udev: Rules to let users access the Oculus Rift')
provides=('secondlife-bin')
conflicts=('secondlife-bin')
source=('hg+https://hg.codeplex.com/ctrlaltstudioviewer'
		'ctrlaltstudio-fix.patch'
		'casviewer.sh')

sha256sums=('SKIP'
			'SKIP'
			'SKIP')

build() {

	cd ${srcdir}/${_basename}

	patch -Np1 < ${srcdir}/ctrlaltstudio-fix.patch

	# Resolve python2/python3 confusion

	find -iname "*.sh" -exec sed -i 's/\/usr\/bin\/python/\/usr\/bin\/python2/g' {} +
	find -iname "*.py" -exec sed -i 's/\/usr\/bin\/python/\/usr\/bin\/python2/g' {} +
	
	if [ ! -d ${srcdir}/pythonfix ]; then
		mkdir ${srcdir}/pythonfix
		ln -sT $(which python2) ${srcdir}/pythonfix/python
	fi
	export PATH="${srcdir}/pythonfix:$PATH"

	# Resolve break on issue
	export CXXFLAGS="${CXXFLAGS} -Wno-error" CFLAGS="${CFLAGS} -Wno-error"

	# Fix issues executing ".sh"
	find -iname "*.sh" -exec chmod +x {} +

	autobuild build -c ReleaseFS_open -- -DFMOD:BOOL=FALSE
}

package() {
	# Modified from install.sh

	mkdir -p ${pkgdir}/opt/firestorm ${pkgdir}/usr/bin
	cp -a ${srcdir}/${_basename}/build-linux-i686/newview/packaged/* ${pkgdir}/opt/firestorm
	install -m 755 ${srcdir}/casviewer.sh ${pkgdir}/usr/bin/casviewer
}
