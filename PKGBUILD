# Maintainer: Levi Zim (kxxt) <rsworktech@outlook.com>
# Contributor: Daniel Bermond <dbermond@archlinux.org>
# Contributor: Mikalai Ramanovich < narod.ru: nikolay.romanovich >
pkgname=onlyoffice
pkgver=8.1.1
pkgrel=2
pkgdesc="An office suite that combines text, spreadsheet and presentation editors allowing to create, view and edit local documents "
arch=(x86_64)
url="https://www.onlyoffice.com/desktop.aspx"
license=('AGPL-3.0-only')
depends=(
    'curl' 'gtk3' 'alsa-lib' 'libpulse' 'gstreamer' 'gst-plugins-base-libs'
    'gst-plugins-ugly' 'libxss' 'nss' 'nspr' 'ttf-dejavu' 'ttf-liberation'
    'ttf-carlito' 'desktop-file-utils' 'hicolor-icon-theme'    
    'qt5-base' 'qt5-multimedia' 'qt5-x11extras' 'qt5-svg'
)
makedepends=(
    git python
    # build_tools/tools/linux/deps.py
    nodejs-lts-hydrogen
    npm
    yarn
    grunt-cli
    cmake
    p7zip
    # v8
    ninja
    # grunt
    jdk11-openjdk
)
optdepends=('libreoffice: for OpenSymbol fonts'
            'otf-takao: for japanese Takao fonts'
            'ttf-ms-fonts: for Microsoft fonts')
conflicts=(onlyoffice-bin onlyoffice-git)
options=(
    '!emptydirs'
    '!lto'
)
_url=https://github.com/ONLYOFFICE
_tag=v$pkgver.39
source=(
    # Source
    "git+${_url}/DesktopEditors#tag=v$pkgver"
    "$pkgname-core::git+${_url}/core#tag=$_tag"
    "$pkgname-desktop-apps::git+${_url}/desktop-apps#tag=$_tag"
    "$pkgname-desktop-sdk::git+${_url}/desktop-sdk#tag=$_tag"
    "$pkgname-dictionaries::git+${_url}/dictionaries#tag=$_tag"
    "$pkgname-sdkjs::git+${_url}/sdkjs#tag=$_tag"
    "$pkgname-sdkjs-forms::git+${_url}/sdkjs-forms#tag=$_tag"
    "$pkgname-web-apps::git+${_url}/web-apps#tag=$_tag"
    "$pkgname-build_tools::git+${_url}/build_tools#tag=$_tag"
    "$pkgname-core-fonts::git+${_url}/core-fonts#tag=$_tag"
    "$pkgname-document-templates::git+${_url}/document-templates#tag=$_tag"
    "onlyoffice.github.io::git+${_url}/onlyoffice.github.io"
    # V8
    "git+https://chromium.googlesource.com/chromium/tools/depot_tools.git#commit=8dde9800ee2b8326ab11a87abd67d3bd9f8c8773"
    "git+https://github.com/v8/v8#branch=9.0-lkgr"
    # Patches
    "v8-89-fix-cstdint.diff"
    "0001-Add-update-only-to-avoid-download-and-build-at-once.patch"
    "0002-Add-no-third-party-update-and-update-third-party-onl.patch"
    "0003-use-QT_VERSION-env-instead-of-guessing.patch"
    "0004-Only-build-tar.patch"
    "0001-Fix-boost-module-import.patch"
    "use-fpermissive.diff"
    "fix-glib-qt-macro-collision.diff"
)
sha256sums=('a1c9b834c350d6c8b779f244b5791e08197370702d13ac564fd49ef8e867087a'
            '09d692f5bcb726e3e69d389897989432f45a5fc8652ea03e95213d493e0756fa'
            'fcd02e10d6dabfd5f9f3e4849aa257e4f4ab9eeb8468327c8dc6d3ae525faba3'
            '9055ece3c0afa107aa9ad1bca5743a658e87a12b591a0158e02504c21a161b4b'
            '1e44d0d8f26f41730b608401cff6a3c86c94a605db94ad624fb4ef194f7891cd'
            '22ee5275a8769596794349860c196d37d785a843ec89e51aa7f6da7b5837b2be'
            '72772ef99d783f5cf002afaaad5655d455ff1a0a764464ce7b81a93b6a9a022f'
            '6faacd37832a87693efc16c47325818cbd379bdbc5e866c3078426af9b13e3dc'
            '5e5cfac259828e5f3043ca3209761ec35bfeba192d478fef74909dccd1315718'
            '3724102cfeeb1fde8f86ae767b14eaa672ca43ec472d87321c84cc1ce18189d1'
            '87af534eac1847f38b09025bce75e491292069a6ef987937d9c22e8b0679df4a'
            'SKIP'
            'cd7a982bf79eae86a8b7727193e2a9feccd1388cd0cc474b8d786ac6dc695cfe'
            'SKIP'
            '36e4855625bf3eaab293208478492393fb4e8f0f1076fac06df56ca828319ffc'
            'a4f2502acfdc48d3daad5ed166c1cd15cb0595a4d5018f22d9390f73f25dd8c6'
            'e9d56d030039ad72e89dc48877f7fccaeac4cfa5ff584833849be2601fca5fb7'
            'bdcf095fd46fb47f2992510078c46cef2b0084000ff4a0c4f956efb0db7e4d57'
            '5a10b6e1b71e79f73344d3f2c7bc1611940986e331d0b41bd294bf5c84328633'
            '005951c1b8a281148a3a7e79261c728c4d80ff2c580e278e823a84c8751916f9'
            '222dab12468f27b2bc1cc098ad2e4ca5bff8df845939f5cba2efff2165eafbcd'
            'bd655f04eb044f19da779911eb7ad6fd13c899a659720fbcf08f38194365669c')

