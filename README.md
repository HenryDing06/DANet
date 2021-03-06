## Usage

1. Install pytorch 

  - The code is tested on python3.6 and official [Pytorch@commitfd25a2a](https://github.com/pytorch/pytorch/tree/fd25a2a86c6afa93c7062781d013ad5f41e0504b#from-source), please install PyTorch from source.
  - The code is modified from [PyTorch-Encoding](https://github.com/zhanghang1989/PyTorch-Encoding). 
  - Download [Anaconda](https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh)
  
  ```shell
  conda install pytorch=0.4.1 cuda90 torchvision -c pytorch
  or conda install pytorch=0.4.1 cuda80 torchvision -c pytorch
  conda install -c conda-forge tqdm
  ```
2. Clone the repository:

   ```shell
   git clone https://github.com/junfu1115/DANet.git 
   cd DANet 
   python setup.py install
   ```
   
3. Dataset

  - Download the [Cityscapes](https://www.cityscapes-dataset.com/) dataset and convert the dataset to [19 categories](https://github.com/mcordts/cityscapesScripts/blob/master/cityscapesscripts/helpers/labels.py) according to [this](https://github.com/junfu1115/DANet/issues/12). 
  - Please put dataset in folder `./datasets`

4 . Evaluation

  - Download trained model [DANet101](https://drive.google.com/open?id=1XmpFEF-tbPH0Rmv4eKRxYJngr3pTbj6p) and put it in folder `./danet/cityscapes/model`
  - Evaluation code is in folder `./danet/cityscapes`
  - `cd danet`

  - For single scale testing, please run:
  
   ```shell
   CUDA_VISIBLE_DEVICES=0,1,2,3 python test.py --dataset cityscapes --model danet --resume-dir cityscapes/model --base-size 2048 --crop-size 768 --workers 1 --backbone resnet101 --multi-grid --multi-dilation 4 8 16 --eval
   ```
   
  - For multi-scale testing, please run:
  
   ```shell
   CUDA_VISIBLE_DEVICES=0,1,2,3 python test.py --dataset cityscapes --model danet --resume-dir cityscapes/model --base-size 2048 --crop-size 1024 --workers 1 --backbone resnet101 --multi-grid --multi-dilation 4 8 16 --eval --multi-scales
   ```  
   
  - If you want to visualize the result of DAN-101, you can run:
 
   ```shell
   CUDA_VISIBLE_DEVICES=0,1,2,3 python test.py --dataset cityscapes --model danet --resume-dir cityscapes/model --base-size 2048 --crop-size 768 --workers 1 --backbone resnet101 --multi-grid --multi-dilation 4 8 16
   ```
   
5. Evaluation Result:

   The expected scores will show as follows:
   
   (single scale testing denotes as 'ss' and multiple scale testing denotes as 'ms')
   
   DANet101 on cityscapes val set (mIoU/pAcc): **79.93/95.97** (ss) and **81.49/96.41** (ms)


6. Training:

  - Training code is in folder `./danet/cityscapes`
  - `cd danet`
  
   You can reproduce our result by run:

  ```shell
   CUDA_VISIBLE_DEVICES=0,1,2,3 python train.py --dataset cityscapes --model  danet --backbone resnet101 --checkname danet101  --base-size 1024 --crop-size 768 --epochs 240 --batch-size 8 --lr 0.003 --workers 2 --multi-grid --multi-dilation 4 8 16
   ```
 
   Note that: We adopt multiple losses in end of the network for better training. 
 
