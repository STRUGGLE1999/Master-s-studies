服务器配置：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1670604091987-d52b47b2-dae7-40fc-bd63-6a850d4f25c7.png#averageHue=%230f0d0d&clientId=u2e439c31-71a6-4&from=paste&height=336&id=u6fe68fa4&originHeight=336&originWidth=1219&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17059&status=done&style=none&taskId=u51681eb8-09e5-43a7-b40b-b3dfe648d85&title=&width=1219)


CPU ：12 核心
内存：43 GB
GPU ：NVIDIA GeForce RTX 3090, 1
存储：
  系统盘/               ：90% 18G/20G
  数据盘/root/autodl-tmp：84% 42G/50G
训练运行命令：
```python
bash tools/dist_train.sh configs/petr/petr_r50_16x2_100e_coco.py 2 --work-dir log/petr_r50_16x2_100e_coco_logs --resume-from log/petr_r50_16x2_100e_coco_logs/epoch_4.pth
```
使用COCO_2017 test_dev测试：
```python
bash tools/dist_test.sh  configs/petr/petr_r50_16x2_100e_coco.py  \
log/petr_r50_16x2_100e_coco_logs/epoch_50.pth  2  \
--format-only --options "jsonfile_prefix=results/petr_r50_16x2_100e_coco"    \
--show-dir output/50_petr_r50_16x2_100e_coco_results    \
--work-dir log/petr_r50_16x2_100e_coco_logs   \
--out output/petr_r50_16x2_100e_coco_50.pkl
```
bash tools/dist_train.sh   configs/petr/petr_r50_16x2_100e_coco.py   2  \
 --work-dir  log/petr_r50_16x2_100e_coco_logs  \
--resume-from log/petr_r50_16x2_100e_coco_logs/epoch_50.pth

python train.py   configs/petr/petr_r50_16x2_100e_coco.py   2  \
 --work-dir  log/petr_r50_16x2_100e_coco_logs  \
--resume-from log/petr_r50_16x2_100e_coco_logs/epoch_50.pth

每跑完一轮就会报以下错误：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1671454967195-81f227f1-f219-4228-8302-3d10604e3d76.png#averageHue=%23090706&clientId=ud9e4faab-1d61-4&from=paste&height=544&id=ucb8ba16b&originHeight=1088&originWidth=1861&originalType=binary&ratio=1&rotation=0&showTitle=false&size=620553&status=done&style=none&taskId=u181d83a8-5d1c-4a91-a3b2-9cc05e4471e&title=&width=930.5)
指令：
ssh -p 39386 root@region-42.seetacloud.com
密码：MkwBzHNntA

报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1677923078348-ce8ec9e2-44f2-49f3-9f0f-498eb1597a56.png#averageHue=%230b0806&clientId=uc3234dff-65bf-4&from=paste&height=147&id=u40a1425a&originHeight=294&originWidth=1872&originalType=binary&ratio=2&rotation=0&showTitle=false&size=211359&status=done&style=none&taskId=u1d2090fd-fdb3-457c-8b01-3c86eec7433&title=&width=936)
解决方案：
参考链接：[https://blog.csdn.net/hxxjxw/article/details/119777443](https://blog.csdn.net/hxxjxw/article/details/119777443)
在报错文件中增加如下代码：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1678585221575-2cd17ef8-cf2a-4998-ae33-8e5a2a343d79.png#averageHue=%23fbfbfa&clientId=uff58fffa-a9fb-4&from=paste&height=412&id=uc85cf5dc&originHeight=824&originWidth=2088&originalType=binary&ratio=2&rotation=0&showTitle=false&size=177580&status=done&style=none&taskId=u8cb89d83-d830-47d7-9c84-83b7e18f9ab&title=&width=1044)
运行如下代码时报错：
```
python tools/train.py configs/petr/petr_r50_16x2_100e_coco.py --work-dir log/petr_r50_16x2_100e_coco_logs
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1678586530880-a89d29c5-974f-4535-83c3-700119fdd41c.png#averageHue=%230b0907&clientId=u5eb4ba9e-a2a8-4&from=paste&height=432&id=u768365ed&originHeight=864&originWidth=1881&originalType=binary&ratio=2&rotation=0&showTitle=false&size=546457&status=done&style=none&taskId=uc05bb336-31d2-4992-9323-b33eb00fb81&title=&width=940.5)
将tools/train.py中的代码改成如下：
第87轮测试结果：




