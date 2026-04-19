# llama.cpp Custom Build Workflows

This repository contains GitHub Actions workflows for building custom llama.cpp binaries with various configurations. Instead of forking the entire llama.cpp repository, these workflows download specific releases and build them with your preferred settings.

## 🚀 Quick Start

1. Copy this folder to a new GitHub repository
2. Go to **Actions** tab in your repository
3. Select the workflow you want to run
4. Click **Run workflow** and configure your build options
5. Download the built artifacts when complete

## 📋 Available Workflows

### 🐧 Linux Builds

#### Linux CUDA Build (`build-linux-cuda.yml`)
Build llama.cpp with NVIDIA CUDA support for Linux (x86_64).

**Configurable Options:**
- **Source repo**: GitHub repo to build from (default: `ggml-org/llama.cpp`). Useful for building forks such as [llama-cpp-turboquant](https://github.com/TheTom/llama-cpp-turboquant)
- **llama.cpp version**: Specific tag (e.g., `b1234`) or `latest`
- **CUDA version**: 12.8.0, 12.6.2, 12.4.0, 12.2.0, 11.8.0
- **Base OS**: Container base OS for binary compatibility (`rockylinux8`, `ubuntu22.04`, `ubuntu20.04`)
- **GPU architectures**: Semicolon-separated list (e.g., `70;75;80;86;89;90`)
  - 70 = Volta (V100)
  - 75 = Turing (RTX 20xx, T4)
  - 80 = Ampere (A100, RTX 30xx)
  - 86 = Ampere (RTX 30xx mobile)
  - 89 = Ada Lovelace (RTX 40xx)
  - 90 = Hopper (H100)
- **Build type**: Release or RelWithDebInfo

**Build notes:**
- NCCL is disabled (`-DGGML_CUDA_USE_NCCL=OFF`) to produce self-contained binaries that work on airgapped machines without requiring `libnccl.so`. Multi-GPU tensor splitting still works via llama.cpp's native CUDA peer-to-peer path.

**Uses**: NVIDIA CUDA container for consistent build environment
**Output**: Includes SHA256 checksum, detailed metadata, and source repo link in release notes

**Example Use Cases:**
- For RTX 3090: Use CUDA 12.6.2 with architecture `86`
- For RTX 4090: Use CUDA 12.6.2 with architecture `89`
- For A100: Use CUDA 12.6.2 with architecture `80`
- For TurboQuant fork: Set source repo to `TheTom/llama-cpp-turboquant`

---

#### Linux ROCm Build (`build-linux-rocm.yml`)
Build llama.cpp with AMD ROCm support for Linux (x86_64).

**Configurable Options:**
- **Source repo**: GitHub repo to build from (default: `ggml-org/llama.cpp`)
- **llama.cpp version**: Specific tag or `latest`
- **ROCm version**: 6.1.2, 6.0.0, 5.7.0
- **GPU architectures**: Semicolon-separated list (e.g., `gfx900;gfx906;gfx908;gfx90a;gfx1030;gfx1100`)
  - gfx900 = Vega (RX Vega)
  - gfx906 = Vega 20 (Radeon VII, MI50)
  - gfx908 = CDNA (MI100)
  - gfx90a = CDNA2 (MI200 series)
  - gfx1030 = RDNA2 (RX 6000 series)
  - gfx1100 = RDNA3 (RX 7000 series)
- **Build type**: Release or RelWithDebInfo

**Uses**: ROCm container for consistent build environment
**Output**: Includes SHA256 checksum and detailed metadata

**Example Use Cases:**
- For RX 6800 XT: Use ROCm 6.1.2 with architecture `gfx1030`
- For RX 7900 XTX: Use ROCm 6.1.2 with architecture `gfx1100`

---

#### Linux CPU Build (`build-linux-cpu.yml`)
Build llama.cpp for CPU-only inference on Linux (x86_64).

**Configurable Options:**
- **Source repo**: GitHub repo to build from (default: `ggml-org/llama.cpp`)
- **llama.cpp version**: Specific tag or `latest`
- **Ubuntu version**: 24.04, 22.04, 20.04
- **Build type**: Release or RelWithDebInfo
- **Enable OpenBLAS**: true/false (accelerated CPU inference)
- **Enable OpenCL**: true/false (GPU acceleration via OpenCL)

**Example Use Cases:**
- For server deployment: Ubuntu 22.04 with OpenBLAS enabled
- For cloud deployment: Latest Ubuntu with optimized settings

---

#### Linux ARM64 Cross-Compile Build (`build-linux-arm64-cross.yml`)
Cross-compile llama.cpp for ARM64/aarch64 Linux systems (e.g., Raspberry Pi 4/5, AWS Graviton, NVIDIA Jetson).

**Configurable Options:**
- **Source repo**: GitHub repo to build from (default: `ggml-org/llama.cpp`)
- **llama.cpp version**: Specific tag or `latest`
- **Build type**: Release or RelWithDebInfo
- **Enable OpenBLAS**: true/false (ARM NEON optimizations)
- **Enable Vulkan**: true/false (GPU acceleration for ARM Mali, etc.)

**Uses**: Cross-compilation from x86_64 to ARM64 using `aarch64-linux-gnu-gcc`
**Output**: Binaries compatible with ARM64 Linux systems

**Example Use Cases:**
- For Raspberry Pi 4/5: Enable OpenBLAS for NEON acceleration
- For NVIDIA Jetson: Enable Vulkan for GPU support
- For AWS Graviton instances: Optimize with OpenBLAS

---

### 🪟 Windows Builds

#### Windows CUDA Build (`build-windows-cuda.yml`)
Build llama.cpp with NVIDIA CUDA support for Windows.

**Configurable Options:**
- **Source repo**: GitHub repo to build from (default: `ggml-org/llama.cpp`)
- **llama.cpp version**: Specific tag or `latest`
- **CUDA version**: 12.6.2, 12.4.0, 12.2.0, 11.8.0
- **GPU architectures**: Semicolon-separated list
- **Build type**: Release or RelWithDebInfo
- **Enable cuBLAS**: true/false

**Note:** Includes CUDA runtime DLLs in the output package.

---

#### Windows CPU Build (`build-windows-cpu.yml`)
Build llama.cpp for CPU-only inference on Windows.

**Configurable Options:**
- **Source repo**: GitHub repo to build from (default: `ggml-org/llama.cpp`)
- **llama.cpp version**: Specific tag or `latest`
- **Architecture**: x64, ARM64
- **Compiler**: MSVC or Clang
- **Build type**: Release or RelWithDebInfo
- **Enable OpenBLAS**: true/false
- **Enable OpenCL**: true/false

---

### 🍎 macOS Builds

#### macOS Metal Build (`build-macos-metal.yml`)
Build llama.cpp with Apple Metal GPU acceleration for macOS.

**Configurable Options:**
- **Source repo**: GitHub repo to build from (default: `ggml-org/llama.cpp`)
- **llama.cpp version**: Specific tag or `latest`
- **Architecture**: arm64 (Apple Silicon), x86_64 (Intel), or universal (both)
- **Build type**: Release or RelWithDebInfo
- **Enable Metal**: true/false

**Example Use Cases:**
- For M1/M2/M3 Macs: arm64 with Metal enabled
- For Intel Macs: x86_64
- For distribution: universal binary

---

## 🎯 Common GPU Architecture Quick Reference

### NVIDIA CUDA Architectures
```
60 = Pascal (P100, GTX 10xx)
61 = Pascal (GTX 1050-1080Ti)
70 = Volta (V100, Titan V)
75 = Turing (RTX 20xx, GTX 16xx, T4)
80 = Ampere (A100, A30)
86 = Ampere (RTX 30xx, A10)
89 = Ada Lovelace (RTX 40xx, L40)
90 = Hopper (H100, H200)
```

### AMD ROCm Architectures
```
gfx900  = Vega (RX Vega 56/64)
gfx906  = Vega 20 (Radeon VII, MI50)
gfx908  = CDNA (MI100)
gfx90a  = CDNA2 (MI200 series)
gfx1030 = RDNA2 (RX 6600-6900)
gfx1100 = RDNA3 (RX 7600-7900)
gfx1101 = RDNA3 (RX 7600M-7900M)
```

## 📦 Output Artifacts

Each workflow produces a compressed archive containing:
- **bin/**: Compiled binaries (`llama-cli`, `llama-server`, examples)
- **lib/**: Libraries (if applicable)
- **meta/**: Detailed metadata files
  - `git-describe.txt`: llama.cpp version info
  - `*-version.txt`: Build tool versions (gcc, cmake, nvcc, etc.)
  - `file-bin.txt` & `ldd-bin.txt`: Binary information and dependencies
  - `os-release.txt`: Build OS information
- **BUILD_INFO.txt**: Human-readable build summary
- **Runtime dependencies**: CUDA/ROCm/OpenBLAS DLLs (when applicable)
- **SHA256 checksum**: For Linux GPU builds (`.tar.gz.sha256`)

Artifacts are retained for **30 days** by default.

## 🔧 Customization Examples

### Building for Multiple GPU Architectures
If you have mixed hardware (e.g., RTX 3090 and RTX 4090), include both:
```
GPU architectures: 86;89
```

### Building for Specific CUDA Version
If your system has CUDA 11.8 installed:
```
CUDA version: 11.8.0
GPU architectures: 75;80;86
```

### Building for Production
For production deployments, use:
```
Build type: Release
llama.cpp version: (specific stable tag, e.g., b1234)
```

### Building for Development/Debugging
For development with debug symbols:
```
Build type: RelWithDebInfo
```

## 🛠️ Advanced Usage

### Pinning to Specific llama.cpp Versions
Instead of using `latest`, specify exact tags for reproducible builds:
```
llama.cpp version: b3950
```

### Optimizing Binary Size
For smaller binaries:
- Use `Release` build type
- Disable unused features (e.g., if CPU-only, don't enable GPU backends)
- Compile only for specific GPU architectures you need

### Building for ARM Devices
Use the **Linux ARM64 Cross-Compile** workflow for:
- **Raspberry Pi 4/5**: Enable OpenBLAS for NEON acceleration
- **NVIDIA Jetson Nano/Xavier/Orin**: Enable Vulkan for GPU support
- **AWS Graviton2/3**: Optimize with OpenBLAS
- **Oracle Cloud ARM instances**: Build optimized binaries

**Note**: Cross-compiled binaries are built on x86_64 and may not be optimized for specific ARM cores. For maximum performance on specific hardware, consider building natively on the target device.

### Cross-Compilation Notes
The ARM64 workflow uses cross-compilation which:
- ✅ Works on standard GitHub runners (no ARM runners needed)
- ✅ Produces compatible ARM64 binaries
- ⚠️ May not have all native optimizations
- ⚠️ Cannot be tested in the build environment

For best performance, test on your target ARM hardware after deployment.

## 📝 Notes

- **First run**: Workflows may take 10-30 minutes depending on configuration
- **Caching**: Not currently implemented, but could be added for faster subsequent builds
- **CUDA versions**: Match your local CUDA installation version for best compatibility
- **GPU architectures**: Only include architectures you need to reduce binary size and compilation time
- **OpenBLAS**: Significantly improves CPU inference performance (~2-4x speedup)
- **ARM Cross-compilation**: Binaries are built on x86_64 runners using cross-compilers
- **Containers**: GPU workflows use official NVIDIA/ROCm containers for consistency
- **Metadata**: All builds include detailed metadata about versions, dependencies, and build environment

## 🔗 Resources

- [llama.cpp Official Repository](https://github.com/ggml-org/llama.cpp)
- [NVIDIA CUDA Compute Capabilities](https://developer.nvidia.com/cuda-gpus)
- [AMD ROCm Documentation](https://rocm.docs.amd.com/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## 📄 License

These workflows are provided as-is for building llama.cpp. llama.cpp itself is licensed under MIT - see the [llama.cpp repository](https://github.com/ggml-org/llama.cpp) for details.

---

**Happy Building! 🚀**
