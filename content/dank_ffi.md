+++
title = "FFI Doesn't Have to Be Awful" 
+++
Integrating Rust into your existing application can go one of two ways.  You can rewrite from scratch, or you can [use the strangler pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/strangler) to rewrite components and call across your [Foreign Function Interface](https://en.wikipedia.org/wiki/Foreign_function_interface) from your existing application.

In a standard case, porting a C++ application to Rust, this means that you need to export some of your Rust APIs via a C interface, then importing them via another C interface to turn them into C++ calls.  This is way more work than we usually do to call a function, but it is worth it:
- We can make use of functionality across language barriers
- We can do it once then automate it and not worry about it anymore.
- We can begin migrating functionality across our ABI instead of in one go

## Getting Started

Most articles about Rust FFI focus on the other way - calling C/C++ from Rust.  We're focusing in the opposite direction, because that's the reality of larger porting efforts.  Because of that, we will make use of the lesser-known [CBindgen crate](https://github.com/eqrion/cbindgen).  This parses our Rust code looking for specific function signatures and generates a .h file from them.  Let's install it in our Cargo.toml:

```toml
[build-dependencies]
cbindgen = "*"
```

In order to call Rust, we'll need to link in our generated binary statically and consume the header file.  This is a highly variable process, especially if your C++ project is larger.  Automation demands simplicity though, so we're going to choose to go with a CMake/Ninja setup.  


```CMake

```

```build.sh
```

I'd highly recommended to use a simple setup for your FFI validation, even if you're not going to consume it in exactly the same build system.  Keeping this part straightforward will allow for CBindgen and Cargo to handle the complexities of this whole adventure, meaning there shouldn't be much manual upkeep.  The benefits of having automated validation on your Rust code outweigh the costs of having a seperate stack in this component.

## Lightweight C++ tests

## Cargo Automation

## CI/CD

