# Contributing to llama.cpp POWER8

Thank you for your interest in contributing! This project provides POWER8-specific optimizations for llama.cpp.

## How to Contribute

### Reporting Issues

- Use GitHub Issues for bug reports and feature requests
- Include system details (POWER8 model, OS version, etc.)
- For bugs: include steps to reproduce and any error messages

### Pull Requests

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/your-feature`
3. **Make** your changes
4. **Test** your changes if applicable
5. **Commit** with clear messages: `git commit -m "Add: description"`
6. **Push** to your fork: `git push origin feature/your-feature`
7. **Submit** a Pull Request

## Build Requirements

### Hardware
- IBM POWER8 server (Power System S824 recommended)
- Ubuntu 20.04 LTS (last POWER8-supported release)

### Software
- GCC with POWER8 support
- CMake 3.14+

### Compiler Flags
```bash
-mcpu=power8 -mvsx -maltivec -O3 -mtune=power8 -funroll-loops
```

### Build Commands
```bash
mkdir build && cd build
cmake .. \
    -DCMAKE_BUILD_TYPE=Release \
    -DGGML_OPENMP=ON \
    -DCMAKE_C_FLAGS="-mcpu=power8 -mvsx -maltivec -O3 -mtune=power8 -funroll-loops" \
    -DCMAKE_CXX_FLAGS="-mcpu=power8 -mvsx -maltivec -O3 -mtune=power8 -funroll-loops"
make -j$(nproc)
```

## Testing

Test your changes on real POWER8 hardware. This project is specifically optimized for POWER8 architecture, so emulators or cross-compilation won't accurately validate performance changes.

## Code Style

- Follow existing code conventions in the project
- Add comments for POWER8-specific optimizations
- Include performance notes if applicable

## Recognition

Contributors will be credited in the README and may earn RTC tokens via GitHub Issues bounties.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
