# Maintainer: Yun Dou <dixyes@gmail.com>

pkgbase=musa
pkgname=(
    mtgpu-dkms
    musa-userspace-chunxiao
    musa-userspace-quyuan
)

pkgver=3.0.1
_mtgpu_version=2.7.0-20240717
pkgrel=1

pkgdesc='MooreThreads MUSA'
arch=('x86_64')
url='https://developer.mthreads.com/sdk/download/musa'
license=('custom')
options=('!strip')

makedepends=(
    'binutils'
)

_mtgpu_deb_name=MT_Linux_Driver_2.7.0/musa_2.7.0_rc0717_Ubuntu_amd64.deb
_mudnn_chunxiao_tar_name=muDNN_rc2.6.1/mudnn_rc2.6.1.tar.gz 
_mudnn_quyuan_tar_name=muDNN_rc2.6.1/mudnn_rc2.6.1.tar.gz 
_mudnn_component_strip=2
_mudnn_prefix=./mudnn
_mccl_chunxiao_tar_name=MCCL_rc1.6.1/mccl_rc1.6.1.tar.gz
_mccl_quyuan_tar_name=MCCL_rc1.6.1/mccl_rc1.6.1.tar.gz
_mccl_component_strip=2
_mccl_prefix=./mccl
_musa_toolkits_chunxiao_tar_name=MUSA_Toolkits_rc3.0.1/musa_toolkits_rc3.0.1.tar.gz
_musa_toolkits_quyuan_tar_name=MUSA_Toolkits_rc3.0.1/musa_toolkits_rc3.0.1.tar.gz
_musa_toolkits_component_strip=1
_musa_toolkits_prefix=musa_toolkits_3.0.1

source=(
    # download it
    # https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version=
    "MUSA+SDK-MUSA+SDK+rc3.0.1.zip::https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version="
    "00-Define-PCI_IRQ_LEGACY-if-not-exist.diff::https://github.com/dixyes/mtgpu-drv/commit/b2e9b27c563b424f66c619ea0f8593fa4a21e395.diff"
    "01-Use-fd_file-macro-if-exist.diff::https://github.com/dixyes/mtgpu-drv/commit/f6cc85171ee4dd0f04d6efca90d6e90f563012b9.diff"
    "02-Remove-the-second-arg-of-__assign_str.diff::https://github.com/dixyes/mtgpu-drv/commit/3d588b1fad860499486b3e8a7a18138e860eb032.diff"
    "03-Add-missing-headers.diff::https://github.com/dixyes/mtgpu-drv/commit/3a868f7a8c2ccb158dcc109c15f9a4ef9b75a7ce.diff"
    "04-Adopt-struct-platform_device-changes.diff::https://github.com/dixyes/mtgpu-drv/commit/f8da479013ee89bd4269150550482fb5c9630762.diff"
    "05-Fake-as-ubuntu.diff::https://github.com/dixyes/mtgpu-drv/commit/bbbffd4d935f07a217798f2db9f4153fedd73610.diff"
    "06-Add-missing-include.diff::https://github.com/dixyes/mtgpu-drv/commit/e9f91bbf42fb26c1715743f124f465bb7fc373bf.diff"
    "07-Fix-missing-include.diff::https://github.com/dixyes/mtgpu-drv/commit/9d721e42ee11449132b87ef38df07ab62babb360.diff"
    "08-Add-FOP_UNSIGNED_OFFSET-flag.diff::https://github.com/dixyes/mtgpu-drv/commit/98b1c323f939dc70f5923511587521f8cc0cfb7d.diff"
    "09-Add-lock-for-find_vma.diff::https://github.com/dixyes/mtgpu-drv/commit/03fc1a0fc1af50f51ec009d6106eaf7e2073e80e.diff"
)
sha256sums=('b6880b553cb7941fa4114cb8b9cf83b05cc3cd7e955956b9499522da67b2927a'
            '161d7eb17d48ee7ea52f7d85d51779088a2b7196489ef5f1d9f06f7a9543372f'
            '1431575cc67bd30d5b6dcc134954cb8f326203f80f4041362fcee5f1fd609275'
            '9caf0cd1059baa1b4647cf8463bada164e0d4496b2fc980fd8dbee37a00875ca'
            '9f4ef75f7d42be51a5e99b18d446a7c420f137972f01ec43223ac1da6d9c6904'
            '25e746bfa023d3d859c04c01b34c976213541e4de2bf34f32a8ad9f9ca20c215'
            '010aa14a0aa7bf3cb187702dda069904f7339d8ce25ac90c2d9f35a439f54981'
            '2a19e845ea881698e4265da12d79e0fa040bc56cee798f1af4341af3464f97f9'
            'af8f7bc0010d957f6ea2d0f5de7f0d6ca605878c74246644482b2a71c00e798c'
            '57df5a3422a9d06d603277447b800a22dd8d02bda170bb87fd70cf394bc21b4d'
            '32ee7687ada951838d18f3401866903d5eb6cdaaeb39f13d53f3fc2a72b421b8')

prepare()
{
    # dkms sources and x things
    # NOTE: at 2.7.0 they are the same
    mkdir mtgpu
    pushd mtgpu
    ar x "../${_mtgpu_deb_name}"
    rm control.tar.* debian-binary
    tar -xf data.tar.*
    rm data.tar.*

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
PACKAGE_VERSION=${_mtgpu_version}
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
    depends=("openssl-1.1")
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")
    _package_musa-userspace "chunxiao"
}

package_musa-userspace-quyuan()
{
    pkgdesc="MooreThreads MUSA userspace libraries for quyuan (S90 S4000) include OpenCL, Vulkan, Xorg drivers, muDNN, MCCL, MUSA Toolkits"
    depends=("openssl-1.1")
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")
    _package_musa-userspace "quyuan"
}
