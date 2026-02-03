# Build whisper.cpp

## Clone

```bash
git clone https://github.com/ggerganov/whisper.cpp.git
cd whisper.cpp
```

## Build with CUDA

```bash
cmake -B build -DGGML_CUDA=ON -DCMAKE_CUDA_ARCHITECTURES=121
cmake --build build -j$(nproc)
```

Binaries are output to `build/bin/`:
- `whisper-cli` — main transcription tool
- `whisper-server` — HTTP server
- `whisper-bench` — benchmarking
- `whisper-quantize` — model quantization

## Download a Model

```bash
./models/download-ggml-model.sh base.en
```

## Test

```bash
./build/bin/whisper-cli -m models/ggml-base.en.bin -f samples/jfk.wav
```