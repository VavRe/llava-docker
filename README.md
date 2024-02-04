# Docker image for LLaVA: Large Language and Vision Assistant

## Installs

* Ubuntu 22.04 LTS
* CUDA 11.8
* Python 3.10.12
* [LLaVA](
  https://github.com/haotian-liu/llava) v1.2.0 (LLaVA 1.6)
* Torch 2.0.1
* xformers 0.0.22
* [runpodctl](https://github.com/runpod/runpodctl)
* [croc](https://github.com/schollz/croc)
* [rclone](https://rclone.org/)
* speedtest-cli
* screen
* tmux
* llava-v1.6-mistral-7b model

## Available on RunPod

This image is designed to work on [RunPod](https://runpod.io?ref=2xxro4sy).
You can use my custom [RunPod template](
https://runpod.io/gsc?template=g7wd33iuwv&ref=2xxro4sy)
to launch it on RunPod.

## Running Locally

### Install Nvidia CUDA Driver

- [Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
- [Windows](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html)

### Start the Docker container

```bash
docker run -d \
  --gpus all \
  -v /workspace \
  -p 3000:3001 \
  -p 8888:8888 \
  -e JUPYTER_PASSWORD=Jup1t3R! \
  ashleykza/llava:latest
```

You can obviously substitute the image name and tag with your own.

## Models

> [!IMPORTANT]
> If you select a 13B or larger model, CUDA will result in OOM errors
> with a GPU that has less than 48GB of VRAM, so A6000 or higher is
> recommended for 13B.

You can add an environment called `MODEL` to your Docker container to
specify the model that should be downloaded.  If the `MODEL` environment
variable is not set, the model will default to `liuhaotian/llava-v1.6-mistral-7b`.

### LLaVA-v1.6

| Model                                                                            | Environment Variable Value       | Version    | LLM           | Default |
|----------------------------------------------------------------------------------|----------------------------------|------------|---------------|---------|
| [llava-v1.6-vicuna-7b](https://huggingface.co/liuhaotian/llava-v1.6-vicuna-7b)   | liuhaotian/llava-v1.6-vicuna-7b  | LLaVA-1.6  | Vicuna-7B     | no      |
| [llava-v1.6-vicuna-13b](https://huggingface.co/liuhaotian/llava-v1.6-vicuna-13b) | liuhaotian/llava-v1.6-vicuna-13b | LLaVA-1.6  | Vicuna-13B    | no      |
| [llava-v1.6-mistral-7b](https://huggingface.co/liuhaotian/llava-v1.6-mistral-7b) | liuhaotian/llava-v1.6-mistral-7b | LLaVA-1.6  | Mistral-7B    | yes     |
| [llava-v1.6-34b](https://huggingface.co/liuhaotian/llava-v1.6-34b)               | liuhaotian/llava-v1.6-34b        | LLaVA-1.6  | Hermes-Yi-34B | no      |

### LLaVA-v1.5

| Model                                                                            | Environment Variable Value       | Version   | Size | Default |
|----------------------------------------------------------------------------------|----------------------------------|-----------|------|---------|
| [llava-v1.5-7b](https://huggingface.co/liuhaotian/llava-v1.5-7b)                 | liuhaotian/llava-v1.5-7b         | LLaVA-1.5 | 7B   | no      |
| [llava-v1.5-13b](https://huggingface.co/liuhaotian/llava-v1.5-13b)               | liuhaotian/llava-v1.5-13b        | LLaVA-1.5 | 13B  | no      |
| [BakLLaVA-1](https://huggingface.co/SkunkworksAI/BakLLaVA-1)                     | SkunkworksAI/BakLLaVA-1          | LLaVA-1.5 | 7B   | no      |

## Ports

| Port | Description |
|------|-------------|
| 3000 | LLaVA       |
| 8888 | Jupyter Lab |

## Environment Variables

| Variable           | Description                                 | Default  |
|--------------------|---------------------------------------------|----------|
| JUPYTER_PASSWORD   | Password for Jupyter Lab                    | Jup1t3R! |
| DISABLE_AUTOLAUNCH | Disable LLaVA from launching automatically  | enabled  |
| MODEL                              | The path of the Huggingface model               | liuhaotian/llava-v1.6-mistral-7b |

## Flask API

### Add port 5000

If you are running the RunPod template, edit your pod and add HTTP port 5000.

If you are running locally, add a port mapping for port 5000.

### Starting the Flask API

```bash
# Stop model worker and controller to free up VRAM
fuser -k 10000/tcp 40000/tcp

# Install dependencies
source /workspace/venv/bin/activate
pip3 install flask protobuf
cd /workspace/LLaVA
export HF_HOME="/workspace"
python -m llava.serve.api -H 0.0.0.0 -p 5000
```

### Sending requests to the Flask API

You can use the [test script](
https://github.com/ashleykleynhans/LLaVA/blob/main/llava/serve/test_api.py)
to test your API.

## Acknowledgements

1. Matthew Berman for giving me a demo on LLaVA, as well as his amazing
   [YouTube videos](https://www.youtube.com/@matthew_berman/videos]).

## Community and Contributing

Pull requests and issues on [GitHub](https://github.com/ashleykleynhans/llava-docker)
are welcome. Bug fixes and new features are encouraged.

You can contact me and get help with deploying your container
to RunPod on the RunPod Discord Server below,
my username is **ashleyk**.

<a target="_blank" href="https://discord.gg/pJ3P2DbUUq">![Discord Banner 2](https://discordapp.com/api/guilds/912829806415085598/widget.png?style=banner2)</a>

## Appreciate my work?

<a href="https://www.buymeacoffee.com/ashleyk" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>
