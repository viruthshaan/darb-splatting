# DARB-Splatting: Generalizing Splatting with Decaying Anisotropic Radial Basis Functions

Vishagar Arunan*, Saeedha Nazar*, Hashiru Pramuditha*, Vinasirajan Viruthshaan*, Ranga Rodrigo*, Sameera Ramasinghe, Simon Lucey (* indicates equal contribution)<br>
<a href="https://github.com/viruthshaan/darb-splatting/"><img style="width:100%;" src="images\teaser.png"> </a>
<!-- | [Webpage](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/) | [Full Paper](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/3d_gaussian_splatting_high.pdf) | [Video](https://youtu.be/T_kXY43VZnk) | [Other GRAPHDECO Publications](http://www-sop.inria.fr/reves/publis/gdindex.php) | [FUNGRAPH project page](https://fungraph.inria.fr) |<br>
| [T&T+DB COLMAP (650MB)](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/datasets/input/tandt_db.zip) | [Pre-trained Models (14 GB)](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/datasets/pretrained/models.zip) | [Viewers for Windows (60MB)](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/binaries/viewers.zip) | [Evaluation Images (7 GB)](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/evaluation/images.zip) |<br>
![Teaser image](assets/teaser.png) -->

<!-- This repository contains the official authors implementation associated with the paper "3D Gaussian Splatting for Real-Time Radiance Field Rendering", which can be found [here](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/). We further provide the reference images used to create the error metrics reported in the paper, as well as recently created, pre-trained models. 

<!-- <a href="https://www.inria.fr/"><img height="100" src="assets/logo_inria.png"> </a>
<a href="https://univ-cotedazur.eu/"><img height="100" src="assets/logo_uca.png"> </a>
<a href="https://www.mpi-inf.mpg.de"><img height="100" src="assets/logo_mpi.png"> </a> 
<a href="https://team.inria.fr/graphdeco/"> <img style="width:100%;" src="assets/logo_graphdeco.png"></a> --> 


## Overview
 We provide CUDA rasterizer code for each DARB function, along with the code needed to reproduce the results presented in our paper. The code is largely based on [3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/). The `.ply` file outputs from DARB Splatting are of the same type as those produced by Gaussian Splatting. To visualize the results, different submodules are required.


## Abstract
Splatting-based 3D reconstruction methods have gained popularity with the advent of 3D Gaussian Splatting, efficiently synthesizing high-quality novel views. These methods commonly resort to using exponential family functions, such as the Gaussian function, as reconstruction kernels due to their anisotropic nature, ease of projection, and differentiability in rasterization. However, the field remains restricted to variations within the exponential family, leaving generalized reconstruction kernels beyond this largely underexplored, partly due to the lack of closed-form solutions for 3D to 2D projections. In this light, we show that a class of decaying anisotropic radial basis functions (DARBFs), which are non-negative functions of the Mahalanobis distance, supports splatting by approximating the Gaussian function's closed-form integration advantage. With this fresh perspective, we demonstrate up to 34% faster convergence during training and a 15% reduction in memory consumption across various DARB reconstruction kernels, while maintaining comparable PSNR, SSIM, and LPIPS results. We believe that our unifying approach opens new avenues in splatting, equipping the community with a broader set of tools and enabling computational savings for future industrial applications.

<!-- <section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
    <h2 class="title">BibTeX</h2>
    <pre><code>@Article{kerbl3Dgaussians,
      author       = {Kerbl, Bernhard and Kopanas, Georgios and Leimk{\"u}hler, Thomas and Drettakis, George},
      title        = {3D Gaussian Splatting for Real-Time Radiance Field Rendering},
      journal      = {ACM Transactions on Graphics},
      number       = {4},
      volume       = {42},
      month        = {July},
      year         = {2023},
      url          = {https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/}
}</code></pre>
  </div>
</section> -->


## Step-by-step Tutorial

We used the Gaussian Splatting as our base code. Jonathan Stephens made a fantastic step-by-step tutorial for setting up Gaussian Splatting on your machine, along with instructions for creating usable datasets from videos. You can find the video [here](https://www.youtube.com/watch?v=UXtuigy_wYc).

## Running
Download the datasets from the [original repository](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/), and place them in the `mip`, `tandt_db` and `nerf_360` directories.


## Cloning the Repository

The repository contains submodules, thus please check it out with 
```shell
# SSH
git clone git@github.com:viruthshaan/darb-splatting.git --recursive
```
or
```shell
# HTTPS
git clone https://github.com/viruthshaan/darb-splatting --recursive
```

### Hardware Requirements

- CUDA-ready GPU with Compute Capability 7.0+
- 24 GB VRAM (to train to paper evaluation quality)
- Please see FAQ for smaller VRAM configurations

### Software Requirements
- Conda (recommended for easy setup)
- C++ Compiler for PyTorch extensions (we used Visual Studio 2019 for Windows)
- CUDA SDK 11 for PyTorch extensions, install *after* Visual Studio (we used 11.8, **known issues with 11.6**)
- C++ Compiler and CUDA SDK must be compatible

### Setup

Our default, provided install method is based on Conda package and environment management:
```shell
SET DISTUTILS_USE_SDK=1 # Windows only
conda env create --file environment.yml
conda activate darb_splatting
```
Please note that this process assumes that you have CUDA SDK **11** installed, not **12**. For modifications, see below. You can also use the same conda environment of the [Gaussian Splatting repo](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/).


## Running
Download the datasets from the [original repository](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/), and place them in the `mip`,`tandt_db` and `nerf_360` directories.

Default the configuration is set to Gaussian. To try different reconstruction kernals, please install submodule using following command.

Raised Cosine:
```
conda activate darb_splatting
cd <dir_to_repo>/darb-splatting
pip install submodules\diff-raised-cosine-rasterization
```

Half Cosine Square:
```
conda activate darb_splatting
cd <dir_to_repo>/darb-splatting
pip install submodules\diff-half-cosine-square-rasterization
```

## Cite
If you find our work useful in your research, please consider citing:

```bibtex

@article{arunan2025darb,
  title={DARB-Splatting: Generalizing Splatting with Decaying Anisotropic Radial Basis Functions},
  author={Arunan, Vishagar and Nazar, Saeedha and Pramuditha, Hashiru and Viruthshaan, Vinasirajan and Ramasinghe, Sameera and Lucey, Simon and Rodrigo, Ranga},
  journal={arXiv preprint arXiv:2501.12369},
  year={2025}
}

=======
