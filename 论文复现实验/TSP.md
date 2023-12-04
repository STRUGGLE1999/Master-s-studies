原文题目：**Rethinking Transformer-based Set Prediction for Object Detection**
原文链接：[https://arxiv.org/abs/2011.10881](https://arxiv.org/abs/2011.10881)
源代码：[https://github.com/Edward-Sun/TSP-Detection](https://github.com/Edward-Sun/TSP-Detection)


### 登录指令：
账号：ssh  root@region-7.autodl.com  29041
密码：JYdPkgi9CX
## 代码复现过程：
先把项目克隆到数据盘：
```
git clone https://github.com/Edward-Sun/TSP-Detection.git
```
修改 environment.yml 文件中的环境名为：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1666610302752-525addc4-b524-4ddd-b143-6a923155c472.png#averageHue=%23f4f2f2&clientId=u0055cab1-84af-4&from=paste&height=83&id=uee022c37&originHeight=165&originWidth=941&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14647&status=done&style=none&taskId=u986fe280-e4cf-4048-b6da-733482fa261&title=&width=470.5)

创建环境：
```
conda env create -f environment.yml
```
安装detectron2:
```
git clone https://github.com/facebookresearch/detectron2.git
cd detectron2
git reset --hard b88c6c06563e4db1139aafbd6d8d97d1fa7a57e4
pip install -e .
```
开始训练模型：
For TSP-FCOS,
```
bash tsp_fcos.sh
```
报错：
AssertionError: Override list has odd length: ...........................； it must be a list of pairs

在tsp_fcos.sh 文件中增加输出路径：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1666610707281-96d85395-9482-4754-9fb9-c1310a723227.png#averageHue=%23f8f4f4&clientId=u0055cab1-84af-4&from=paste&height=148&id=u1ef31718&originHeight=295&originWidth=1206&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64361&status=done&style=none&taskId=ue0cae7dc-6db1-4694-a7d6-2232ba435d7&title=&width=603)

在tsp_fcos.sh 文件中添加数据集路径为：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1666786333163-b8761188-976c-49cf-b52c-463d64123544.png#averageHue=%23fcfcfc&clientId=uda081c0c-11ea-4&from=paste&height=105&id=uec0abc30&originHeight=209&originWidth=1875&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18904&status=done&style=none&taskId=ubf5eec3c-3987-4a9d-b9a4-70f80ca43a0&title=&width=937.5)


import torch.distributed as dist
