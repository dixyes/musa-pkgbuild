# Maintainer: Yun Dou <dixyes@gmail.com>

pkgbase=musa
pkgname=(
    mtgpu-dkms
    musa-userspace-chunxiao
    musa-userspace-quyuan
)

pkgver=2.7.0
pkgrel=1

pkgdesc='MooreThreads MUSA'
arch=('x86_64')
url='https://developer.mthreads.com/sdk/download/musa'
license=('custom')
options=('!strip')

makedepends=(
    'binutils'
)

_mtgpu_deb_name=MT_Linux_Driver_2.7.0/musa_2.7.0-rc1_Ubuntu_amd64.deb
_mudnn_chunxiao_tar_name=muDNN_rc2.5.0/mudnn_rc2.5.0-chunxiao.tar.gz
_mudnn_quyuan_tar_name=muDNN_rc2.5.0/mudnn_rc2.5.0-quyuan.tar.gz
_mudnn_component_strip=2
_mudnn_prefix=./mudnn
_mccl_chunxiao_tar_name=MCCL_rc1.4.0/mccl_rc1.4.0-chunxiao.tar.gz
_mccl_quyuan_tar_name=MCCL_rc1.4.0/mccl_rc1.4.0-quyuan.tar.gz
_mccl_component_strip=2
_mccl_prefix=./mccl
_musa_toolkits_chunxiao_tar_name=MUSA_Toolkits_rc2.1.0/musa_toolkits_rc2.1.0-chunxiao.tar.gz
_musa_toolkits_quyuan_tar_name=MUSA_Toolkits_rc2.1.0/musa_toolkits_rc2.1.0-quyuan.tar.gz
_musa_toolkits_component_strip=1
_musa_toolkits_prefix=musa_toolkits_2.0.0

source=(
    # download it
    # https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version=
    "MUSA+SDK-rc2.1.0_Intel_CPU_Ubuntu_chunxiao.zip::https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version="
    "MUSA+SDK-rc2.1.0_Intel_CPU_Ubuntu_quyuan.zip::https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version="
    "02-fake-ubuntu-${pkgver}-${pkgrel}.diff::https://github.com/dixyes/mtgpu-drv/commit/81e52ddbbcc587a800724d25584d8060a1228429.diff"
)
sha256sums=('de4a8c9a7aae6e70a814ec7b0c1419e78fd5a73d0ffdc0d0790b15f3c8d5dcc7'
            'e1a66aed4c30337757ef85d8c8067149027adb0678b11eccc69b3e993b74923e'
            '37ae2287018d52a8bdd6909ccfbee005a049ab90f8c8b4d8159473867be950d2')

