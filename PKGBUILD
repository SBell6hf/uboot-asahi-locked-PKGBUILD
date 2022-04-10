# Maintainer: SBell6hf <SBell6hf@icloud.com>

pkgname=uboot-asahi-locked
pkgver=2022.04_rc4.20220319
pkgrel=1
pkgdesc='Locked U-Boot for Apple Silicon Macs'
_commit_id=89dbe1bf776ac909319247bd66f73c5d2cdac838
_srcname=u-boot-${_commit_id}
arch=('aarch64')
url='http://asahilinux.org'
license=('MIT' 'GPL2')
provides=(uboot-asahi)
conflicts=(uboot-asahi)
optdepends=('grub: lock grub')
makedepends=( bc imagemagick sed )
source=(
  "u-boot-${_commit_id}.tar.gz::https://github.com/AsahiLinux/u-boot/archive/${_commit_id}.tar.gz"
  "apple_m1_defconfig.patch"
  "include_configs_apple.h.patch"
  "45_lock"
  "40-grub-unrestrict-linux-simple-entry.hook"
  "90-asahi-boot-lock-update-grub.hook"
)
sha256sums=('d6b90132e7ededc6529f2516475218f76596900ace3082fcd027a0f01065553f'
  '5f7c0f11fcbfefa0a556fc039e48e4f80a2ca8371d4f5b1aa379bf9dcf08ce39'
  'a828f59da9a139f208ae5ea184759f1b6e6e59351b840e0d4bf2a343074bb3db'
  'da82b040151e848545a036980d422798a374870454b9b4dc25eb39cca4ba19e6'
  '3877c102269b3f1696472bb0432ef6c254fb23d6f74f15f6a9cbf4dbef55cca4'
  '8edd2c523f20271872c896287b993bb33a545a43dce6edeab76955ca7370d71f')
b2sums=('d9940e5196150fee0cd6fb61e4d4fab88d29df01511b6aa97b01c105581441e89c1ba7b7976d8cc096083c55480077c7732094ce36b0b3294d34ff8109d5400a'
  'ba89cedc3798aea72c94dbd0f6197a176132176198cf2773550aae8ea69e97046ebba8dfde62b2537d6fb52512fe5e0b31c5ccc9b166809fc0fbdc62fd09f9ff'
  '42f39f0f70330f7a97a0a52c1236569d840b53a6eb0902d976267b4961547889cc0b4337e99e4eb011e40216efb91ec3c3dc37b2680294b1ffd4c4df5f042dd0'
  '68a4bec3f3c5a69794b2746f748c3bf0535efefd5511dab6ff48992cd7790c9a0ecd68c1c0fa9928e7267b615ee6419d424645f3ca77bbaf19d84436b607a860'
  '790c2997660c5a459f5a0d8e9da596af7724dcd27eb4d28ff72d6f780ad3a106712c51f1a37572f395fe43c7b7470ae9944971e68a7269cbda5a2df98019e871'
  'd8559153d03a015aad78d072df9e6fb4d19f9f3471ea4fa755628df640338a075c95e3650199e84c6fa15436d7aad1f15698685f363d3fa7294224274ba45859')

prepare() {
  patch "${srcdir}/$_srcname/configs/apple_m1_defconfig" <(sed "s/__RAND_HERE__/$(tr -dc 'a-zA-Z0-9' </dev/random | head -c 100)/g" apple_m1_defconfig.patch)
  patch "${srcdir}/$_srcname/include/configs/apple.h" include_configs_apple.h.patch
  cd "${srcdir}/$_srcname"
  make apple_m1_defconfig
}

build() {
  cd "${srcdir}/$_srcname"
  make -j "$(nproc)"
}

package() {
  cd "${srcdir}"

  tgtdir="$pkgdir/usr/lib/asahi-boot"

  install -Dt "$tgtdir" -m644 "$_srcname/u-boot-nodtb.bin"
  install -Dt "$tgtdir/dtb" -m644 "$_srcname/arch/arm/dts/"t[86]*.dtb

  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 90-asahi-boot-lock-update-grub.hook
  install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 40-grub-unrestrict-linux-simple-entry.hook
  install -Dt "$pkgdir/etc/grub.d" -m755 45_lock
}
