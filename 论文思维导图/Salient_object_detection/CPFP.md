# Contrast Prior and Fluid Pyramid Integration for RGBD Salient Object Detection

## 论文信息
### 作者Jia-Xing Zhao1,∗
Contrast Prior and Fluid Pyramid Integration for RGBD Salient Object Detection
Yang Cao1,∗ Deng-Ping Fan1,* Ming-Ming Cheng1 Xuan-Yi Li1 Le Zhang 2 1 TKLNDST, CS, Nankai University 2 A*STAR
### 论文源代码
#### https://mmcheng.net/rgbdsalpyr/
### 发表年份
#### 2019
## 摘要
### 背景
#### 深度传感器方便获取给RGBD图像的SOD研究带来了有价值的互补信息。然而，深度图与RGB之间存在本质区别，如果直接使用ImageNet预训练模型提取深度通道特征，再把它们与RGB特征融直接融合，带来的结果可能大大下降。
### 解决方案
#### 使用对比先验，运用在CNN结构中，可以增强深度信息，把增强的深度信息线索进一步与RGB特征整合
### 结果
#### CPFP网络在5个数据集上与9种最好的模型进行实验比较，展示了卓越的性能。
## 实验
### 数据集
#### 训练集：1400 samples from the NJU2000 [32] and 650 sam- ples from the NLPR [42] for training
#### 100 images from NJU2000 and 50 from NLPR as the validation set.
#### NJU2000，NLPR，RGBD1000，STEREO，LFSD，RGBD135，DES。
### 训练参数
####  a single NVIDIA TITAN X GPU，batch size为1，iter size为10。
### 评估指标
#### S-measure，mean F-measure，max F-measure，mean absolute error (MAE)，
## 引言
### 基于RGBD的SOD方法主流通过简单的连接操作把RGB和深度特征拼接，然后通过早期融合，后期融合和中期融合进行特征融合，作者认为这样导致结果性能大大下降。
### 子主题 2
### 存在两大挑战
#### 高质量的深度图短缺
##### 缺乏好的预训练backbone网络从深度图提取有效特征。
#### 多尺度跨模态融合方案欠优化
##### 两种模态内在的区别会导致简单融合策略时不一致问题。
### 本文方案
#### 使用对比先验增强深度信息，增强的深度信息作为注意力映射和RGB融合得到高质量SOD结果。
#### 提出了对比损失，衡量显著性区域和非显著性区域的对比度和联系。
#### 设计了流金字塔整合网络融合RGB个深度特征，由一组从高级别CNN层到低级别CNN层的短连接层组成，以金字塔的方式整合特征。
