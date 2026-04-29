# Docker Images for C++ CI Workflows

This repository provides Docker images optimized for use in GitHub Actions CI 
workflows targeting C++ projects.

All images are built on top of a common Ubuntu 24.04 base with several useful 
packages installed via APT:

- `binutils`
- `bison`
- `cmake`
- `conan` (installed via `pipx`)
- `flex`
- `git`
- `make`
- `ninja`
- `pkgconf`

## GCC images

- `ghcr.io/mattkretz/cplusplus-ci/gcc9`
- `ghcr.io/mattkretz/cplusplus-ci/gcc10`
- `ghcr.io/mattkretz/cplusplus-ci/gcc11`
- `ghcr.io/mattkretz/cplusplus-ci/gcc12`
- `ghcr.io/mattkretz/cplusplus-ci/gcc13`
- `ghcr.io/mattkretz/cplusplus-ci/gcc14`
- `ghcr.io/mattkretz/cplusplus-ci/gcc15`
- `ghcr.io/mattkretz/cplusplus-ci/gcc16`
- `ghcr.io/mattkretz/cplusplus-ci/gcc17`
- `ghcr.io/mattkretz/cplusplus-ci/gccp0917`

The GCC images provide `gcc` and `g++` aliases to the respective version. GCC 
is always installed with multilib support, i.e. `-m32`, `-mx32`, and `-m64` are 
supported.

The [P0917](https://wg21.link/P0917) image is a patched GCC 17 with overloadable 
`operator?:`.

## Clang images

- `ghcr.io/mattkretz/cplusplus-ci/clang14`
- `ghcr.io/mattkretz/cplusplus-ci/clang15`
- `ghcr.io/mattkretz/cplusplus-ci/clang16`
- `ghcr.io/mattkretz/cplusplus-ci/clang17`
- `ghcr.io/mattkretz/cplusplus-ci/clang18`
- `ghcr.io/mattkretz/cplusplus-ci/clang19`
- `ghcr.io/mattkretz/cplusplus-ci/clang20`
- `ghcr.io/mattkretz/cplusplus-ci/clang21`
- `ghcr.io/mattkretz/cplusplus-ci/clang22`
- `ghcr.io/mattkretz/cplusplus-ci/clang23`

The Clang images provide `clang`, `clang++`, `clang-tidy`, and `clang-format` 
aliases to the respective version.

---

## Example GitHub CI Workflow

```yaml
name: Clang

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  clang:
    strategy:
      fail-fast: false
      matrix:
        version: [21, 22]

    runs-on: ubuntu-latest

    container:
      image: ghcr.io/mattkretz/cplusplus-ci/clang${{ matrix.version }}

    steps:
      - uses: actions/checkout@v4

      - name: Run test suite
        env:
          CXX: clang++
        run: make check
```

## Latest CMake

If you want to use latest CMake add the following step to your workflow before 
a step that needs `cmake`:

```yaml
      - name: Set PATH for cmake-latest
        run: echo /opt/cmake-latest/bin >> $GITHUB_PATH
```

## Updates

The `gcc15`, `gcc16`, and `gcc17` images are updated once per week. All other images are 
updated once per month. There may also be manual updates whenever the feature 
set needs to change.
