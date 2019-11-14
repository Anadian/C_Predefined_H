# C_Predefined_H
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)
[![Semantic Versioning 2.0.0](https://img.shields.io/badge/semver-2.0.0-brightgreen?style=flat-square)](https://semver.org/spec/v2.0.0.html)
[![License](https://img.shields.io/github/license/Anadian/C_Predefined_H)](https://github.com/Anadian/C_Predefined_H/Documents/LICENSE)

> A single C header file which collects and normalizes hundreds on predefined processor macros.
# Table of Contents
- [Background](#Background)
- [Usage](#Usage)
- [API](#API)
	- [Architectures](#Architectures)
	- [Compilers](#Compilers)
	- [Systems](#Systems)
	- [Standards](#Standards)
		- [Unix Standards](#unix-standards)
		- [Helpful Macros](#helpful-macros)
	- [Options](#Options)
- [Contributing](#Contributing)
- [License](#License)
# Background
Predefined macros and constants are essential to writing portable C/C++ code; unfortunately, due to these predefined constants never being standardised and there being many different equally-unintuitive conventions for naming them, finding and using these predefined macros can be a nightmare. I created this project to provide a single C header file which collects hundreds of these enigmatic macros, from the most common architectures/compilers/system to the most obscure and deprecated ones, and present them in a simple, consistent format.
# Usage
The main "c_predefined.h" header file is designed to be a single-file, drop-in, pseudo-library (sort of in the spirit of [Sean T Barret's](https://github.com/nothings/stb) [single-file libs](https://github.com/nothings/single_file_libs) except even simpler); literally just place the "c_predefined.h" file in your source/include directory and you're good to go: no additional attributions, mandatory configuration, or external dependencies to worry about. 
# API
[`c_predefined.h`](./c_predefined.h) wraps (or shadows) hundreds of c-preproccessor constants (`#define`s) for tons of architectures, compilers, and systems to be more consistent and clear. These shadowed-macros are wrapped in new macros, all following the consstent-naming format of `C_<section>_<item>` with `<section>` either being [`ARCHITECTURE`](#Architectures), [`COMPILER`](#Compilers), [`SYSTEM`](#Systems), or [`STANDARD`](#Standards) and the `<item>` part being a specific architecture/compiler/system/standard: both written with all alphabetical characters in uppercase. Additionally, for the first three, architecure/compiler/system sections, omitting the item part, just using `C_<section>` gives you a human-reable string of the name of the most-applicable (meaning the most specific identifier which can be used to accurately describe that section for the current system/build environment) item for that section (more on this below).
## Architectures
To find out if a specific architecture is in use 
```c
#if C_ARCHITECTURE_<architecture name in all caps> == 1
```
or simply
```c
#if define(C_ARCHITECTURE_<architecture name in all caps>) /* or #ifdef C_ARCHITECTURE_<capitalised architecture name> */
```
either of the above will be true if the specified architecture is the primary architecture in use.
Additionally, a human-readable and properly-cased string for whatever the current architecture is, is defined for `C_ARCHITECTURE`. 
Note, `c_predefined.h` uses whatever the **most-specific applicable architecture name** is so for example, it will use `C_ARCHITECTURE_ARM64` over simply `C_ARCHITECTURE_ARM` on 64-bit ARM processors.
Full example:
```c
#include "c_predefined.h"
#include <stdio.h>

int main(int argc, char *argv[]){
	#if C_ARCHITECTURE_X86_64
	printf("This code was compiled for %s architecture.", C_ARCHITECTURE); /* Will print: "This code was compiled for x86_64 architecture" on 64-bit x86 machines. */
	#elif C_ARCHITECTURE_I386
	printf("This code was compiled for %s architecture.", C_ARCHITECTURE); /* Will print: "This code was compiled for i386 architecture" on 32-bit x86 machines. */
	#endif
}
```
The [non-obscure](#Options) architectures `c_predefined.h` will recognise by default are: 

| The capitalised macro-name part (for `C_ARCHITECTURE_<macro-name part>`) | The human-readable string (given by `C_ARCHITECTURE`) |
| --- | --- |
| `X86_64` | `"x86_64"` |
| `ARM64` | `"ARM64"` |
| `I386` | `"i386"` |
| `ARM` | `"ARM"` |
| `MIPS` | `"MIPS"` |
| `POWERPC` | `"PowerPC"` |
| `SPARC` | `"SPARC"` |

Tons more are recognised by defining `C_PREDEFINED_OBSCURE_ARCHITECTURES 1` before including "c_predefined.h" (see [Options](#Options) for more info).

## Compilers
The convention for getting compilers is basically identical to architectures just with "COMPILER" replacing "ARCHITECTURE" in in macro names.
To find out if a specific compiler is in use 
```c
#if C_COMPILER_<compiler name in all caps> == 1
```
or simply
```c
#if define(C_COMPILER_<compiler name in all caps>) /* or #ifdef C_COMPILER_<capitalised compiler name> */
```
either of the above will be true if the specified compiler is the primary compiler in use.
Additionally, a human-readable and properly-cased string for whatever the current compiler is, is defined for `C_COMPILER`. 
Note, `c_predefined.h` uses whatever the **most-specific applicable compiler name** is so for example, it will use `C_COMPILER_CLANG` over simply `C_COMPILER_LLVM` in [clang](https://clang.llvm.org/) even though Clang is built on top of [LLVM](https://www.llvm.org/) (with `C_COMPILER_LLVM` being used for other, unrecognised, LLVM-based compilers.)
Full example:
```c
#include "c_predefined.h"
#include <stdio.h>

int main(int argc, char *argv[]){
	#if C_COMPILER_CLANG
	printf("This code was compiled with %s.", C_COMPILER); /* Will print: "This code was compiled with Clang" on Clang. */
	#elif C_COMPILER_LLVMj
	printf("This code was compiled with %s.", C_COMPILER); /* Will print: "This code was compiled with LLVM" on un-named, LLVM-based compilers. */
	#endif
}
```
The [non-obscure](#Options) compilers `c_predefined.h` will recognise by default are: 

| The capitalised macro-name part (for `C_COMPILER_<macro-name part>`) | The human-readable string (given by `C_COMPILER`) |
| --- | --- |
| `MINGW` | `"MinGW"` |
| `MICROSOFT_VISUAL` | `"Microsoft Visual C++"` |
| `IBM_XL` | `"IBM XL C/C++"` |
| `INTEL` | `"Intel C/C++"` |
| `HP_ACPP` | `"HP aC++"` |
| `PORTLAND_GROUP` | `"Portland Group"` |
| `CLANG` | `"Clang"` |
| `GCC` | `"GCC"` |
| `ORACLE_PRO` | `"Oracle Pro C"` |
| `SUN_PRO` | `"Sun Pro"` |
| `LLVM` | `"LLVM"` |
| `HP_ANSI` | `"HP ANSI C"` |
| `TINYC` | `"Tiny C"` |
| `METROWERKS_CODEWARRIOR` | `"Metrowerks CodeWarrior"` |
| `BORLAND` | `"Borland C++"` |

Tons more are recognised by defining `C_PREDEFINED_OBSCURE_COMPILERS 1` before including "c_predefined.h" (see [Options](#Options) for more info).

## Systems
The convention for getting systems is basically identical to architectures and compilers just with "SYSTEM" replacing "ARCHITECTURE/COMPILER" in in macro names.
To find out if a specific system is in use 
```c
#if C_SYSTEM_<system name in all caps> == 1
```
or simply
```c
#if define(C_SYSTEM_<system name in all caps>) /* or #ifdef C_SYSTEM_<capitalised system name> */
```
either of the above will be true if the specified system is the primary system in use.
Additionally, a human-readable and properly-cased string for whatever the current system is, is defined for `C_SYSTEM`. 
Note, `c_predefined.h` uses whatever the **most-specific applicable system name** is so for example, `C_SYSTEM_GNU_LINUX` will be preferred over simply `C_SYSTEM_LINUX` OR `C_SYSTEM_UNIX` where appropriate.
Full example:
```c
#include "c_predefined.h"
#include <stdio.h>

int main(int argc, char *argv[]){
	#if C_SYSTEM_CYGWIN
	printf("This code was compiled for %s.", C_SYSTEM); /* Will print: "This code was compiled for Cygwin" on Cygwin. */
	#elif C_SYSTEM_LLVMj
	printf("This code was compiled for %s.", C_SYSTEM); /* Will print: "This code was compiled for Windows CE" on Windows CE based systems. */
	#endif
}
```
The [non-obscure](#Options) systems `c_predefined.h` will recognise by default are: 

| The capitalised macro-name part (for `C_SYSTEM_<macro-name part>`) | The human-readable string (given by `C_SYSTEM`) |
| --- | --- |
| `CYGWIN` | `"Cygwin"` |
| `WINDOWSCE` | `"Windows CE"` |
| `WINDOWS` | `"Windows"` |
| `APPLE` | `"Apple (mac/i/tv)OS"` |
| `ANDROID` | `"Android"` |
| `OPENBSD` | `"OpenBSD"` |
| `NETBSD` | `"NetBSD"` |
| `GNU_HURD` | `"GNU/Hurd"` |
| `SOLARIS` | `"Sun Solaris"` |
| `FREEBSD_KERNEL` | `"GNU/FreeBSD Kernel"` |
| `FREEBSD` | `"FreeBSD"` |
| `MSDOS` | `"MSDOS"` |
| `MACINTOSH` | `"Macintosh"` |
| `LYNX` | `"Lynx"` |
| `MINIX` | `"Minix"` |
| `QNX` | `"QNX"` |
| `BSDI` | `"BSD/OS"` |
| `GNU_LINUX` | `"GNU/Linux"` |
| `BSD` | `"BSD"` |
| `GNU` | `"GNU"` |
| `LINUX` | `"Linux"` |
| `UNIX` | `"Unix"` |

Tons more are recognised by defining `C_PREDEFINED_OBSCURE_SYSTEMS 1` before including "c_predefined.h" (see [Options](#Options) for more info).
## Standards
Additionally, `c_predefined.h` recognises several language-standards for both C and C++ compilers:

- `C_STANDARD_C90`
- `C_STANDARD_C94`
- `C_STANDARD_C99`
- `C_STANDARD_C11`
- `C_STANDARD_HOSTED`
- `C_STANDARD_CPP98`
- `C_STANDARD_CPP11`
- `C_STANDARD_CPP14`
- `C_STANDARD_CLI`
- `C_STANDARD_EMBEDDED`

will all evaluate to `1` if the corresponding standard is being implemented. Unlike the architecture/compiler/system constants specified above, standards are cummulative so for example, it's possible for both `C_STANDARD_C94` and `C_STANDARD_C99` to evaluate to `1` at the same time.
### Unix Standards
`c_predefined.h` can also wrap Unix standards specified `<unistd.h>` if `C_PREDEFINED_UNIX_STANDARDS` is defined to equal `1` before including `"c_predefined.h"`; obviously this options should only be used on POSIX-compliant unix-based systems. I may implement an automatic check for if this should be enabled in future updates.
### Helpful Macros
If `C_PREDEFINED_HELPFUL_MACROS` is defined to `1` before including `"c_predefined.h"`, `c_predefined.h` can also wrap some other, less-ubiquitos, helpful macros under the `C_STANDARD` moniker as follows:

- `C_STANDARD_ELF`
- `C_STANDARD_CHAR_UNSIGNED`
- `C_STANDARD_WCHAR_UNSIGNED`
- `C_STANDARD_BYTE_SIZE`
- `C_STANDARD_LITTLE_ENDIAN`
- `C_STANDARD_BIG_ENDIAN`
- `C_STANDARD_PDP_ENDIAN`
- `C_STANDARD_FLOAT_WORD_ORDER_LITTLE_ENDIAN`
- `C_STANDARD_FLOAT_WORD_ORDER_BIG_ENDIAN`
- `C_STANDARD_OBJECTIVEC`
- `C_STANDARD_ASSEMBLY`
- `C_STANDARD_STRICT_ANSI`
- `C_STANDARD_DEPRECATED`
- `C_STANDARD_EXCEPTIONS`
- `C_STANDARD_64BIT_LONG_INT_AND_POINTER`

## Options
The sheer breadth of macros `c_predefined.h` can check for and wrap as needed can be adjust by defining any number of the following macros to a value of `1` before include `"c_predefined.h"`:

| Macro name | Effect |
| --- | --- |
| `C_PREDEFINED_OBSCURE_ARCHITECTURES` | Adds tons of macros for vintage and obscure [architectures](#Architectures) to be checked against, and wrapped if found. |
| `C_PREDEFINED_OBSCURE_COMPILERS` | Adds tons of macros for vintage and obscure [compilers](#Compilers) to be checked against, and wrapped if found. |
| `C_PREDEFINED_OBSCURE_SYSTEMS` | Adds tons of macros for vintage and obscure [systems](#Systems) to be checked against, and wrapped if found. |
| `C_PREDEFINED_UNIX_STANDARDS` | Wraps [macros](#unix-standards) from `<unistd.h>` with the same `C_STANDARD_<language-standard code>` convention as other standards. |
| `C_PREDEFINED_HELPFUL_MACROS` | Adds additional, [helpful](#helpful-macros) but less common macros to `C_STANDARD_<macro name>`

Example:
```c
#define C_PREDEFINED_OBSCURE_ARCHITECTURES 1
#define C_PREDEFINED_OBSCURE_COMPILERS 1
#define C_PREDEFINED_OBSCURE_SYSTEMS 1
#define C_PREDEFINED_UNIX_STANDARDS 1
#define C_PREDEFINED_HELPFUL_MACROS 1
#include "c_predefined.h"
```
# Contributing
Changes are tracked in [CHANGES.md](./CHANGES.md).

Any pull request to add additional predefined macros are welcome, just be sure to provide attribution to the official documentation for the architecture/compiler/system which recognises them. 
# License
MIT ©2019 Anadian

SEE LICENSE IN [LICENSE](./LICENSE)
