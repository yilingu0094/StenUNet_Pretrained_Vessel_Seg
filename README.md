<a href="https://arxiv.org/abs/2310.14961" alt="Citation"><img src="https://img.shields.io/badge/cite-citation-blue" /></a>
# Introduction
This algorithm is for the stenosis detection task in [ARCADE Challenge](https://arcade.grand-challenge.org/), which was held at MICCAI 2023. We are ranked ${\textsf{\color{red}3rd}}$ !

Our publication:  StenUNet: Automatic Stenosis Detection from X-ray Coronary Angiography [Arxiv](https://arxiv.org/abs/2310.14961)

Please refer to [MICCAI-ARCADE](https://github.com/NMHeartAI/MICCAI_ARCADE.git) for the segmentation detection task.


## Installation
python>=3.9 and torch>=2.0.0

      conda create -n stenunet_env python=3.9
      conda activate stenunet_env
      git clone https://github.com/HuiLin0220/StenUNet.git
      cd StenUNet
      pip install  -r ./requirements.txt

## Prepare data
- The training data folder structure is like this:

         Raw_data/Dataset_train_val/  
          ├── imagesTr
          │   ├── sten_0000_0000.png
          │   ├── sten_0000_0001.png
          │   ├── ...
          │   ├── sten_0001_0000.png      
          │   ├── sten_0001_0001.png      
          │   ├── ... 
          │   ├── sten_0002_0000.png
          │   ├── sten_0002_0001.png
          │   ├── ...
          ├── labelsTr
          │   ├── sten_0000.png
          │   ├── sten_0001.png
          │   ├── sten_0002.png
          │   ├── ...
          ├── dataset.json

      (1) sten_0000_0000.png and sten_0000_0001.png are considered two different modalities for the same raw image (sten_0000).
      (2) We provide some preprocessing methods in [preprocess.py](pre_process/preprocess.py) You can do some preprocessing on the raw image and get several modalities for training.
      (3) Note that inference and training should use the same preprocessing strategies.

- Rename and put the training images in this folder "./nnNet_training/Raw_data/"
- Edit dataset.json
  ("numTraining" indicates the number of training samples in your dataset.)
## Train
- Planning hyper_parameters

      python training_planning.py 
- Training from scratch

      CUDA_VISIBLE_DEVICES=0 python training.py 0
      #CUDA_VISIBLE_DEVICES=X python train.py fold_ID(0,1,2,3,4)
- Finetune the pre-trained model on your own data

      CUDA_VISIBLE_DEVICES=0 python training.py 0 -pretrained_weights MODEL_WEIGHTS_PATH
  if you want to use [Shared weights](https://drive.google.com/file/d/1BO4whry0i50h_yzqQwUw1k7QyyLUk2U3/view?usp=sharing), you need to replace the 
## Inference
1. Rename and put the test images in this folder'./dataset_test/raw';
2. Run
  
         python inference.py -chk MODEL_WEIGHTS_PATH

3.Sharing StenUnet's weight ([Google drive](https://drive.google.com/file/d/1BO4whry0i50h_yzqQwUw1k7QyyLUk2U3/view?usp=sharing)).   
4. You will get the preprocessed images, raw prediction after StenUNet, and post_prediction after postprocessing.

You can integrate your own preprocessing/postprocessing strategies in [preprocess.py](pre_process/preprocess.py)/[post_process](post_process/remove_small_segments.py)

The inference folder structure is like this:

      daset_test/
          ├── raw
          │   ├── sten_0000_0000.png
          │   ├── sten_0001_0000.png
          │   ├── ...
          ├── preprocessed
          │   ├── sten_0000_0000.png       # prerpocessing method0
          │   ├── sten_0000_0001.png       # prerpocessing method1
          │   ├── sten_0000_0003.png       # prerpocessing method2
          │   ├── ... 
          │   ├── sten_0001_0000.png
          │   ├── sten_0001_0001.png
          │   ├── sten_0001_0003.png
          │   ├── ...
          ├── raw_prediction
          │   ├── sten_0000.png
          │   ├── sten_0001.png
          │   ├── ...
          ├── post_prediction
          │   ├── sten_0000.png
          │   ├── sten_0001.png
          │   ├── ...
## References
[nnunet](https://github.com/MIC-DKFZ/nnUNet)

## Citation
Please cite the following paper when using SteUNet:

      @article{lin2023stenunet,
        title={StenUNet: Automatic Stenosis Detection from X-ray Coronary Angiography},
        author={Lin, Hui and Liu, Tom and Katsaggelos, Aggelos and Kline, Adrienne},
        journal={arXiv preprint arXiv:2310.14961},
        year={2023}
      }

## Contact Us
Feel free to contact me at huilin2023@u.northwestern.edu

## To-do List
