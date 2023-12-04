服务器账号和密码：

ssh -p 10033 root@region-41.seetacloud.com
6V28OtOWT7
github链接：[https://github.com/IDEA-Research/DN-DETR](https://github.com/IDEA-Research/DN-DETR)
创建环境：
python=3.7.3,pytorch=1.9.0,cuda=11.1
```
conda create -n DN python=3.7.3
```
安装pytorch:(pytorch链接：[https://pytorch.org/get-started/previous-versions/](https://pytorch.org/get-started/previous-versions/)）
```
conda install pytorch==1.9.0 torchvision==0.10.0 torchaudio==0.9.0 cudatoolkit=11.3 -c pytorch -c conda-forge
```
查看cuda版本命令：
```
nvcc -V
```
安装依赖：
```
pip install -r requirements.txt
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1678606713176-eafcfb16-cf3e-4c0f-b8d9-9b1efab0e249.png#averageHue=%23040302&clientId=u3b5b37b7-8178-4&from=paste&height=311&id=u23daf255&originHeight=621&originWidth=1559&originalType=binary&ratio=2&rotation=0&showTitle=false&size=216641&status=done&style=none&taskId=udb67a43c-0953-4821-a635-696a62d27f1&title=&width=779.5)
是因为requirements.txt文件夹中有两个链接下载不了导致的，然后我把链接先提出来，就可以正常安装依赖了。安装完依赖后再下载那两个git链接：
```
安装panopticapi
pip install git+https://github.com/cocodataset/panopticapi.git
下载cocoapi
pip install cython

git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
 
python setup.py build_ext --inplace
python setup.py build_ext install
```
Compiling CUDA operators
```
cd models/dn_dab_deformable_detr/ops
python setup.py build install
# unit test (should see all checking is True)
python test.py
cd ../../..
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679129016685-6e800b69-33a6-4e4e-a175-6eec95456e39.png#averageHue=%230c0a08&clientId=u0b320b0c-9816-4&from=paste&height=544&id=u64d60eb8&originHeight=1087&originWidth=1868&originalType=binary&ratio=2&rotation=0&showTitle=false&size=794098&status=done&style=none&taskId=udd3c0a58-1f2c-431c-8ccb-0c13b25a6a1&title=&width=934)
在运行python test.py 时报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679129123180-17b3a4ed-6c83-49af-8cee-9ad2de02cb5d.png#averageHue=%230a0806&clientId=u0b320b0c-9816-4&from=paste&height=472&id=ucdcc5458&originHeight=943&originWidth=1830&originalType=binary&ratio=2&rotation=0&showTitle=false&size=663599&status=done&style=none&taskId=u0573711e-9a07-43c1-b652-2b39e29c010&title=&width=915)
然后我把通道数量改成了
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679707013498-8d53e335-73d5-4586-aca1-e50b8973b35d.png#averageHue=%23fbfafa&clientId=u8e3c9be2-a425-4&from=paste&height=82&id=udce59323&originHeight=164&originWidth=1107&originalType=binary&ratio=2&rotation=0&showTitle=false&size=16745&status=done&style=none&taskId=ufb863f3e-fed7-4d16-8e82-1e03ac366f1&title=&width=553.5)
71, 1025, 2048, 3096]
### Training your own models
```
# for dn_detr
python main.py -m dn_dab_detr \
  --output_dir logs/dn_DABDETR/R50 \
  --batch_size 1 \
  --epochs 50 \
  --lr_drop 40 \
  --coco_path coco  
  --use_dn
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679134973201-b7c8780c-ae7e-4d67-9df8-9fcd3da6b669.png#averageHue=%23080605&clientId=u9d6dad6c-e6a4-4&from=paste&height=536&id=uae7f8f66&originHeight=1072&originWidth=1867&originalType=binary&ratio=2&rotation=0&showTitle=false&size=530767&status=done&style=none&taskId=u3e0dcda7-6253-42bf-9331-05d3ace8eec&title=&width=933.5)
在运行语句上加上--use_dn

正确训练：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679136744725-ad3bd859-24f2-4917-bd19-648947b12b50.png#averageHue=%23130f0c&clientId=ubc567ee3-986f-4&from=paste&height=60&id=u686d3ace&originHeight=119&originWidth=1877&originalType=binary&ratio=2&rotation=0&showTitle=false&size=134486&status=done&style=none&taskId=uc2a3ca89-33be-405d-9154-a8f7adf6cff&title=&width=938.5)

测试代码：
```
# for dn_deformable_detr: 49.5 AP
python main.py -m dn_dab_deformable_detr \
  --output_dir logs/dab_deformable_detr/R50 \
  --batch_size 1 \
  --coco_path coco \    
  --resume checkpoint/checkpoint0049.pth \   
  --transformer_activation relu \
  --use_dn \
  --eval
```
报错说有不可识别的参数：
改了测试代码为：
```
python main.py \
-m dn_dab_deformable_detr \
--output_dir \logs/dab_deformable_detr/R50 \
--batch_size 1 \
--coco_path coco \
--resume checkpoint/checkpoint0049.pth \
--eval \
--use_dn

```
可以测试：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679708863897-3f009831-fa41-432f-bcfa-082e4488f225.png#averageHue=%2315110d&clientId=u8e3c9be2-a425-4&from=paste&height=285&id=uf97debb3&originHeight=569&originWidth=1864&originalType=binary&ratio=2&rotation=0&showTitle=false&size=700790&status=done&style=none&taskId=u380f687d-9577-44a1-83a7-d5f6aebd0e4&title=&width=932)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679709016120-9441ce28-de1b-4dd8-955a-053105872f56.png#averageHue=%230f0c09&clientId=u8e3c9be2-a425-4&from=paste&height=233&id=uc8dcd134&originHeight=466&originWidth=1188&originalType=binary&ratio=2&rotation=0&showTitle=false&size=245002&status=done&style=none&taskId=u5180eb44-dbcd-4bea-aeb6-b36311621f0&title=&width=594)
运行inference.py
```
python inference.py
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679711998000-b29ee4a0-f819-43f8-b863-2e637268413c.png#averageHue=%23090806&clientId=u8e3c9be2-a425-4&from=paste&height=112&id=ub3e26c58&originHeight=224&originWidth=1254&originalType=binary&ratio=2&rotation=0&showTitle=false&size=89099&status=done&style=none&taskId=u6604526c-9aa7-43a6-94e5-c7695db8770&title=&width=627)
安装**opencv-python**
```
pip install opencv-python  
pip install opencv-contrib-python 
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679817545336-247eb071-5854-476a-a993-8bbc54965192.png#averageHue=%23f5f3f1&clientId=ua00d6bde-75d9-4&from=paste&height=231&id=u0773a1e1&originHeight=461&originWidth=1910&originalType=binary&ratio=2&rotation=0&showTitle=false&size=97506&status=done&style=none&taskId=u8453af99-aca4-4599-918a-06b6f70578b&title=&width=955)
重装pytorch
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679818777754-5fa82ede-61f0-4056-a14d-5776359b08fc.png#averageHue=%23080604&clientId=u0717b52f-c51f-4&from=paste&height=111&id=ud57c530a&originHeight=222&originWidth=1866&originalType=binary&ratio=2&rotation=0&showTitle=false&size=122503&status=done&style=none&taskId=u9173f5fb-8720-4ecf-a493-35fc9af7edf&title=&width=933)

报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679901310868-54fec083-3bfd-4e05-b565-27b9bb3b73f2.png#averageHue=%23090705&clientId=ubd54a482-0909-4&from=paste&height=259&id=uade5c9de&originHeight=518&originWidth=1873&originalType=binary&ratio=2&rotation=0&showTitle=false&size=332967&status=done&style=none&taskId=ucec699b2-8381-4c6f-b488-456ba18f3be&title=&width=936.5)
使用 .is_cuda（)判断哪些参数在cuda上，哪些不在。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679901394373-4ec6dc73-c3e9-4e8c-bdfa-d4263123cf94.png#averageHue=%23fcf9f9&clientId=ubd54a482-0909-4&from=paste&height=70&id=u2b2c0e4b&originHeight=140&originWidth=1407&originalType=binary&ratio=2&rotation=0&showTitle=false&size=30418&status=done&style=none&taskId=u293448ba-9e4a-4cda-92a9-e78ff92a6fc&title=&width=703.5)
输出：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679901407381-766ac942-3298-4aa8-9a01-45c3150a207d.png#averageHue=%23f7f6f5&clientId=ubd54a482-0909-4&from=paste&height=78&id=u1523a7dd&originHeight=156&originWidth=600&originalType=binary&ratio=2&rotation=0&showTitle=false&size=12495&status=done&style=none&taskId=u4d2518d1-ce6b-47d8-b59b-4beeb4a9c1a&title=&width=300)
说明weight在cpu上，而input在gpu上
所以在functional.py上加上两句代码：
```
device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679901825764-2348d58b-ac1d-4d27-a9dd-26c80d6e1a8e.png#averageHue=%23fcfbf9&clientId=ubd54a482-0909-4&from=paste&height=364&id=u67fa39ac&originHeight=727&originWidth=1599&originalType=binary&ratio=2&rotation=0&showTitle=false&size=184149&status=done&style=none&taskId=ue8d19d8a-d180-498b-8867-2d7ac5eae73&title=&width=799.5)
报错：
TypeError: cannot assign 'torch.cuda.FloatTensor' as parameter 'weight' (torch.nn.Parameter or None expected)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679903632573-7d78112e-963e-4eb7-9016-2a49bdfd225a.png#averageHue=%230c0a07&clientId=ubd54a482-0909-4&from=paste&height=398&id=u867c5340&originHeight=796&originWidth=1604&originalType=binary&ratio=2&rotation=0&showTitle=false&size=481967&status=done&style=none&taskId=u3624e2dd-e30f-4fd7-b239-47b68350b56&title=&width=802)
错误原因是：错误原因应该是类型不匹配，不可以将torch.cuda.FloatTensor赋值给Parameter类型，应该是赋值给Parameter的data属性，即
self.T.data = xxx
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679904080346-812a63e3-d0b3-4ba3-91b5-be3418e1a99b.png#averageHue=%23fdf9f9&clientId=ubd54a482-0909-4&from=paste&height=179&id=ue6dce40d&originHeight=358&originWidth=1066&originalType=binary&ratio=2&rotation=0&showTitle=false&size=58181&status=done&style=none&taskId=u50f4a5d5-3125-4be4-b3a1-d410dd71447&title=&width=533)
