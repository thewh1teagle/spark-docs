# Build OnnxRuntime

## Related Issues

- [onnxruntime#26351](https://github.com/microsoft/onnxruntime/issues/26351)
- [onnxruntime#25936](https://github.com/microsoft/onnxruntime/issues/25936)
- [onnxruntime#26671](https://github.com/microsoft/onnxruntime/pull/26671) — include path fix

## Prerequisites

```console
sudo apt install build-essential python3-dev python3-pip python3-numpy python3-numpy-dev
```

cuDNN should already be installed via the system packages (`cudnn9-cuda-13`).
If not, download from [NVIDIA cuDNN Downloads](https://developer.nvidia.com/cudnn-downloads?target_os=Linux&target_arch=arm64-sbsa&Compilation=Native&Distribution=Ubuntu&target_version=24.04&target_type=deb_local&Configuration=Full).

> **Note:** On DGX Spark, cuDNN headers are installed at `/usr/include/aarch64-linux-gnu/`
> and libraries at `/usr/lib/aarch64-linux-gnu/`, not under `/usr/local/cuda/`.
> The build command below uses custom cmake defines to handle this.

## Clone

```console
git clone --recursive https://github.com/microsoft/onnxruntime
cd onnxruntime
```

## Build

Apply the include path fix from [#26671](https://github.com/microsoft/onnxruntime/pull/26671), then:

```console
export CMAKE_BUILD_PARALLEL_LEVEL=$(nproc)

export CPLUS_INCLUDE_PATH=/usr/local/cuda/targets/sbsa-linux/include/cccl:$CPLUS_INCLUDE_PATH
export C_INCLUDE_PATH=/usr/local/cuda/targets/sbsa-linux/include/cccl:$C_INCLUDE_PATH
export CPATH=/usr/local/cuda/targets/sbsa-linux/include/cccl:$CPATH

./build.sh \
  --config Release \
  --build_shared_lib \
  --parallel \
  --use_cuda \
  --cuda_home /usr/local/cuda \
  --cudnn_home /usr \
  --cmake_extra_defines CMAKE_CUDA_ARCHITECTURES=120 \
    "CUDNN_INCLUDE_DIR=/usr/include/aarch64-linux-gnu" \
    "CUDNN_LIBRARY=/usr/lib/aarch64-linux-gnu/libcudnn.so" \
  --build_wheel \
  --skip_tests
# Note: --cuda_version is optional - it auto-detects from /usr/local/cuda/version.json
```

## Install

The wheel is output to `build/Linux/Release/dist/`:

```console
pip install "numpy<2"
pip install build/Linux/Release/dist/onnxruntime_gpu-*.whl
```

> **Note:** The wheel is built against NumPy 1.x, so `numpy<2` is required.

## Alternative: Pre-built Wheels

Instead of building from source, try the pre-built wheel index at
<https://pypi.jetson-ai-lab.io/sbsa/cu130>.