# MVEdit

Official PyTorch implementation of the paper:

**Generic 3D Diffusion Adapter Using Controlled Multi-View Editing**
<br>
[Hansheng Chen](https://lakonik.github.io/)<sup>1</sup>, 
[Ruoxi Shi](https://rshi.top/)<sup>2</sup>, 
[Yulin Liu](https://liuyulinn.github.io/)<sup>2</sup>, 
[Bokui Shen](https://cs.stanford.edu/people/bshen88/)<sup>3</sup>,
[Jiayuan Gu](https://pages.ucsd.edu/~ztu/)<sup>2</sup>, 
[Gordon Wetzstein](http://web.stanford.edu/~gordonwz/)<sup>1</sup>, 
[Hao Su](https://cseweb.ucsd.edu/~haosu/)<sup>2</sup>, 
[Leonidas Guibas](https://geometry.stanford.edu/member/guibas/)<sup>1</sup><br>
<sup>1</sup>Stanford University, <sup>2</sup>UCSD, <sup>3</sup>Apparate Labs
<br>

[[project page](https://lakonik.github.io/mvedit)] [[Web UI](https://lakonik.github.io/mvedit_demo/)] [[Web UI🤗](https://huggingface.co/spaces/Lakonik/MVEdit)] [[paper](https://arxiv.org/abs/2403.12032)]

https://github.com/Lakonik/MVEdit/assets/53893837/062a0622-47f8-4068-b478-aa4a80e5b715

## Todos

- [x] Add Zero123++ v1.2 to the Web UI
- [x] Release the complete codebase, including the Web UI that can be deployed on your own machine
- [ ] Add non-Gradio scripts and instructions

## Installation

### Prerequisites

The code has been tested in the environment described as follows:

- Ampere or later NVIDIA GPU (for bfloat16 support)
- Linux (tested on Ubuntu 20 & 22)
- [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit-archive) 12
- [PyTorch](https://pytorch.org/get-started/previous-versions/) 2.1.2
- FFmpeg, x264 (optional, for exporting videos)

Other dependencies can be installed via `pip install -r requirements.txt`. 

An example of installation commands is shown below (you may change the CUDA version yourself):

```bash
# Export the PATH of CUDA toolkit
export PATH=/usr/local/cuda-12.1/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.1/lib64:$LD_LIBRARY_PATH

# Create conda environment
conda create -y -n mvedit python=3.10
conda activate mvedit

# Install FFmpeg (optional)
conda install -c conda-forge ffmpeg x264

# Install PyTorch
conda install pytorch==2.1.2 torchvision==0.16.2 pytorch-cuda=12.1 -c pytorch -c nvidia

# Clone this repo and install other dependencies
git clone https://github.com/Lakonik/MVEdit && cd MVEdit
pip install -r requirements.txt
```

This codebase may work on Windows systems, but it has not been tested extensively.

## Usage

We recommend using the Gradio Web UI and its APIs. You need a GPU with at least 24G of VRAM to run the Web UI.

Run the following command to start the Web UI:

```bash
python app.py --advanced --empty-cache --unload-models
```

The Web UI will be available at [http://localhost:7860](http://localhost:7860). If you add the `--share` flag, a temporary public URL will be generated for you to share the Web UI with others.

All models will be automatically loaded on demand. The first run will take a very long time to download the models. Check your network connection to GitHub, Google Drive and Hugging Face if the download fails.

The API docs are available at [http://localhost:7860/?view=api](http://localhost:7860/?view=api). The docs are automatically generated by Gradio, and the data types and default values may be incorrect. Please use the default values in the Web UI as a reference.

Instructions for advanced usage of MVEdit pipelines will be added soon.

## Known Issues

The Web UI has CPU memory leaks (presumably a Gradio issue). Avoiding frequent switching between mutliple tabs in the Web UI may alleviate the issue.

If you deploy the Web UI on your own server and encounter the memory leak issue, a temporary workaround is to restart the Web UI after it's killed using a while loop:

```bash
while (true) do
    echo starting...
    python app.py --empty-cache --unload-models
done
```

## Acknowledgements

This codebase is built upon the following repositories:
- Base library modified from [SSDNeRF](https://github.com/Lakonik/SSDNeRF)
- NeRF renderer and DMTet modified from [Stable-DreamFusion](https://github.com/ashawkey/stable-dreamfusion)
- Mesh I/O modified from [DreamGaussian](https://github.com/dreamgaussian/dreamgaussian)
- [Zero123++](https://github.com/SUDO-AI-3D/zero123plus) for image-to-3D initialization
- [IP-Adapter](https://github.com/tencent-ailab/IP-Adapter) for extra conditioning
- [TRACER](https://github.com/Karel911/TRACER) for background removal
- [LoFTR](https://github.com/zju3dv/LoFTR) for pose estimation in image-to-3D
- [Omnidata](https://github.com/EPFL-VILAB/omnidata) for normal prediction in image-to-3D
- [Image Packer](https://github.com/theFroh/imagepacker) for mesh preprocessing

## Citation

```bibtex
@misc{mvedit2024,
    title={Generic 3D Diffusion Adapter Using Controlled Multi-View Editing},
    author={Hansheng Chen and Ruoxi Shi and Yulin Liu and Bokui Shen and Jiayuan Gu and Gordon Wetzstein and Hao Su and Leonidas Guibas},
    year={2024},
    eprint={2403.12032},
    archivePrefix={arXiv},
    primaryClass={cs.CV}
}
```
