登录指令：
ssh -p 44890 root@region-41.seetacloud.com
密码：
2qvTsVo85Q

ssh -p 43456 root@region-41.seetacloud.com

1、克隆项目：
```
git clone https://github.com/rmcong/CIRNet_TIP2022.git
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679808404066-fa7cc369-e864-49fe-83dc-c8c197de2415.png#averageHue=%230a0806&clientId=uf80f0796-03ab-4&from=paste&height=65&id=uaa35d54e&originHeight=130&originWidth=1878&originalType=binary&ratio=2&rotation=0&showTitle=false&size=80071&status=done&style=none&taskId=u2a2625fd-788f-415a-afbc-1041899f84d&title=&width=939)
解决方案：
解除ssl验证后，再次git即可
```
git config --global http.sslVerify false

```
创建虚拟环境：
```
conda create -n CIR python=3.7.3
conda install pytorch==1.9.0 torchvision==0.10.0 torchaudio==0.9.0 cudatoolkit=11.3 -c pytorch -c conda-forge
```
Test:
```
python3 CIRNet_test.py --backbone R50 --test_model CIRNet_R50.pth
```
测试原来训练的模型：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682146870193-c449946e-9e30-4d18-ac4c-978cb5c8265e.png#averageHue=%23120e0b&clientId=u4cc42c59-1816-4&from=paste&height=128&id=uf8c23fe8&originHeight=255&originWidth=747&originalType=binary&ratio=2&rotation=0&showTitle=false&size=103980&status=done&style=none&taskId=ue0c12ceb-f976-4788-b159-6a2edc00b34&title=&width=373.5)
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1679809873081-6f605b7b-045e-4c90-99bc-dde7fcba60d2.png#averageHue=%23070604&clientId=uf80f0796-03ab-4&from=paste&height=74&id=u3843d037&originHeight=148&originWidth=1773&originalType=binary&ratio=2&rotation=0&showTitle=false&size=68701&status=done&style=none&taskId=ub8825829-c55e-466c-acbf-9ccb8756e7a&title=&width=886.5)
解决方法：
```
pip install opencv-python  
pip install opencv-contrib-python 
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1680314512143-0fd8b6c4-6d8e-45fb-a694-29c7edf30874.png#averageHue=%230b0906&clientId=u8f438192-ae0e-4&from=paste&height=147&id=u75ffd642&originHeight=293&originWidth=1864&originalType=binary&ratio=2&rotation=0&showTitle=false&size=210265&status=done&style=none&taskId=u01921767-0e14-4e73-a159-3e27e3d2483&title=&width=932)
百度说可能是pytorch版本不符合，但是我下载的是和github上相同版本的pytorch。还有一个原因是模型损坏。
## Train
```
python3 CIRNet_train.py --backbone R50
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1680319784439-bcb9bca0-163d-4434-b1e6-ee67c5f9b84c.png#averageHue=%23090705&clientId=u8f438192-ae0e-4&from=paste&height=350&id=uc37b8fdd&originHeight=700&originWidth=1651&originalType=binary&ratio=2&rotation=0&showTitle=false&size=347142&status=done&style=none&taskId=ub2417938-2edd-419e-ac33-47c31790286&title=&width=825.5)
解决办法：
在CIRNet_train.py上加上以下代码：
```
torch.cuda.current_device()
torch.cuda._initialized = True
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1680320851037-5c36f802-111f-4458-9b9c-cc02f8c4da68.png#averageHue=%23f1efed&clientId=u8f438192-ae0e-4&from=paste&height=54&id=u65d854a3&originHeight=108&originWidth=2045&originalType=binary&ratio=2&rotation=0&showTitle=false&size=30770&status=done&style=none&taskId=ufc980aa5-67e1-4275-99f0-d87795795c0&title=&width=1022.5)
修改batchsize大小，
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1680320883082-4c6c2368-7620-4df1-87c4-81197a086b23.png#averageHue=%23f9f5f4&clientId=u8f438192-ae0e-4&from=paste&height=323&id=u9b754492&originHeight=645&originWidth=1933&originalType=binary&ratio=2&rotation=0&showTitle=false&size=180185&status=done&style=none&taskId=u66b59280-082c-418e-84e0-e79dad668bc&title=&width=966.5)
正确训练：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1680320822637-06f82159-4fa7-4465-824a-f00330750d35.png#averageHue=%230f0c08&clientId=u8f438192-ae0e-4&from=paste&height=270&id=ucf4a35cc&originHeight=540&originWidth=1186&originalType=binary&ratio=2&rotation=0&showTitle=false&size=335869&status=done&style=none&taskId=u3a612714-9f2b-4374-a812-024968b1472&title=&width=593)
错误：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681052486106-1de6ac85-c6ec-4c64-88c2-c65f417799d3.png#averageHue=%230a0806&clientId=u0690e5b8-5c95-4&from=paste&height=198&id=u06932b49&originHeight=395&originWidth=1870&originalType=binary&ratio=2&rotation=0&showTitle=false&size=86295&status=done&style=none&taskId=uc6d07f5a-e9c8-4dfd-9fa5-db9b0776292&title=&width=935)
原因:tensorboard版本太低了
```
conda install tensorboard==2.0.0
```
将tensorboard升级