prepare() {
    for _module in core desktop-{apps,sdk} dictionaries sdkjs{,-forms} \
        web-apps build_tools document-templates core-fonts
    do
        mv "$pkgname-${_module}" "${_module}"
    done
    cd "$srcdir/build_tools"/tools/linux

    # Pretend that we have all dependencies installed
    touch packages_complete

    # Use system qt
    # ref: build_tools/tools/linux/use_system_qt.py
    mkdir -p system_qt/gcc_64
    ln -s /usr/bin            ./system_qt/gcc_64/bin
    ln -s /usr/lib            ./system_qt/gcc_64/lib
    ln -s /usr/lib/qt/plugins ./system_qt/gcc_64/plugins

    # Check build_tools/tools/linux/automate.py for options to configure.py
    cd "$srcdir/build_tools"
    patch -Np1 -i ../0001-Fix-boost-module-import.patch
    patch -Np1 -i ../0001-Add-update-only-to-avoid-download-and-build-at-once.patch
    patch -Np1 -i ../0002-Add-no-third-party-update-and-update-third-party-onl.patch
    # Fix the way to get qt version
    patch -Np1 -i ../0003-use-QT_VERSION-env-instead-of-guessing.patch
    # Don't build debs/rpm/..
    patch -Np1 -i ../0004-Only-build-tar.patch

    # We manually update the sources, so --update 0
    ./configure.py --module desktop --update 0 --branch "tags/$_tag" --qt-dir "$(realpath tools/linux/system_qt)"
    ./make.py --update-only

    # fetch V8 before updating third party to have a chance to patch it
    cd "$srcdir/"
    mv depot_tools core/Common/3dParty/v8_89/
    cd core/Common/3dParty/v8_89/
    cat >.gclient <<EOF
solutions = [
  {
    "name": "v8",
    "url": "file://${srcdir}/v8@makepkg",
    "deps_file": "DEPS",
    "managed": False,
    "custom_deps": {},
    "custom_vars": {},
  },
]
EOF
    export PATH="$(pwd)/depot_tools:$PATH" DEPOT_TOOLS_UPDATE=0
    # ref: build_tools/scripts/core_common/modules/v8_89.py
    gclient sync --force
    # V8 use system libstdc++
    ln -sf /usr/lib/libstdc++.so.6 \
        v8/third_party/llvm-build/Release+Asserts/lib/libstdc++.so.6
    patch -Np1 -d v8 < "$srcdir/v8-89-fix-cstdint.diff"

    cd "$srcdir"
    # Fix some compile error
    patch -Np1 -d core < use-fpermissive.diff
    patch -Np1 -d desktop-apps < fix-glib-qt-macro-collision.diff
    # patch -Np1 -d desktop-apps < package-add-dir-target.diff
    # pragma (end)?region breaks if-else-if. Is this a gcc bug?
    sed -Ei '/pragma (end)?region/d' core/MsBinaryFile/XlsFile/Format/Logic/Biff_structures/StringPtgParser.cpp 
    # Fix missing unistd.h
    # TODO: use system zlib
    sed -i '1s/^/#include<unistd.h>\n/' core/OfficeUtils/src/zlib-1.2.11/gzwrite.c
    sed -i '1s/^/#include<unistd.h>\n/' core/OfficeUtils/src/zlib-1.2.11/gzread.c
    sed -i '1s/^/#include<unistd.h>\n/' core/OfficeUtils/src/zlib-1.2.11/gzlib.c
    sed -i '1s/^/#include<cmath>\n/' desktop-sdk/ChromiumBasedEditors/videoplayerlib/src/qtimelabel.cpp
    sed -i '1s/^/#include<cmath>\n/' desktop-sdk/ChromiumBasedEditors/videoplayerlib/src/qvideoslider.cpp
    sed -i '1s/^/#include<cmath>\n/' desktop-sdk/ChromiumBasedEditors/videoplayerlib/src/qvideoplaylist.cpp
    sed -i '1s/^/#include<QPainterPath>\n/' desktop-apps/win-linux/src/windows/platform_linux/cwindowplatform.cpp

    cd "$srcdir/build_tools"
    unset CFLAGS
    unset CXXFLAGS
    ./make.py --update-third-party-only
}

