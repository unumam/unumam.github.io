# C* Compiler

Most languages are either compiled or interpreted, but fundamentally, there are no such constraints for the language itself.<br/> 
Most modern languages are built around the LLVM toolchain, which makes it easy to target many platforms out-of-the-box.<br/> 
However, LLVM is a huge project, that is hard to validate and search for bugs. We are trying to build a much simpler tool.

## Existing Ecosystems

Distribution of files by language in some compilers.

| Language\Project | [LLVM](https://github.com/llvm-mirror/llvm) | [GCC](https://github.com/gcc-mirror/gcc) | [TF](https://github.com/tensorflow/tensorflow) |
| --- | :---: | :---: | :---: |
| C | 0 | 48,486 | 198 |
| C++ | 5,139 | 10,163 | 7,415 |
| Fortran | 0 | 6,627 | 0 |
| Ada | 0 | 5,694 | 0 |
| Go | 0 | 3,721 | 0 |
| Python | 0 | 0 | 2,975 |
| Unix Assembly | 4,555 | 579 | 0 |
| Other | ~5,000 | ~1,000 | ~500 |
| Total | ~15,000 files | ~75,000 files | ~11,000 files |