pickle文件内容
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681118979897-3c70e50b-c34e-4918-8bdc-c96c20fc2182.png#averageHue=%23ebebeb&clientId=ucf2e3dbe-9c90-4&from=paste&height=317&id=ue80c1481&originHeight=634&originWidth=1867&originalType=binary&ratio=2&rotation=0&showTitle=false&size=317969&status=done&style=none&taskId=u07862e14-9c1e-4380-844e-6444cd348ba&title=&width=933.5)

改变数据集之后训练图：
突然变得很快，有问题
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681182125605-6a41bff3-e44e-4ab9-bfab-6f777b57f223.png#averageHue=%23030202&clientId=uc8956a94-e4ad-4&from=paste&height=534&id=uf66f5bc5&originHeight=1068&originWidth=1442&originalType=binary&ratio=2&rotation=0&showTitle=false&size=190226&status=done&style=none&taskId=u3a358969-734e-408b-bcb9-4cc35bbc176&title=&width=721)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681214954418-e92aecf8-3877-4850-b805-bf22037f5765.png#averageHue=%23faf9f8&clientId=u0dabf9e1-37ff-4&from=paste&height=180&id=uc3784e5e&originHeight=359&originWidth=1216&originalType=binary&ratio=2&rotation=0&showTitle=false&size=38718&status=done&style=none&taskId=u0b4bb3d7-14a0-47b1-8796-7621144eec4&title=&width=608)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681215069166-bc2c38c4-b6ef-46bb-91eb-a67ab8cd3a22.png#averageHue=%23f6f5f4&clientId=u0dabf9e1-37ff-4&from=paste&height=140&id=ud36bc113&originHeight=280&originWidth=1314&originalType=binary&ratio=2&rotation=0&showTitle=false&size=34998&status=done&style=none&taskId=u995c331e-19b2-4766-a52e-9b5f181bb7f&title=&width=657)
改进之后可以正常训练了，但是GPU利用率不高；
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681228358163-d0404b7f-2b78-4783-a0cb-77cad1cfe153.png#averageHue=%23ebd7b4&clientId=u0dabf9e1-37ff-4&from=paste&height=1499&id=ua5ac1e65&originHeight=2997&originWidth=2556&originalType=binary&ratio=2&rotation=0&showTitle=false&size=314600&status=done&style=none&taskId=u32f4b715-a9ec-4eba-a095-4624abafb7f&title=&width=1278)

