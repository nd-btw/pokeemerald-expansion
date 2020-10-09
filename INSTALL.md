# Prerequisites

Independently from the specific OS, make sure that the `gcc`, `g++`, `make`, `git`, and `libpng-dev` packages or their equivalents are installed and accessible to the development tools that are used by the project (this means that, for example, on Windows, the packages have to be installed in the WSL environment). The package names and installation methods may vary with each OS.

Install the devkitARM toolchain of devkitPro as per [the instructions on their wiki](https://devkitpro.org/wiki/devkitPro_pacman). On Windows, follow the Linux instructions inside WSL as any steps about the Windows installer do not apply.

# Installation

To set up the repository:

	git clone https://github.com/nadiadavis/ndpoke
	git clone https://github.com/pret/agbcc

	cd ./agbcc
	./build.sh
	./install.sh ../ndpoke

	cd ../ndpoke

To build **ndpoke.gba** for the first time and confirm it matches the official ROM image:

	make compare

If an OK is returned, then the installation went smoothly.

**Windows users:** Consider adding exceptions for the `pokeemerald` and `agbcc` folders in Windows Security using [these instructions](https://support.microsoft.com/help/4028485). This prevents Microsoft Defender from scanning them which might improve performance while building.


# Start

To build **pokeemerald.gba** with your changes:

	make

**macOS users:** If the base tools are not found in new Terminal sessions after the first successful build, run `echo "if [ -f ~/.bashrc ]; then . ~/.bashrc; fi" >> ~/.bash_profile` once to prevent the issue from occurring again. Verify that the `devkitarm-rules` package is installed as well; if not, install it by running `sudo dkp-pacman -S devkitarm-rules`.


# Building guidance


## Parallel builds

See [the GNU docs](https://www.gnu.org/software/make/manual/html_node/Parallel.html) and [this Stack Exchange thread](https://unix.stackexchange.com/questions/208568) for more information.

To speed up building, run:

	make -j$(nproc)

`nproc` is not available on macOS. The alternative is `sysctl -n hw.ncpu` ([relevant Stack Overflow thread](https://stackoverflow.com/questions/1715580)).


## Debug info

To build **pokeemerald.elf** with enhanced debug info:

	make DINFO=1


## devkitARM's C compiler

This project supports the `arm-none-eabi-gcc` compiler included with devkitARM r52. To build this target, simply run:

	make modern


## Other toolchains

To build using a toolchain other than devkitARM, override the `TOOLCHAIN` environment variable with the path to your toolchain, which must contain the subdirectory `bin`.

	make TOOLCHAIN="/path/to/toolchain/here"

The following is an example:

	make TOOLCHAIN="/usr/local/arm-none-eabi"

To compile the `modern` target with this toolchain, the subdirectories `lib`, `include`, and `arm-none-eabi` must also be present.


# Useful additional tools

* [porymap](https://github.com/huderlem/porymap) for viewing and editing maps
* [poryscript](https://github.com/huderlem/poryscript) for scripting ([VS Code extension](https://marketplace.visualstudio.com/items?itemName=karathan.poryscript))
* [Tilemap Studio](https://github.com/Rangi42/tilemap-studio) for viewing and editing tilemaps

# Debugging

To compile a target with a built-in debug menu, pass `DDEBUG=1` as a make argument:

	make DDEBUG=1

To compile a target with debugging symbols, pass `DINFO=1` as a make argument:

	make DINFO=1

To utilize all available CPU cores to compile the `modern` target with a menu & symbols for debugging:

	make -j$(nproc) modern DDEBUG=1 DINFO=1

See mGBA and gdb documentation for more information on debugging.
