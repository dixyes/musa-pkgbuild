# Maintainer: Yun Dou <dixyes@gmail.com>

pkgbase=musa
pkgname=(
    mtgpu-dkms
    musa-userspace
)

pkgver=4.0.1
_mtgpu_version=3.0.0
pkgrel=1

pkgdesc='MooreThreads MUSA'
arch=('x86_64')
url='https://developer.mthreads.com/sdk/download/musa'
license=('custom')
options=('!strip')

makedepends=(
    'binutils'
)

# 早期不区分
# 2.1.0 - rc3.1.1期间userspace区分quyuan和chunxiao
# 4.0.1不区分
_mtgpu_deb_name=MT_Linux_Driver_3.0.0/musa_3.0.0_amd64.deb
_mtgpu_dkms_dirname=mtgpu-3.0.0
_mudnn_tar_name=muDNN_2.8.1/mudnn_2.8.1.tar.gz
_mudnn_component_strip=2
_mudnn_prefix=./mudnn
_mtml_deb_name=MTML_2.0.0/ee1ce2e41_mtml_2.0.0-linux-R_amd64.deb
_musa_toolkits_tar_name=MUSA_Toolkits_4.0.1/musa_toolkits_4.0.1.tar.gz
_musa_toolkits_component_strip=1
_musa_toolkits_prefix=musa_toolkits_4.0.0 # 打包仙人了属于是

source=(
    # download it
    # https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version=
    "musasdk_v4.0.1_Intel_Ubuntu.zip::https://developer.mthreads.com/sdk/download/musa?equipment=&os=&driverVersion=&version="
    "00-Donot-define-os_follow_pfn-after-6.10.diff::https://github.com/dixyes/mtgpu-drv/commit/ff3466ccb9a80210234b40ebcf24c77af5675e58.diff"
    "01-Define-PCI_IRQ_LEGACY-if-not-exist.diff::https://github.com/dixyes/mtgpu-drv/commit/ab1d7dd2f766f1bf1d809907b458567c08b778cd.diff"
    "02-Use-fd_file-macro-if-exist.diff::https://github.com/dixyes/mtgpu-drv/commit/e52ca0202af67e5da5976f0dd9f5d44e6a7d3ab1.diff"
    "03-Remove-the-second-arg-of-__assign_str.diff::https://github.com/dixyes/mtgpu-drv/commit/0a30454a16830490f320d5ac2241a227cd4bf204.diff"
    "04-Add-missing-headers.diff::https://github.com/dixyes/mtgpu-drv/commit/833d80762b737c2d3dabbcc7ef0bf870a8f88995.diff"
    "05-Adopt-struct-platform_device-changes.diff::https://github.com/dixyes/mtgpu-drv/commit/14afae3e1a0f5a77cd6e250f7b70d1b861871b97.diff"
    "06-Fake-as-ubuntu.diff::https://github.com/dixyes/mtgpu-drv/commit/1509f9b50402a9532733de2b4ea9c138f5e15670.diff"
    "07-Add-FOP_UNSIGNED_OFFSET-flag.diff::https://github.com/dixyes/mtgpu-drv/commit/983a895a8af9577dd04e31758276c3ad1ec101c1.diff"
    "08-Add-lock-for-find_vma.diff::https://github.com/dixyes/mtgpu-drv/commit/2847ec7a64aad013491e4d20079b49d6f1af0b9f.diff"
    "09-Use-string-for-6.13.diff::https://github.com/dixyes/mtgpu-drv/commit/b1914a8c58590b8b29e556db01b8b4553ad19cc0.diff"
    "10-Adopt-6.14-drm-change.diff::https://github.com/dixyes/mtgpu-drv/commit/a18b13cabed1429ddac1e88143589c6edb8f6328.diff"
    "11-Adopt-6.13-__get_task_comm-change.diff::https://github.com/dixyes/mtgpu-drv/commit/7842afc22e88ff9b44a182e05acb22c0cdd172e4.diff"
    "12-Fix-6.14-function-signatures.diff::https://github.com/dixyes/mtgpu-drv/commit/472b0f3ba2650de739a78642e8654dfcabda5587.diff"
    "gcc-14-musa-header.diff"
)
sha256sums=('6c91eab385d5d439b60d82f03b0fa30a94d4fa53763399d8eec95674d4172547'
            '7c9270e0960b01de0e76a736e57b52f33c89a0fdee5ac5f086d0a4b8f657e824'
            '5011ba3e7920aba535ec82a686771fdab9faf3b0688614f3663f402382f77165'
            '717c255e716f30a35b7e53c274c7cbaf856ed8d98eae4f71fda43811d9e5503e'
            '9caf0cd1059baa1b4647cf8463bada164e0d4496b2fc980fd8dbee37a00875ca'
            'b6dde236fe3adaed6d21a12c20fd2126c6e1caed525d693bd4b8bd8b34d89564'
            'f34833b34cb682652b416b0e1af74fa2f2c19c430a8e05eb88a95d58b4aeface'
            '2aca50228ece16469613887e507db4b4f025f5cf75805ea92af414d9ddb3c35c'
            '9ee1fabfae2817307d5a4f8d94cf2a399ba51fc2a778bc6c61af918edf4e53e9'
            'f8e139d458594e3650da0a97b47bfeffc1bcb5a6d06b7c2ae92b6f065ee28441'
            'fb712890c10a96c44015614a0503035f89544807cfde930ba8d120aa3d9570cd'
            '42383e577c8e431da97ce5d038b06c8a7b68c369b781812877b1d4210ab63287'
            '1a0b6f3c119f5eeec149fe405d04317bb95aa5700300eeb974b686dabb7d0790'
            'c884b95a48cbef5d84fa257c0a3b8f85f611ee1d30b7366d1074987547701f10'
            'e32accdf1716bfb377f396f6c93bc9b5e9dea88ae31fd68180f31998e28c4fc6')

