# OnnxRuntime


Maybe build from source


```console
git clone --recursive https://github.com/microsoft/onnxruntime
cd onnxruntime
```

Build

```console
./build.sh \
  --config Release \
  --parallel \
  --update \
  --build \
  --build_wheel \
  --use_cuda \
  --cuda_home /usr/local/cuda \
  --cudnn_home /usr/lib/aarch64-linux-gnu \
  --skip_tests
```

Maybe use this pre built wheel index

https://pypi.jetson-ai-lab.io/sbsa/cu130