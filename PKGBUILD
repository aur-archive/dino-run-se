# Maintainer: Tanguy ALEXIS (tanguy at metatux dot fr)

pkgname=dino-run-se
pkgver=1.1
pkgrel=2
pkgdesc="A retro-styled, action-packed prehistoric multiplayer racing game."
url="http://www.pixeljam.com/dinorunse/"
license=('custom: "commercial"')
groups=('indieroyalesummer' 'indieroyale')
arch=('i686' 'x86_64')
depends=('bash')
# Warning for x64 arch, the aur lib32-gstreamer0.10-base have a lot of dep
[ "${CARCH}" = "x86_64" ] && depends=("${depends[@]}" 'lib32-gstreamer0.10' 'lib32-gstreamer0.10-base' 'lib32-libxtst' 'lib32-nss' 'flashplayer' 'lib32-curl' 'lib32-gtk2' 'lib32-libsm' 'lib32-libxt')
[ "${CARCH}" = "i686" ]   && depends=("${depends[@]}" 'gstreamer0.10' 'gstreamer0.10-base' 'libxtst' 'nss' 'flashplayer' 'curl' 'gtk2' 'libsm' 'libxt') 
[ "${CARCH}" = "x86_64" ] && optdepends=('lib32-alsa-lib: sound support' 'lib32-openal: sound support')
[ "${CARCH}" = "i686" ]   && optdepends=('alsa-lib: sound support' 'openal: sound support')
options=(!strip)
PKGEXT='.pkg.tar'
source=("${pkgname}.desktop" "${pkgname}-exec")
md5sums=('3245e4993a85be30be7e3d2d666ae588'
         '55ed20551fc03af68f176798eeb97478')
_gamepkg="${pkgname}-${pkgver}.tar.bz2"

build() {
   cd "${srcdir}"
   msg "You need a full copy of this game in order to install it"
   msg "Searching for ${_gamepkg} in dir: $(readlink -f `pwd`)"
   if [[ -f "${_gamepkg}" ]]; then
       msg "Found game package, installing..."
       ln -fs "../${_gamepkg}" .
   elif [ -n "${_indieroyalesummerkey}" ]; then
       msg "Game package not found, trying to download..."
       rm -f bundle_index.html
       wget http://www.indieroyale.com/bundle/key/${_indieroyalesummerkey} -O bundle_index.html
       wget http://www.indieroyale.com$(cat bundle_index.html | grep -B 1 28.32mb | cut -d '"' -f2 | grep /) -O ${_gamepkg}
       rm -f bundle_index.html
   else
       msg "Game package not found and download failed."
       msg "You can add \'export _indieroyalesummerkey\=\<Your key here\>\' to \.bashrc if you want automated download ability."
       error "Please type absolute path to ${_gamepkg} :"
       read pkgpath
       if [[ -f "${pkgpath}/${_gamepkg}" ]]; then
           msg "Found game package, installing..."
           ln -fs "${pkgpath}/${_gamepkg}" .
       else
           error "Unable to find game package."
           return 1
       fi
   fi
   tar xjf ${_gamepkg}
}

package(){
  cd "${srcdir}"
   install -d "${pkgdir}/opt/${pkgname}"
   cp -R "${srcdir}"/dino_run_se/* "${pkgdir}/opt/${pkgname}/"
   chmod +rx "${pkgdir}/opt/${pkgname}/Dino_Run_SE"
   chmod +rx "${pkgdir}/opt/${pkgname}/README.txt"
   chmod -R +wrx "${pkgdir}/opt/${pkgname}/Icons"
  # create Launcher
  install -d "${pkgdir}/usr/bin/"
  install -D -m755 "${srcdir}/${pkgname}-exec" "${pkgdir}/usr/bin/${pkgname}"
  # Install Desktop File and Icon
  install -D -m644 "${srcdir}/${pkgname}.desktop" \
        "${pkgdir}/usr/share/applications/${pkgname}.desktop"
  install -D -m644 "${srcdir}/dino_run_se/Icons/DinoRunSEIcon05_256x256.png" \
        "${pkgdir}/usr/share/icons/${pkgname}-icon.png"
}

