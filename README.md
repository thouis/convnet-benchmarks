convnet-benchmarks
==================

Easy benchmarking of all public open-source implementations of convnets.
A summary is provided in the section below.

Machine: `6-core Intel Core i7-5930K CPU @ 3.50GHz` + `NVIDIA Titan X` + `Ubuntu 14.04 x86_64`

##Imagenet Winners Benchmarking
I pick some popular imagenet models, and I clock the time for a full forward + backward pass. I average my times over 10 runs. I ignored dropout and softmax layers.

######One small note: 
The CuDNN benchmarks are done using Torch bindings. One can also do the same via Caffe bindings or bindings of any other library. This note is here to clarify that **Caffe (native)** and **Torch (native)** are the convolution kernels which are present as a default fallback. Some of the frameworks like TensorFlow and Chainer are benchmarked with CuDNN, but it is not explicitly mentioned, and **hence one might think that these frameworks as a whole are faster, than for example Caffe, which might not be the case**.

**[AlexNet (One Weird Trick paper)](https://code.google.com/p/cuda-convnet2/source/browse/layers/layers-imagenet-1gpu.cfg)** - Input 128x3x224x224

| Library         | Class                                                                                                                | Time (ms)  | forward (ms) | backward (ms) |
|:------------------------:|:-----------------------------------------------------------------------------------------------------------:| ----------:| ------------:| -------------:|
| **Nervana-fp16**    | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                    |   **92**   |  **29**      |    **62**     |
| CuDNN[R3]-fp16 (Torch)     | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)       |      96    |  30          |   66          |
| CuDNN[R3]-fp32 (Torch)      | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)       |      96    |  32          |   64          |
| Nervana-fp32        | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                    |      101   |  32          |    69         |
| fbfft   (Torch)                  | [fbnn.SpatialConvolution](https://github.com/facebook/fbcunn/tree/master/src/fft)                           |      104   |  31          |    72         |
| Chainer                 |  [Convolution2D](https://github.com/pfnet/chainer/blob/master/chainer/links/connection/convolution_2d.py)    |    177 | 40 | 136 |
| cudaconvnet2*            | [ConvLayer](https://github.com/soumith/cuda-convnet2.torch/blob/master/cudaconv3/src/filter_acts.cu)        |      177   |  42          |   135         |
| CuDNN[R2] *             | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)       |      231   |  70          |   161         |
| TensorFlow               | [conv2d](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/ops/nn.py)                  |      277   |  70          |   207         |
| Caffe (native)           | [ConvolutionLayer](https://github.com/BVLC/caffe/blob/master/src/caffe/layers/conv_layer.cu)                |      324   | 121          |   203         |
| Torch-7 (native)         | [SpatialConvolutionMM](https://github.com/torch/cunn/blob/master/SpatialConvolutionMM.cu)                   |      342   | 132          |   210         |
| CL-nn (Torch)            | [SpatialConvolutionMM](https://github.com/hughperkins/clnn/blob/master/SpatialConvolutionMM.cl)             |      963   | 388          |   574         |
| Caffe-CLGreenTea         | [ConvolutionLayer](https://github.com/naibaf7/caffe)                                                        |      1442   | 210          |   1232         |

**[Overfeat [fast]](http://arxiv.org/abs/1312.6229)** - Input 128x3x231x231

| Library                  | Class                                                                                                                    | Time (ms)         | forward (ms)            | backward (ms)            |
|:------------------------:|:------------------------------------------------------------------------------------------------------------------------:| -----------------:| -----------------------:| ------------------------:|
| **CuDNN[R3]-fp16**  (Torch)      | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |         **313**       |  **107**                    |  **206**             |
| CuDNN[R3]-fp32  (Torch)      | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |         326       |  113                    |   213                    |
| fbfft  (Torch)                   | [SpatialConvolutionCuFFT](https://github.com/facebook/fbcunn/tree/master/src/fft)                                        |         342       |  114                    |   227                    |
| Nervana-fp16          | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                                 |         355       |  112                    |   242                    |
| Nervana-fp32            | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                                 |         398       |  124                    |   273                    |
| Chainer                 |  [Convolution2D](https://github.com/pfnet/chainer/blob/master/chainer/links/connection/convolution_2d.py)    |    620 | 135 | 484 |
| cudaconvnet2*            | [ConvLayer](https://github.com/soumith/cuda-convnet2.torch/blob/master/cudaconv3/src/filter_acts.cu)                     |         723       |  176                    |   547                    |
| CuDNN[R2] *             | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |         810       |  234                    |   576                    |
| Caffe                    | [ConvolutionLayer](https://github.com/BVLC/caffe/blob/master/src/caffe/layers/conv_layer.cu)                             |         823       |  355                    |   468                    |
| TensorFlow               | [conv2d](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/ops/nn.py)                               |      842         |  216                    |   626                    |
| Torch-7 (native)         | [SpatialConvolutionMM](https://github.com/torch/cunn/blob/master/SpatialConvolutionMM.cu)                                |         878       |  379                    |   499                    |
| CL-nn (Torch)            | [SpatialConvolutionMM](https://github.com/hughperkins/clnn/blob/master/SpatialConvolutionMM.cl)                          |         963       |  388                    |   574                    |
| Caffe-CLGreenTea         | [ConvolutionLayer](https://github.com/naibaf7/caffe)                                                                     |      2857   | 616          |   2240         |

**[OxfordNet [Model-A]](http://arxiv.org/abs/1409.1556/)** - Input 64x3x224x224

| Library                  | Class                                                                                                                    | Time (ms)         | forward (ms)            | backward (ms)            |
|:------------------------:|:------------------------------------------------------------------------------------------------------------------------:| -----------------:| -----------------------:| ------------------------:|
| **Nervana-fp16**    | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                                 |    **529**        |  **167**                |   **362**                |
| Nervana-fp32        | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                                 |        590        |  180                    |   410                    |
| CuDNN[R3]-fp16  (Torch)     | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |       615         |  179                    |   436                    |
| CuDNN[R3]-fp32  (Torch)     | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |       615         |  196                    |   418                    |
| Chainer                 |  [Convolution2D](https://github.com/pfnet/chainer/blob/master/chainer/links/connection/convolution_2d.py)    |    885 | 251 | 632 |
| fbfft    (Torch)                 | [SpatialConvolutionCuFFT](https://github.com/facebook/fbcunn/tree/master/src/fft)                                        |       1092        |  355                    |   737                    |
| cudaconvnet2*            | [ConvLayer](https://github.com/soumith/cuda-convnet2.torch/blob/master/cudaconv3/src/filter_acts.cu)                     |       1229        |  408                    |   821                    |
| CuDNN[R2] *             | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |       1099        |  342                    |   757                    |
| Caffe                    | [ConvolutionLayer](https://github.com/BVLC/caffe/blob/master/src/caffe/layers/conv_layer.cu)                             |       1068        |  323                    |   745                    |
| Torch-7 (native)         | [SpatialConvolutionMM](https://github.com/torch/cunn/blob/master/SpatialConvolutionMM.cu)                                |       1105        |  350                    |   755                    |
| TensorFlow               | [conv2d](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/ops/nn.py)                               |      1510         |  315                    |   1195                    |
| CL-nn (Torch)            | [SpatialConvolutionMM](https://github.com/hughperkins/clnn/blob/master/SpatialConvolutionMM.cl)                          |       3437        |  875                    |   2562                   |
| Caffe-CLGreenTea         | [ConvolutionLayer](https://github.com/naibaf7/caffe)             |      5620   | 988          |   4632         |


**[GoogleNet V1](http://research.google.com/pubs/pub43022.html)** - Input 128x3x224x224

| Library                  | Class                                                                                                                    | Time (ms)         | forward (ms)            | backward (ms)            |
|:------------------------:|:------------------------------------------------------------------------------------------------------------------------:| -----------------:| -----------------------:| ------------------------:|
| **Nervana-fp16**    | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                                 |    **283**        |  **85**                 |   **197**                |
| Nervana-fp32        | [ConvLayer](https://github.com/soumith/convnet-benchmarks/blob/master/nervana/README.md)                                 |        322        |  90                     |   232                    |
| CuDNN[R3]-fp32  (Torch)      | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |       431         |  117                    |   313                    |
| CuDNN[R3]-fp16   (Torch)     | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    |       501         |  109                    |   392                    |
| Chainer                 |  [Convolution2D](https://github.com/pfnet/chainer/blob/master/chainer/links/connection/convolution_2d.py)              |    687            |               189      |   497                       |
| TensorFlow               | [conv2d](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/ops/nn.py)                               |      1084         |  246                    |   838                    |
| Caffe                    | [ConvolutionLayer](https://github.com/BVLC/caffe/blob/master/src/caffe/layers/conv_layer.cu)                             |       1935        |  786                    |   1148                   |
| CL-nn (Torch)            | [SpatialConvolutionMM](https://github.com/hughperkins/clnn/blob/master/SpatialConvolutionMM.cl)                          |       7016        |  3027                   |   3988                   |
| Caffe-CLGreenTea         | [ConvolutionLayer](https://github.com/naibaf7/caffe)                                                                     |      9462   | 746          |   8716         |

## Layer-wise Benchmarking (Last Updated April 2015)

###Spatial Convolution layer (3D input 3D output, densely connected)
##### forward + backprop (wrt input and weights)

| Original Library         | Class/Function Benchmarked                                                                                               | Time (ms)         | forward (ms)            | backward (ms)            |
|:------------------------:|:------------------------------------------------------------------------------------------------------------------------:| -----------------:| -----------------------:| ------------------------:|
| **fbfft**                | [SpatialConvolutionCuFFT](https://github.com/facebook/fbcunn/tree/master/src/fft)                                        |  **256**          |  **101**                | **155**                  |
| cuda-convnet2 *          | [ConvLayer](https://github.com/soumith/cuda-convnet2.torch/blob/master/cudaconv3/src/filter_acts.cu)                     | 977               |  201                    | 776                      |
| cuda-convnet**           | [pylearn2.cuda_convnet](https://github.com/lisa-lab/pylearn2/blob/master/pylearn2/sandbox/cuda_convnet/filter_acts.cu)   | 1077              |  312                    | 765                      |
| CuDNN R2 *               | [cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                    | 1019              |  269                    | 750                      |
| Theano                   | CorrMM                                                                                                                   | 1225              |  407                    | 818                      |
| Caffe                    | [ConvolutionLayer](https://github.com/BVLC/caffe/blob/master/src/caffe/layers/conv_layer.cu)                             | 1231              |  396                    |   835                    |
| Torch-7                  | [SpatialConvolutionMM](https://github.com/torch/cunn/blob/master/SpatialConvolutionMM.cu)                                | 1265              |  418                    | 877                      |
| DeepCL                   | [ConvolutionLayer](https://github.com/hughperkins/DeepCL/blob/master/src/ConvolutionalLayer.cpp)                         |  6280             |  2648                   | 3632                     |
| _cherry-picking_****     | _best per layer_                                                                                                         | _235_             |  _79_                   |   _155_                  |

This table is ___NOT UPDATED For TITAN-X___. These numbers below were on Titan Black and are here only for informational and legacy purposes.

| Original Library         | Class/Function Benchmarked | Time (ms)         | forward (ms)            | backward (ms)            |
|:------------------------:|:------------------------------------------------------------------------------------------------------------------------:| -----------------:| -----------------------:| ------------------------:|
| Theano (experimental)*** | [conv2d_fft](https://github.com/Theano/Theano/blob/master/theano/sandbox/cuda/fftconv.py)                                | **1178**          |  **304**                | **874**                  |
| Torch-7                  | [nn.SpatialConvolutionBHWD](https://github.com/qassemoquab/nnbhwd/blob/master/SpatialConvolutionBHWD.lua)                | 1892              |  581                    | 1311                     |
| ccv                      | [ccv_convnet_layer](https://github.com/liuliu/ccv/blob/unstable/lib/cuda/cwc_convnet.cu)                                 | 809+bw            |  809                    |                          |
| Theano (legacy)          | [conv2d](https://github.com/Theano/Theano/blob/master/theano/sandbox/cuda/blas.py#L674)                                  | 70774             |  3833                   | 66941                    |

* \* indicates that the library was tested with Torch bindings of the specific kernels.
* ** indicates that the library was tested with Pylearn2 bindings.
* *** This is an experimental module which used FFT to calculate convolutions. [It uses a lot of memory according to @benanne](https://github.com/soumith/convnet-benchmarks/pull/5#issuecomment-50548946)
* **** The last row shows results obtainable when choosing the best-performing library for each layer.
* L1 - Input: `128x128` Batch-size `128`, Feature maps:    `3->96`,  Kernel Size: `11x11`,  Stride: `1x1`
* L2 - Input: `64x64`   Batch-size `128`, Feature maps:  `64->128`,  Kernel Size:   `9x9`,  Stride: `1x1`
* L3 - Input: `32x32`   Batch-size `128`, Feature maps: `128->128`,  Kernel Size:   `9x9`,  Stride: `1x1`
* L4 - Input: `16x16`   Batch-size `128`, Feature maps: `128->128`,  Kernel Size:   `7x7`,  Stride: `1x1`
* L5 - Input: `13x13`   Batch-size `128`, Feature maps: `384->384`,  Kernel Size:   `3x3`,  Stride: `1x1`
* The table is ranked according to the total time forward+backward calls for layers (L1 + L2 + L3 + L4 + L5)

#####Breakdown
###### forward
Columns L1, L2, L3, L4, L5, Total are times in **milliseconds**

| Original Library         | Class/Function Benchmarked                                                                                                        |  L1 |   L2 |  L3 | L4 |  L5 | Total |
|:------------------------:|:---------------------------------------------------------------------------------------------------------------------------------:| ---:| ----:| ---:| --:| ---:| -----:|
| fbfft                    | [SpatialConvolutionCuFFT](https://github.com/facebook/fbcunn/tree/master/src/fft)                                                   | 57 |  27 |   6 |  2 |  9 | 101 |
| cuda-convnet2 *          | [ConvLayer](https://github.com/soumith/cuda-convnet2.torch/blob/master/cudaconv3/src/filter_acts.cu)                              | 36 | 113 |  40 |  4 |  8 | 201 |
| cuda-convnet**           | [pylearn2.cuda_convnet](https://github.com/lisa-lab/pylearn2/blob/master/pylearn2/sandbox/cuda_convnet/filter_acts.cu)            | 38 | 183 |  68 |  7 | 16 | 312 |
| CuDNN R2                 |[cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                              | 56 | 143 |  53 |  6 | 11 | 269 |
| Theano                   | CorrMM                                                                                                                            | 91 | 143 | 121 | 24 | 28 | 407 |
| Caffe                    | [ConvolutionLayer\<Dtype>](https://github.com/BVLC/caffe/blob/master/src/caffe/layers/conv_layer.cu)                              | 93 | 136 | 116 | 24 | 27 | 396 |
| Torch-7                  |[nn.SpatialConvolutionMM](https://github.com/torch/cunn/blob/master/SpatialConvolutionMM.cu)                                       | 94 | 149 | 123 | 24 | 28 | 418 |
| DeepCL                   | [ConvolutionLayer](https://github.com/hughperkins/DeepCL/blob/master/src/ConvolutionalLayer.cpp)                                  | 738| 1241 | 518| 47 |104 |2648 |
| _cherry-picking_****     | _best per layer_                                                                                                                  |_36_|_27_ |  _6_| _2_| _8_|  79 |

###### backward (gradInput + gradWeight)
Columns L1, L2, L3, L4, L5, Total are times in **milliseconds**

| Original Library         | Class/Function Benchmarked                                                                                                        |  L1 | L2  |  L3 | L4 |  L5| Total |
|:------------------------:|:---------------------------------------------------------------------------------------------------------------------------------:| ---:| ---:| ---:| --:| --:| -----:|
| fbfft                    | [SpatialConvolutionCuFFT](https://github.com/facebook/fbcunn/tree/master/src/fft)                                                   |  76 |  45 |  12 |  4 | 18 | 155   |
| cuda-convnet2 *          | [ConvLayer](https://github.com/soumith/cuda-convnet2.torch/blob/master/cudaconv3/src/filter_acts.cu)                              | 103 | 467 | 162 | 15 | 29 | 776   |
| cuda-convnet**           | [pylearn2.cuda_convnet](https://github.com/lisa-lab/pylearn2/blob/master/pylearn2/sandbox/cuda_convnet/filter_acts.cu)            | 136 | 433 | 147 | 15 | 34 | 765   |
| CuDNN R2                 |[cudnn.SpatialConvolution](https://github.com/soumith/cudnn.torch/blob/master/SpatialConvolution.lua)                              | 139 | 401 | 159 | 19 | 32 | 750   |
| Theano                   | CorrMM                                                                                                                            | 179 | 405 | 174 | 29 | 31 | 818   |
| Caffe                    | [ConvolutionLayer\<Dtype>](https://github.com/BVLC/caffe/blob/master/src/caffe/layers/conv_layer.cu)                              | 200 | 405 | 172 | 28 | 30 | 835   |
| Torch-7                  |[nn.SpatialConvolutionMM](https://github.com/torch/cunn/blob/master/SpatialConvolutionMM.cu)                                       | 206 | 432 | 178 | 29 | 32 | 877   |
| DeepCL                   | [ConvolutionLayer](https://github.com/hughperkins/DeepCL/blob/master/src/ConvolutionalLayer.cpp)                                  | 484 |2144 | 747 | 59 |198 |  3632 |
| _cherry-picking_****     | _best per layer_                                                                                                                  | _76_| _45_| _12_|_4_ |_18_|_155_  |
