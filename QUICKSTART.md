# Quick Start Guide

## Overview
This folder contains GitHub Actions workflows for building custom llama.cpp binaries **without forking** the main repository. The workflows download official llama.cpp releases and build them with your custom configurations.

## Available Workflows

| Workflow | Platform | Hardware | File |
|----------|----------|----------|------|
| Linux CUDA | Linux x86_64 | NVIDIA GPU | `build-linux-cuda.yml` |
| Linux ROCm | Linux x86_64 | AMD GPU | `build-linux-rocm.yml` |
| Linux CPU | Linux x86_64 | CPU-only | `build-linux-cpu.yml` |
| Linux ARM64 | Linux ARM64 | CPU/Vulkan | `build-linux-arm64-cross.yml` |
| Windows CUDA | Windows x64 | NVIDIA GPU | `build-windows-cuda.yml` |
| Windows CPU | Windows x64/ARM64 | CPU-only | `build-windows-cpu.yml` |
| macOS Metal | macOS | Apple Silicon/Intel | `build-macos-metal.yml` |

## Setup Steps

1. **Create a new GitHub repository**
   ```bash
   # Example:
   # Repository name: my-llama-builds
   ```

2. **Copy this folder to your repository**
   ```bash
   # Copy the entire llama-cpp-custom-builds folder
   # to your new repository root
   ```

3. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Add custom llama.cpp build workflows"
   git push origin main
   ```

4. **Run a workflow**
   - Go to your repository on GitHub
   - Click **Actions** tab
   - Select a workflow (e.g., "Linux CUDA Custom Build")
   - Click **Run workflow** button
   - Fill in your options:
     - llama.cpp version (or use "latest")
     - CUDA/ROCm version
     - GPU architectures
     - Build options
   - Click **Run workflow**

5. **Download artifacts**
   - Wait for the workflow to complete (10-30 minutes)
   - Click on the completed workflow run
   - Scroll down to **Artifacts** section
   - Download your custom build

## Common Configurations

### NVIDIA RTX 4090 (Linux)
```yaml
Workflow: build-linux-cuda.yml
llama.cpp version: latest
CUDA version: 12.6.2
GPU architectures: 89
Build type: Release
Enable cuBLAS: true
Enable Flash Attention: true
```

### AMD RX 7900 XTX (Linux)
```yaml
Workflow: build-linux-rocm.yml
llama.cpp version: latest
ROCm version: 6.1.2
GPU architectures: gfx1100
Build type: Release
```

### Raspberry Pi 5 (ARM64)
```yaml
Workflow: build-linux-arm64-cross.yml
llama.cpp version: latest
Build type: Release
Enable OpenBLAS: true
Enable Vulkan: false
```

### macOS Apple Silicon (M1/M2/M3)
```yaml
Workflow: build-macos-metal.yml
llama.cpp version: latest
Architecture: arm64
Build type: Release
Enable Metal: true
```

### Windows CPU (with OpenBLAS)
```yaml
Workflow: build-windows-cpu.yml
llama.cpp version: latest
Architecture: x64
Compiler: msvc
Build type: Release
Enable OpenBLAS: true
Enable OpenCL: false
```

## Key Features

✅ **No fork required** - Workflows download official releases
✅ **Customizable** - Choose CUDA version, GPU arch, build options
✅ **Reproducible** - Pin to specific llama.cpp versions
✅ **Metadata-rich** - Includes build info, checksums, dependencies
✅ **Container-based** - GPU builds use official containers
✅ **Cross-compilation** - Build ARM binaries on x86 runners

## Tips

- **For production**: Pin to specific llama.cpp version tags (not "latest")
- **For testing**: Use "latest" to always get newest release
- **GPU architectures**: Only include what you need to reduce build time
- **OpenBLAS**: Enable for significant CPU performance boost (2-4x)
- **ARM builds**: Cross-compiled, test on actual hardware

## Support

For issues with:
- **llama.cpp itself**: https://github.com/ggml-org/llama.cpp/issues
- **These workflows**: Check workflow logs in Actions tab

## License

These workflows are provided as-is. llama.cpp is MIT licensed.
