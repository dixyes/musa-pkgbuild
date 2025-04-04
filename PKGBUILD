# Maintainer: Yun Dou <dixyes@gmail.com>

pkgbase=musa
pkgname=(
    mtgpu-dkms
    musa-userspace-chunxiao
    musa-userspace-quyuan
)

pkgver=3.1.1
_mtgpu_version=2.7.1-20240822
pkgrel=1

pkgdesc='MooreThreads MUSA'
arch=('x86_64')
url='https://developer.mthreads.com/sdk/download/musa'
license=('custom')
options=('!strip')

makedepends=(
    'binutils'
)

_mtgpu_deb_name=MT_Linux_Driver_2.7.1/musa_2.7.1-rc3-0822_Ubuntu_amd64.deb
_mudnn_chunxiao_tar_name=muDNN_rc2.7.0/mudnn_rc2.7.0.tar.gz
_mudnn_quyuan_tar_name=
_mudnn_component_strip=2
_mudnn_prefix=./mudnn
_mccl_chunxiao_tar_name=
_mccl_quyuan_tar_name=
_mccl_component_strip=
_mccl_prefix=
_musa_toolkits_chunxiao_tar_name=MUSA_Toolkits_rc3.1.1/musa_toolkits_rc3.1.1.tar.gz
_musa_toolkits_quyuan_tar_name=
_musa_toolkits_component_strip=1
_musa_toolkits_prefix=musa_toolkits_3.1.1

source=(
    # download it
    # https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version=
    "musasdk_rc3.1.1.zip::https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version="
    "00-Define-PCI_IRQ_LEGACY-if-not-exist.diff::https://github.com/dixyes/mtgpu-drv/commit/bc3767406947b3cb3730d1e7e0205df83598e9a3.diff"
    "01-Use-fd_file-macro-if-exist.diff::https://github.com/dixyes/mtgpu-drv/commit/c6035a6a7d27daedbd51475c1e4c6bb10a75e512.diff"
    "02-Remove-the-second-arg-of-__assign_str.diff::https://github.com/dixyes/mtgpu-drv/commit/9f9211392670304c3d5542ce7c493eb15cbb9830.diff"
    "03-Add-missing-headers.diff::https://github.com/dixyes/mtgpu-drv/commit/391804d02e937c22a8a62f5b7a5028923f37aaff.diff"
    "04-Adopt-struct-platform_device-changes.diff::https://github.com/dixyes/mtgpu-drv/commit/c9a51a963efbefdba6db37e138b6aa6d25ca8c4f.diff"
    "05-Fake-as-ubuntu.diff::https://github.com/dixyes/mtgpu-drv/commit/fc624f25e429389565b9547a41b8e6302cb4ee82.diff"
    "06-Add-missing-include.diff::https://github.com/dixyes/mtgpu-drv/commit/bb98b5ad39b99727f54fc1608f10468ddfb1e1aa.diff"
    "07-Fix-missing-include.diff::https://github.com/dixyes/mtgpu-drv/commit/65c16d90a505d43e2019d9ce2ac0d9c0ab8583fa.diff"
    "08-Add-FOP_UNSIGNED_OFFSET-flag.diff::https://github.com/dixyes/mtgpu-drv/commit/944b836130aeb951d03c0b8bd275b9ba7629aa49.diff"
    "09-Add-lock-for-find_vma.diff::https://github.com/dixyes/mtgpu-drv/commit/099f7ea5afced34f9424618a9e2ec7aa37dba024.diff"
    "10-Use-string-for-6.13.diff::https://github.com/dixyes/mtgpu-drv/commit/cddb937187bfcbdee23cd6e7cb1b07126fc6b5a9.diff"
    "11-Adopt-6.13-drm-change.diff::https://github.com/dixyes/mtgpu-drv/commit/64f75ca9d261ea92f600f0041c12d78e1b134603.diff"
    "12-Adopt-6.13-__get_task_comm-change.diff::https://github.com/dixyes/mtgpu-drv/commit/cadbcd1f699e38b96379a4487a0e122c31b321d4.diff"
)
sha256sums=('6786019819c97e71839d729f32be3b5312e45149b52ba41bd9b28ac69eb0c650'
            '5ab28919c8743724aa0c86b5c4fc53b30ccc143f2da2d1f548e51b6e8c8edd2e'
            '3f8a7900006af9806618b80c6c8c995836b6c2553dd3f028d3bcd101bee720f1'
            '9caf0cd1059baa1b4647cf8463bada164e0d4496b2fc980fd8dbee37a00875ca'
            '3c499ab6c3c7bf3eb760bbb91ff9542b284efbbdcc106a3a067465b47ee667ab'
            '7ffee73da577c63fbe6c07ed8866f68aad59312b43046f5de80715bc981ceba9'
            '0123f0f2593ee49ca8e1e34fddb57ee64e898b92fb343e9db260f924db6a63dc'
            '94abaf20ecc03472b69d0a95c18f60cc750af8f84b3ce29ed72fedc6275e6949'
            'af8f7bc0010d957f6ea2d0f5de7f0d6ca605878c74246644482b2a71c00e798c'
            'bf01a728aa44f7df7116315be98908183a598cbb3715cd810b2806704e63812e'
            '46d2475f97072b9f709fe0b1b3cf9fc4b1a8dcfbe58ca5814b25ab80fd916110'
            '1c7833c0ca0a49a0e95c25f45ea8e9459f0b844a802323c7c3b12f4a81b99a22'
            '977288703836b165bc6400dc198db6cf96b122ff439f5126a4be3d586cf53408'
            '27e8c228407f1bc0b2115ab9e125cdfdcd0dbf1a6f754743cbd57e318bf39f6a')

