# Maintainer: Maud Spierings <maud_spierings@hotmail.com>

pkgname=python-libuuu
pkgver=1.5.197
pkgrel=1
pkgdesc='A python wrapper for libuuu'
arch=('x86_64' 'aarch64')
url='https://github.com/nxp-imx/mfgtools'
license=(BSD-3-Clause)
depends=('bzip2' 'zlib' 'libusb' 'libzip' 'openssl' 'tinyxml2' 'python-setuptools-scm')
makedepends=('cmake' 'python-build' 'python-installer' 'python-wheel' 'zip')
changelog=History.md
source=(
	"git+$url#commit=03f16e5b3668a88c4c7e4684794d858518d921a2" # 1.5.197
	"git+https://github.com/microsoft/vcpkg.git"
)
sha256sums=(
	'SKIP'
	'SKIP'
)

build() {
	export VCPKG_FORCE_SYSTEM_BINARIES=1
	cd "${srcdir}"

	export VCPKG_ROOT="${srcdir}/vcpkg"
	$VCPKG_ROOT/bootstrap-vcpkg.sh -disableMetrics

	cd mfgtools/wrapper
	cmake \
		--preset unix \
		-B build \
		-DCMAKE_TOOLCHAIN_FILE="${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
	cmake --build build
	mkdir -p libuuu/lib
	cp build/libuuu.so libuuu/lib
	python -m build --wheel --no-isolation
}

package() {
	cd "${srcdir}/mfgtools/wrapper"
	python -m installer --destdir="${pkgdir}" dist/libuuu-${pkgver}-py3-none-any.whl
	install -Dm644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}

