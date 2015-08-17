# Maintainer: Jacob Hinkle <jacob@sci.utah.edu>
pkgname=pyca-git
pkgver=20130228
pkgrel=1
pkgdesc="Python for Computational Anatomy"
arch=('x86_64')
url="http://bitbucket.org/scicompanat/pyca"
license=('BSD')
groups=()
depends=()
makedepends=('git' 'python2' 'swig' 'fftw')
optdepends=('insight-toolkit: For ITK file support (needs rebuild)'
            'cuda: For GPU computation ability (needs rebuild)')
provides=(pyca)
conflicts=()
replaces=()
backup=()
options=()
install=
source=()
noextract=()
md5sums=() #generate with 'makepkg -g'

_gitroot=git@bitbucket.org:scicompanat/pyca.git
_gitname=pyca

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
  mkdir "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #
  # BUILD HERE
  #
  cmake -D CMAKE_BUILD_TYPE=Release \
        -D PYTHON_EXECUTABLE=/usr/bin/python2 \
        -D PYTHON_INCLUDE_DIR=/usr/include/python2.7 \
        -D BUILD_SHARED_LIBS=ON \
        -D PYTHON_LIBRARY=/usr/lib/libpython2.7.so \
        -D PYTHON_INSTALL_DIR=/usr/lib/python2.7/site-packages \
        -D CUDA_NVCC_FLAGS_RELEASE=--pre-include\ $srcdir/$_gitname/preinc.h $srcdir/$_gitname
  make
}

package() {
  cd "$srcdir/$_gitname-build"
  make DESTDIR=$pkgdir install
}

# vim:set ts=2 sw=2 et:
