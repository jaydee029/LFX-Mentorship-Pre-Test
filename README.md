# LFX-Mentorship-Pre-Test
## Applying for 'Integrate whisper.cpp as a new WASI-NN backend' [#3170](https://github.com/WasmEdge/WasmEdge/issues/3169)

## Table of Contents
- [Profile Information](#Profile-Information)
- [System configuration](#System-configuration)
- [Whisper.cpp execution](#Whisper.cpp-execution)
- [Building WASMEDGE](#Building-WASMEDGE)
- [Building a WASM example](#Building-a-WASM-example)
- [Building using WebUI](#Building-using-WebUI)


### Profile Information
Name: Dhruv Jain

Email: dhruvj797@gmail.com

### System configuration
```
Ubuntu 20.04 Linux Distribution (64_86 architecture)
```

### Whisper.cpp execution
Whisper.cpp is c/c++ port of OpenAI's whisper AI model, which helps in transcription of any audio.It offers multi-speaker and multi language support.
whisper.cpp is a light weight inference which allows easy integration in various applications.

First we install whisper.cpp following the basic steps from the project documentation.
```
git clone https://github.com/ggerganov/whisper.cpp.git
```
We then install one of the available models , then build the model using the following commands
```
bash ./models/download-ggml-model.sh base.en
make base.en
```
In the above commands `base.en` was installed and built.

Here is the basic menu printed after installation.

![whis1](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/whis1.jpg)

Now we translate one of the audio samples provided [here](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/files/jfk.wav) in the .wav format. Along with that we have enabled `-pc` which enables colored transcription and `-pp` which prints transcription progress.
The following command was used, `-f` symbolises that a file is being provided for transcription.
```
./main -f samples/jfk.wav -pp -pc
```
![whis2](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/whis2.jpg)

The above image shows the transcription of the audio file along with the timestamps, and progress percentage.

Next I have used an audio sample recorded in my voice [here](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/files/Recording_2.wav). In the below commands we use an extra argument `-oj` which saves the transcription in json format.
```
./main -f samples/Recording_2.wav -pc -oj
```
![whis3](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/whis3.jpg)

The above image shows transcription of the audio , along with the timestamps. The save json file can be seen [here](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/files/Recording_2.wav.json).

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

### Building a WASM example
In this example we are building a chat server which uses one of the llama models , the model can be then interacted with using the cli as well as frontend.

In the following steps the api server file was downloaded , along with a few llama models.For the sake of efficiency a smaller model was used ie **TinyLlama-1.1B-Chat-v1**
as seen in the log files.

![wasm2](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm2.jpg)
![wasm11](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm11.jpg)

The wasm server was then hosted locally using wasmedge runtime through the following command.

![wasm3](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm3.jpg)

The `--nn-preload` command in `wasmedge --dir .:. --nn-preload` allows the user to run the preloaded models in the WASINN plugin.
The server then starts at the default port ie `8080` as seen above.

We open a different terminal after this and start querying the model using a predefined json format.
We use the following command to test the server 
```
curl -X POST http://localhost:8080/v1/chat/completions -H 'accept:application/json' -H 'Content-Type: application/json' -d '{"messages":[{"role":"system", "content": "You are a helpful assistant."}, {"role":"user", "content": "What is web assembly?"}], "model":"llama-2-chat"}'
```
![wasm4](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm4.jpg)

The above image shows the response of the question asked, we recieve the answer in a json format , where the system informs us about the token usage, operation performed and other choices made.
Another operation that we are performing is chat completions, we give the model an incomplete sentence/phrase and it tries to complete it. The following format is used for querying the server.
```
curl -X POST http://50.112.58.64:8080/v1/completions \
    -H 'accept:application/json' \
    -H 'Content-Type: application/json' \
    -d '{"prompt":["Long long ago, "], "model":"tinyllama"}'
 ```

![wasm5](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm5.jpg)

The above image shows the result of chat completions in a json format.
Below is another example of server querying, using the same json format.

![wasm6](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm6.jpg)

### Building using WebUI
The provided web UI template was downloaded on the system using the curl command.The installed file is then extracted.
![wasm8](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm8.jpg)
The web ui is then executed along with the already installed **Tiny llama model**. We use the following command to do the same
```
wasmedge --dir .:. --nn-preload default:GGML:AUTO:TinyLlama-1.1B-Chat-v1.0-GGUF/resolve/main/TinyLlama-1.1B-Chat-v1.0-Q5_K_M.gguf llama-api-server.wasm -p llama-2-chat
```
![wasm9](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm9.jpg)

Once the server is live , we open the `https://localhost:8080` on our local systems to query the wasm-llama server using the web ui.

![wasm10](https://github.com/jaydee029/LFX-Mentorship-Pre-Test/blob/main/images/wasm10.jpg)
The above process shows building of llama server using a wasm runtime, and was executed succesfully.
Note: The accuracy of answers depends upon the llama model used which in this case is **TinyLlama-1.1B-Chat-v1**
