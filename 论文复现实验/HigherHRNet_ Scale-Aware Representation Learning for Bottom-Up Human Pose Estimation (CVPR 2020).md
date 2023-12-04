## SSH 连接
账号：ssh root@region-7.autodl.com  20356
密码：3S0bEp12st

源代码位置：[https://github.com/HRNet/HigherHRNet-Human-Pose-Estimation](https://github.com/HRNet/HigherHRNet-Human-Pose-Estimation)
原文：[https://openaccess.thecvf.com/content_CVPR_2020/papers/Cheng_HigherHRNet_Scale-Aware_Representation_Learning_for_Bottom-Up_Human_Pose_Estimation_CVPR_2020_paper.pdf](https://openaccess.thecvf.com/content_CVPR_2020/papers/Cheng_HigherHRNet_Scale-Aware_Representation_Learning_for_Bottom-Up_Human_Pose_Estimation_CVPR_2020_paper.pdf)
## 跑实验
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1663846320984-43b9eff9-f649-4128-b8da-eb10771b61ca.png#averageHue=%23f6f5f5&clientId=u5ec2d4b0-96e6-4&from=paste&height=343&id=ubb575d5b&originHeight=686&originWidth=515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=48059&status=done&style=none&taskId=u45c70288-1e99-47db-b930-a4097230d9c&title=&width=257.5)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1663849680951-81727541-23bd-41aa-8807-11d6a268d7ca.png#averageHue=%23f9f8f8&clientId=u5ec2d4b0-96e6-4&from=paste&height=213&id=u514665c4&originHeight=425&originWidth=497&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24691&status=done&style=none&taskId=ua7af4e64-1331-4a47-98d3-aaf0111d93d&title=&width=248.5)

- 安装pytorch:（pytorch 官网地址：[https://pytorch.org/](https://pytorch.org/)）
```python
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
```

- 克隆项目
```python
git clone https://github.com/HRNet/HigherHRNet-Human-Pose-Estimation.git
```

- 安装依赖
```python
pip install -r requirements.txt
```

- 安装COCOAPI
```python
# COCOAPI=/path/to/clone/cocoapi
git clone https://github.com/cocodataset/cocoapi.git $COCOAPI
cd $COCOAPI/PythonAPI
# Install into global site-packages
make install
# Alternatively, if you do not have permissions or prefer
# not to install the COCO API into global site-packages
python3 setup.py install --user
```

- 安装[CrowdPoseAPI](https://github.com/Jeff-sjtu/CrowdPose) 
```python
git clone https://github.com/Jeff-sjtu/CrowdPose.git

cd autodl-tmp/HigherHRNet/CrowdPose/crowdpose-api/PythonAPI

sh install.sh
```
这里需要修改/root/autodl-tmp/HigherHRNet/CrowdPose/crowdpose-api/PythonAPI/crowdposetools/cocoeval.py  中的几行代码，参考以下链接：
[https://github.com/Jeff-sjtu/CrowdPose/commit/785e70d269a554b2ba29daf137354103221f479e](https://github.com/Jeff-sjtu/CrowdPose/commit/785e70d269a554b2ba29daf137354103221f479e)

## 训练和测试
#### Testing on COCO val2017 dataset

- single-scale testing:
```python
python tools/valid.py \
    --cfg experiments/coco/higher_hrnet/w32_512_adam_lr1e-3.yaml \
    TEST.MODEL_FILE models/pytorch/pose_coco/pose_higher_hrnet_w32_512.pth
```

test without flip:
```python
python tools/valid.py \
    --cfg experiments/coco/higher_hrnet/w32_512_adam_lr1e-3.yaml \
    TEST.MODEL_FILE models/pytorch/pose_coco/pose_higher_hrnet_w32_512.pth \
    TEST.FLIP_TEST False
```
Multi-scale testing：

```python
python tools/valid.py \
    --cfg experiments/coco/higher_hrnet/w32_512_adam_lr1e-3.yaml \
    TEST.MODEL_FILE models/pytorch/pose_coco/pose_higher_hrnet_w32_512.pth \
    TEST.SCALE_FACTOR '[0.5, 1.0, 2.0]'
```
#### Training on COCO train2017 dataset
```python
python tools/dist_train.py \
    --cfg experiments/coco/higher_hrnet/w32_512_adam_lr1e-3.yaml \
    FP16.ENABLED True FP16.DYNAMIC_LOSS_SCALE True \
    MODEL.SYNC_BN True
```

#### Training on CrowdPose trainval dataset

```python
python tools/dist_train.py \
    --cfg experiments/crowd_pose/higher_hrnet/w32_512_adam_lr1e-3.yaml \
    FP16.ENABLED True FP16.DYNAMIC_LOSS_SCALE True \
    MODEL.SYNC_BN True
```

