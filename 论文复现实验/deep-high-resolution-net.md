源代码地址：[https://github.com/leoxiaobin/deep-high-resolution-net.pytorch](https://github.com/leoxiaobin/deep-high-resolution-net.pytorch)
源代码参考：[https://github.com/WZMIAOMIAO/deep-learning-for-image-processing/tree/master/pytorch_keypoint/HRNet](https://github.com/WZMIAOMIAO/deep-learning-for-image-processing/tree/master/pytorch_keypoint/HRNet)


python autodl-tmp/HRNet/demo/demo.py --image autodl-tmp/HRNet/videos/01.JPG --showFps --write


## SSH 连接
账号：ssh root@region-7.autodl.com  21546     
密码：ePSbNXe8JT
# 复现过程

- 创建虚拟环境：
```python
conda create -n HRNet python=3.6
```
# 跑demo.py遇到的问题
根据 github上demo文件夹中的readme 步骤，然后运行MP4文件，运行以下代码：
```python
python autodl-tmp/HRNet/demo/inference.py --cfg autodl-tmp/HRNet/demo/inference-config.yaml     --videoFile autodl-tmp/HRNet/videos/02.MP4     --writeBoxFrames     --outputDir /root/autodl-tmp/HRNet/output     TEST.MODEL_FILE autodl-tmp/HRNet/models/pytorch/pose_coco/pose_hrnet_w32_256x192.pth 
```

- 报错，找不到模块，最后原因是没有把_init_paths.py文件导入

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660634404544-e68f678b-fc4b-484c-849d-e6da3b3bf2bc.png#averageHue=%23faf8f5&clientId=u2bb8e555-c1e2-4&from=paste&height=104&id=u5f00b3d7&originHeight=104&originWidth=1330&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13878&status=done&style=none&taskId=ub66840db-855a-40bc-8bb2-d092386bdcb&title=&width=1330)

我的文件结构是：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660634630849-7db847af-d253-4d73-ab9f-de544fefe1a9.png#averageHue=%23f1f0ef&clientId=ue198933d-4627-4&from=paste&height=816&id=u722ef4b1&originHeight=816&originWidth=264&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30159&status=done&style=none&taskId=u6ef3eb6d-34d3-427e-9c49-9bbdee95ef4&title=&width=264)

inference.py文件如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660634664952-adcff4d1-bce5-4d9f-b5d2-8056a2edec53.png#averageHue=%23fcfbfa&clientId=ue198933d-4627-4&from=paste&height=465&id=ue0b0e91b&originHeight=465&originWidth=761&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41247&status=done&style=none&taskId=u3f7223fe-326e-4855-9eab-f490e6222ab&title=&width=761)

_init_paths.py文件如下:
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660634725942-cc3c60a0-7f5d-4d79-b69d-ce3748acda5f.png#averageHue=%23fdfdfc&clientId=ue198933d-4627-4&from=paste&height=529&id=u300d04d0&originHeight=529&originWidth=892&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43506&status=done&style=none&taskId=ufcedb443-ef1d-4498-bdfc-bb0e193fb1c&title=&width=892)
这里参考了博客：[https://blog.csdn.net/weixin_42815609/article/details/105172127](https://blog.csdn.net/weixin_42815609/article/details/105172127)（以后写代码可以参考里面提到的自定义路径引用写法）
然后可以正常运行demo.py，但是报错: cannot connect to X server
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660635198212-8dc6abaa-8e74-42a3-9261-321991017e76.png#averageHue=%23101010&clientId=ue198933d-4627-4&from=paste&height=158&id=ua91d81ea&originHeight=158&originWidth=1441&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14465&status=done&style=none&taskId=u33f46dbd-be21-4ebd-b553-ed7f7542ac0&title=&width=1441)

上网查原因是因为项目中有显示图片的代码，但是因为服务器没有图形显示界面，所以不能显示出来。
在inference.py文件中把显示图片的代码注释掉

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660706030914-1db506bd-e0ca-41fe-908e-59d0ff1190cf.png#averageHue=%23fdfafa&clientId=ua0c77164-8d79-4&from=paste&height=471&id=u09a756e5&originHeight=471&originWidth=1038&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63788&status=done&style=none&taskId=uaa736845-8acb-4966-8438-a330791f64a&title=&width=1038)

然后就可以正常运行，可以在output文件夹中看到很多帧图片以及一个视频
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660706106141-d3c595e1-a4d4-469a-90fd-08588c289c97.png#averageHue=%234e8d55&clientId=ua0c77164-8d79-4&from=paste&height=926&id=u2ddceccf&originHeight=926&originWidth=835&originalType=binary&ratio=1&rotation=0&showTitle=false&size=863993&status=done&style=none&taskId=u7a3360c5-fd20-4612-9b92-5acb52a5e62&title=&width=835)
至此，demo运行完毕。
```python
python /root/autodl-tmp/HRNet/demo/demo.py --image /root/autodl-tmp/HRNet/videos/01.JPG --showFps --write
```
python demo/demo.py --image obs_images/1.jpg --showFps --write
## 训练模型
#### Testing on MPII dataset using model zoo's models
切换到HRNet目录下：cd autodl-tmp/HRNet
```python
python tools/test.py \
    --cfg experiments/mpii/hrnet/w32_256x256_adam_lr1e-3.yaml \
    TEST.MODEL_FILE models/pytorch/pose_mpii/pose_hrnet_w32_256x256.pth 
```
报错：AssertionError: Invalid device id
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660886328684-2e97503d-4330-4b2e-b7b7-e9746fcaaf10.png#averageHue=%230a0806&clientId=u9e214200-c0e7-4&from=paste&height=309&id=ud56a5844&originHeight=617&originWidth=1757&originalType=binary&ratio=1&rotation=0&showTitle=false&size=385756&status=done&style=none&taskId=uf50e86b2-deec-4932-b4c3-d8f687a5930&title=&width=878.5)
需要在/root/autodl-tmp/HRNet/experiments/mpii/hrnet/w32_256x256_adam_lr1e-3.yaml中修改一下GPU的数量：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660886406281-40cd1500-2dbd-4250-bc7b-b01d18ae0be4.png#averageHue=%23faf9f8&clientId=u9e214200-c0e7-4&from=paste&height=189&id=ud590e242&originHeight=378&originWidth=1223&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49120&status=done&style=none&taskId=u998c36f7-8bef-4ab5-9b77-5a92b977c2c&title=&width=611.5)
再次运行不报错，但是我不知道是不是正确的运行结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660886467705-b541d265-b8b1-4c09-b3f6-85a065f890fe.png#averageHue=%23f8f6f5&clientId=u9e214200-c0e7-4&from=paste&height=233&id=u824ddbd2&originHeight=465&originWidth=1956&originalType=binary&ratio=1&rotation=0&showTitle=false&size=265815&status=done&style=none&taskId=u86a9c76b-b7ea-4f1f-9073-af2e066ed5f&title=&width=978)
然后在output文件夹下生成了文件
#### Training on MPII dataset
运行代码：
```python
python tools/train.py \
    --cfg experiments/mpii/hrnet/w32_256x256_adam_lr1e-3.yaml
```
报错：AttributeError: module 'torch.onnx' has no attribute 'set_training'
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660959435006-ebd3dcb4-8d4f-4610-80b6-b76b8f2d236f.png#averageHue=%230e0c0b&clientId=u9e214200-c0e7-4&from=paste&height=159&id=u694ab3d6&originHeight=317&originWidth=1776&originalType=binary&ratio=1&rotation=0&showTitle=false&size=173245&status=done&style=none&taskId=u34327512-ce99-42f0-ac24-3333b817b7d&title=&width=888)
找到/root/miniconda3/envs/HRNet/lib/python3.6/site-packages/tensorboardX/pytorch_graph.py 的220行，然后把set_training 换成 select_model_mode_for_export              不再报错。
报错：TypeError: __new__() got an unexpected keyword argument 'serialized_options'
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660960756038-7072a5c5-4a3e-4568-a69a-70cc51bad589.png#averageHue=%230f0c0a&clientId=u9e214200-c0e7-4&from=paste&height=340&id=u15ee5024&originHeight=680&originWidth=1880&originalType=binary&ratio=1&rotation=0&showTitle=false&size=553190&status=done&style=none&taskId=u5f946ea9-d716-4c0d-b33a-bdddc83c450&title=&width=940)
使用以下命令将protobuf卸载后再重新安装：
```python
sudo pip3 uninstall protobuf
sudo pip3 install -U protobuf
```
报错：AttributeError: module 'torch.jit' has no attribute 'get_trace_graph'
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660960846978-bbeea805-8f16-47d1-9fd7-31c7b6368fc6.png#averageHue=%230c0b09&clientId=u9e214200-c0e7-4&from=paste&height=159&id=uf6ab8817&originHeight=318&originWidth=1680&originalType=binary&ratio=1&rotation=0&showTitle=false&size=167724&status=done&style=none&taskId=uf64079c5-ddb1-4730-953d-cedca87ad5e&title=&width=840)
这是因为版本变化，1.1torch可以用get_trace_graph，把get_trace_graph改为_get_trace_graph

报错：AttributeError: 'torch._C.Graph' object has no attribute 'set_graph'
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1660961418399-09813fe0-b294-4f92-bc8c-aa4286b9e20c.png#averageHue=%23090705&clientId=u9e214200-c0e7-4&from=paste&height=183&id=ub0f5ca26&originHeight=366&originWidth=1712&originalType=binary&ratio=1&rotation=0&showTitle=false&size=215559&status=done&style=none&taskId=u7fe0ed10-9d2c-4d8a-9c1f-5fe0bd99b05&title=&width=856)
解决方案：升级tensorboardx
```python
pip install --upgrade tensorboardx
```
解决完上面的错误之后就可以正常训练了。
#### Testing on COCO val2017 dataset using model zoo's models
运行代码：
```python
python tools/test.py \
    --cfg experiments/coco/hrnet/w32_256x192_adam_lr1e-3.yaml \
    TEST.MODEL_FILE models/pytorch/pose_coco/pose_hrnet_w32_256x192.pth \
    TEST.USE_GT_BBOX False
```
#### Training on COCO train2017 dataset
运行代码：
```python
python tools/train.py \
    --cfg experiments/coco/hrnet/w32_256x192_adam_lr1e-3.yaml 
```
#### Visualizing predictions on COCO val
运行代码：
```python
python visualization/plot_coco.py    \ 
--prediction output/coco/pose_hrnet/w32_256x192_adam_lr1e-3/results/keypoints_val2017_results_0.json  \  
--save-path visualization/results

```

#### Visualizing predictions on MPII val
```python
python visualization/plot_mpii.py --prediction visualization/pred.mat --save-path visualization/results_mpii
```