prepare()
{
    # dkms sources and X11 things
    # NOTE: at 2.7.0 they are the same
    mkdir mtgpu
    pushd mtgpu
    ar x "../${_mtgpu_deb_name}"
    rm control.tar.* debian-binary
    tar -xf data.tar.*
    rm data.tar.*

    if [ -d "usr/src/mtgpu-1.0.0" ]; then
        mv "usr/src/mtgpu-1.0.0" "usr/src/mtgpu-${_mtgpu_version%%-*}"
    fi
    pushd "usr/src/mtgpu-${_mtgpu_version%%-*}"
    
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
    popd # "usr/src/mtgpu-${_mtgpu_version%%-*}"
    popd # mtgpu

    # install packages

    mkdir chunxiao quyuan

    # chunxiao(quyuan1) variant (S70 S80 S3000)

    if [[ -n "$_mudnn_chunxiao_tar_name" ]]; then
        tar -xf "$_mudnn_chunxiao_tar_name" -C chunxiao --strip-components "${_mudnn_component_strip}" \
            "${_mudnn_prefix}/bin" \
            "${_mudnn_prefix}/lib" \
            "${_mudnn_prefix}/include" \
            "${_mudnn_prefix}/docs"
    fi
    if [[ -n "$_mccl_chunxiao_tar_name" ]]; then
        tar -xf "$_mccl_chunxiao_tar_name" -C chunxiao --strip-components "${_mccl_component_strip}" \
            "${_mccl_prefix}/bin" \
            "${_mccl_prefix}/lib" \
            "${_mccl_prefix}/include" \
            "${_mccl_prefix}/cmake" \
            "${_mccl_prefix}/LICENSE.txt"
    fi
    if [[ -n "$_musa_toolkits_chunxiao_tar_name" ]]; then
        tar -xf "$_musa_toolkits_chunxiao_tar_name" -C chunxiao \
            --strip-components "${_musa_toolkits_component_strip}" \
            "${_musa_toolkits_prefix}" \
            --exclude="${_musa_toolkits_prefix}/install.sh" \
            --exclude="${_musa_toolkits_prefix}/version.json"
    fi

    # quyuan2 variant (S90 S4000)

    if [[ -n "$_mudnn_quyuan_tar_name" ]]; then
        tar -xf "$_mudnn_quyuan_tar_name" -C quyuan --strip-components "${_mudnn_component_strip}" \
            "${_mudnn_prefix}/bin" \
            "${_mudnn_prefix}/lib" \
            "${_mudnn_prefix}/include" \
            "${_mudnn_prefix}/docs"
    fi
    if [[ -n "$_mccl_quyuan_tar_name" ]]; then
        tar -xf "$_mccl_quyuan_tar_name" -C quyuan --strip-components "${_mccl_component_strip}" \
            "${_mccl_prefix}/bin" \
            "${_mccl_prefix}/lib" \
            "${_mccl_prefix}/include" \
            "${_mccl_prefix}/cmake" \
            "${_mccl_prefix}/LICENSE.txt"
    fi
    if [[ -n "$_musa_toolkits_quyuan_tar_name" ]]; then
        tar -xf "$_musa_toolkits_quyuan_tar_name" -C quyuan \
            --strip-components "${_musa_toolkits_component_strip}" \
            "${_musa_toolkits_prefix}" \
            --exclude="${_musa_toolkits_prefix}/install.sh" \
            --exclude="${_musa_toolkits_prefix}/version.json"
    fi
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
    install -d -m0755 "${pkgdir}/usr/src/mtgpu-${_mtgpu_version%%-*}"
    cp -dr --no-preserve='ownership' usr/src/mtgpu-${_mtgpu_version%%-*} "${pkgdir}/usr/src"

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
/usr/local/lib/weston
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
    pkgdesc="MooreThreads MUSA userspace libraries for chunxiao(quyuan1) (S70 S80 S3000) include OpenCL, Vulkan, Xorg drivers, muDNN, MCCL, MUSA Toolkits"
    depends=("openssl-1.1")
    optdepends=(
        "nettle7: for X support"
        "libweston-9.so: for Wayland support"
    )
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")
    _package_musa-userspace "chunxiao"
}

package_musa-userspace-quyuan()
{
    pkgdesc="MooreThreads MUSA userspace libraries for quyuan2 (S90 S4000) include OpenCL, Vulkan, Xorg drivers, muDNN, MCCL, MUSA Toolkits"
    depends=("openssl-1.1")
    optdepends=(
        "nettle7: for X support"
        "libweston-9.so: for Wayland support"
    )
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")
    _package_musa-userspace "quyuan"
}
