# OnnxRuntime


Issue: https://github.com/microsoft/onnxruntime/issues/26351
https://github.com/microsoft/onnxruntime/issues/25936
https://github.com/microsoft/onnxruntime/pull/26671
Maybe build from source


```console
git clone --recursive https://github.com/microsoft/onnxruntime
cd onnxruntime
```

Build

```console
export CPLUS_INCLUDE_PATH=/usr/local/cuda/targets/sbsa-linux/include/cccl:$CPLUS_INCLUDE_PATH
export C_INCLUDE_PATH=/usr/local/cuda/targets/sbsa-linux/include/cccl:$C_INCLUDE_PATH
export CPATH=/usr/local/cuda/targets/sbsa-linux/include/cccl:$CPATH
./build.sh \
  --config Release \
  --build_shared_lib \
  --parallel \
  --use_cuda \
  --cuda_home /usr/local/cuda \
  --cudnn_home /usr/local/cuda \
  --cmake_extra_defines CMAKE_CUDA_ARCHITECTURES=90 \
  --build_wheel \
  --skip_tests
# Note: --cuda_version is optional - it auto-detects from /usr/local/cuda/version.json
```

sudo apt install python3-numpy python3-numpy-dev
sudo apt install build-essential python3-dev python3-pip

fix include path with 
https://github.com/microsoft/onnxruntime/pull/26671


Download cudnn

https://developer.nvidia.com/cudnn-downloads?target_os=Linux&target_arch=arm64-sbsa&Compilation=Native&Distribution=Ubuntu&target_version=24.04&target_type=deb_local&Configuration=Full

Maybe use this pre built wheel index

https://pypi.jetson-ai-lab.io/sbsa/cu130