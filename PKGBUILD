# Maintainer: Giovanni Lena <giovannilena at mail dot com>

pkgname=badlion-client
pkgver=0.0.1
pkgrel=1
pkgdesc="Minecraft PvP client with integrated anticheat made by Badlion"
arch=('x86_64')
url="https://client-updates.badlion.net/BadlionClient"
license=('unknown')
_desktop_name="BadlionClient.desktop"
options=(!strip)

_filename="BadlionClient"
source_x86_64=("https://client-updates.badlion.net/${_filename}")
sha256sums_x86_64=("SKIP")

prepare() {
    chmod +x "${_filename}"
    ./"${_filename}" --appimage-extract
}

build() {
    # Adjust .desktop so it will work outside of AppImage container
    sed -i -E "s|Exec=AppRun|Exec=env DESKTOPINTEGRATION=false /usr/bin/${_desktop_name} |" \
            "squashfs-root/${_desktop_name}"
    # Fix permissions; .AppImage permissions are 700 for all directories
    chmod -R a-x+rX squashfs-root/usr
}

package() {
    # AppImage
    install -Dm755 "${srcdir}/${_filename}" "${pkgdir}/opt/${pkgname}/${pkgname}"

    # Desktop file
    install -Dm644 "${srcdir}/squashfs-root/${_desktop_name}" \
            "${pkgdir}/usr/share/applications/${_desktop_name}"

    # Icon images
    install -dm755 "${pkgdir}/usr/share/"
    cp -a "${srcdir}/squashfs-root/usr/share/icons" "${pkgdir}/usr/share/"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${pkgname}" "${pkgdir}/usr/bin/${pkgname}"
}
