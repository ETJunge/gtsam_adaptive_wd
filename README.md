
# Adapted GTSAM

<!-- Badges -->
![Upstream GTSAM](https://img.shields.io/badge/Upstream%20GTSAM-4.3.0-green)
![License](https://img.shields.io/github/license/borglab/gtsam)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Ubuntu-blue)
![Python](https://img.shields.io/badge/python-3.11--3.14-blue)

This repository is a **fork of [borglab/gtsam](https://github.com/borglab/gtsam)** with custom modifications and build adaptations.

GTSAM (Georgia Tech Smoothing And Mapping) is a C++ library for smoothing and mapping (SAM) in robotics and computer vision, based on **factor graphs** and **Bayes networks**.

This fork is based on **GTSAM 4.3.0** and focuses on:
- Custom code modifications
- Improved Windows build stability

---

## 📑 Table of Contents

- [Features of This Fork](#-features-of-this-fork)
- [Supported Platforms](#-supported-platforms)
- [Prerequisites](#-prerequisites)
- [Build Instructions (Windows)](#-build-instructions-windows)
- [Build Instructions (Ubuntu 22.04)](#-build-instructions-ubuntu-2204)
- [Python Usage](#-python-usage)
- [Building Python Wheels (Windows)](#-building-python-wheels-windows)
- [References](#-references)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)
- [Disclaimer](#-disclaimer)
- [Contact & Support](#contact--support)

## 🚀 Features of This Fork

Compared to the upstream GTSAM repository, this fork includes:

- Custom modifications to the GTSAM source code, including variable-lag iSAM2 smoother, deep-clone of `iSAM` object, and advanced `CustomFactor` class
- Improved compatibility with **Windows 10/11 + Visual Studio 2022**
- Support for **Python 3.11-3.14**
- Adjusted CMake configuration for:
  - Boost
  - Intel MKL / TBB (optional)
- Disabled `/WX` (treat warnings as errors) in multiple projects for MSVC
- Fixed some bugs

> ⚠️ This repository is intended for **research, experimentation, and development**.  
> It is not an official replacement for upstream GTSAM.

---


## 🧩 Supported Platforms

| Platform | Compiler | Python | Status |
|--------|----------|--------|--------|
| Windows 10 / 11 | MSVC 14.3 (VS 2022) | 3.11 | Tested |
| Ubuntu 22.04 | GCC | 3.11-3.14 | Tested |

---


## 📦 Prerequisites

### Common
- CMake ≥ 3.16
- Git
- Boost ≥ 1.81
- Python (virtual environment recommended)

### Optional (Performance)
- Intel oneAPI (MKL, TBB, MPI)

---


## 🪟 Build Instructions (Windows)

**Tested environment**
- Windows 10/11
- Visual Studio 2022 (Desktop development with C++)
- Anaconda + Python 3.11
- GTSAM 4.3.0

### 1. Create Python Environment
```bash
conda create -n py3_11_gtsam_43 python=3.11
conda activate py3_11_gtsam_43
conda install numpy pyparsing matplotlib pybind11
pip install pybind11-stubgen cibuildwheel
```
### 2. Configure with CMake

From the repository root:
```bash
cmake -S . -B build ^
  -DCMAKE_BUILD_TYPE=Release ^
  -DGTSAM_BUILD_PYTHON=ON ^
  -DGTSAM_PYTHON_VERSION=3.11 ^
  -DGTSAM_ENABLE_BOOST_SERIALIZATION=OFF ^
  -DGTSAM_USE_BOOST_FEATURES=OFF
```
Optional flags:

```bash
-DGTSAM_BUILD_UNSTABLE=OFF
-DGTSAM_WITH_TBB=ON
-DGTSAM_WITH_EIGEN_MKL=ON
```
### 3. Build with Visual Studio

- Open build/GTSAM.sln as Administrator
- Switch to Release
- Build target: ALL_BUILD

## 🐧 Build Instructions (Ubuntu 22.04)
Some pre-built wheels are available in the `Releases` page of this repository
### 1. Install Dependencies
```bash
sudo apt update
sudo apt install build-essential cmake libboost-all-dev python3-pip git
```

### 2. Python Dependencies
```bash
pip install -r python/dev_requirements.txt
pip install pybind11-stubgen
```

### 3. Build
```bash
mkdir build && cd build
cmake .. -DGTSAM_BUILD_PYTHON=ON -DCMAKE_INSTALL_PREFIX=./install
make -j4
make python-install
```
⚠️ Avoid using sudo during installation to prevent permission issues.
## 🐍 Python Usage
After installation:
```bash
import gtsam
print(gtsam.__version__)
```
Python examples are located in:
```bash
python/gtsam/examples
```
## 📦 Building Python Wheels (Windows)
This fork supports building Python wheels using `cibuildwheel`.
```bash
python -m cibuildwheel build/python ^
  --output-dir ./wheelhouse ^
  --config-file pyproject.toml
```
Install locally:
```bash
pip install wheelhouse/gtsam-4.3.0.dev1-cp311-cp311-win_amd64.whl
```

## 📚 References
- GTSAM GitHub: https://github.com/borglab/gtsam
- GTSAM Documentation: https://gtsam.org
- F. Dellaert, Factor Graphs for Robot Perception, Foundations and Trends in Robotics, 2012

## 📄 License
This project follows the same license as the upstream GTSAM project.

See the upstream license file:
https://github.com/borglab/gtsam/blob/develop/LICENSE

## 🙏 Acknowledgements
- Frank Dellaert and the GTSAM authors
- BorgLab, Georgia Institute of Technology

## ⚠️ Disclaimer
This is an independent fork and is not officially supported by the GTSAM maintainers.