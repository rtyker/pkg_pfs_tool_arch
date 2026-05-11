PS4 PKG/PFS tool (c) 2017-2021 by flatz
Forked by rtyker to compilation on Archlinux

Fork notes:
* This fork tracks local Linux build fixes for `mbedtls`/`uthash` include discovery.
* `config.ini` is copied into `build/` after compilation.
* See `AGENTE_IA.md` for the exact rebuild procedure used in this environment.

Dependencies:
* mbedtls
* uthash
* zlib

For Arch Linux:
```bash
sudo pacman -S --needed base-devel cmake mbedtls uthash zlib
cmake -S . -B build
cmake --build build -j"$(nproc)"
./build/pkg_pfs_tool --help
```

For ubuntu-ish:
```bash
sudo apt install libmbedtls-dev uthash-dev zlib1g-dev
```

To produce windows executable from ubuntu-ish via mingw:
```bash
sudo apt install mingw-w64 libz-mingw-w64-dev
```
Then pass e.g. `-DCMAKE_TOOLCHAIN_FILE=../cmake/mingw-w64-x86_64.cmake` to cmake.

P.S. In memory of Maxton Garrett (maxton), this release is dedicated to you.