num_workers=8
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681281587783-2c405c76-c673-42f5-a16b-e86f88c32ac3.png#averageHue=%23120f0c&clientId=ue5b31315-b8c6-4&from=paste&height=75&id=u1e81f333&originHeight=150&originWidth=1129&originalType=binary&ratio=2&rotation=0&showTitle=false&size=81895&status=done&style=none&taskId=ubdbcf00e-7f34-40b8-8c2e-7955680bcdd&title=&width=564.5)
改动：
增加了IOU函数，测试函数模型：
```
python3 CIRNet_test.py --backbone R50 --test_model CIRNet_R50.pth
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681890861046-8c7be50b-d58c-4c38-af46-6ae05635d1ac.png#averageHue=%230a0907&clientId=u9043597b-be03-4&from=paste&height=448&id=uf81b8778&originHeight=895&originWidth=1892&originalType=binary&ratio=2&rotation=0&showTitle=false&size=420092&status=done&style=none&taskId=u50d71587-e3b0-453c-a320-b0a17fd3c73&title=&width=946)

报错：
TypeError: 'test_dataset' object is not iterable

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681972408324-d6351353-e00b-4a79-b718-58131ce83d7e.png#averageHue=%23f8f7f6&clientId=u3ba26c03-d804-4&from=paste&height=108&id=ud344838f&originHeight=215&originWidth=1983&originalType=binary&ratio=2&rotation=0&showTitle=false&size=34703&status=done&style=none&taskId=u6e297992-650e-496c-a1a3-a01acb8474c&title=&width=991.5)
正确写法：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681972591398-7ea8679a-24fe-4ec0-98b6-75ca67f522df.png#averageHue=%23fbf6f5&clientId=u3ba26c03-d804-4&from=paste&height=216&id=u82004b89&originHeight=432&originWidth=2022&originalType=binary&ratio=2&rotation=0&showTitle=false&size=133391&status=done&style=none&taskId=uef817eb7-508d-4f06-a281-8cc96dc9869&title=&width=1011)
报错：
ValueError: not enough values to unpack (expected 4, got 1)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1681972866972-249f05ed-a080-4fd1-af7b-a7c5f22c516b.png#averageHue=%23070604&clientId=u3ba26c03-d804-4&from=paste&height=98&id=u1af45c48&originHeight=196&originWidth=1748&originalType=binary&ratio=2&rotation=0&showTitle=false&size=90165&status=done&style=none&taskId=ua8a53620-95eb-40cf-908b-52144196f1a&title=&width=874)

报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682040774123-83ba4805-8566-40e5-8a5e-1ced611cfaa7.png#averageHue=%23070604&clientId=uee4f3393-3fc3-4&from=paste&height=156&id=u3a9b5b11&originHeight=311&originWidth=1608&originalType=binary&ratio=2&rotation=0&showTitle=false&size=129575&status=done&style=none&taskId=uc2e9d3b1-6017-475a-9de3-be36be9cd1a&title=&width=804)
报错：
RuntimeError: Error(s) in loading state_dict for CIRNet_R50:
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682146734073-7e3876bc-ec4a-47f0-b6f6-2a55244f3544.png#averageHue=%230f0d0b&clientId=u4cc42c59-1816-4&from=paste&height=226&id=u6cbe7e3b&originHeight=452&originWidth=1893&originalType=binary&ratio=2&rotation=0&showTitle=false&size=276013&status=done&style=none&taskId=ue504df7e-a7c2-444b-a72e-3625f5d7970&title=&width=946.5)
解决办法：在加载模型的语句中把strict改成False：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682150702333-5984e083-852d-42b3-81d8-295bd62a931e.png#averageHue=%23fdfcfc&clientId=u4cc42c59-1816-4&from=paste&height=88&id=u5e90403b&originHeight=176&originWidth=1962&originalType=binary&ratio=2&rotation=0&showTitle=false&size=33827&status=done&style=none&taskId=udf5b22fb-0cd4-4ba2-ab2b-5b7ffec5be9&title=&width=981)
训练代码：
```
python3 CIRNet_train2.py --backbone R50
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682242468885-9f6f1c90-99e9-4b56-ba13-cd40ab7fabb1.png#averageHue=%230a0806&clientId=ua245f2d8-328a-4&from=paste&height=427&id=ub8441e11&originHeight=853&originWidth=1877&originalType=binary&ratio=2&rotation=0&showTitle=false&size=539919&status=done&style=none&taskId=u5cb716b6-fb98-4ff0-8934-4145196acd0&title=&width=938.5)
一直到这里，我和原来的模型输出的通道是一样的：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682242527563-d438269b-184c-481c-b9cd-712114a680f2.png#averageHue=%230c0a08&clientId=ua245f2d8-328a-4&from=paste&height=202&id=ua7c3ec22&originHeight=404&originWidth=869&originalType=binary&ratio=2&rotation=0&showTitle=false&size=94125&status=done&style=none&taskId=u6c0a4df3-72f5-4859-b93d-106e3974179&title=&width=434.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682242579249-6074c9b6-57f2-490e-8c43-8aee0e3add12.png#averageHue=%23120f0e&clientId=ua245f2d8-328a-4&from=paste&height=238&id=u37f26b07&originHeight=476&originWidth=754&originalType=binary&ratio=2&rotation=0&showTitle=false&size=111470&status=done&style=none&taskId=ud816f78a-8208-4a2d-abe7-bb1c6617518&title=&width=377)
把通道数改了一下之后，可以跑通了：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682253161697-5e90f24f-254a-471b-86be-55cc5a086f41.png#averageHue=%230a0806&clientId=ua245f2d8-328a-4&from=paste&height=514&id=ufa6c9f4a&originHeight=1027&originWidth=1387&originalType=binary&ratio=2&rotation=0&showTitle=false&size=567343&status=done&style=none&taskId=u01206894-e65f-4e47-9252-633e9a4d5d4&title=&width=693.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682253189516-b58d6774-db63-4ed2-9191-a9380e39bb74.png#averageHue=%23fefcfb&clientId=ua245f2d8-328a-4&from=paste&height=287&id=ueee4db11&originHeight=573&originWidth=1309&originalType=binary&ratio=2&rotation=0&showTitle=false&size=85974&status=done&style=none&taskId=u6293fe6e-3df1-4778-9cf0-edf0f9872ed&title=&width=654.5)

