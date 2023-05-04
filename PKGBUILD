# Maintainer: Pratham Patel <thefirst1322@gmail.com>
pkgname=uboot-visionfive2
pkgver=v2023.07+rc1
pkgrel=1
pkgdesc='U-Boot, for the StarFive VisionFive 2 SBC'
arch=('riscv64')
url='https://source.denx.de/u-boot/u-boot'
license=('GPL-2.0')
groups=('firmware-visionfive2')
makedepends=('bc' 'bison' 'dtc' 'flex' 'openssl' 'python' 'python-setuptools' 'swig' 'opensbi-visionfive2')
options=('!lto')
source=("$pkgname::git+https://source.denx.de/u-boot/u-boot#tag=${pkgver//+/-}"
        "sftools::git+https://github.com/starfive-tech/Tools")
sha512sums=('SKIP'
            'SKIP')

export OPENSBI='/usr/share/opensbi/lp64/generic/firmware/fw_dynamic.bin'

prepare() {
    cd $pkgname

    echo "Cleaning any possible residue..."
    make clean

    echo "Preparing config..."
    sed -i 's@CONFIG_DEFAULT_FDT_FILE="starfive/jh7110-starfive-visionfive-2-v1.3b.dtb"@CONFIG_DEFAULT_FDT_FILE="starfive/jh7110-visionfive-v2.dtb"@' "$srcdir/$pkgname/configs/starfive_visionfive2_defconfig"
    make -C "$srcdir/$pkgname" starfive_visionfive2_defconfig
}

build() {
    echo "Building U-Boot..."
    make -C "$srcdir/$pkgname" -j$(nproc)

    echo "Fixing the SPL header..."
    make -C "$srcdir/sftools/spl_tool"
    "$srcdir/sftools/spl_tool/spl_tool" -c -f "$srcdir/$pkgname/spl/u-boot-spl.bin"
}

package() {
    install -D -p -m 0644 "$srcdir/$pkgname/spl/u-boot-spl.bin.normal.out" \
        "$pkgdir/opt/$pkgname/u-boot-spl.bin.normal.out"

    install -D -p -m 0644 "$srcdir/$pkgname/u-boot.itb" "$pkgdir/opt/$pkgname/u-boot.itb"
}