prepare()
{
    # dkms sources and X11 things
    mkdir mtgpu
    pushd mtgpu
    {
        ar x "../${_mtgpu_deb_name}"
        rm control.tar.* debian-binary
        tar -xf data.tar.*
        rm data.tar.*

        if [ "${_mtgpu_dkms_dirname}" != "mtgpu-${_mtgpu_version%%-*}" ]; then
            mv "usr/src/${_mtgpu_dkms_dirname}" "usr/src/mtgpu-${_mtgpu_version%%-*}"
        fi
        pushd "usr/src/mtgpu-${_mtgpu_version%%-*}"
        {
        
            for patch in "${srcdir}"/*.diff; do
                if [[ "${patch##*/}" = "gcc-14-musa-header.diff" ]]; then
                    # Skip this patch
                    continue
                fi
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
        }
        popd # "usr/src/mtgpu-${_mtgpu_version%%-*}"
    }
    popd # mtgpu

    # mtml
    mkdir mtml
    pushd mtml
    {
        ar x "../${_mtml_deb_name}"
        rm control.tar.* debian-binary
        tar -xf data.tar.*
        rm data.tar.*
    }
    popd # mtml

    mkdir mudnn musa_toolkits

    if [[ -n "$_mudnn_tar_name" ]]; then
        tar -xf "$_mudnn_tar_name" -C mudnn --strip-components "${_mudnn_component_strip}" \
            "${_mudnn_prefix}/bin" \
            "${_mudnn_prefix}/lib" \
            "${_mudnn_prefix}/include" \
            "${_mudnn_prefix}/docs"
    fi
    if [[ -n "$_musa_toolkits_tar_name" ]]; then
        tar -xf "$_musa_toolkits_tar_name" -C musa_toolkits \
            --strip-components "${_musa_toolkits_component_strip}" \
            "${_musa_toolkits_prefix}" \
            --exclude="${_musa_toolkits_prefix}/install.sh" \
            --exclude="${_musa_toolkits_prefix}/version.json"
        
        pushd musa_toolkits
        {
            patch -p1 < "${srcdir}/gcc-14-musa-header.diff"
        }
        popd # musa_toolkits
    fi
}

