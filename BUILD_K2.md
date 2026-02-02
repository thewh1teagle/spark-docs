# Build K2

## Prerequisites

```console
sudo apt install cmake build-essential
```

## CPU Build

```console
git clone https://github.com/k2-fsa/k2.git k2-repo
cd k2-repo

export K2_MAKE_ARGS="-j$(nproc)"
export K2_CMAKE_ARGS="-DCMAKE_BUILD_TYPE=Release -DK2_WITH_CUDA=OFF -DCUDAARCHS="

uv venv -p3.12
uv pip install -r requirements.txt
uv run python setup.py bdist_wheel
uv pip install dist/*.whl
```

## CUDA Build

```console
git clone https://github.com/k2-fsa/k2.git k2-repo
cd k2-repo

# Enable CUDA
export K2_WITH_CUDA=1
export CUDA_HOME=/usr/local/cuda
export CUDACXX=$CUDA_HOME/bin/nvcc

# GB10 / sm_121
export TORCH_CUDA_ARCH_LIST="12.1"

export K2_MAKE_ARGS="-j$(nproc)"
export K2_CMAKE_ARGS="-DCMAKE_BUILD_TYPE=Release"

uv venv -p3.12 --seed
uv pip install -r requirements.txt
uv pip install -U torch torchaudio --index-url https://download.pytorch.org/whl/cu130
uv pip install numpy

uv run python setup.py bdist_wheel
uv pip install dist/*.whl
```