
源码地址：[https://paperswithcode.com/paper/fcos-fully-convolutional-one-stage-object](https://paperswithcode.com/paper/fcos-fully-convolutional-one-stage-object)
MindSpore版本号：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1685632957599-7be99095-4811-4dd7-81e7-ff78f347ca23.png#averageHue=%2316181b&clientId=uc6a396bf-b524-4&from=paste&height=90&id=ub68ce57c&originHeight=179&originWidth=1781&originalType=binary&ratio=2&rotation=0&showTitle=false&size=34103&status=done&style=none&taskId=u0e7135b6-dcd5-4e96-8a56-ef537ad2174&title=&width=890.5)



![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1691745228301-7ef69bb9-712e-4d52-b33a-397a38027816.png#averageHue=%23090807&clientId=u22ff4a06-bfee-4&from=paste&height=279&id=u72e56130&originHeight=558&originWidth=1405&originalType=binary&ratio=2&rotation=0&showTitle=false&size=57261&status=done&style=none&taskId=u4442f8e1-a6b9-4f1a-9ff7-b3be3380c31&title=&width=702.5)
## 安装MindSpore
检查驱动脚本：
```
nvidia-smi
```
结果如下：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1691744705794-0370955a-3000-40cc-9166-dfd8a9d4252e.png#averageHue=%230e0d0c&clientId=u22ff4a06-bfee-4&from=paste&height=388&id=u4b7e1f0e&originHeight=776&originWidth=1335&originalType=binary&ratio=2&rotation=0&showTitle=false&size=72921&status=done&style=none&taskId=u61ed1a9e-bc78-48fe-b179-bd41cb28b15&title=&width=667.5)
自动安装脚本：
```
wget https://gitee.com/mindspore/mindspore/raw/r2.0/scripts/install/ubuntu-gpu-pip.sh
# 安装MindSpore 2.0.0，Python 3.7和CUDA 11.1。
MINDSPORE_VERSION=2.0.0 bash -i ./ubuntu-gpu-pip.sh
# 如需指定安装Python 3.9，CUDA 10.1以及MindSpore 1.6.0，使用以下方式
# PYTHON_VERSION=3.9 CUDA_VERSION=10.1 MINDSPORE_VERSION=1.6.0 bash -i ./ubuntu-gpu-pip.sh
```


conda create -n  FCOS python==3.8
尝试使用conda安装：
conda install mindspore=2.0.0 -c mindspore -c conda-forge

使用如下命令验证是否安装成功：
python -c "import mindspore;mindspore.run_check()"

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1691892457277-5859609a-58eb-4739-88d0-bb52045233d5.png#averageHue=%230f0d0c&clientId=uf7ca16a2-f532-4&from=paste&height=76&id=udccee801&originHeight=152&originWidth=1854&originalType=binary&ratio=2&rotation=0&showTitle=false&size=31992&status=done&style=none&taskId=u4face159-9caf-479e-8f64-3e127b2636f&title=&width=927)
代表安装成功
 
# 安装教程

镜像：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696395559471-91f4a2a8-34d6-4cdc-83d3-fe6aa0182d5d.png#averageHue=%23fefdfd&clientId=ub0d321c6-c725-4&from=paste&height=557&id=u1c94503a&originHeight=1114&originWidth=2314&originalType=binary&ratio=2&rotation=0&showTitle=false&size=155661&status=done&style=none&taskId=uc9f081d7-7f7e-4246-803b-34f92c773e5&title=&width=1157)

创建虚拟环境
```
conda create --name FCOS python=3.8
```
因为我在启智平台上进行调试任务，当前的进程使用的是 **sh，导致看到当前目录，所以要转换成其他shell名称**
```
bash

```

这将启动 Bash shell 并将您的当前命令行会话切换到 Bash。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696396276405-8d3ee4e7-adf7-4bc3-9da1-eace786040ea.png#averageHue=%23faf8f7&clientId=ub0d321c6-c725-4&from=paste&height=44&id=uddb1b782&originHeight=88&originWidth=813&originalType=binary&ratio=2&rotation=0&showTitle=false&size=22191&status=done&style=none&taskId=uf45f02fb-1612-431f-934a-1855fc05c4a&title=&width=406.5)
下载cocoapi

安装Pytorch 

```
pip install torch==1.8.1+cu101 torchvision==0.9.1+cu101 torchaudio==0.8.1 -f https://download.pytorch.org/whl/torch_stable.html
```
导出onnx模型
```
python onnx/export_model_to_onnx.py \
    --config-file configs/fcos/fcos_imprv_R_50_FPN_1x.yaml \
    MODEL.WEIGHT FCOS_imprv_R_50_FPN_1x.pth
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696505315079-e031e7df-62ad-4d71-b656-cfd1e79ed539.png#averageHue=%23faf8f7&clientId=uf7cc6e24-2bd9-4&from=paste&height=98&id=ua38fcbd5&originHeight=196&originWidth=1886&originalType=binary&ratio=2&rotation=0&showTitle=false&size=116268&status=done&style=none&taskId=ufd60a50f-cfab-4464-a010-a18a16fbe4d&title=&width=943)
要修改export_model_to_onnx.py文件
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696505359277-4ebcdd9a-5ce1-41d4-bc5f-c87c11b97449.png#averageHue=%23fbf7f7&clientId=uf7cc6e24-2bd9-4&from=paste&height=276&id=u4f0bb1e0&originHeight=551&originWidth=1237&originalType=binary&ratio=2&rotation=0&showTitle=false&size=85993&status=done&style=none&taskId=u1401e0ec-1f4d-456b-9025-b0a6e9766e6&title=&width=618.5)
执行上述代码继续报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696505777855-5581ac18-414e-444d-a650-76fb19b71807.png#averageHue=%23fbfaf8&clientId=uf7cc6e24-2bd9-4&from=paste&height=154&id=u3b410d8f&originHeight=308&originWidth=1869&originalType=binary&ratio=2&rotation=0&showTitle=false&size=156226&status=done&style=none&taskId=u9f8ef83b-1f6b-406a-aba2-b2001c86080&title=&width=934.5)
解决办法：
修改FCOS/fcos_core/utils/imports.py文件内容如下：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696505820917-ce60af10-d40f-493a-b91e-fcba2625ba50.png#averageHue=%23fdfdfd&clientId=uf7cc6e24-2bd9-4&from=paste&height=162&id=u748e25f1&originHeight=323&originWidth=1192&originalType=binary&ratio=2&rotation=0&showTitle=false&size=36368&status=done&style=none&taskId=u70b058ef-50e4-45f1-839e-491b7f8b31a&title=&width=596)
报错：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696506520139-313de97d-7ae4-441a-bf19-8b852300314c.png#averageHue=%23fbfaf8&clientId=uf7cc6e24-2bd9-4&from=paste&height=330&id=ud889f900&originHeight=660&originWidth=1856&originalType=binary&ratio=2&rotation=0&showTitle=false&size=349437&status=done&style=none&taskId=ua4426d4f-984b-4733-8289-71792768699&title=&width=928)
解决办法：
```
apt-get update
apt-get install libglib2.0-dev
```

报错：
ImportError: cannot import name '_C' from 'fcos_core' (/code/FCOS/fcos_core/__init__.py)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696506568604-b5db56d3-1308-463d-9f63-08e4a2942ab4.png#averageHue=%23fbf9f8&clientId=uf7cc6e24-2bd9-4&from=paste&height=279&id=ubdec9224&originHeight=557&originWidth=1541&originalType=binary&ratio=2&rotation=0&showTitle=false&size=249761&status=done&style=none&taskId=uc414a2a6-49d4-496f-a9c2-21a7267b6b4&title=&width=770.5)


![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1696510647877-7df229b0-9ad9-4bab-b8fb-4b936f057f6b.png#averageHue=%23faf9f7&clientId=uf7cc6e24-2bd9-4&from=paste&height=351&id=ud3c9450b&originHeight=702&originWidth=1854&originalType=binary&ratio=2&rotation=0&showTitle=false&size=464154&status=done&style=none&taskId=u9c36c0ba-003f-4600-8f88-3d5c26eba26&title=&width=927)




更换策略：先在autodl上跑通，再放到启智平台去
启智平台：


测试命令：
```
python tools/test_net.py \
    --config-file configs/fcos/fcos_imprv_R_50_FPN_1x.yaml \
    MODEL.WEIGHT FCOS_imprv_R_50_FPN_1x.pth \
    TEST.IMS_PER_BATCH 4  
```

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700564955848-22f70a6a-ccdc-4316-a598-05f9e2c61d71.png#averageHue=%230b0a08&clientId=u15f5e067-f12d-4&from=paste&height=542&id=uea6883c7&originHeight=1084&originWidth=1881&originalType=binary&ratio=2&rotation=0&showTitle=false&size=553298&status=done&style=none&taskId=u81aa5070-fe47-48bd-9df1-ea12eb80de3&title=&width=940.5)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700565140609-17adbb0f-6239-4ebc-b4b4-6675d0ff8777.png#averageHue=%23131110&clientId=u15f5e067-f12d-4&from=paste&height=228&id=ue0f5a52a&originHeight=455&originWidth=1891&originalType=binary&ratio=2&rotation=0&showTitle=false&size=203352&status=done&style=none&taskId=u12cdcda9-7662-47c4-93b2-0c223984825&title=&width=945.5)

报错：AttributeError: module 'numpy' has no attribute 'float'.
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700565317848-dfadf9c9-1872-47be-9f22-541302673949.png#averageHue=%23080604&clientId=u15f5e067-f12d-4&from=paste&height=389&id=ub2677833&originHeight=777&originWidth=1848&originalType=binary&ratio=2&rotation=0&showTitle=false&size=446528&status=done&style=none&taskId=uba365aa9-ac11-4eb9-9cc7-8edc723756c&title=&width=924)

把/root/miniconda3/envs/FCOS/lib/python3.8/site-packages/pycocotools-2.0-py3.8-linux-x86_64.egg/pycocotools/cocoeval.py中的 
np.float改为float
再次运行可以正常：
Loading and preparing results...
DONE (t=3.32s)
creating index...
index created!
Running per image evaluation...
Evaluate annotation type *bbox*
DONE (t=40.68s).
Accumulating evaluation results...
DONE (t=10.90s).
 Average Precision  (AP) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.387
 Average Precision  (AP) @[ IoU=0.50      | area=   all | maxDets=100 ] = 0.572
 Average Precision  (AP) @[ IoU=0.75      | area=   all | maxDets=100 ] = 0.419
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= small | maxDets=100 ] = 0.227
 Average Precision  (AP) @[ IoU=0.50:0.95 | area=medium | maxDets=100 ] = 0.423
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= large | maxDets=100 ] = 0.501
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=  1 ] = 0.325
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets= 10 ] = 0.536
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.572
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= small | maxDets=100 ] = 0.367
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=medium | maxDets=100 ] = 0.618
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= large | maxDets=100 ] = 0.726
Maximum f-measures for classes:
[0.768105191197219, 0.5761467889908257, 0.6490865218890038, 0.6907241462823929, 0.8279508305062343, 0.7700498220640569, 0.7887217476755612, 0.5221627164995443, 0.5152105499775214, 0.5565604001414641, 0.8235294117647058, 0.708176827506501, 0.6208742194469223, 0.36652798988888297, 0.5822072072072072, 0.8643021425977484, 0.7724704313340119, 0.7288886246767529, 0.6833340070334292, 0.7430093209054593, 0.830107954828118, 0.8565072302558399, 0.8578749572941579, 0.8723597252818452, 0.3360000000000001, 0.5903176126598666, 0.3467702768334143, 0.5588100686498857, 0.5647058823529412, 0.8169014084507044, 0.45407815693317644, 0.509090909090909, 0.6603332579356103, 0.5997605311853705, 0.5015661707126077, 0.6169135570973187, 0.7522388059701494, 0.5753854428110433, 0.7511035653650255, 0.5789109696316336, 0.5683491062039958, 0.5812779396392589, 0.4712328767123288, 0.3391620869316965, 0.2982456140350877, 0.5538863728414153, 0.4769185097224284, 0.36448896941276776, 0.4954128440366973, 0.49067303387554095, 0.4750593824228028, 0.39919354838709675, 0.5153563840061812, 0.6986800299640947, 0.6344133099824869, 0.5355698667066048, 0.4675239755884917, 0.5777373093557934, 0.512743628185907, 0.6634593090656156, 0.4678198061205094, 0.725413724043428, 0.7078823102070468, 0.7250459277403553, 0.7817589576547231, 0.4812407263477709, 0.6783969804618117, 0.5786452353616532, 0.7332508396676685, 0.5813554028732043, 0.4971751412429378, 0.5740318906605922, 0.6900381811870879, 0.3749481542928245, 0.7285024154589372, 0.5504472644060745, 0.44974446337308355, 0.668391056554143, 0.14754098360655737, 0.42488619119878607]
Score thresholds for classes (used in demos for visualization purposes):
[0.5138152837753296, 0.4968557357788086, 0.5236637592315674, 0.4952307641506195, 0.5052431225776672, 0.551977276802063, 0.5090348124504089, 0.5128800272941589, 0.4887359142303467, 0.45368823409080505, 0.6123636960983276, 0.6680353879928589, 0.5932036638259888, 0.49370139837265015, 0.41021421551704407, 0.5543239712715149, 0.5736200213432312, 0.5268376469612122, 0.48511454463005066, 0.47042253613471985, 0.5329775810241699, 0.5816246867179871, 0.528559684753418, 0.5280802249908447, 0.4845416843891144, 0.48227694630622864, 0.4918561577796936, 0.5263693928718567, 0.494226336479187, 0.5268523097038269, 0.47445714473724365, 0.538428783416748, 0.4800601005554199, 0.44550901651382446, 0.5101462602615356, 0.5688150525093079, 0.49220529198646545, 0.5033478140830994, 0.5067787766456604, 0.48865488171577454, 0.5063015222549438, 0.5103254914283752, 0.4859100580215454, 0.4748985171318054, 0.4137371778488159, 0.531545877456665, 0.43881601095199585, 0.46433159708976746, 0.5231248140335083, 0.48821520805358887, 0.48594722151756287, 0.45846009254455566, 0.46956494450569153, 0.5007169842720032, 0.4563981294631958, 0.48203137516975403, 0.4703395366668701, 0.4908272922039032, 0.5006271004676819, 0.5121785998344421, 0.48977741599082947, 0.6013232469558716, 0.6057623028755188, 0.5123581886291504, 0.5048255324363708, 0.46509069204330444, 0.5198003649711609, 0.5421395301818848, 0.569165050983429, 0.5090237259864807, 0.5335904359817505, 0.4833342730998993, 0.5528267621994019, 0.4215298593044281, 0.5632297992706299, 0.5180739164352417, 0.5104530453681946, 0.5178412795066833, 0.43741121888160706, 0.4984203279018402]
2023-11-21 19:33:30,861 fcos_core.inference INFO: OrderedDict([('bbox', OrderedDict([('AP', 0.3873268792341088), ('AP50', 0.5724785579355487), ('AP75', 0.41876218658108605), ('APs', 0.22674958002195558), ('APm', 0.42339431329994887), ('APl', 0.501098349186377)]))])


![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700566550328-2769f5df-8e8f-49e1-85cb-932b86aff50c.png#averageHue=%23110e0b&clientId=u15f5e067-f12d-4&from=paste&height=525&id=u8dc5a13d&originHeight=1049&originWidth=1891&originalType=binary&ratio=2&rotation=0&showTitle=false&size=983741&status=done&style=none&taskId=u2dfb6cc0-43dd-4fd6-aa10-2571e7b6e10&title=&width=945.5)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700566562391-97f48fe1-bd5e-4dc0-aab6-65f7675a4231.png#averageHue=%231b1712&clientId=u15f5e067-f12d-4&from=paste&height=442&id=u9f15ea12&originHeight=884&originWidth=1897&originalType=binary&ratio=2&rotation=0&showTitle=false&size=1293008&status=done&style=none&taskId=uf0189114-2e64-48a4-8e04-36f6f9a13bf&title=&width=948.5)

安装MindSpore2.2.0
```
conda install mindspore=2.2.0 -c mindspore -c conda-forge
```

验证是否安装成功：
```
python -c "import mindspore;mindspore.set_context(device_target='GPU');mindspore.run_check()"
```

输出以下代表成功：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700570207989-d1fd519b-6192-4dcd-a173-aa69aac7217c.png#averageHue=%23070504&clientId=u15f5e067-f12d-4&from=paste&height=87&id=ueb310284&originHeight=174&originWidth=1855&originalType=binary&ratio=2&rotation=0&showTitle=false&size=86022&status=done&style=none&taskId=u46755d2b-65cf-4c9a-9002-ee19215e50c&title=&width=927.5)

## 分析算法及网络结构

基于 Faster R-CNN 架构

| 模块 | 实现 |
| --- | --- |
| backbone | ResNet-50 |
| neck |  FPN  |
| head | FCOS |

trick

| trick | 记录 |
| --- | --- |
| 数据增强 |  |
| 学习率衰减策略 | 学习率预热（warm-up）的策略为常数 |
| 优化器参数 | 不带动量 SGD ，**weight_decay **L2正则化 0.0001， |
| 训练参数 |  |
| 网络结构优化点 |  |
| 训练流程优化点 |  |

训练模型：
```cpp
python -m torch.distributed.launch \
    --nproc_per_node=1 \
    --master_port=$((RANDOM + 10000)) \
    tools/train_net.py \
    --config-file configs/fcos/fcos_imprv_R_50_FPN_1x.yaml \
    DATALOADER.NUM_WORKERS 2 \
    OUTPUT_DIR training_dir/fcos_imprv_R_50_FPN_1x
```
下载预训练模型：
```cpp
wget https://dl.fbaipublicfiles.com/detectron/ImageNetPretrained/MSRA/R-50.pkl
```
上述模型放到目录/root/.torch/models/R-50.pkl下

在npu上训练：
```
sh run_standalone_train_npu.sh  \
/home/ma-user/work/FCOS/dataset/train2017  \
/home/ma-user/work/FCOS/dataset/annotations/instances_train2017.json \
/home/ma-user/work/FCOS/resnet50.ckpt  \
checkpoint 0 1

```

```
bash FCOS/scripts/run_distribute_train_npu.sh && FCOS/dataset/annotations/instances_train2017.json && FCOS/pretrained_model/resnet50.ckpt &&  echo "Training completed successfully!"

```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701498862303-7b7a0f06-4271-45d5-b5d3-6711b0a66509.png#averageHue=%23fefefe&clientId=u3ae53878-fd37-4&from=paste&height=508&id=uffd0c48c&originHeight=1016&originWidth=2450&originalType=binary&ratio=2&rotation=0&showTitle=false&size=98649&status=done&style=none&taskId=u4b5378bc-ecaf-4804-aa18-c3a460dc8f5&title=&width=1225)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701498874102-69793e02-5d8c-43a6-9e5c-a4bfde564a9e.png#averageHue=%23dfc3a2&clientId=u3ae53878-fd37-4&from=paste&height=408&id=ufdb567d9&originHeight=815&originWidth=2307&originalType=binary&ratio=2&rotation=0&showTitle=false&size=120789&status=done&style=none&taskId=u65d6c643-ab54-428d-a839-31cec595e48&title=&width=1153.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701498893578-6603e656-5028-4bb0-a407-61478958d51e.png#averageHue=%23fdfdfd&clientId=u3ae53878-fd37-4&from=paste&height=484&id=u95ceb759&originHeight=967&originWidth=2416&originalType=binary&ratio=2&rotation=0&showTitle=false&size=111938&status=done&style=none&taskId=ua483b6c9-3068-43c9-ab40-eb8c7a4cd49&title=&width=1208)
报错：

bash run_distribute_train_gpu.sh [TRAIN_DATA_PATH] [ANNO_DATA_PATH] [PRETRAIN_PATH] [CKPT_SAVE_PATH]

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701500666148-e1605de9-bd3f-45b0-aec2-4665e0ee40a7.png#averageHue=%23f7f7f7&clientId=u3ae53878-fd37-4&from=paste&height=192&id=u19f3eddd&originHeight=383&originWidth=1158&originalType=binary&ratio=2&rotation=0&showTitle=false&size=42928&status=done&style=none&taskId=u3f23d099-756d-4b92-adbb-f963272bce1&title=&width=579)
解决办法，设置输入超参数时，把“--”去掉：如图所示：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701503747903-af85b332-6047-4435-b454-015c975e16f4.png#averageHue=%23dfc4a3&clientId=u3ae53878-fd37-4&from=paste&height=404&id=u0198daee&originHeight=808&originWidth=2140&originalType=binary&ratio=2&rotation=0&showTitle=false&size=119405&status=done&style=none&taskId=ufdc37d63-db4d-41be-95a9-2694f70634f&title=&width=1070)

cp: cannot access '/home/ma-user/package/RocksDB': Permission denied

/modelarts-job-e35a875e-51b2-4161-8786-25c38ed1e390/MA-CUSTOM-COMMAND.sh: line 2: --train_path=/home/ma-user/modelarts/inputs/--train_path_0/: No such file or directory
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701501543718-fe2aa998-8041-4dd4-962d-6bf9b5095b4d.png#averageHue=%23ebd0cf&clientId=u3ae53878-fd37-4&from=paste&height=111&id=u6eedf88f&originHeight=222&originWidth=1447&originalType=binary&ratio=2&rotation=0&showTitle=false&size=57263&status=done&style=none&taskId=ucc9263a2-297b-4ac5-a1c3-8a5236ac180&title=&width=723.5)

报错：
/modelarts-job-e35a875e-51b2-4161-8786-25c38ed1e390/MA-CUSTOM-COMMAND.sh: line 2: --train_path=/home/ma-user/modelarts/inputs/--train_path_0/: No such file or directory
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701502680908-e6233fc5-f217-4815-a79b-cb9cd9719707.png#averageHue=%23ebe5e4&clientId=u3ae53878-fd37-4&from=paste&height=188&id=u1f238f03&originHeight=376&originWidth=1588&originalType=binary&ratio=2&rotation=0&showTitle=false&size=115219&status=done&style=none&taskId=u54c426a1-9a35-4a62-be43-86087b58de1&title=&width=794)
解决办法：把启动命令改为：bash FCOS/scripts/run_distribute_train_npu.sh && echo "Training completed successfully!"
我当前的代码目录为：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701503334401-e3b3caac-9fd7-4ccb-9645-bfd7cefe2e88.png#averageHue=%23fefefe&clientId=u3ae53878-fd37-4&from=paste&height=381&id=u9c1a13a2&originHeight=762&originWidth=1543&originalType=binary&ratio=2&rotation=0&showTitle=false&size=61234&status=done&style=none&taskId=u3aaec614-31c4-4563-8ec8-b029fc4c260&title=&width=771.5)



报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701503218299-fed9c5ef-5e5b-4cd8-bb09-3d2f5fa4553f.png#averageHue=%23f5f4f2&clientId=u3ae53878-fd37-4&from=paste&height=133&id=u5d2db6cf&originHeight=265&originWidth=1482&originalType=binary&ratio=2&rotation=0&showTitle=false&size=50071&status=done&style=none&taskId=u98888291-7307-466d-8240-ab49d5c34f0&title=&width=741)

报错：Usage: bash run_distribute_train_gpu.sh [TRAIN_DATA_PATH] [ANNO_DATA_PATH] [PRETRAIN_PATH] [CKPT_SAVE_PATH]
/modelarts-job-f1494e05-6bac-429f-91cc-ac25c500f883/MA-CUSTOM-COMMAND.sh: line 2: --ckpt_save_path=/home/ma-user/modelarts/outputs/--ckpt_save_path_0/: No such file or directory
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701505882382-aca5b845-ca3c-4502-8a2a-ecc66daa894e.png#averageHue=%23ede1e1&clientId=u3ae53878-fd37-4&from=paste&height=254&id=u3a37ed88&originHeight=507&originWidth=1625&originalType=binary&ratio=2&rotation=0&showTitle=false&size=143518&status=done&style=none&taskId=u0c8df90b-b7a1-4a77-803c-d12437f9378&title=&width=812.5)


sh run_distribute_train_npu.sh   /home/ma-user/work/FCOS/dataset/train2017/  /home/ma-user/work/FCOS/dataset/annotations/instances_train2017.json  /home/ma-user/work/FCOS/resnet50.ckpt  /home/ma-user/work/FCOS/dataset/chkecpoint/
