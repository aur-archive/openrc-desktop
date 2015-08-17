# Maintainer: artoo <artoo@manjaro.org>

# file vars for easy update
_Cacpi=acpid-2.0.16-conf.d
_Iacpi=acpid-2.0.16-init.d
_Calsa=alsasound.confd-r4
_Ialsa=alsasound.initd-r6
_Ick=consolekit-0.2.rc
_Cxdm=xdm.confd-4
_Ixdm1=xdm.initd-11
_Ixdm2=xdm-setup.initd-1
_Sxdm=startDM.sh
_Cgpm=gpm.conf.d
_Igpm=gpm.rc6-2
_Cblue=rfcomm-conf.d
_Iblue1=rfcomm-init.d-r2
_Iblue2=bluetooth-init.d-r3
_Cwpa=wpa_supplicant-conf.d
_Iwpa=wpa_supplicant-init.d
_Swpa=wpa_cli.sh

_gentoo_uri="http://sources.gentoo.org/cgi-bin/viewvc.cgi/gentoo-x86"

pkgbase=openrc-desktop
pkgname=('acpid-openrc'
	'alsa-utils-openrc'
	'avahi-openrc'
	'consolekit-openrc'
	'displaymanager-openrc'
	'gpm-openrc'
	'bluez-openrc'
	'wpa_supplicant-openrc')
pkgver=20150119
pkgrel=1
pkgdesc="OpenRC init scripts"
arch=('any')
url="https://github.com/manjaro/packages-openrc"
license=('GPL2')
groups=('openrc' 'openrc-desktop')
conflicts=('openrc'
	  'openrc-git'
	  'openrc-arch-services-git'
	  'initscripts'
	  'systemd-sysvcompat')
source=("${_gentoo_uri}/sys-power/acpid/files/${_Cacpi}"
	"${_gentoo_uri}/sys-power/acpid/files/${_Iacpi}"
	"${_gentoo_uri}/media-sound/alsa-utils/files/${_Calsa}"
	"${_gentoo_uri}/media-sound/alsa-utils/files/${_Ialsa}"
	"${_gentoo_uri}/sys-auth/consolekit/files/${_Ick}"
	"${_gentoo_uri}/x11-base/xorg-server/files/${_Cxdm}"
	"${_gentoo_uri}/x11-base/xorg-server/files/${_Ixdm1}"
	"${_gentoo_uri}/x11-base/xorg-server/files/${_Ixdm2}"
	"${_gentoo_uri}/x11-apps/xinit/files/${_Sxdm}"
	"${_gentoo_uri}/sys-libs/gpm/files/${_Cgpm}"
	"${_gentoo_uri}/sys-libs/gpm/files/${_Igpm}"
	'avahi-daemon'
	'avahi-dnsconfd'
	"${_gentoo_uri}/net-wireless/bluez/files/${_Cblue}"
	"${_gentoo_uri}/net-wireless/bluez/files/${_Iblue1}"
	"${_gentoo_uri}/net-wireless/bluez/files/${_Iblue2}"
	"${_gentoo_uri}/net-wireless/wpa_supplicant/files/${_Cwpa}"
	"${_gentoo_uri}/net-wireless/wpa_supplicant/files/${_Iwpa}"
	"${_gentoo_uri}/net-wireless/wpa_supplicant/files/${_Swpa}")

pkgver() {
  date +%Y%m%d
}

_shebang='s|#!/sbin/runscript|#!/usr/bin/openrc-run|'
_runpath='s|/var/run|/run|g'
_binpath='s|/usr/sbin|/usr/bin|g'

package_acpid-openrc() {
    pkgdesc="OpenRC acpid init script"
    depends=('openrc-core' 'acpid')
    backup=('etc/conf.d/acpid')
    install=acpid.install

    install -Dm755 "${srcdir}/${_Cacpi}" "${pkgdir}/etc/conf.d/acpid"
    install -Dm755 "${srcdir}/${_Iacpi}" "${pkgdir}/etc/init.d/acpid"

    sed -e "${_shebang}" -e "${_binpath}" -i "${pkgdir}/etc/init.d/acpid"
}

