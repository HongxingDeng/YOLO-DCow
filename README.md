# YOLO-DCow

Official repository for the paper **"Deep learning-based cow segmentation for precision livestock farming using RGB-D data"**.

This work proposes a deep learning framework for cow segmentation and 3D shape extraction in commercial farm environments. The method uses RGB-D images as input, introduces a dual-modal YOLO-DCow network, and maps image-level segmentation results to point clouds for pixel-level 3D cow segmentation.

![Framework](https://github.com/dontlearncpp/YOLOv8-with-RGB-D-and-AFFP/assets/103402250/7b23319f-baab-4b61-ad93-54caeccb09f3)

## Overview

Traditional RGB-D animal segmentation methods often rely on background subtraction or point-cloud statistical processing. These methods are sensitive to scene changes, point-cloud noise, and multiple-animal cases. To address these limitations, our work proposes an end-to-end framework with the following features:

- RGB-D dual-modal input for real-scale cow segmentation.
- A novel mask-generation module named **AFFP** (Adaptively Feature Fusion Protonet).
- An improved regression loss based on **WIoU-v3** to enhance robustness on low-quality examples.
- Pixel-level mapping from segmented RGB-D images to 3D point clouds.

This framework is designed for downstream precision livestock farming tasks such as body size analysis, weight estimation, body condition scoring, and cleanliness assessment.

## Main Contributions

1. We extend instance segmentation from RGB to RGB-D input by adding a depth channel, which improves segmentation under complex farm conditions.
2. We propose **AFFP** to fuse transposed convolution and bilinear interpolation during mask generation, reducing checkerboard artifacts and preserving high-frequency features.
3. We build an end-to-end 3D cow shape segmentation pipeline that directly transfers 2D segmentation results to point clouds without point-cloud preprocessing.

## Results

The proposed YOLO-DCow achieved the best performance among the compared instance segmentation models on our cow RGB-D dataset:

- **AP: 95.5%**
- **AP50: 98.9%**
- **AP75: 98.7%**
- **AP90: 97.8%**

Compared with YOLOv8-n, the proposed method improved AP by **1.5%**. Compared with other baseline methods, the AP improvement ranged from **1.5% to 10.5%**.

## Dataset and Experimental Setting

- RGB-D images were collected from **3 commercial dairy farms**.
- The dataset contains images of **360 lactating Holstein cows**.
- A total of **2100 RGB-D images** were collected.
- After filtering, **1739 images** were used for training and testing:
  - **1391** for training
  - **348** for testing

## Code Organization

The code for different stages is maintained in different branches.

## Train and Evaluation

Please refer to the `Train&Eval` branch for training and evaluation.

The main modified files include:

- `predictor.py`
- `stream_loaders.py`
- `base.py`
- related segmentation modules in the YOLOv8 codebase

### Depth Image Path

The depth image path is specified in:

- `ultralytics/yolo/data/base.py`

### AFFP Module

The AFFP implementation is located in:

- `ultralytics/nn/modules.py`

![AFFP](https://github.com/dontlearncpp/YOLOv8-with-RGB-D-and-AFFP/assets/103402250/19cbfbfe-ba9c-4ec2-a4a5-31fb68fa81dd)

## Prediction

Please refer to the prediction branch or related prediction code for inference.

The main modified files include:

- `predictor.py`
- `stream_loaders.py`
- `base.py`
- `ultralytics/yolo/v8/segment/predict.py`

The input and output image paths are configured in:

- `ultralytics/yolo/v8/segment/predict.py`

## 3D Point Cloud Segmentation

After obtaining segmentation masks from RGB-D images, the segmented depth results can be transformed into point clouds using the camera intrinsics. This enables pixel-level cow point-cloud extraction in dynamic farm scenes and multi-cow scenarios.

![Point Cloud Segmentation](https://github.com/dontlearncpp/YOLOv8-with-RGB-D-and-AFFP/assets/103402250/0fb74830-da11-4623-b90b-e2e9660a0ede)

## Manuscript

This repository also contains the manuscript source and figures:

- `sn-article.tex`
- `sn-article.pdf`
- `images/`

## Citation

If you find this work helpful, please cite our related work:

```bibtex
@article{yang2023extracting,
  title={Extracting cow point clouds from multi-view RGB images with an improved YOLACT++ instance segmentation},
  author={Yang, Guangyuan and Li, Rui and Zhang, Shuang and Wen, Yu and Xu, Xiaoli and Song, Huaibo},
  journal={Expert Systems with Applications},
  volume={230},
  pages={120730},
  year={2023}
}
```
