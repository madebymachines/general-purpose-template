# Docker image for A1111 Stable Diffusion Web UI, Kohya_ss and ComfyUI

## Installs

* Ubuntu 22.04 LTS
* CUDA 11.8
* Python 3.10.12
* [Automatic1111 Stable Diffusion Web UI](
  https://github.com/AUTOMATIC1111/stable-diffusion-webui) 1.7.0
* [Dreambooth extension](
  https://github.com/d8ahazard/sd_dreambooth_extension) 1.0.14
* [Deforum extension](
  https://github.com/deforum-art/sd-webui-deforum)
* [ControlNet extension](
  https://github.com/Mikubill/sd-webui-controlnet) v1.1.440
* [After Detailer extension](
  https://github.com/Bing-su/adetailer) v24.1.2
* [Locon extension](
  https://github.com/ashleykleynhans/a1111-sd-webui-locon)
* [ReActor extension](https://github.com/Gourieff/sd-webui-reactor) (replaces roop)
* [Inpaint Anything extension](https://github.com/Uminosachi/sd-webui-inpaint-anything)
* [Infinite Image Browsing extension](https://github.com/zanllp/sd-webui-infinite-image-browsing)
* [CivitAI extension](https://github.com/civitai/sd_civitai_extension)
* [CivitAI Browser+ extension](https://github.com/BlafKing/sd-civitai-browser-plus)
* [Kohya_ss](https://github.com/bmaltais/kohya_ss) v22.6.2
* [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
* [ComfyUI Manager](https://github.com/ltdrdata/ComfyUI-Manager)
* Torch 2.0.1
* xformers 0.0.22
* Jupyter Lab
* [sd_xl_base_1.0.safetensors](
  https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors)
* [sd_xl_refiner_1.0.safetensors](
  https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/resolve/main/sd_xl_refiner_1.0.safetensors)
* [sdxl_vae.safetensors](
  https://huggingface.co/madebyollin/sdxl-vae-fp16-fix/resolve/main/sdxl_vae.safetensors)
* [inswapper_128.onnx](
  https://github.com/facefusion/facefusion-assets/releases/download/models/inswapper_128.onnx)
* [runpodctl](https://github.com/runpod/runpodctl)
* [OhMyRunPod](https://github.com/kodxana/OhMyRunPod)
* [RunPod File Uploader](https://github.com/kodxana/RunPod-FilleUploader)
* [croc](https://github.com/schollz/croc)
* [rclone](https://rclone.org/)
* [Application Manager](https://github.com/ashleykleynhans/app-manager)

## Building the Docker image

need at least 32GB of CPU/system memory (not VRAM) due to the large size of the models.  If you have less
than 32GB of system memory, you can comment out or remove the code in the
`Dockerfile` that caches the models.

```bash
# Clone the repo
git clone https://github.com/madebymachines/ultimate-stable-diffusion.git

# Download the models
cd stable-diffusion-docker
wget https://huggingface.co/sd-legacy/stable-diffusion-v1-5/resolve/main/v1-5-pruned.safetensors
wget https://huggingface.co/stabilityai/sd-vae-ft-mse-original/resolve/main/vae-ft-mse-840000-ema-pruned.safetensors
wget https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0.safetensors
wget https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/resolve/main/sd_xl_refiner_1.0.safetensors
wget https://huggingface.co/madebyollin/sdxl-vae-fp16-fix/resolve/main/sdxl_vae.safetensors

# Build and tag the image
docker build -t username/image-name:1.0.0 .

# Log in to Docker Hub
docker login

# Push the image to Docker Hub
docker push username/image-name:1.0.0
```

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
  -p 3010:3011 \
  -p 3020:3021 \
  -p 6006:6066 \
  -p 8000:8000 \
  -p 8888:8888 \
  -p 2999:2999 \
  -e JUPYTER_PASSWORD=Jup1t3R! \
  -e ENABLE_TENSORBOARD=1 \
  ashleykza/stable-diffusion-webui:latest
```

### Ports

| Connect Port | Internal Port | Description                   |
|--------------|---------------|-------------------------------|
| 3000         | 3001          | A1111 Stable Diffusion Web UI |
| 3010         | 3011          | Kohya_ss                      |
| 3020         | 3021          | ComfyUI                       |
| 6006         | 6066          | Tensorboard                   |
| 8000         | 8000          | Application Manager           |
| 8888         | 8888          | Jupyter Lab                   |
| 2999         | 2999          | RunPod File Uploader          |

### Environment Variables

| Variable           | Description                                  | Default  |
|--------------------|----------------------------------------------|----------|
| JUPYTER_PASSWORD   | Password for Jupyter Lab                     | Jup1t3R! |
| DISABLE_AUTOLAUNCH | Disable Web UIs from launching automatically | enabled  |
| ENABLE_TENSORBOARD | Enables Tensorboard on port 6006             | enabled  |

## Logs

| Application             | Log file                     |
|-------------------------|------------------------------|
| Stable Diffusion Web UI | /workspace/logs/webui.log    |
| Kohya SS                | /workspace/logs/kohya_ss.log |
| ComfyUI                 | /workspace/logs/comfyui.log  |