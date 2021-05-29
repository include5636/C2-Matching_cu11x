# C2-Matching (CVPR2021)

![Python 3.7](https://img.shields.io/badge/python-3.7-green.svg?style=plastic)
![pytorch 1.4.0](https://img.shields.io/badge/pytorch-1.4.0-green.svg?style=plastic)
![CUDA 10.0](https://camo.githubusercontent.com/5e1f2e59c9910aa4426791d95a714f1c90679f5a/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f637564612d31302e312d677265656e2e7376673f7374796c653d706c6173746963)

This repository contains the implementation of the following paper:
> **Robust Reference-based Super-Resolution via C2-Matching**<br>
> Yuming Jiang, Kelvin C.K. Chan, Xintao Wang, Chen Change Loy, Ziwei Liu<br>
> IEEE Conference on Computer Vision and Pattern Recognition (**CVPR**), 2021<br>

[[Paper](https://yumingj.github.io/projects/C2_matching/)]
[[Project Page](https://yumingj.github.io/projects/C2_matching/)]

## Overview
![overall_structure](./assets/framework.png)



## Dependencies and Installation

- Python >= 3.7
- PyTorch >= 1.4
- CUDA 10.0

1. Clone Repo

   ```bash
   git clone git@github.com:yumingj/C2-Matching.git
   ```

1. Create Conda Environment

   ```bash
   conda create --name c2_matching python=3.7
   conda activate c2_matching
   ```

1. Install Dependencies

   ```bash
   cd C2-Matching
   conda install pytorch=1.4.0 torchvision cudatoolkit=10.0 -c pytorch
   conda install mmcv==0.4.4
   pip install -r requirements.txt
   ```

1. Install MMSR and DCNv2

    ```bash
    python setup.py develop
    cd mmsr/models/archs/DCNv2
    python setup.py build develop
    ```


## Dataset Preparation

- Train Set: [CUFED Dataset](https://drive.google.com/drive/folders/1hGHy36XcmSZ1LtARWmGL5OK1IUdWJi3I)
- Test Set: [WR-SR Dataset](https://drive.google.com/file/d/1dB-GDGBOMHRSdN2v7IzJBMpHYHBIlNq-/view?usp=sharing), [CUFED5 Dataset](https://drive.google.com/file/d/1Fa1mopExA9YGG1RxrCZZn7QFTYXLx6ph/view)

Please refer to [Datasets.md](datasets/README.md) for pre-processing and more details.

## Get Started

### Pretrained Models
Downloading the pretrained models from this [link](https://drive.google.com/drive/folders/1dTkXMzeBrHelVQUEx5zib5MdmvqDaSd9?usp=sharing) and put them under `experiments/pretrained_models folder`.

### Test

We provide quick test code with the pretrained model.

1. Modify the paths to dataset and pretrained model in the following yaml files for configuration.

    ```bash
    ./options/test/test_C2_matching.yml
    ./options/test/test_C2_matching_mse.yml
    ```

1. Run test code for models trained using **GAN loss**.

    ```bash
    python mmsr/test.py -opt "options/test/test_C2_matching.yml"
    ```

   Check out the results in `./results`.

1. Run test code for models trained using only **reconstruction loss**.

    ```bash
    python mmsr/test.py -opt "options/test/test_C2_matching_mse.yml"
    ```

   Check out the results in in `./results`


### Train

All logging files in the training process, *e.g.*, log message, checkpoints, and snapshots, will be saved to `./experiments` and `./tb_logger` directory.

1. Modify the paths to dataset in the following yaml files for configuration.
   ```bash
   ./options/train/stage1_teacher_contras_network.yml
   ./options/train/stage2_student_contras_network.yml
   ./options/train/stage3_restoration_gan.yml
   ```

1. Stage 1: Train teacher contrastive network.
   ```bash
   python mmsr/train.py -opt "options/train/stage1_teacher_contras_network.yml"
   ```

1. Stage 2: Train student contrastive network.
   ```bash
   # add the path to *pretrain_model_teacher* in the following yaml
   # the path to *pretrain_model_teacher* is the model obtained in stage1
   ./options/train/stage2_student_contras_network.yml
   python mmsr/train.py -opt "options/train/stage2_student_contras_network.yml"
   ```

1. Stage 3: Train restoration network.
   ```bash
   # add the path to *pretrain_model_feature_extractor* in the following yaml
   # the path to *pretrain_model_feature_extractor* is the model obtained in stage2
   ./options/train/stage3_restoration_gan.yml
   python mmsr/train.py -opt "options/train/stage3_restoration_gan.yml"

   # if you wish to train the restoration network with only mse loss
   # prepare the dataset path and pretrained model path in the following yaml
   ./options/train/stage3_restoration_mse.yml
   python mmsr/train.py -opt "options/train/stage3_restoration_mse.yml"
   ```

## Visual Results

For more results on the benchmarks, you can directly download our C2-Matching results from [here](https://drive.google.com/drive/folders/1-WE-f8XyG_MEZY77IGyS2le-UmDwhou9?usp=sharing).

![result](assets/visual_comp.png)


## Citataion

   If you find our repo useful for your research, please consider citing our paper:

   ```bibtex
   @InProceedings{jiang2020c2matching,
      author = {Yuming Jiang and Kelvin C.K. Chan and Xintao Wang and Chen Change Loy and Ziwei Liu},
      title = {Robust Reference-based Super-Resolution via C2-Matching},
      booktitle={The IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
      year = {2021}
   }
   ```


## License and Acknowledgement

This project is open sourced under MIT license. The code framework is mainly modified from [BasicSR](https://github.com/xinntao/BasicSR). Please refer to the original repo for more usage and documents.


## Contact

If you have any question, please feel free to contact us via `yuming002@ntu.edu.sg`.