```
python3 CIRNet_test2.py --tag 20230425
```
报错：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682384950177-ad95a6c9-8c58-4c09-99d5-fe01f92b124e.png#averageHue=%23090706&clientId=ua806d851-71c6-4&from=paste&height=243&id=u3d9bf77a&originHeight=485&originWidth=1615&originalType=binary&ratio=2&rotation=0&showTitle=false&size=194345&status=done&style=none&taskId=u9e90739b-ab1b-4da8-95fc-317756adb7a&title=&width=807.5)
将self.net.cuda()改成  self.net().cuda()
报错：
TypeError: cannot unpack non-iterable CIRNet_R50_1 object
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682387219491-4c608960-659c-4c19-b3e8-075ad438bb0a.png#averageHue=%23070604&clientId=ua806d851-71c6-4&from=paste&height=95&id=u03d863e5&originHeight=190&originWidth=1670&originalType=binary&ratio=2&rotation=0&showTitle=false&size=84430&status=done&style=none&taskId=u75899e31-250f-4af4-ae3e-d5085362a57&title=&width=835)
net网络后面要加一对括号

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682388630276-1cc06c82-efa1-4848-abf5-13f20941c495.png#averageHue=%23fefdfc&clientId=ua806d851-71c6-4&from=paste&height=53&id=uadcec1d4&originHeight=106&originWidth=1136&originalType=binary&ratio=2&rotation=0&showTitle=false&size=18794&status=done&style=none&taskId=u3ea9c511-280f-4817-9e3d-c5797c41f5c&title=&width=568)

