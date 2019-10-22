# C* Compiler

Most languages are either compiled or interpreted, but fundamentally, there are no such constraints for the language itself.<br/> 
Most modern languages are built around the LLVM toolchain, which makes it easy to target many platforms out-of-the-box.<br/> 
However, LLVM is a huge project, that is hard to validate and search for bugs. We are trying to build a much simpler tool.

## Existing Ecosystems

Distribution of files by language in some compilers.

| Language\Project | LLVM | GCC |
| --- | --- | --- |
| C | 0 | 48486 |
| C++ | 5139 | 10163 |
| Fortran | 0 | 6627 |
| Ada | 0 | 5694 |
| Go | 0 | 3721 |
| Unix Assembly | 4555 | 579 |
| YAML | 2267 | 0 |
| Text | 1001 | 0 |
| Other | ~2000 | ~1000 |
| Total | ~15000 files | ~75000 files |
