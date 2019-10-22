# C* Programming Language

## Purpose

The number of programming languages is constantly growing, but most of them aren't solving any issues. Furthermore, most of the general-purpose languages fail to introduce any new design ideas and just shuffle/sample features of older siblings.

> The definition of insanity is doing the same thing over and over again and expecting a different result. Albert Einstein.

So why design a language in the first place?

- For efficient code generation we need tailor-made compilers, but C++ is too hard to parse and [has many other issues](https://yosefk.com/c++fqa/index.html).
- Languages like [LISP](https://en.wikipedia.org/wiki/Lisp_(programming_language)) are dynamically typed, which makes them too slow for HPC.
- C doesn't have [RAII](https://en.cppreference.com/w/cpp/language/raii), [overload resolution](https://en.cppreference.com/w/cpp/language/overload_resolution), [templates](https://en.cppreference.com/w/cpp/language/templates) or generics.
- Languages like [Rust](https://www.rust-lang.org), [Swift](https://developer.apple.com/swift/) and [D](https://dlang.org) are also overcomplicated with enums and modern pattern matching.
- [OpenCL](https://www.khronos.org/opencl/) and many other parallel programming language are not properly maintained any more.

## Language Goals

- Performance higher than in C++/C... thanks to [kernel fusion](https://arxiv.org/abs/1305.1183), the next evolutionary step after inlining.
- Parsing speed equal to [Common LISP](https://lisp-lang.org).
- Simple prototyping and optimization like in [Halide](https://halide-lang.org).

## Language Specs

- Context-free grammar. *(In unique way)*
- Extendable keywords set. *(In unique way)*
- Static typing and manual memory control.
- Interpretable to C, OpenCL and CUDA.
- Support for wrapper classes.
- Modular compiler.
- Compile-time reflections for [consistent data](https://github.com/protocolbuffers/protobuf) exchanges.
- Implicit Array-of-Structs to Struct-of-Arrays [optimization](https://github.com/bryancatanzaro/trove).
- Functions allow multiple input arguments and multiple outputs.
- Functions allow partial specialization adn serialization, like data.
