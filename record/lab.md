# TFW 识别结果

|                    |    TFW        |   TFW_rgb   |   LFW_112   |  LFW_224  |   CelebA_224 | VGGFace2_224 | 
|   ---              |     ---       |     ---     |    ---      |   ---     |    ---       |   ---        |
|MobileFaceNet       |    0.7792     |   0.9589    |   0.9863    |  0.966    |   0.9458     | 0.9245       | 
|InceptionResNetV1   |    0.7385     |   0.9672    |   0.9906    |  0.9593   |   0.9593     | 0.948        |
|mobilnet            |     < 60      |   0.9405    |   0.9911    |   -       |    -         |       -      |
|IResNet50           |     < 60      |    0.7248   |   0.9983    |  0.9823   |   0.9706     | 0.9562       |
|IResNet100          |     < 60      |   < 60      |   0.9898    |  0.9715   |   0.9588     | 0.9306       |

# TFW 下采样到40x40大小的识别结果

|                    |  TFW     | TFW_rgb |
|   ---              |   ---    |   ---   |
| MobileFaceNet      |  0.7239  | 0.9132  | 
| InceptionResNetV1  |  0.6889  | 0.9276  |


# 超分结果比较

| model| modal | pnsr | ssim | pretrained| misc |
| ---  | --- | ---  | ---  | ---    | --- |
| EDSR | thremal | 24.0462  | 0.7570  |  EDSR_Mx4_f64b16_DIV2K_official | --- |
| EDSR | thermal | 23.9512  | 0.7507  |  EDSR_Lx4_f256b32_DIV2K_official | --- |
| EDSR | rgb | 30.4859 | 0.8529 | EDSR_Mx4_f64b16_DIV2K_official | --- |
| EDSR | rgb | 31.2785 | 0.8820 | sejongface_Mx4_f64b16 | --- |
| EDSR | rgb | 31.2810 | 0.8820 | sejongface && div2k | --- |
| EDSR | thermal | 24.0137 | 0.7747 | sejongface_Mx4_f64b16 | --- |
| EDSR | thermal | 24.2811 | 0.7776 | sejongface && div2k | --- |
| SwinIR | rgb | 31.2655 | 0.8818 | sejongface | --- |
| SwinIR | thermal | 23.9281 | 0.7833 | sejongface | --- |
| EDSR | rgb | 30.8318 | 0.8734 | celebA | --- |
| EDSR | thermal | 24.2274 | 0.7754 | celebA | --- |

为进一步探索超分模型对结果的影响。在这里使用两个经典的超分模型作为benchmark。然而，实验结果给出的结论是发现两个模型使用统一训练模型测试后的结果差别不大，因此，进一步分析是否预训练模型是否是决定结果好坏的重要影响因素之一呢？



# 不同超分模型超分到160x160时TFW识别结果

| 识别模型 | 超分模型 | TFW | TFW_rgb | SR pretrained |
| --- | --- | --- | --- | --- |
| MobileFaceNet| GT | 0.7792 | 0.9589 | - |
| InceptionResNetV1 | GT | 0.7385| 0.9672 | - |
| MobileFaceNet| EDSR | 0.6994 | 0.8989 | DIV2k |
| InceptionResNetV1 | EDSR | 0.7155 | 0.9109 | DIV2k |


分析：

从上述实验结果可以得出两个点：

+ 低分辨的识别率确实比原图识别率低
+ 对低分辨的超分结果反而没有原低分图像效果好，这意味着用双线性插值的resize效果比用edsr等一些深度学习模型的识别结果要好。这或许和超分预训练模型有关。

# 使用sejongface的预训练模型进行推理的EDSR超分的识别结果

motivation：上述实验结果表明，直接使用DIV2k的训练模型进行超分，其识别结果比原始的低分辨率结果还低，因此，推断可能是超分模型的问题。
| 识别模型 | 超分模型 | TFW | TFW_rgb | SR pretrained |
| --- | --- | --- | --- | --- |
| InceptionResNetV1 | EDSR | 0.7366 | 0.9338 | sejongface |
| MobileFaceNet| EDSR | 0.7412 | 0.9214 | sejonface |
| InceptionResNetV1 | EDSR | 0.7487 | 0.9335 | sejongface + div2k |
| MobileFaceNet| EDSR | 0.7489 | 0.9207 | sejonface + div2k |
| InceptionResNetV1 | SwinIR | 0.7442 | 0.9336 | sejongface |
| MobileFaceNet| SwinIR | 0.7481 | 0.9182 | sejonface |
| InceptionResNetV1 | EDSR | 0.7255 | 0.9318 | celebA |
| MobileFaceNet| EDSR | 0.7356 | 0.9181 | celebA |


实验结果发现：

+ 对比了不同超分模型，识别率和超分的pnsr以及ssim在一定程度区域正相关，如可见光图像的超分远比热图的结果更加好，从而识别率也更好。
+ 目前的情况来看，用了sejongface和celebA用来训练EDSR模型，在TFW上测试的效果都差不多，并没有本质提升。

**TODO**

- [x] 为探究预训练模型问题，尝试使用更多与人脸相关的数据集或者超分模型进行`inference`，然后进行识别。
- [ ] 既然考虑了预训练的影响后，应该进一步探索模型的效果提升了，目前用作实验的超分模型皆来自自然图像，因此，有必要探索一下人脸超分模型的效果如何。
- [ ] 热图和可见光图识别率之间的差距是一个令人苦恼的问题，寻找有什么方法能弥补这两者之间的差距，亦或是结合热图内在特征提升其识别率。

# 使用AdaFace进行识别
