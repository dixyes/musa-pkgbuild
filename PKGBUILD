# Maintainer: Yun Dou <dixyes@gmail.com>

pkgbase=musa
pkgname=(
    mtgpu-dkms
    musa-userspace-chunxiao
    #musa-userspace-quyuan
)

pkgver=2.5.1
pkgrel=1

pkgdesc='MooreThreads MUSA'
arch=('aarch64')
url='https://developer.mthreads.com/sdk/download/musa'
license=('custom')
options=('!strip')

makedepends=(
    'binutils'
)

_mtgpu_rpm_name=Driver/musa_2.5.0-server-Kylin_arm64.rpm
_mudnn_chunxiao_tar_name=muDNN/mudnn_rtm2.3.1_Kunpeng_Kylin.tar.gz
# _mudnn_quyuan_tar_name=muDNN_rc2.4.0/mudnn_rc2.4.0-qy2.tar.gz # shitty naming
_mudnn_component_strip=1
_mudnn_prefix=mudnn
# _mccl_chunxiao_tar_name=MCCL_rc1.4.0/mccl_rc1.4.0-chunxiao.tar.gz
# _mccl_quyuan_tar_name=MCCL_rc1.4.0/mccl_rc1.4.0-quyuan.tar.gz
# _mccl_component_strip=2
# _mccl_prefix=./mccl
_musa_toolkits_chunxiao_tar_name=MUSA\ Toolkits/musa_toolkits_rtm1.5.2_Kunpeng_Kylin.tar.gz
#_musa_toolkits_quyuan_tar_name=MUSA_Toolkits_rc2.0.0/musa_toolkits_rc2.0.0-quyuan.tar.gz
_musa_toolkits_component_strip=1
_musa_toolkits_prefix=musa_toolkits_1.5.2

if [ -n "${_mtgpu_rpm_name}" ]; then
    makedepends+=('rpmextract')
fi

source=(
    # download it
    # # https://www.mthreads.com/pes/drivers/driver-info?productType=DESKTOP&productModel=DESKTOP_MTT_S80&osVersion=MTT_S80_Ubuntu
    # "musa_${pkgver}-Ubuntu_amd64.deb"::"invalid://musa_${pkgver}-Ubuntu_amd64.deb"
    # https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version=
    "MUSA+SDK-v1.5.2+Kunpeng920_+Kylin+V10+.zip::https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version="
    "mtgpu-drv::git+https://github.com/dixyes/mtgpu-drv.git#branch=2.5.0-6.8-img-rouge"
)
sha512sums=('1b4bb6a7e21c34f5231c9e7a56889eecf9be59dfdbd468b15facc0e87d58d18bd157ecb8c8e31beb5bf2433afe84025e940ce493d6262f539198ba12cd2c6ee2'
            'SKIP')