prepare()
{
    # dkms sources and x things
    # NOTE: at 2.7.0 they are the same
    mkdir mtgpu
    pushd mtgpu
    ar x "../${_mtgpu_deb_name}"
    rm control.tar.xz debian-binary
    tar -xf data.tar.xz
    rm data.tar.xz

    mv usr/src/mtgpu-1.0.0 "usr/src/mtgpu-${pkgver}"
    pushd "usr/src/mtgpu-${pkgver}"
    
    for patch in "${srcdir}"/*.diff; do
        patch -p1 < "${patch}"
    done
    cat > dkms.conf << EOF
MAKE="make ARCH=x86_64 KERNELVER=\$kernelver"
CLEAN="make ARCH=x86_64 clean"
BUILT_MODULE_NAME[0]="mtgpu"
DEST_MODULE_LOCATION[0]="/extra"
PACKAGE_NAME=mtgpu
PACKAGE_VERSION=${pkgver}
AUTOINSTALL="yes"
EOF
    popd # "usr/src/mtgpu-${pkgver}"
    popd # mtgpu

    # install packages

    mkdir chunxiao quyuan

    # chunxiao(quyuan1) variant (S70 S80 S3000)

    tar -xf "$_mudnn_chunxiao_tar_name" -C chunxiao --strip-components "${_mudnn_component_strip}" \
        "${_mudnn_prefix}/bin" \
        "${_mudnn_prefix}/lib" \
        "${_mudnn_prefix}/include" \
        "${_mudnn_prefix}/docs"
    tar -xf "$_mccl_chunxiao_tar_name" -C chunxiao --strip-components "${_mccl_component_strip}" \
        "${_mccl_prefix}/bin" \
        "${_mccl_prefix}/lib" \
        "${_mccl_prefix}/include" \
        "${_mccl_prefix}/cmake" \
        "${_mccl_prefix}/LICENSE.txt"
    tar -xf "$_musa_toolkits_chunxiao_tar_name" -C chunxiao \
        --strip-components "${_musa_toolkits_component_strip}" \
        "${_musa_toolkits_prefix}" \
        --exclude="${_musa_toolkits_prefix}/install.sh" \
        --exclude="${_musa_toolkits_prefix}/version.json"

    # quyuan2 variant (S90 S4000)

    tar -xf "$_mudnn_quyuan_tar_name" -C quyuan --strip-components "${_mudnn_component_strip}" \
        "${_mudnn_prefix}/bin" \
        "${_mudnn_prefix}/lib" \
        "${_mudnn_prefix}/include" \
        "${_mudnn_prefix}/docs"
    tar -xf "$_mccl_quyuan_tar_name" -C quyuan --strip-components "${_mccl_component_strip}" \
        "${_mccl_prefix}/bin" \
        "${_mccl_prefix}/lib" \
        "${_mccl_prefix}/include" \
        "${_mccl_prefix}/cmake" \
        "${_mccl_prefix}/LICENSE.txt"
    tar -xf "$_musa_toolkits_quyuan_tar_name" -C quyuan \
        --strip-components "${_musa_toolkits_component_strip}" \
        "${_musa_toolkits_prefix}" \
        --exclude="${_musa_toolkits_prefix}/install.sh" \
        --exclude="${_musa_toolkits_prefix}/version.json"
}

package_mtgpu-dkms()
{
    pkgdesc="MooreThreads mtgpu dkms driver"
    depends=("dkms")

    pushd mtgpu

    # modprobe config
    install -D -m0644 etc/modprobe.d/mtgpu.conf "${pkgdir}/etc/modprobe.d/mtgpu.conf"

    # firmwares
    install -d -m0755 "${pkgdir}/usr/lib/firmware/mthreads"
    cp -r --no-preserve='ownership' lib "${pkgdir}/usr/"

    # dkms sources
    install -d -m0755 "${pkgdir}/usr/src/mtgpu-${pkgver}"
    cp -dr --no-preserve='ownership' usr/src/mtgpu-${pkgver} "${pkgdir}/usr/src"

    popd # mtgpu
}

_package_musa-userspace()
{
    local _variant="$1"

    pushd mtgpu

    # ldconfig
    install -d -m0755 "${pkgdir}/etc/ld.so.conf.d"
    cat > "${pkgdir}/etc/ld.so.conf.d/musa.conf" <<EOF
/usr/lib/musa
/usr/local/musa/lib
EOF

    # config files
    install -D -m0644 etc/OpenCL/vendors/MT.icd "${pkgdir}/etc/OpenCL/vendors/MT.icd"
    install -D -m0644 etc/vulkan/icd.d/musaicdconf.json "${pkgdir}/etc/vulkan/icd.d/musaicdconf.json"

    # install -D -m0644 etc/musa_config "${pkgdir}/etc/musa_config"
    install -D -m0644 etc/musa.ini "${pkgdir}/etc/musa.ini"

    # binaries and libraries
    install -d -m0755 "${pkgdir}/usr"
    cp -r --no-preserve='ownership' usr/bin usr/local usr/share "${pkgdir}/usr/"
    install -d -m0755 "${pkgdir}/usr/lib/musa"
    cp -r --no-preserve='ownership' usr/lib/x86_64-linux-gnu/musa/* "${pkgdir}/usr/lib/musa"
    install -d -m0755 "${pkgdir}/usr/lib/dri"
    cp -r --no-preserve='ownership' usr/lib/x86_64-linux-gnu/dri/* "${pkgdir}/usr/lib/dri"
    install -d -m0755 "${pkgdir}/usr/lib/gbm"
    cp -r --no-preserve='ownership' usr/lib/x86_64-linux-gnu/gbm/* "${pkgdir}/usr/lib/gbm"

    # ucm2
    install -d -m0755 "${pkgdir}/usr/share/alsa/ucm2"
    ln -sf MooreThreads/ucm2/MooreThreads "${pkgdir}/usr/share/alsa/ucm2/"

    popd # mtgpu

    cp -r --no-preserve='ownership' "${_variant}" "${pkgdir}/usr/local/musa"
}

package_musa-userspace-chunxiao()
{
    pkgdesc="MooreThreads MUSA userspace libraries for chunxiao (S70 S80 S3000) include OpenCL, Vulkan, Xorg drivers, muDNN, MCCL, MUSA Toolkits"
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")
    _package_musa-userspace "chunxiao"
}

package_musa-userspace-quyuan()
{
    pkgdesc="MooreThreads MUSA userspace libraries for quyuan (S90 S4000) include OpenCL, Vulkan, Xorg drivers, muDNN, MCCL, MUSA Toolkits"
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")
    _package_musa-userspace "quyuan"
}
