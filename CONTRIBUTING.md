# Contributing to llama.cpp POWER8

Thank you for your interest in contributing to this POWER8 optimization project for llama.cpp!

## Getting Started

### Prerequisites

- **Operating System**: Ubuntu 20.04 LTS (last POWER8-supported release)
- **Compiler**: GCC with POWER8 VSX support
- **Build Tools**: CMake 3.14 or higher
- **Hardware**: IBM POWER8 server (recommended: IBM Power System S824)

### Required Compiler Flags

```bash
-mcpu=power8 -mvsx -maltivec -O3 -mtune=power8 -funroll-loops
```

### Build Dependencies

```bash
# Install build essentials
sudo apt-get update
sudo apt-get install -y build-essential cmake gcc-powerpc64le-linux-gnu g++-powerpc64le-linux-gnu

# Optional: IBM MASS library for optimized math functions
# Available from IBM SDK for Linux
```

## Development Workflow

### 1. Fork and Clone

```bash
# Fork this repository on GitHub
git clone https://github.com/Scottcjn/llama-cpp-power8.git
cd llama-cpp-power8
```

### 2. Create a Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/bug-description
```

### 3. Make Changes

- Follow the existing code style
- Add comments for POWER8-specific optimizations
- Test on real POWER8 hardware when possible

### 4. Submit a Pull Request

```bash
git add .
git commit -m "Description of your changes"
git push origin your-branch-name
```

Then create a PR on GitHub.

## Pull Request Guidelines

### What to Include

1. **Description**: What does this change do?
2. **Testing**: 
   - Did you test on POWER8 hardware?
   - What benchmark results did you see?
3. **Compatibility**: Does it work on both POWER8 and POWER9?

### Code Style

- Use meaningful variable names
- Comment POWER8-specific intrinsics
- Keep lines under 100 characters when possible

### Testing

Run the AltiVec benchmark to verify optimizations:

```bash
# Compile and run benchmark
gcc -mcpu=power8 -mvsx -maltivec -O3 -o altivec_benchmark altivec_benchmark.c
./altivec_benchmark
```

Expected output should show VSX acceleration working.

## Reporting Issues

When reporting bugs:

1. **Hardware Info**: CPU model, RAM, OS version
2. **Build Log**: Full cmake/make output
3. **Error Messages**: Exact error text
4. **Reproduction Steps**: Clear steps to reproduce

## Communication

- **Discord**: Join [RustChain Discord](https://discord.gg/tQ4q3z4M) for POWER8 discussions
- **Issues**: Use GitHub Issues for bugs and feature requests

## Recognition

Contributors will be mentioned in the README and credited appropriately.

---

*Built with 💚 on POWER8 hardware*