package_alsa-utils-openrc() {
    pkgdesc="OpenRC alsa-utils init script"
    depends=('openrc-core' 'alsa-utils')
    backup=('etc/conf.d/alsasound')
    install=alsa-utils.install

    install -Dm755 "${srcdir}/${_Calsa}" "${pkgdir}/etc/conf.d/alsasound"
    install -Dm755 "${srcdir}/${_Ialsa}" "${pkgdir}/etc/init.d/alsasound"

    sed -e "${_shebang}" -i "${pkgdir}/etc/init.d/alsasound"
}

package_avahi-openrc() {
    pkgdesc="OpenRC avahi init script"
    depends=('avahi' 'dbus-openrc')
    install=avahi.install

    install -Dm755 "${srcdir}/avahi-daemon" "${pkgdir}/etc/init.d/avahi-daemon"
    install -Dm755 "${srcdir}/avahi-dnsconfd" "${pkgdir}/etc/init.d/avahi-dnsconfd"
}

package_consolekit-openrc() {
    pkgdesc="OpenRC consolekit init script"
    depends=('consolekit' 'dbus-openrc')
    install=consolekit.install

    install -Dm755 "$srcdir/${_Ick}" "$pkgdir/etc/init.d/consolekit"

    sed -e "${_shebang}" -e "${_runpath}" -e "${_binpath}" -i "${pkgdir}/etc/init.d/consolekit"
}

package_displaymanager-openrc() {
    pkgdesc="OpenRC dm init script"
    depends=('openrc-core' 'xorg-server' 'xorg-xinit')
    optdepends=('consolekit-openrc: consolekit initscript'
			  'dbus-openrc: dbus initscript')
    backup=('etc/conf.d/xdm')
    install=displaymanager.install

    install -Dm755 "${srcdir}/${_Cxdm}" "${pkgdir}/etc/conf.d/xdm"
    install -Dm755 "${srcdir}/${_Ixdm1}" "${pkgdir}/etc/init.d/xdm"
    install -Dm755 "${srcdir}/${_Ixdm2}" "${pkgdir}/etc/init.d/xdm-setup"
    install -Dm755 "${srcdir}/${_Sxdm}" "${pkgdir}/etc/X11/startDM.sh"

    local _p1='s|/etc/profile.env|/etc/profile|g' \
	  _p2='s|{ROOTPATH}|{PATH}|g'
    sed -e "${_shebang}" -e "${_binpath}" -e "${_runpath}" -e "${_p1}" -e "${_p2}" -i "${pkgdir}/etc/init.d/xdm"
    sed -e "${_shebang}" -i "${pkgdir}/etc/init.d/xdm-setup"
}

package_gpm-openrc() {
    pkgdesc="OpenRC gpm init script"
    depends=('openrc-core' 'gpm')
    backup=('etc/conf.d/gpm')
    install=gpm.install

    install -Dm755 "${srcdir}/${_Cgpm}" "${pkgdir}/etc/conf.d/gpm"
    install -Dm755 "${srcdir}/${_Igpm}" "${pkgdir}/etc/init.d/gpm"

    sed -e "${_shebang}" -e "${_binpath}" -e "${_runpath}" -i "${pkgdir}/etc/init.d/gpm"
}

package_bluez-openrc() {
    pkgdesc="OpenRC bluez init script"
    groups=('openrc' 'openrc-mobile')
    depends=('bluez' 'dbus-openrc')
    backup=('etc/conf.d/rfcomm')
    install=bluez.install

    install -Dm755 "${srcdir}/${_Cblue}" "${pkgdir}/etc/conf.d/rfcomm"
    install -Dm755 "${srcdir}/${_Iblue1}" "${pkgdir}/etc/init.d/rfcomm"
    install -Dm755 "${srcdir}/${_Iblue2}" "${pkgdir}/etc/init.d/bluetooth"

    local _p1='s|/usr/sbin|/usr/lib/bluetooth|g' _p2='s/libexec/lib/'
    sed -e "${_shebang}" -e "${_p1}" -e "${_p2}" -i "${pkgdir}/etc/init.d/bluetooth"
    sed -e "${_shebang}" -e "${_binpath}" -i "${pkgdir}/etc/init.d/rfcomm"
}

