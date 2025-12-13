Build (CPU)

```console
sudo apt install cmake build-essential
git clone https://github.com/k2-fsa/k2.git k2-repo
export K2_MAKE_ARGS="-j$(nproc)"
export K2_CMAKE_ARGS="-DCMAKE_BUILD_TYPE=Release -DK2_WITH_CUDA=OFF -DCUDAARCHS="
uv run python setup.py install
```