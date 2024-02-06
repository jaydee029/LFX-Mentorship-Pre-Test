# LFX-Mentorship-Pre-Test
## Applying for 'Integrate whisper.cpp as a new WASI-NN backend' [#3170](https://github.com/WasmEdge/WasmEdge/issues/3169)

### System configuration
```
Ubuntu 20.04 Linux Distribution (64_86 architecture)
```
### Building WASMEDGE
The following branch was used [hydai/0.13.5_ggml_lts](https://github.com/WasmEdge/WasmEdge/tree/hydai/0.13.5_ggml_lts)

```
git clone -b  hydai/0.13.5_ggml_lts https://github.com/WasmEdge/WasmEdge.git
cd wasmedge
```
The wasmedge along with the ggml plugin was built using OpenBLAS in a Ubuntu distribution using the following code.
```
cmake -GNinja -Bbuild -DCMAKE_BUILD_TYPE=Release \
  -DWASMEDGE_PLUGIN_WASI_NN_BACKEND="GGML" \
  -DWASMEDGE_PLUGIN_WASI_NN_GGML_LLAMA_BLAS=ON \
  .

cmake --build build

cmake --install build
```

![wasm1](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm1.jpg)

![wasm2](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm2.jpg)

![wasm3](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm3.jpg)

![wasm4](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm4.jpg)
![wasm5](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm5.jpg)
![wasm6](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm6.jpg)
![wasm7](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm7.jpg)
![wasm8](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm8.jpg)
![wasm9](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm9.jpg)
![wasm10](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm10.jpg)
