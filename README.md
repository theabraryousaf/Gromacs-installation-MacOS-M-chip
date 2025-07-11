# Gromacs-installation-MacOS-M-chip
This guide walks you through installing **GROMACS 2025.2** with **OpenCL GPU acceleration** on a MacBook Pro with an M4 chip. It’s tailored for Apple Silicon and avoids common pitfalls like OpenMP issues with Apple Clang.
## Prerequisites

### Install Xcode Command Line Tools

```bash
xcode-select --install
```

### Install Homebrew (if not already installed)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Install Required Packages

```bash
brew install cmake fftw git
```

---

## Build and Install GROMACS

### 1. Download and Extract GROMACS

```bash
curl -O https://ftp.gromacs.org/gromacs/gromacs-2025.2.tar.gz
tar -xzf gromacs-2025.2.tar.gz
cd gromacs-2025.2
```

### 2. Create a Build Directory

```bash
mkdir build
cd build
```

### 3. Configure with CMake (OpenCL, OpenMP disabled)

```bash
cmake .. \
  -DGMX_BUILD_OWN_FFTW=ON \
  -DGMX_GPU=OpenCL \
  -DGMX_OPENMP=OFF \
  -DREGRESSIONTEST_DOWNLOAD=ON \
  -DCMAKE_INSTALL_PREFIX=/opt/gromacs
```

### 4. Compile and Install

```bash
make -j$(sysctl -n hw.logicalcpu)
make check
sudo make install
```

---

## Post-Installation Setup

### Source GROMACS Environment

```bash
source /opt/gromacs/bin/GMXRC
```

### To make this permanent:

#### For Zsh

```bash
echo 'source /opt/gromacs/bin/GMXRC' >> ~/.zshrc
source ~/.zshrc
```

#### For Bash

```bash
echo 'source /opt/gromacs/bin/GMXRC' >> ~/.bash_profile
source ~/.bash_profile
```

---

## Verify Installation

### Check GROMACS Version and GPU Support

```bash
gmx mdrun -version
```

You should see output indicating:

- **GPU support: OpenCL**  
- **OpenCL device: Apple M4 GPU**  
- **FFT library: VkFFT**

## Optional Cleanup

After installation, you can safely delete the source files:

```bash
cd ~
rm -rf gromacs-2025.2*
