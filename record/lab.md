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



# 不同超分模型超分到160x160时TFW识别结果

| 识别模型 | 超分模型 | TFW | TFW_rgb |
| --- | --- | --- | --- |
| MobileFaceNet| GT | 0.7792 | 0.9589 |
| InceptionResNetV1 | GT | 0.7385| 0.9672 |
| MobileFaceNet| EDSR | 0.6994 | 0.8989 |
| InceptionResNetV1 | EDSR | 0.7155 | 0.9109 |