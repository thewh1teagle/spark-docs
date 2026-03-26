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


 For the CPU build, add these notes:

  1. Remove -DCUDAARCHS= from K2_CMAKE_ARGS — passing an empty value causes CMake to error on newer versions                                                     
  2. Install CPU torch explicitly before building: uv pip install torch --index-url https://download.pytorch.org/whl/cpu — even with K2_WITH_CUDA=OFF, torch's
  cmake will try to enable CUDA if it finds a CUDA-built torch in the venv                                                                                       
  3. Use .venv/bin/python setup.py bdist_wheel instead of uv run python — uv run resolves to the workspace venv if run inside a larger project directory, picking
   up the wrong torch                                                                                                                                            
  4. Unset CUDA env vars if you previously attempted a CUDA build in the same shell: unset CUDACXX K2_WITH_CUDA TORCH_CUDA_ARCH_LIST

                                                                                                                                       
                                                                                                                                       
                                                                                                                                       
                                                                                                                                        Two fixes:                                                                                                                                                     
                                                                                                                                                               
  1. -DCUDAARCHS=121 in K2_CMAKE_ARGS — tells cmake's arch detection to use sm_121 instead of autodetecting. Without it, cmake's CUDA_DETECT_INSTALLED_GPUS      
  returns 12.1(2.0) which the old cmake script doesn't understand.
  2. SEND_ERROR → WARNING in select_compute_arch.cmake lines 250/254 — the unknown arch name 12.1(2.0) triggered a SEND_ERROR which causes cmake to mark         
  configure as incomplete and skip generating the Makefile, even though k2's own arch candidate loop (lines 294-308) correctly handles sm_121 independently.     
   
  Together: cmake no longer errors out on the unknown arch string, and k2's own loop builds for all candidates including 121.    
