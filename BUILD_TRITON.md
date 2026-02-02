# Build Triton

For use with [Unsloth](https://github.com/unslothai/unsloth).

```console
git clone https://github.com/triton-lang/triton.git
cd triton
git checkout c5d671f91d90f40900027382f98b17a3e04045f6

export CMAKE_BUILD_PARALLEL_LEVEL=$(nproc)

uv sync --active
uv pip install setuptools pybind11
uv run --active python setup.py bdist_wheel
```