build() {
	cd build_tools
    local qt_ver="$(pacman -Q qt5-base | awk '{print $2}')"
    export QT_VERSION="${qt_ver//+*}"

    export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
    export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
    # -O2 causes a linker segfault during LTO
    export CFLAGS="${CFLAGS/-O2/}"
    export CXXFLAGS="${CXXFLAGS/-O2/}"
    # TODO: figure out which flag is causing linker error
    unset CFLAGS
    unset CXXFLAGS

    ./make.py --no-third-party-update
}

package() {
    cd build_tools
    ./make_package.py -P linux_x86_64 -T desktop -V "$pkgver" -B "$pkgrel"
	cd ../desktop-apps/win-linux/package/linux/common

    install -d "$pkgdir"/opt/onlyoffice
    cp -r usr "$pkgdir"
    chmod +x "$pkgdir"/usr/bin/*
    cp -r opt/desktopeditors "$pkgdir"/opt/onlyoffice
    # Remove template files
    find  $pkgdir -name '*.m4' -type f -exec rm {} \;

    local _file
    local _res
    while read -r -d '' _file
    do
        _res="$(sed 's/\.png$//;s/^.*-//' <<< "$_file")"
        install -d -m755 "${pkgdir}/usr/share/icons/hicolor/${_res}x${_res}/apps"
        ln -s "../../../../../../opt/onlyoffice/desktopeditors/asc-de-${_res}.png" \
            "${pkgdir}/usr/share/icons/hicolor/${_res}x${_res}/apps/onlyoffice-desktopeditors.png"
    done < <(find "${pkgdir}/opt/onlyoffice/desktopeditors" -maxdepth 1 -type f -name 'asc-de-*.png' -print0)
    # We are using system Qt5 and icu
    rm "$pkgdir"/opt/onlyoffice/desktopeditors/libQt5*
    rm "$pkgdir"/opt/onlyoffice/desktopeditors/libicu*
}
