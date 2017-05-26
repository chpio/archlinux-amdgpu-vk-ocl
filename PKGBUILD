# Maintainer: grmat <grmat@sub.red>

pkgname=(libdrm-amdgpo opencl-amd vulkan-amd)
pkgver=16.60.379184
pkgrel=4
arch=('x86_64')
url='http://www.amd.com'
license=('custom:AMD')
makedepends=('wget')
depends=('ocl-icd')
optdepends=('linux-cik: amdgpu support for CIK cards (Bonaire/Hawaii series)')
conflicts=('amdgpocl')

DLAGENTS='https::/usr/bin/wget --referer https://support.amd.com/en-us/kb-articles/Pages/AMDGPU-PRO-Driver-for-Linux-Release-Notes.aspx -N %u'

prefix='amdgpu-pro-'
major='16.60'
minor='379184'
shared="opt/amdgpu-pro/lib/x86_64-linux-gnu"

source=("https://www2.ati.com/drivers/linux/ubuntu/${prefix}${major}-${minor}.tar.xz")
sha256sums=('d88b5a747ac765a75eb738aaf5641428c3b1c9c02bc6fde452c423de7dd620bd')

pkgver() {
	echo "${major}.${minor}"
}

package_libdrm-amdgpo() {
	pkgdesc="libdrm interface as provided in the amdgpu-pro driver stack, modified to work along with the free amdgpu stack."

	mkdir "${srcdir}/libdrm"
	cd "${srcdir}/libdrm"
	ar x "${srcdir}/${prefix}${major}-${minor}/libdrm-amdgpu-pro-amdgpu1_2.4.70-${minor}_amd64.deb"
	tar xJf data.tar.xz
	cd ${shared}
	rm "libdrm_amdgpu.so.1"
	mv "libdrm_amdgpu.so.1.0.0" "libdrm_amdgpo.so.1.0.0"
	ln -s "libdrm_amdgpo.so.1.0.0" "libdrm_amdgpo.so.1"
	mkdir -p ${pkgdir}/usr/lib
	cp "${srcdir}/libdrm/${shared}/libdrm_amdgpo.so.1.0.0" "${pkgdir}/usr/lib/"
	cp "${srcdir}/libdrm/${shared}/libdrm_amdgpo.so.1" "${pkgdir}/usr/lib/"
}

package_opencl-amd() {
	pkgdesc="OpenCL userspace driver as provided in the amdgpu-pro driver stack, modified to work along with the free amdgpu stack."
	depends=("libdrm-amdgpo=${pkgver}-${pkgrel}")
	optdepends=('opencl-headers: headers necessary for OpenCL development')
	provides=('opencl-driver')

	mkdir -p "${srcdir}/opencl"
	cd "${srcdir}/opencl"
	ar x "${srcdir}/${prefix}${major}-${minor}/opencl-amdgpu-pro-icd_${major}-${minor}_amd64.deb"
	tar xJf data.tar.xz
	cd ${shared}
	sed -i "s|libdrm_amdgpu|libdrm_amdgpo|g" libamdocl64.so
	mv "${srcdir}/opencl/etc" "${pkgdir}/"
	mkdir -p ${pkgdir}/usr/lib
	cp "${srcdir}/opencl/${shared}/libamdocl64.so" "${pkgdir}/usr/lib/"
	cp "${srcdir}/opencl/${shared}/libamdocl12cl64.so" "${pkgdir}/usr/lib/"
}

package_vulkan-amd() {
	pkgdesc="Vulkan userspace driver as provided in the amdgpu-pro driver stack, modified to work along with the free amdgpu stack."
	depends=("libdrm-amdgpo=${pkgver}-${pkgrel}")
	provides=('vulkan-driver')

	mkdir -p "${srcdir}/vulkan"
	cd "${srcdir}/vulkan"
	ar x "${srcdir}/${prefix}${major}-${minor}/vulkan-amdgpu-pro_${major}-${minor}_amd64.deb"
	tar xJf data.tar.xz
	cd ${shared}
	sed -i "s|libdrm_amdgpu|libdrm_amdgpo|g" amdvlk64.so
	cd "${srcdir}/vulkan/etc/vulkan/icd.d"
	sed -i "s|opt/amdgpu-pro/lib/x86_64-linux-gnu|usr/lib|g" amd_icd64.json
	mkdir -p ${pkgdir}/usr/share
	mv "${srcdir}/vulkan/etc/vulkan" "${pkgdir}/usr/share/"
	mkdir -p ${pkgdir}/usr/lib
	cp "${srcdir}/vulkan/${shared}/amdvlk64.so" "${pkgdir}/usr/lib/"
}
