# Image Denoising Implementation
We have applied various ML models for Image Denoising on CIFAR-10 Dataset

### Models -
1. Denoising Autoencoder(DAE)
1. DeNoising Convolutional Neural Network(DnCNN)
1. Wide Inference Network(WIN)

### Dataset -
We have used CIFAR-10 dataset which consists of 60000 32x32 colour images in 10 classes, with 6000 images per class. There are 50000 training images and 10000 test images.

We have added noise to this dataset.
To do this we added gaussian noise with mean=0 and std=0.1 and then clip values back to 0-1.</br>
Mean=0 noise makes some parts of the image darker and some lighter after addition.

![GitHub Logo](/images/noisy_image.png)

## Denoising Autoencoder(DAE)
Now we define the building blocks of our DAE: a convolutional block and a deconvolutional block.</br>

Convolutional blocks consist of 3 operations: 2D convolution, batch normalization and ReLu activation. We use strides=2 to downsample data going through the network.</br>

Deconvolutional blocks also consist of 3 operations: 2D transposed convolution, batch normalization and also ReLu activation. Here strides=2 is used to upsample the data.

#### Our model architecture consist of:

* 4 convolutional blocks with downsampling
* 1 convolutional block without downsampling
* 4 deconvolutional blocks with upsampling, interleaving concatenations
* 1 final deconvolution that recreates image size (32, 32, 3)
* 1 activation layer with sigmoid that scales values to 0-1.

**Loss Function**: mean squared error</br>
**Optimizer**: Adam

#### Hyperparamters:
* Batch Size = 128
* no. of epochs = 40

### Results
![GitHub Logo](/images/result1.png)

### Evaluation
**PSNR**: 27.972 </br>
**SSIM**: 0.983

## DeNoising Convolutional Neural Network(DnCNN)
DNCNN consists of the normalized convolutional layers, the pooling layers and the normalized fully connected layers.

#### Our model architecture consist of:
There are 3 types of layers -
* **Conv+ReLU**: 64 filters of size 3×3×3 are used to generate 64 feature maps.
* **Conv+BN+ReLU**: 64 filters of size 3×3×64 are used, and batch normalization is added between convolution and ReLU.
* **Conv**: for the last layer, 3 filters of size 3×3×64 are used to reconstruct the output.

<p align="center"><img src="/images/dncnn_archi.png" width="400" height="120"/></p>

**Loss Function**: mean squared error</br>
**Optimizer**: Adam

#### Hyperparamters:
* Batch Size = 32
* no. of epochs = 30

### Results
![GitHub Logo](/images/result2.png)

### Evaluation
**PSNR**: 27.085 </br>
**SSIM**: 0.980

## Wide Inference Network(WIN)
The key to our proposed network architecture is to employ larger perceptions ﬁeldsthrough wider and shallower networks with more concentrated convolutions to capture the priorimage distribution from the noisy images, and yields better overall generalization power to new,unseen noisy images.

#### Our model architecture consist of:
There are 3 types of layers -
* **Conv+BN+ReLU**: 64 filters of size 7×7×3 are used to generate 64 feature maps, and batch normalization is added between convolution and ReLU.
* **Conv+BN+ReLU**: 64 filters of size 7×7×64 are used, and batch normalization is added between convolution and ReLU.
* **Conv+BN**: for the last layer, 3 filters of size 7×7×64 are used to reconstruct the output, and batch normalization is added between convolution and ReLU.

**Loss Function**: mean squared error</br>
**Optimizer**: Adam

#### Hyperparamters:
* Batch Size = 32
* no. of epochs = 30

### Results
![GitHub Logo](/images/result3.png)

### Evaluation
**PSNR**: 28.449 </br>
**SSIM**: 0.985

## Conclusion
We have used PSNR and SSIM for image similarity tests. They are used to determine, how much quality is lost with the encoding process. PSNR is an engineering abbreviation for **peak signal-to-noise ratio**. SSIM stands for **structural similarity index** and is a more complex test aimed to determine perceivable difference between two images.</br>
One of the advantages of the SSIM metric is that it better represents human visual perception than does PSNR. SSIM is more complex, however, and takes more time to calculate.

 Models | PSNR | SSIM
------------ | ------------ | -------------
 DeNoising CNN(DnCNN) | 27.085 | 0.980
 Denoising Autoencoder(DAE) | 27.972 | 0.983
 Wide Inference Network(WIN) | 28.449 | 0.985
 
#### Frameworks
* Keras
* Tensorflow for Backend
## References 
* [U-Net: Convolutional Networks for Biomedical Image Segmentation](https://link.springer.com/chapter/10.1007%2F978-3-319-24574-4_28)
* [Medical image denoising using convolutional denoising autoencoders](https://arxiv.org/pdf/1608.04667.pdf)
* [Beyond a Gaussian Denoiser: Residual Learning of Deep CNN for Image Denoising](https://arxiv.org/pdf/1608.03981.pdf) 
* [Wide Inference Network for Image Denoising via Learning Pixel-distribution Prior](https://arxiv.org/pdf/1707.05414.pdf) 
