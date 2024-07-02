# Do's and Don'ts

The next two lists are heavily based on the excellent gist [Effective Modern CMake]. That list is much longer and more detailed, feel free to read it as well.

## CMake Antipatterns

- **Do not use global functions**: This includes `link_directories`, `include_libraries`, and similar.
- **Don't add unneeded PUBLIC requirements**: You should avoid forcing something on users that is not required (`-Wall`). Make these PRIVATE instead.
- **Don't GLOB files**: Make or another tool will not know if you add files without rerunning CMake. Note that CMake 3.12 adds a `CONFIGURE_DEPENDS` flag that makes this far better if you need to use it.
- **Link to built files directly**: Always link to targets if available.
- **Never skip PUBLIC/PRIVATE when linking**: This causes all future linking to be keyword-less.

## CMake Patterns

- **Treat CMake as code**: It is code. It should be as clean and readable as all other code.
- **Think in targets**: Your targets should represent concepts. Make an (IMPORTED) INTERFACE target for anything that should stay together and link to that.
- **Export your interface**: You should be able to run from build or install.
- **Write a Config.cmake file**: This is what a library author should do to support clients.
- **Make ALIAS targets to keep usage consistent**: Using `add_subdirectory` and `find_package` should provide the same targets and namespaces.
- **Combine common functionality into clearly documented functions or macros**: Functions are better usually.
- **Use lowercase function names**: CMake functions and macros can be called lower or upper case. Always use lower case. Upper case is for variables.
- **Use `cmake_policy` and/or range of versions**: Policies change for a reason. Only piecemeal set OLD policies if you have to.

## Selecting a minimum in 2022:

What minimum CMake should you _run_ locally, and what minimum should you _support_ for people using your
code? Since you are reading this, you should be able to get a release in the last few versions of CMake;
do that, it will make your development easier. For support, there are two ways to pick minimums: based on
features added (which is what a developer cares about), or on common pre-installed CMakes (which is what a
user cares about).

Never select a minimum version older than the oldest compiler version you support. CMake should always be
at least as new as your compiler.

### What minimum to choose - OS support:

- 3.4: The bare minimum. Never set less.
- 3.7: Debian old-stable.
- 3.10: Ubuntu 18.04.
- 3.11: CentOS 8 (use EPEL or AppSteams, though)
- 3.13: Debian stable.
- 3.16: Ubuntu 20.04.
- 3.19: First to support Apple Silicon.
- latest: pip/conda-forge/homebew/chocolaty, etc.

### What minimum to choose - Features:

- 3.8: C++ meta features, CUDA, lots more
- 3.11: `IMPORTED INTERFACE` setting, faster, FetchContent, `COMPILE_LANGUAGE` in IDEs
- 3.12: C++20, `cmake --build build -j N`, `SHELL:`, FindPython
- 3.14/3.15: CLI, FindPython updates
- 3.16: Unity builds / precompiled headers, CUDA meta features
- 3.17/3.18: Lots more CUDA, metaprogramming

[effective modern cmake]: https://gist.github.com/mbinna/c61dbb39bca0e4fb7d1f73b0d66a4fd1
