ImageNet training in PyTorch
============================

This implements training of popular model architectures, such as ResNet, AlexNet, and VGG on the ImageNet dataset.

This version has been modified to use DALI. It assumes that the imagenet dataset is in a Caffe2 lmdb format.
This version has been modified to use the DistributedDataParallel module in APEx instead of the one in upstream PyTorch. Please install APEx from `here <https://www.github.com/nvidia/apex>`_.

To run use the following commands

.. code-block:: bash

   ln -s /path/to/train/lmdb/ train
   ln -s /path/to/validation/lmdb/ val
   python -m apex.parallel.multiproc main.py -a resnet50 -b 128 --fp16 .

Requirements
------------

.. role:: bash(code)
   :language: bash

- `APEx <https://www.github.com/nvidia/apex>`_ - optional, required for fp16 mode or distributed (multi-GPU) operation
- Install PyTorch from source, master branch of `pytorch on github <https://www.github.com/pytorch/pytorch>`_
- :bash:`pip install -r requirements.txt`
- Download the ImageNet dataset and move validation images to labeled subfolders

  - To do this, you can use the following `script <https://raw.githubusercontent.com/soumith/imagenetloader.torch/master/valprep.sh>`_

Training
--------

To train a model, run :fileref:`docs/examples/pytorch/main.py` with the desired model architecture and the path to the ImageNet dataset:

.. code-block:: bash

   python main.py -a resnet18 [imagenet-folder with train and val folders]

The default learning rate schedule starts at 0.1 and decays by a factor of 10 every 30 epochs. This is appropriate for ResNet and models with batch normalization, but too high for AlexNet and VGG. Use 0.01 as the initial learning rate for AlexNet or VGG:

.. code-block:: bash

   python main.py -a alexnet --lr 0.01 [imagenet-folder with train and val folders]

Usage
-----

.. code-block:: bash

   main.py [-h] [--arch ARCH] [-j N] [--epochs N] [--start-epoch N] [-b N] [--lr LR] [--momentum M] [--weight-decay W] [--print-freq N] [--resume PATH] [-e] [--pretrained] DIR

   PyTorch ImageNet Training

   positional arguments:
   DIR                         path(s) to dataset (if one path is provided, it is assumed to have subdirectories named "train" and "val"; alternatively, train and val paths can be specified directly by providing both paths as arguments)

   optional arguments:
   -h, --help                  show this help message and exit
   --arch ARCH, -a ARCH        model architecture: alexnet | resnet | resnet101 | resnet152 | resnet18 | resnet34 | resnet50 | vgg | vgg11 | vgg11_bn | vgg13 | vgg13_bn | vgg16 | vgg16_bn | vgg19 | vgg19_bn (default: resnet18)
   -j N, --workers N           number of data loading workers (default: 4)
   --epochs N                  number of total epochs to run
   --start-epoch N             manual epoch number (useful on restarts)
   -b N, --batch-size N        mini-batch size (default: 256)
   --lr LR, --learning-rate LR initial learning rate
   --momentum M                momentum
   --weight-decay W, --wd W    weight decay (default: 1e-4)
   --print-freq N, -p N        print frequency (default: 10)
   --resume PATH               path to latest checkpoint (default: none)
   -e, --evaluate              evaluate model on validation set
   --pretrained                use pre-trained model
