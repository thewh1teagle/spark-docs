# CTranslate2 Build Instructions

Complete guide for building CTranslate2 from source with CUDA and cuDNN support.

## Prerequisites

- CUDA 13.x installed
- cuDNN 9.x installed
- Python 3.12
- CMake
- Build tools (g++, make, etc.)

## Step 1: Clone Repository

```bash
git clone https://github.com/OpenNMT/CTranslate2.git --recursive
cd CTranslate2
```

## Step 2: CMake Configuration

Configure the build with CUDA, cuDNN, and Python support:

```bash
cmake -B build \
  -DWITH_CUDA=ON \
  -DCMAKE_BUILD_TYPE=Release \
  -DWITH_PYTHON=ON \
  -DWITH_MKL=OFF \
  -DOPENMP_RUNTIME=COMP \
  -DWITH_CUDNN=ON
```

**Note:** `-DOPENMP_RUNTIME=COMP` is required to use the compiler's OpenMP instead of Intel's libiomp5.

## Step 3: Build C++ Library

Build the C++ library using all available CPU cores:

```bash
cmake --build build --parallel
```

## Step 4: Setup Python Environment

Navigate to the Python directory and create a virtual environment with Python 3.12:

```bash
cd python
uv venv --python 3.12
source .venv/bin/activate
```

## Step 5: Install Build Dependencies

Install Python build requirements:

```bash
uv pip install -r install_requirements.txt
```

## Step 6: Build Python Wheel

Build the Python wheel package:

```bash
export CTRANSLATE2_ROOT=/path/to/CTranslate2/build
uv run python setup.py bdist_wheel
```

**Note:** Replace `/path/to/CTranslate2/build` with the actual path to your build directory (e.g., `/home/yakov/Documents/google-tts-generator/CTranslate2/build`).

The wheel will be created in `python/dist/` directory.

## Step 7: Install the Wheel

Install the built wheel:

```bash
uv pip install /path/to/CTranslate2/python/dist/ctranslate2-4.6.2-cp312-cp312-linux_aarch64.whl
```

## Step 8: Set Library Path (Runtime)

When running Python scripts that use CTranslate2, set the library path:

```bash
export LD_LIBRARY_PATH=/path/to/CTranslate2/build:$LD_LIBRARY_PATH
```

Or run your command with the environment variable:

```bash
LD_LIBRARY_PATH=/path/to/CTranslate2/build:$LD_LIBRARY_PATH uv run --no-sync your_script.py
```

## Complete Example

Here's a complete example with actual paths:

```bash
# Clone
git clone https://github.com/OpenNMT/CTranslate2.git --recursive
cd CTranslate2

# Configure
cmake -B build -DWITH_CUDA=ON -DCMAKE_BUILD_TYPE=Release -DWITH_PYTHON=ON -DWITH_MKL=OFF -DOPENMP_RUNTIME=COMP -DWITH_CUDNN=ON

# Build C++
cmake --build build --parallel

# Setup Python
cd python
uv venv --python 3.12
source .venv/bin/activate
uv pip install -r install_requirements.txt

# Build wheel
export CTRANSLATE2_ROOT=$(pwd)/../build
uv run python setup.py bdist_wheel

# Install wheel
uv pip install dist/ctranslate2-4.6.2-cp312-cp312-linux_aarch64.whl

# Set library path for runtime
export LD_LIBRARY_PATH=$(pwd)/../build:$LD_LIBRARY_PATH
```

## Troubleshooting

### Error: Intel OpenMP runtime libiomp5 not found
**Solution:** Use `-DOPENMP_RUNTIME=COMP` in CMake configuration.

### Error: cuDNN not found
**Solution:** Ensure cuDNN is installed and `-DWITH_CUDNN=ON` is set in CMake configuration.

### Error: libctranslate2.so.4: cannot open shared object file
**Solution:** Set `LD_LIBRARY_PATH` to point to the build directory containing `libctranslate2.so.4`.

### Error: Conv1D on GPU requires cuDNN
**Solution:** Rebuild with `-DWITH_CUDNN=ON` and reinstall the Python wheel.