报错：
RuntimeError: Input type (torch.cuda.FloatTensor) and weight type (torch.FloatTensor) should be the same
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682388676924-13d6e497-f69e-4018-b4fa-fa355f5916c0.png#averageHue=%23f4f3f1&clientId=ua806d851-71c6-4&from=paste&height=215&id=u3bf44d68&originHeight=429&originWidth=1861&originalType=binary&ratio=2&rotation=0&showTitle=false&size=92807&status=done&style=none&taskId=u502e0084-0c02-4438-bfdf-a6ca23c8267&title=&width=930.5)
解决：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682388995964-6c4d664d-12cd-489c-9846-b554e29bc8b5.png#averageHue=%23efeeed&clientId=ua806d851-71c6-4&from=paste&height=398&id=u17cf7013&originHeight=795&originWidth=1565&originalType=binary&ratio=2&rotation=0&showTitle=false&size=217474&status=done&style=none&taskId=uf10c150a-509f-4372-a622-38ff8dd25ae&title=&width=782.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682391215021-4bf2d280-0ec4-493c-a9f6-7ee74e8773ff.png#averageHue=%23f9f8f8&clientId=ua806d851-71c6-4&from=paste&height=43&id=u79084248&originHeight=85&originWidth=1417&originalType=binary&ratio=2&rotation=0&showTitle=false&size=22249&status=done&style=none&taskId=u810eabb0-d6f6-4858-8512-120bd060c67&title=&width=708.5)

报错：
TypeError: sigmoid(): argument 'input' (position 1) must be Tensor, not tuple
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682588858457-c2ecaf5b-ab84-4a0b-8d8e-2256d2af509f.png#averageHue=%230b0b0b&clientId=udb596d71-a09d-4&from=paste&height=179&id=u2bede70d&originHeight=357&originWidth=1422&originalType=binary&ratio=2&rotation=0&showTitle=false&size=20493&status=done&style=none&taskId=u9fc9a3ea-2c9b-4906-8dda-94f325a633d&title=&width=711)
出现这个错误的原因是，self.net(image, d)有三个输出，而我只设置了一个变量接受，正确改为：
_, _, out  = self.net(image, d)  (我的模型返回三个参数)
报错:
Unexpected key(s) in state_dict
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682590548259-acebe2d4-012c-44c6-ba1a-7c1d8152b74e.png#averageHue=%23161616&clientId=udb596d71-a09d-4&from=paste&height=325&id=u4a09427c&originHeight=649&originWidth=1441&originalType=binary&ratio=2&rotation=0&showTitle=false&size=52584&status=done&style=none&taskId=u55d032ef-c1dd-4682-ab42-06760c1bc9a&title=&width=720.5)

解决办法:在load_state_dict的时候加上strict=False。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682590614618-1cbfba79-55a6-4f61-8fe5-97c6a4720078.png#averageHue=%23fcfbfa&clientId=udb596d71-a09d-4&from=paste&height=55&id=u79eb305e&originHeight=110&originWidth=1647&originalType=binary&ratio=2&rotation=0&showTitle=false&size=21401&status=done&style=none&taskId=u8663b62d-6312-40c0-b28d-2381ba63e0b&title=&width=823.5)

报错：TypeError: upsample_bilinear2d(): argument 'output_size' must be tuple of ints, but found element of type Tensor at pos
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682591526541-b066ab9b-2ebe-4586-bd8c-e6f94cd7e932.png#averageHue=%230d0d0d&clientId=u44d8905a-c818-4&from=paste&height=194&id=u1810c6b7&originHeight=387&originWidth=1163&originalType=binary&ratio=2&rotation=0&showTitle=false&size=23250&status=done&style=none&taskId=u88512eb2-2deb-4538-9da4-232b78e2d89&title=&width=581.5)
解决
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682591723119-d6935faa-92b4-44f5-96e6-3f57e3265a6b.png#averageHue=%23eceae8&clientId=u44d8905a-c818-4&from=paste&height=80&id=u734bc645&originHeight=160&originWidth=953&originalType=binary&ratio=2&rotation=0&showTitle=false&size=29273&status=done&style=none&taskId=ub3863883-a6d7-4b5b-9f11-1a705873f1b&title=&width=476.5)
batch_size要设置为1，我本来想设置大一点，但是大于1就会报错。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1682591900850-c45f647b-f2d4-4a33-80d4-0de06fc7a475.png#averageHue=%23f8f7f5&clientId=u44d8905a-c818-4&from=paste&height=19&id=u253c51d7&originHeight=37&originWidth=759&originalType=binary&ratio=2&rotation=0&showTitle=false&size=6378&status=done&style=none&taskId=u836fe03f-aca4-42bb-aaf6-97a6e73d97c&title=&width=379.5)
