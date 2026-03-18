pkgname=linux-arctis-manager-git
_pkgname=linux-arctis-manager
pkgver=2.2.1
pkgrel=1
pkgdesc="A replacement for SteelSeries GG software, to manage your Arctis device on Linux"
arch=('x86_64')
url="https://github.com/christopher/Linux-Arctis-Manager"
license=('GPL-3.0-only')
depends=(
  'python'
  'python-dbus-next'
  'python-pulsectl'
  'pyside6'
  'python-pyudev'
  'python-pyusb'
  'python-ruamel-yaml'
)
makedepends=(
  'python-build'
  'python-installer'
  'python-uv-build'
  'python-wheel'
)
source=()
sha256sums=()

pkgver() {
  cd "$startdir"
  /usr/bin/python - <<'PY'
try:
    import tomllib
except ModuleNotFoundError:
    import tomli as tomllib

with open("pyproject.toml", "rb") as f:
    print(tomllib.load(f)["project"]["version"])
PY
}

build() {
  cd "$startdir"
  /usr/bin/python -m build --wheel --no-isolation
}

package() {
  cd "$startdir"
  /usr/bin/python -m installer --destdir="$pkgdir" dist/*.whl
}
