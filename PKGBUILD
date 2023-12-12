# Maintainer: Robin Ekman <robin [dot] seth [dot] ekman [at] gmail [dot] com>
# Contributor: ToadKing <toadking at toadking dot com>
# Contributor: Soukyuu <chrno-sphered at hotmail dot com>
# Contributor: archtux <antonio dot arias99999 at gmail dot com>
_pkgname=deadbeef
pkgname=${_pkgname}-ekman-git
pkgver=1.9.6.r213.gb93374bcf
pkgrel=3
pkgdesc="A GTK+ audio player for GNU/Linux (Robin Ekman's fork)"
url="https://deadbeef.sourceforge.io/"
arch=('i686' 'x86_64')
license=('GPL2'
         'LGPL2.1'
         'ZLIB')
depends=(
    'alsa-lib'
    'hicolor-icon-theme'
    'jansson'
    'libblocksruntime'
    'libdispatch'
)
makedepends=(
    'curl'
    'faad2'
    'flac'
    'git'
    'intltool'
    'imlib2'
    'libcddb'
    'libcdio'
    'libmad'
    'libpulse'
    'libsamplerate'
    'libvorbis'
    'libx11'
    'libzip'
    'wavpack'
    'yasm'
    'ffmpeg'
    'gtk2'
    'gtk3'
    'clang'
    'libpipewire'
)
optdepends=(
    'gtk2: for the GTK2 interface'
    'gtk3: for the GTK3 interface'
    'libsamplerate: for dsp_libsrc plugin (resampler)'
    'libsm: optional dependency for gtkui session client support'
    'libice: optional dependency for gtkui session client support'
    'alsa-lib: ALSA support'
    'alsa-oss: for OSS output plugin'
    'libvorbis: for ogg vorbis plugin'
    'libogg: for ogg vorbis plugin'
    'libmad: for mp3 plugin (mpeg1,2 layers1,2,3)'
    'flac: for flac plugin'
    'curl: for last.fm, vfs_curl (shoutcast/icecast), artwork plugins'
    'imlib2: for artwork plugin'
    'wavpack: for wavpack plugin'
    'libsndfile: for sndfile plugin'
    'libcdio: for cd audio plugin'
    'libcddb: for cd audio plugin'
    'cdparanoia: for cd audio plugin'
    'faad2: for AAC plugin'
    'dbus: for notification daemon support (OSD current song notifications)'
    'pulseaudio: for PulseAudio output plugin'
    'libx11: for global hotkeys plugin'
    'zlib: for Audio Overload plugin (psf, psf2, etc), GME (for vgz)'
    'yasm: required to build assembly portions of ffap plugin'
    'libzip: for vfs_zip plugin'
    'ffmpeg: for ffmpeg plugin'
    'opusfile: for opus plugin'
    'mpg123: for MP1/MP2/MP3 playback'
    'libpipewire: for pipewire plugin'
)
options=('!libtool')
conflicts=('deadbeef' 'deadbeef-git')
provides=("deadbeef=${pkgver}")
source=('git+https://github.com/rsekman/deadbeef.git#branch=ekman')
md5sums=('SKIP')

prepare() {
  cd "$srcdir/deadbeef"
  # skip osx/deps and external/apbuild submodules
  git -c submodule."osx/deps".update=none -c submodule."external/apbuild".update=none submodule update --init --recursive
}

build() {
  cd "$srcdir/${_pkgname}"

  ./autogen.sh
  CC=clang CXX=clang++ ./configure --prefix=/usr
  make -j
}

package() {
  cd "$srcdir/${_pkgname}"

  make DESTDIR="$pkgdir" install
  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

pkgver() {
  cd "$srcdir/${_pkgname}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

check() {
  cd "$srcdir/${_pkgname}"

  travis/download-linux-static-deps.sh
  ./scripts/test.sh
}