package_mtgpu-dkms()
{
    pkgdesc="MooreThreads mtgpu dkms driver"
    depends=("dkms")

    pushd mtgpu
    {
        # modprobe config
        install -D -m0644 etc/modprobe.d/mtgpu.conf "${pkgdir}/etc/modprobe.d/mtgpu.conf"

        # firmwares
        install -d -m0755 "${pkgdir}/usr/lib/firmware/mthreads"
        cp -r --no-preserve='ownership' lib "${pkgdir}/usr/"

        # dkms sources
        install -d -m0755 "${pkgdir}/usr/src/mtgpu-${_mtgpu_version%%-*}"
        cp -dr --no-preserve='ownership' usr/src/mtgpu-${_mtgpu_version%%-*} "${pkgdir}/usr/src"
    }
    popd # mtgpu
}

package_musa-userspace()
{
    pkgdesc="MooreThreads MUSA userspace libraries include OpenCL, Vulkan, Xorg drivers, muDNN, MTML, MUSA Toolkits"
    depends=("openssl-1.1")
    optdepends=(
        "nettle7: for X support"
        "libweston-9.so: for Wayland support"
    )
    provides=("musa-userspace")
    conflicts=("musa-userspace")
    replaces=("musa-userspace")

    pushd mtgpu
    {
        # ldconfig
        install -d -m0755 "${pkgdir}/etc/ld.so.conf.d"
        cat > "${pkgdir}/etc/ld.so.conf.d/musa.conf" <<EOF
/usr/lib/musa
/usr/local/musa/lib
/usr/local/lib/weston
EOF

        # config files
        # install -D -m0644 etc/OpenCL/vendors/MT.icd "${pkgdir}/etc/OpenCL/vendors/MT.icd"
        install -D -m0644 etc/vulkan/icd.d/musaicdconf.json "${pkgdir}/etc/vulkan/icd.d/musaicdconf.json"

        # install -D -m0644 etc/musa_config "${pkgdir}/etc/musa_config"
        install -D -m0644 etc/musa.ini "${pkgdir}/etc/musa.ini"

        # binaries and libraries
        install -d -m0755 "${pkgdir}/usr"
        cp -r --no-preserve='ownership' usr/bin usr/local usr/share "${pkgdir}/usr/"
        # install -d -m0755 "${pkgdir}/usr/lib/musa"
        # cp -r --no-preserve='ownership' usr/lib/x86_64-linux-gnu/musa/* "${pkgdir}/usr/lib/musa"
        install -d -m0755 "${pkgdir}/usr/lib/xorg"
        cp -r --no-preserve='ownership' usr/lib/xorg/* "${pkgdir}/usr/lib/xorg"
        install -d -m0755 "${pkgdir}/usr/lib/dri"
        cp -r --no-preserve='ownership' usr/lib/x86_64-linux-gnu/dri/* "${pkgdir}/usr/lib/dri"
        install -d -m0755 "${pkgdir}/usr/lib/gbm"
        cp -r --no-preserve='ownership' usr/lib/x86_64-linux-gnu/gbm/* "${pkgdir}/usr/lib/gbm"

        # ucm2
        install -d -m0755 "${pkgdir}/usr/share/alsa/ucm2"
        ln -sf MooreThreads/ucm2/MooreThreads "${pkgdir}/usr/share/alsa/ucm2/"
    }
    popd # mtgpu

    cp -r --no-preserve='ownership' mtml/usr/* "${pkgdir}/usr/local/musa/lib"
    cp -r --no-preserve='ownership' mudnn/* "${pkgdir}/usr/local/musa"
    cp -r --no-preserve='ownership' musa_toolkits/* "${pkgdir}/usr/local/musa"
}