package_wpa_supplicant-openrc() {
    pkgdesc="OpenRC wpa_supplicant init script"
    groups=('openrc' 'openrc-mobile')
    depends=('openrc-core' 'wpa_supplicant')
    backup=('etc/conf.d/wpa_supplicant')
    install=wpa_supplicant.install

    install -Dm755 "${srcdir}/${_Cwpa}" "${pkgdir}/etc/conf.d/wpa_supplicant"
    install -Dm755 "${srcdir}/${_Iwpa}" "${pkgdir}/etc/init.d/wpa_supplicant"
    install -Dm755 "${srcdir}/${_Swpa}" "${pkgdir}/etc/wpa_supplicant/wpa_cli.sh"

    sed -e "${_shebang}" -e "${_binpath}" -e "${_runpath}" -i "${pkgdir}/etc/init.d/wpa_supplicant"
    if [[ -f /etc/os-release ]];then
	    . /etc/os-release
	    sed -e "s|gentoo-release|${ID}-release|" -i "${pkgdir}/etc/wpa_supplicant/wpa_cli.sh"
    else
	    sed -e 's|gentoo-release|arch-release|' -i "${pkgdir}/etc/wpa_supplicant/wpa_cli.sh"
    fi
}

sha256sums=('3755d4eb8bb64a1304e5defedb949305ac550565da36fe4f94d5f31beee821ba'
            '980468e6bf96c7677898330cadbcff165b4d15f1197cd544548bd0f8c376983d'
            'd1c55400b701a72dcb8bb85e016b5013fa3eb6a2766ffc20dae278d0ee4c1a43'
            '5fdcb0212bf8a4be74f410534534fdda6dd8d57df0d2a6c4a158464f705fed18'
            'da849bae527a7a5c257301a99ac3fb5ec2ded48103ec114552ca7d2a24b12e49'
            '9d26b72bb28611a60a6b9f942b8d8cfe47b59f926be89af9709b5912668344d8'
            '86a17c9ba172481318d5fd51c3aadfdcad9e5d52ed7478379723ce1784061930'
            '942ce5e8d1a0770543b683dcc388bae7619a24eb9741c1cd678ed3df97c01406'
            'e7f2d95b3b4b6b5d711f926f8a3b7b0163b4d9e40b40489bcbd1316806e47499'
            '73e7483fdc4b12ab4225a4cb13bbe7da71b07b9e69b17e3a6a4c63cb5e2287c8'
            'e692e7b97efdd79f6e92fbdaed60f7a71bfd23a82c5561b160b88a7aa50c8461'
            '876788303553fe773e64917f76f0208f5e8adf7b91d4af24aa9d6a68a147d646'
            'e128576d72981e402ff106bb481108ab6d5ba941ab1b0f5f53e96a7831fc1d15'
            '672498957049fd301f9c9c1dc9fa49430e5e6d3c3f1f3cdce80df3af7d425d08'
            'e633ab4690db7d89d8a325bbdff73253cb4e3994c4cc5daa0c81205576bc1d09'
            'de7f4a890cf994e1c283251ac5ac6b0aedb29104d708e5e7a77702ac2055dec7'
            '61ec59007f66ac5bacc0aa095d1f2ccbc977a687038e161a463d1727223d5a90'
            '62a3655ea88b3dfff5243666a4e90d3f0eef6370a7889affb849e178ba4a82b0'
            'a60d145a8874b57a944c6775fdf500d03dd1ce73c24357b00d3de37b14620179')
