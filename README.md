# C_Predefined_H
A single C header file which collects and normalizes hundreds on predefined-processor macros.

## Features:
- Single, MIT-licensed, C header file: just drop in!
- Shadows hundreds of idosyncratic and obscure compiler macros, converting the to a standardized format in process.
- Macros corressponding modern environments/systems are shadowed by default; older and more obscure environments can be shadowed if needed.

### Premise
For every architecture, compiler, operating system, and language standard, there's a set of unique, predefined, C-processor macros for it. Unfortunately, they're often erratically named, overly ambiguous, and poorly documented. When I finally got sick of having find, normalize, and enumerate these macros, I created c_predefined.h with the goal of collecting *EVERY* useful macro in one place and in one intuitive format.

### Format and Hierarchy
The shadow macros are divided into four discrete sections:
- Architectures: C_ARCHITECTURE
- Compilers: C_COMPILER
- Systems: C_SYSTEM
- Standards: C_STANDARD

For the first three of these sections, the use bare form, C_[SECTION], which will turn into a null-terminated byte string the section's exclusive, human readable, value. e.g.
```c
C_COMPILER //"Clang"
```
Alternatively, and for every section, you can use the explicit form, C_[SECTION]_[NAME], which evaluates to one if environment.

c_predefined.h
