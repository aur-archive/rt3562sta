# Maintainer: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Based on SUSE spec https://build.opensuse.org/package/files?package=rt3562sta&project=driver%3Awireless

pkgname=rt3562sta
pkgver=2.4.1.1
pkgrel=1
pkgdesc="Ralink RT3562 PCI WLAN adaptors Linux Driver with SUSE patches"
arch=(i686 x86_64)
url="http://www.ralinktech.com/support.php?s=2"
license=('GPL')
source=(
	http://dl.dropbox.com/u/362439/DPO_RT3562_3592_3062_LinuxSTA_V${pkgver}_20101217.tgz
	$pkgname-$pkgver-config.patch
	$pkgname-$pkgver-gcc-warnings-x86_64.patch
	$pkgname-$pkgver-WPA-mixed.patch
	$pkgname-$pkgver-convert-devicename-to-wlanX.patch
	$pkgname-$pkgver-remove-potential-conflicts-with-rt2860sta.patch
	$pkgname-$pkgver-return_nonvoid.patch
	$pkgname-$pkgver-reduce_debug_output.patch
	$pkgname-$pkgver-remove_date_time.patch
)

build() {
	cd "$srcdir/DPO_RT3562_3592_3062_LinuxSTA_V2.4.1.1_20101217"
	patch -p0 -i "$srcdir/$pkgname-$pkgver-config.patch"
	if [ "$CARCH" == "x86_64" ]; then
		patch -p0 -i "$srcdir/$pkgname-$pkgver-gcc-warnings-x86_64.patch"
	fi
	patch -p0 -i "$srcdir/$pkgname-$pkgver-WPA-mixed.patch"
	patch -p0 -i "$srcdir/$pkgname-$pkgver-convert-devicename-to-wlanX.patch"
	patch -p0 -i "$srcdir/$pkgname-$pkgver-remove-potential-conflicts-with-rt2860sta.patch"
	patch -p0 -i "$srcdir/$pkgname-$pkgver-return_nonvoid.patch"
	patch -p0 -i "$srcdir/$pkgname-$pkgver-reduce_debug_output.patch"
	patch -p0 -i "$srcdir/$pkgname-$pkgver-remove_date_time.patch"

	# clean up this mess of mixing RT2860STA with RT3562STA
	# in documentation files
	mv RT2860STA.dat RT3562STA.dat
	mv RT2860STACard.dat RT3562STACard.dat
	sed -i 's/2860/3562/g' *STA* iwpriv_usage.txt

	# as we change the default name of the interface from raX to wlanX, change respective references in documentation, too
	sed -i 's|ra0|wlan0|g' *.txt README* *.dat
	sed -i 's|ra1|wlan1|g' *.txt README* *.dat
	sed -i 's|ra2|wlan2|g' *.txt README* *.dat

	export EXTRA_CFLAGS="-DVERSION=$pkgver"
	make
}

package() {
	cd "$srcdir/DPO_RT3562_3592_3062_LinuxSTA_V2.4.1.1_20101217"
	install -Dm 0640 RT3562STA.dat "$pkgdir/etc/Wireless/RT3562STA/RT3562STA.dat"
	install -Dm 0644 os/linux/rt3562sta.ko "$pkgdir/lib/modules/$(uname -r)/kernel/drivers/net/wireless/rt3562sta.ko"
	install -dm 0755 "$pkgdir/usr/share/doc/$pkgname"
	install -m 0644 iwpriv_usage.txt README* RT3562STA* sta_ate_iwpriv_usage.txt "$pkgdir/usr/share/doc/$pkgname"
}

md5sums=('a5e9af70abccdb7b86d1ef605bb34491'
         '5cff38fb070fd181cf4beb52acc99616'
         'ca40e951c0799d395a74bf274459b950'
         '743b45ad2bf60bf757e53439c26edb71'
         'f2d0fbe845415cdb4f31e365876f740e'
         '874619383576e38c5c048f0a068c9844'
         '466df4e09896836082db417e52f0224e'
         'edc79dd0de1e8fabcec1e4791feb4391'
         'c8e174ea362bf94ce46038f8388439b6')
