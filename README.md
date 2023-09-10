# NOVA: NOvel View Augmentation for Neural Composition of Dynamic Objects

 [![arXiv](https://img.shields.io/badge/arXiv-2108.00946-b31b1b.svg)](https://arxiv.org/abs/2308.12560)

[Project Website](https://mscvprojects.ri.cmu.edu/2023team1/) | [Paper](https://arxiv.org/pdf/2308.12560.pdf)

This repository contains the code of the short paper NOVA accepted to the [CV4Metaverse ICCV 2023 Workshop](https://sites.google.com/view/cv4metaverse/). If you find this paper and code useful for your research, please consider citing the following paper:

```
@misc{agrawal2023nova,
      title={NOVA: NOvel View Augmentation for Neural Composition of Dynamic Objects},
      author={Dakshit Agrawal and Jiajie Xu and Siva Karthik Mustikovela and Ioannis Gkioulekas and Ashish Shrivastava and Yuning Chai},
      year={2023},
      eprint={2308.12560},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

## Contents

1. [Setup Instructions and Dependencies](#1-setup-instructions-and-dependencies)
2. [Dataset](#2-dataset)
3. [Train NOVA](#3-train-nova)
4. [Render Samples](#4-render-samples)
5. [Evaluation](#5-evaluation)
6. [License](#6-license)
7. [Acknowledgements](#7-acknowledgement)


## 1. Setup Instructions and Dependencies

The code is test with
* Linux (tested on Ubuntu 18.04)
* Miniconda 3
* Python 3.9
* Pytorch 2.0
* CUDA 11.7
* GPU with 24 GB VRAM

To get started, please create the conda environment `nova` by running

```bash
conda create --name nova python=3.9
conda activate nova
conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
pip install -r requirements.txt
```

## 2. Dynamic Scene Dataset

The [Dynamic Scene Dataset](https://research.nvidia.com/publication/2020-06_novel-view-synthesis-dynamic-scenes-globally-coherent-depths) is used for our experiments. Please download the pre-processed data by running:

```bash
cd $ROOT_PATH
gdown https://drive.google.com/uc\?id\=14E6jIUVx_cuXPKlSefHo5tEtDMt7WPUd
unzip data.zip
rm data.zip
```

## 3. Train NOVA
You can train a model from scratch by running:

```bash
cd $ROOT_PATH/
python run_nerf.py --config configs/config_Balloon1.txt
```

## 4. Render Samples

You can render the results by running:

```bash
cd $ROOT_PATH/
python render_samples.py --config logs/Balloon1_H270_NOVA/config.txt
```

To render multiple objects in the scene, comment lines `202-203` and change lines `204-206` in `render_samples.py` to the following (only rotation supported for now):

```python
# list of axis along which rotation occurs
axis = []
# rotation angle in degrees, can be negative, must have the same number of elements as axis
angle = []
# 0 specifies background, specify the object id corresponding to each element in axis
render_kwargs_test.update({"cam_order": [0, ...]})


# For example, if there are two objects in the scene, and you want one instance of first
# object and two instances of the second object, you can define something like this:
axis = ["x", "x", "y"]
angle = [0, -10, 15]
render_kwargs_test.update({"cam_order": [0, 1, 2, 2]})
```

**[TODO]** We provide our trained models. You can download them by running:

```bash
cd $ROOT_PATH/
gdown https://drive.google.com/uc\?id\=
unzip logs.zip
rm logs.zip
```

## 5. Evaluation

We quantitatively evaluate the fix-view-change-time results of the following methods:

`NeRF + t` \
`Yoon et al.` \
`NSFF` \
`DynamicNeRF` \
**[TODO]** `NOVA (ours)`

Please download the results by running:

```bash
cd $ROOT_PATH/
gdown https://drive.google.com/uc\?id\=1vMh2GUxXiHF_gHo_gIdVMABRL8UXrmI7
unzip results.zip
rm results.zip
```

Then you can calculate the PSNR/SSIM/LPIPS by running:

```bash
cd $ROOT_PATH
python utils/evaluation.py
```

## 6. License

This work is licensed under MIT License. See [LICENSE](LICENSE) for details.

## 7. Acknowledgments
Our training code is build upon [DynamicNeRF](https://github.com/gaochen315/DynamicNeRF).
