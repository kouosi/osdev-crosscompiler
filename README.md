# OSDev Docker Crosscompiler

This Docker image is designed for cross-compiling code for OS development
([OSDev](https://wiki.osdev.org/GCC_Cross-Compiler)) targeting both `i686-elf`
and `x86_64-elf` architectures. It provides the latest versions of GCC and 
Binutils for cross-compilation. The Docker image includes the following 
components:

- **x86_64-elf Toolchain:** This toolchain is configured to compile code for the x86_64 architecture, suitable for bare-metal OS development.
- **i686-elf Toolchain:** Similar to the x86_64-elf toolchain, this toolchain is tailored for the `i686` architecture, commonly used in OS development for older systems.
- **Make:** The `make` utility simplifies the build process by managing dependencies and executing tasks defined in a Makefile.

## Usage
To use this Docker image for cross-compilation in your OS development project. For example:
```bash
docker run --rm -v $PWD:/env osdev-docker-crosscompiler:latest bash -c "make"
```

