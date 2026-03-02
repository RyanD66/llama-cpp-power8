# Contributing to llama.cpp POWER8

Thank you for your interest in contributing to the llama.cpp POWER8 optimization project! This document outlines the process for contributing and the standards we follow.

## Table of Contents

- [Building the Project](#building-the-project)
- [PR Workflow](#pr-workflow)
- [Testing](#testing)
- [Coding Standards](#coding-standards)
- [Communication](#communication)

## Building the Project

### Prerequisites

- **OS**: Ubuntu 20.04 LTS (last POWER8-supported release)
- **Compiler**: GCC with POWER8 support
- **Build Tools**: CMake 3.14+
- **Hardware**: IBM POWER8 system (for full testing)

### Build Requirements

```bash
# Install build dependencies
sudo apt-get update
sudo apt-get install -y build-essential cmake gcc-powerpc64le-linux-gnu g++-powerpc64le-linux-gnu

# Or for native POWER8 build
sudo apt-get install -y build-essential cmake
```

### Build Commands

```bash
# Clone the repository
git clone https://github.com/Scottcjn/llama-cpp-power8.git
cd llama-cpp-power8

# Create build directory
mkdir build && cd build

# Configure for POWER8
cmake .. \
    -DCMAKE_BUILD_TYPE=Release \
    -DGGML_OPENMP=ON \
    -DCMAKE_C_FLAGS="-mcpu=power8 -mvsx -maltivec -O3 -mtune=power8 -funroll-loops" \
    -DCMAKE_CXX_FLAGS="-mcpu=power8 -mvsx -maltivec -O3 -mtune=power8 -funroll-loops"

# Build
make -j$(nproc)
```

### Optional: IBM MASS Library

For additional math optimizations, install the IBM Mathematical Acceleration Subsystem (MASS):

```bash
# Install MASS (requires IBM license)
# Add MASS flags to cmake:
    -DGGML_USE_MASS=1 -I/opt/ibm/mass/include \
    -DCMAKE_EXE_LINKER_FLAGS="-L/opt/ibm/mass/lib -lmassvp8 -lmass"
```

## PR Workflow

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feat/your-feature`
3. **Make** your changes following our coding standards
4. **Test** your changes (see [Testing](#testing))
5. **Commit** with clear, descriptive messages
6. **Push** to your fork: `git push origin feat/your-feature`
7. **Submit** a Pull Request against `main` branch

### PR Requirements

- PRs must pass all existing tests
- Include a clear description of the changes
- Reference any related issues
- For POWER8-specific changes, document hardware used for testing

## Testing

### Basic Build Test

```bash
# Verify the project builds successfully
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j$(nproc)
```

### Benchmark Test

```bash
# Run benchmark to verify optimizations work
./bin/llama-bench -m <model_path> -t 64 -p 128 -n 32
```

### Altivec Benchmark

```bash
# Test AltiVec/VSX performance
gcc -O3 -mcpu=power8 -mvsx -maltivec -o altivec_bench ../altivec_benchmark.c
./altivec_bench
```

### Expected Performance

| Model | Expected (tokens/s) |
|-------|---------------------|
| TinyLlama 1.1B Q4 | ~85 |
| Llama-7B Q4 | ~20 |

## Coding Standards

### C/C++

- Follow the existing code style (see `.clang-format` if available)
- Use meaningful variable names
- Comment POWER8-specific optimizations
- Keep lines under 100 characters when possible

### POWER8-Specific Guidelines

- Use `-mcpu=power8 -mvsx -maltivec` compiler flags
- Prefer VSX intrinsics over pure C for performance-critical code
- Use the provided `power8-compat.h` for POWER9→POWER8 compatibility
- Test cache prefetch hints with `ggml-dcbt-resident.h`

### Memory Alignment

POWER8 prefers 128-byte aligned data for optimal VSX performance:

```c
// Use aligned allocations
#include <mm_malloc.h>
float* data = (float*)_mm_malloc(size * sizeof(float), 128);
```

## Communication

- **Discord**: Join the [RustChain Discord](https://discord.gg/tQ4q3z4M) for PowerPC/POWER8 AI discussion
- **Issues**: Use GitHub Issues for bug reports and feature requests
- **Discussions**: Use GitHub Discussions for questions

## Recognition

Contributors will be recognized in:
- README.md credits section
- Release notes

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

*Thank you for helping advance POWER8 AI capabilities!*
