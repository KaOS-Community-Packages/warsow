pkgname=warsow
pkgver=2.1
pkgrel=1
pkgdesc='Free online multiplayer competitive FPS based on the Qfusion engine'
url='https://www.warsow.gg/'
license=('GPL' 'custom:Warsow Content License')
arch=('x86_64')
depends=('sdl2' 'libpng' 'curl' 'libvorbis' 'freetype2'
         'libxinerama' 'libxxf86vm' 'libxrandr' 'libtheora' 'libxi')
makedepends=('mesa' 'openal' 'imagemagick' 'gendesk' 'cmake')
source=('warsow.launcher'
        'wsw-server.launcher'
        'wswtv-server.launcher'
        'http://mirror.null.one/warsow_21_sdk.tar.gz' 
        'http://mirror.null.one/warsow_21_unified.tar.gz')

md5sums=('003f8a0974f0cd5c2b9e78d49cab24c9'
         '45a3f846fd6ea3b7dc857e60501d0e12'
         '13d520525638c544565d8f799ffdca48'
         '56c02b5e9bd6f921fbc980e868c2b48d' 
         'fac70b30d7295c0bc4c3f0432c4b7937')

prepare() {
  gendesk -n -f --pkgname 'warsow' --pkgdesc "${pkgdesc}" --name 'Warsow' --categories 'Game;ActionGame'
}

build() {
  cd "${srcdir}/source/source"
  cmake .
  make ${MAKE_FLAGS}
}

package() {
  local builddir="${srcdir}/source/source/build"

  # Create Destination Directories
  install -d "${pkgdir}/opt/warsow/"

  # Move Compiled Data to Destination Directory except basewsw.
  # NOTE: We don't need cgame library because it's a pure lib provided by
  # modules_16.pk3 from warsow-data package.
  cp -r "${builddir}/libs" "${pkgdir}/opt/warsow"
  cp "${builddir}/warsow.${CARCH}" "${pkgdir}/opt/warsow/warsow"
  cp "${builddir}/wsw_server.${CARCH}" "${pkgdir}/opt/warsow/wsw_server"
  cp "${builddir}/wswtv_server.${CARCH}" "${pkgdir}/opt/warsow/wswtv_server"
  cp -r $srcdir/warsow_20/{basewsw,docs} $pkgdir/opt/warsow
  find "${pkgdir}/opt/warsow" -type d | xargs chmod 755
  find "$pkgdir/opt/warsow" -type f | xargs chmod 644
  find "${pkgdir}/opt/warsow" -type f | xargs chmod 755 # only executables here
  
  # Install launchers to /usr/bin
  install -D -m 0755 "${srcdir}/warsow.launcher" "${pkgdir}/usr/bin/warsow"
  install -D -m 0755 "${srcdir}/wsw-server.launcher" "${pkgdir}/usr/bin/wsw-server"
  install -D -m 0755 "${srcdir}/wswtv-server.launcher" "${pkgdir}/usr/bin/wswtv-server"

  # Install the menu entry
  install -D -m 0644 "${srcdir}/warsow.desktop" "${pkgdir}/usr/share/applications/warsow.desktop"

  # Install the launcher icon
  convert "${srcdir}/source/icons/warsow256x256.xpm" "${srcdir}/warsow.png"
  install -D -m 0644 "${srcdir}/warsow.png" "${pkgdir}/usr/share/pixmaps/warsow.png"
  
   # Install Custom License: Warsow Content License
  install -Dm0644 "warsow_20/docs/license.txt" "$pkgdir/usr/share/licenses/${pkgname}/license.txt"
}


