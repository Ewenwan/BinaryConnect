**Please checkout our latest work,  
[BinaryNet: Training Deep Neural Networks with Weights and Activations Constrained to +1 or -1](http://arxiv.org/abs/1602.02830),  
and the associated [github repository](https://github.com/MatthieuCourbariaux/BinaryNet).**


# 混有单精度与二值的神经网络BinaryConnect 

    首先点燃战火的是Matthieu Courbariaux，
    他来自深度学习巨头之一的Yoshua Bengio领导的蒙特利尔大学的研究组。
    他们的文章于2015年11月出现在arxiv.org上。
    与此前二值神经网络的实验不同，Matthieu只关心系数的二值化，
    并采取了一种混和的策略，
    构建了一个混有单精度与二值的神经网络BinaryConnect：
    当网络被用来学习时，系数是单精度的，因此不会受量化噪声影响；
    而当被使用时，系数从单精度的概率抽样变为二值，从而获得加速的好处。
    这一方法在街拍门牌号码数据集(SVHN)上石破天惊地达到超越单精度神经网络的预测准确率，
    同时超越了人类水平，打破了此前对二值网络的一般印象，并奠定了之后一系列工作的基础。
    然而由于只有系数被二值化，Matthieu的BinaryConnect只能消减乘法运算，
    在CPU和GPU上一般只有2倍的理论加速比，但在FPGA甚至ASIC这样的专用硬件上则有更大潜力。


## Motivations

The goal of this repository is to enable the reproduction of the experiments described in  
[BinaryConnect: Training Deep Neural Networks with binary weights during propagations](http://arxiv.org/abs/1511.00363).  
You may want to checkout our subsequent work:
* [Neural Networks with Few Multiplications](http://arxiv.org/abs/1510.03009)
* [BinaryNet: Training Deep Neural Networks with Weights and Activations Constrained to +1 or -1](http://arxiv.org/abs/1602.02830)

## Requirements

* Python, Numpy, Scipy
* [Theano](http://deeplearning.net/software/theano/install.html) (Bleeding edge version)
* [Pylearn2](http://deeplearning.net/software/pylearn2/)
* [Lasagne](http://lasagne.readthedocs.org/en/latest/user/installation.html)
* [PyTables](http://www.pytables.org/usersguide/installation.html) (only for the SVHN dataset)
* a fast Nvidia GPU or a large amount of patience

## MNIST

    python mnist.py
    
This python script trains an MLP on MNIST with the stochastic version of BinaryConnect.
It should run for about 30 minutes on a GTX 680 GPU.
The final test error should be around **1.15%**.
Please note that this is NOT the experiment reported in the article (which is in the "master" branch of the repository).

## CIFAR-10

    python cifar10.py
    
This python script trains a CNN on CIFAR-10 with the stochastic version of BinaryConnect.
It should run for about 20 hours on a Titan Black GPU.
The final test error should be around **8.27%**.

## SVHN

    export SVHN_LOCAL_PATH=/Tmp/SVHN/
    python svhn_preprocessing.py

This python script (taken from Pylearn2) computes a preprocessed (GCN and LCN) version of the SVHN dataset in a temporary folder (SVHN_LOCAL_PATH).

    python svhn.py
    
This python script trains a CNN on SVHN with the stochastic version of BinaryConnect.
It should run for about 2 days on a Titan Black GPU.
The final test error should be around **2.15%**.

## How to play with it

The python scripts mnist.py, cifar10.py and svhn.py contain all the relevant hyperparameters.
It is very straightforward to modify them.
binary_connect.py contains the binarization function (called binarization).

Have fun!
