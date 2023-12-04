ssh -p 39386 root@region-42.seetacloud.com

环境：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679400652942-3bb527d6-c7eb-4502-bd45-c0a35a5fecb1.png#averageHue=%23f9f9f9&clientId=ua1238e17-fca8-4&from=paste&height=312&id=ue603398a&originHeight=623&originWidth=868&originalType=binary&ratio=2&rotation=0&showTitle=false&size=69049&status=done&style=none&taskId=uef41eac3-d0e8-4818-9b35-473c47b522f&title=&width=434)
服务器账户名：ssh -p 44890 root@region-41.seetacloud.com
密码：2qvTsVo85Q

下载好数据集和测试集，放在VST/Data文件夹下：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679401826786-483a09d5-ca0d-42ff-b61f-2906c675aa73.png#averageHue=%23f1f4f6&clientId=ua1238e17-fca8-4&from=paste&height=387&id=ud19df829&originHeight=774&originWidth=527&originalType=binary&ratio=2&rotation=0&showTitle=false&size=25720&status=done&style=none&taskId=u790cb8b9-627e-47f6-8ade-bd5f62aa5e2&title=&width=263.5)
### RGBD_SOD
### Training, Testing, and Evaluation
```
python train_test_eval.py --Training True --Testing True --Evaluation True    --epochs  20
```
每一轮训练完之后：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679542182312-d4f56b43-3f12-4a2d-93fb-6b1ac2f8db79.png#averageHue=%23090806&clientId=u71339c99-b9bd-4&from=paste&height=53&id=ud53a69ee&originHeight=105&originWidth=1120&originalType=binary&ratio=2&rotation=0&showTitle=false&size=13683&status=done&style=none&taskId=u4b09378b-bc43-4c21-a7ab-d70f588c91a&title=&width=560)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679796947918-d3c3c658-bc30-41c2-a470-5367d9ecaad9.png#averageHue=%23030201&clientId=u9aa9b8c1-6bf9-4&from=paste&height=125&id=u8ed2d4d4&originHeight=249&originWidth=780&originalType=binary&ratio=2&rotation=0&showTitle=false&size=32256&status=done&style=none&taskId=u86a88e3f-0429-402d-aa7d-19362ea63a9&title=&width=390)
### Testing on Our Pretrained RGB-D VST Model
```
python train_test_eval.py --Testing True --Evaluation True
```
Q/K/V含义：
_Q:指的是query,相当于decoder的内容 K:指的是key,相当于encoder的内容 V:指的是value,相当于encoder的内容_ 


![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679804587918-b77cff37-2bdf-4441-8a24-eaaf2d400b6c.png#averageHue=%23fafaf9&clientId=u9aa9b8c1-6bf9-4&from=paste&height=434&id=u511c8bcb&originHeight=868&originWidth=823&originalType=binary&ratio=2&rotation=0&showTitle=false&size=67546&status=done&style=none&taskId=u3415498a-586c-4f44-aa4c-1f8a748177b&title=&width=411.5)