prepare()
{
    # dkms sources and x things
    # NOTE: at 2.6.0 they are the same
    mkdir -p mtgpu
    pushd mtgpu
    
    if [ -n "${_mtgpu_rpm_name}" ]; then
        rpmextract.sh "../${_mtgpu_rpm_name}"
    else        
        ar x "../${_mtgpu_deb_name}"
        rm control.tar.gz debian-binary
        tar -xf data.tar.gz
        rm data.tar.gz
    fi

    #mv usr/src/mtgpu-1.0.0 "usr/src/mtgpu-${pkgver}"
    cp -r ../mtgpu-drv "usr/src/mtgpu-${pkgver}"
    pushd "usr/src/mtgpu-${pkgver}"
    
    # for patch in "${srcdir}"/*.diff; do
    #     patch -p1 < "${patch}"
    # done
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

    mkdir -p chunxiao # quyuan

    # chunxiao(quyuan1) variant (S70 S80 S3000)

    tar -xf "$_mudnn_chunxiao_tar_name" -C chunxiao --strip-components "${_mudnn_component_strip}" \
        "${_mudnn_prefix}/lib" \
        "${_mudnn_prefix}/include" \
        "${_mudnn_prefix}/docs"
    # tar -xf "$_mccl_chunxiao_tar_name" -C chunxiao --strip-components "${_mccl_component_strip}" \
    #     "${_mccl_prefix}/bin" \
    #     "${_mccl_prefix}/lib" \
    #     "${_mccl_prefix}/include" \
    #     "${_mccl_prefix}/cmake" \
    #     "${_mccl_prefix}/LICENSE.txt"
    tar -xf "$_musa_toolkits_chunxiao_tar_name" -C chunxiao \
        --strip-components "${_musa_toolkits_component_strip}" \
        "${_musa_toolkits_prefix}" \
        --exclude="${_musa_toolkits_prefix}/install.sh" \
        --exclude="${_musa_toolkits_prefix}/version.json"

    # quyuan2 variant (S90 S4000)

    # tar -xf "$_mudnn_quyuan_tar_name" -C quyuan --strip-components "${_mudnn_component_strip}" \
    #     "${_mudnn_prefix}/bin" \
    #     "${_mudnn_prefix}/lib" \
    #     "${_mudnn_prefix}/include" \
    #     "${_mudnn_prefix}/docs"
    # tar -xf "$_mccl_quyuan_tar_name" -C quyuan --strip-components "${_mccl_component_strip}" \
    #     "${_mccl_prefix}/bin" \
    #     "${_mccl_prefix}/lib" \
    #     "${_mccl_prefix}/include" \
    #     "${_mccl_prefix}/cmake" \
    #     "${_mccl_prefix}/LICENSE.txt"
    # tar -xf "$_musa_toolkits_quyuan_tar_name" -C quyuan \
    #     --strip-components "${_musa_toolkits_component_strip}" \
    #     "${_musa_toolkits_prefix}" \
    #     --exclude="${_musa_toolkits_prefix}/install.sh" \
    #     --exclude="${_musa_toolkits_prefix}/version.json"
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
    cp -r --no-preserve='ownership' usr/lib/aarch64-linux-gnu/musa/* "${pkgdir}/usr/lib/musa"
    install -d -m0755 "${pkgdir}/usr/lib/dri"
    cp -r --no-preserve='ownership' usr/lib/aarch64-linux-gnu/dri/* "${pkgdir}/usr/lib/dri"
    install -d -m0755 "${pkgdir}/usr/lib/gbm"
    cp -r --no-preserve='ownership' usr/lib/aarch64-linux-gnu/gbm/* "${pkgdir}/usr/lib/gbm"
    install -d -m0755 "${pkgdir}/usr/lib/xorg/modules/drivers"
    cp -r --no-preserve='ownership' usr/lib/xorg/modules/drivers/* "${pkgdir}/usr/lib/xorg/modules/drivers"

    # ucm2
    install -d -m0755 "${pkgdir}/usr/share/alsa/ucm2"
    ln -sf MooreThreads/ucm2/MooreThreads "${pkgdir}/usr/share/alsa/ucm2/"

    popd # mtgpu

    cp -r --no-preserve='ownership' "${_variant}" "${pkgdir}/usr/local/musa"
}

package_musa-userspace-chunxiao()
{
    pkgdesc="MooreThreads MUSA userspace libraries for chunxiao (S70 S80 S3000) include OpenCL, Vulkan, Xorg drivers, muDNN, /*MCCL,*/ MUSA Toolkits"
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")
    _package_musa-userspace "chunxiao"
}

# package_musa-userspace-quyuan()
# {
#     pkgdesc="MooreThreads MUSA userspace libraries for quyuan (S90 S4000) include OpenCL, Vulkan, Xorg drivers, muDNN, MCCL, MUSA Toolkits"
#     provides=("musa-userspace")
#     conflicts=("musa-userspace")
#     replaces=("musa-userspace")
#     _package_musa-userspace "quyuan"
# }
