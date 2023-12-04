源代码解读：[https://blog.csdn.net/nbxuwentao/article/details/100409438](https://blog.csdn.net/nbxuwentao/article/details/100409438)

文章：[Kanazawa_End-to-End_Recovery_of_CVPR_2018_paper.pdf](https://www.yuque.com/attachments/yuque/0/2022/pdf/22838017/1654500883862-8ae45da4-fd3f-4fbb-af62-64004b4f54d3.pdf?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2022%2Fpdf%2F22838017%2F1654500883862-8ae45da4-fd3f-4fbb-af62-64004b4f54d3.pdf%22%2C%22name%22%3A%22Kanazawa_End-to-End_Recovery_of_CVPR_2018_paper.pdf%22%2C%22size%22%3A2702108%2C%22type%22%3A%22application%2Fpdf%22%2C%22ext%22%3A%22pdf%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22mode%22%3A%22title%22%2C%22download%22%3Atrue%2C%22taskId%22%3A%22u6ebbff9c-3017-4dcd-b4e1-76f92248f33%22%2C%22taskType%22%3A%22upload%22%2C%22__spacing%22%3A%22both%22%2C%22id%22%3A%22u0de616da%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22file%22%7D)
### 论文阅读笔记：
[https://blog.csdn.net/qq_36113487/article/details/83089774](https://blog.csdn.net/qq_36113487/article/details/83089774)
[https://www.codeleading.com/article/29935471864/](https://www.codeleading.com/article/29935471864/)
[http://www.4k8k.xyz/article/qq_42167293/82724102](http://www.4k8k.xyz/article/qq_42167293/82724102)
[https://zhuanlan.zhihu.com/p/412698390](https://zhuanlan.zhihu.com/p/412698390)
### 实验复现笔记：
[https://blog.51cto.com/u_15290914/3276011](https://blog.51cto.com/u_15290914/3276011)
[https://www.mianshigee.com/project/MandyMo-pytorch_HMR](https://www.mianshigee.com/project/MandyMo-pytorch_HMR)
[https://www.cnblogs.com/X-Jun/p/14049168.html](https://www.cnblogs.com/X-Jun/p/14049168.html)
[https://blog.csdn.net/qq_31688497/article/details/104024194](https://blog.csdn.net/qq_31688497/article/details/104024194)
# 跑实验

1. 使用anaconda 建立虚拟环境
```python
conda create -n hmr python=2.7


```
激活环境：conda activate hmr
安装tensorflow-gpu:pip install tensorflow-gpu==1.3.0
安装依赖：pip install -r requirements.txt
报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1656936532877-a8d1abb8-3b4f-4922-b5ce-1316dc5e8977.png#averageHue=%23080302&clientId=u1e09a020-9ae7-4&from=paste&height=127&id=u8a2c0dc5&originHeight=127&originWidth=784&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10694&status=done&style=none&taskId=u638c0f82-4a67-4c5e-a6c5-bbcc9f23e4e&title=&width=784)
安装numpy:pip install numpy==1.14.0
安装opencv:pip install opencv-python==4.2.0.32


mmpose:
[https://mmpose.readthedocs.io/zh_CN/latest/install.html](https://mmpose.readthedocs.io/zh_CN/latest/install.html)



# hmr2.0实验
链接：[https://github.com/russoale/hmr2.0](https://github.com/russoale/hmr2.0)


hmr 实验报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657444685176-cf70a5d2-c716-4619-bc61-74407b72ea4d.png#averageHue=%23130e09&clientId=ue7e4f656-2526-4&from=paste&height=25&id=ua51fb5de&originHeight=25&originWidth=508&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2733&status=done&style=none&taskId=u443a3b9f-2e0c-45ca-9b1f-352e74d7fca&title=&width=508)
解决办法：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657450129934-2421f6ee-ad5b-4591-9022-65c899405233.png#averageHue=%23afc793&clientId=ue7e4f656-2526-4&from=paste&height=246&id=uf0c96896&originHeight=246&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11952&status=done&style=none&taskId=u07504a87-29c2-4684-b00d-7b2a70f2a89&title=&width=727)
报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657450253144-510bc3d4-f4d8-4cb2-bd1a-7d4d43322321.png#averageHue=%2315100c&clientId=ue7e4f656-2526-4&from=paste&height=34&id=uf328d635&originHeight=34&originWidth=608&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4962&status=done&style=none&taskId=u8befaaeb-d6a5-4c01-9aeb-bd9fb03b513&title=&width=608)
解决方案：
在"("前加上“\”
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657450291759-43c2fda4-2d06-4446-8d2e-a249734eee28.png#averageHue=%2315100b&clientId=ue7e4f656-2526-4&from=paste&height=22&id=u0be167e9&originHeight=22&originWidth=637&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3117&status=done&style=none&taskId=uad781b3b-9f94-49b2-b734-8a9ce7b14cf&title=&width=637)
报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657454927896-5aa3d489-dfaa-4b93-94a9-bbba37b82082.png#averageHue=%230c0806&clientId=ue7e4f656-2526-4&from=paste&height=368&id=ua26209ce&originHeight=368&originWidth=940&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46530&status=done&style=none&taskId=u0d1a758e-ad6d-4db8-86b5-d10ba7bc58c&title=&width=940)
未解决
# 调试代码
在VS Code中调试代码报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657543246888-52242866-bba7-46c1-9870-9e400ac64b24.png#averageHue=%23f9f7f4&clientId=uf4b05c85-4100-4&from=paste&height=64&id=ue611a52a&originHeight=64&originWidth=681&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6046&status=done&style=none&taskId=udac5ad5c-e5a1-49e0-b842-e1d253ed677&title=&width=681)
这是由于这段代码引起的：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657543274769-db08d21b-fd61-4a28-bd95-a6dbebe20a22.png#averageHue=%23fefdfc&clientId=uf4b05c85-4100-4&from=paste&height=102&id=u00b881f6&originHeight=102&originWidth=600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9088&status=done&style=none&taskId=u2bffd620-88a6-410f-bc52-641df64f7ff&title=&width=600)
因为我要在visualise文件夹下的demo.py文件中导入main文件夹下config.py中的模块。我的文件结构如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657543380545-ec134ac8-7eb1-4e6e-95c2-555308acb4f7.png#averageHue=%23f1f0ef&clientId=uf4b05c85-4100-4&from=paste&height=574&id=u2cc3a07a&originHeight=574&originWidth=251&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17074&status=done&style=none&taskId=uc377c505-16cd-4b6c-a4c5-c0080db1b35&title=&width=251)
所以是要在不同文件夹中导入模块，需要在demo.py文件中加上如下语句：
```python
import os,inspect
current_dir=os.path.dirname(os.path.abspath(inspect.getfile(inspect.currentframe())))
os.chdir(current_dir)
import sys
sys.path.append('../')
```
此后就可以正常编译。
然后还会报错：
![HWW29}O}5%VUA`XHDNQM2_B.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657610785429-eb50c662-511b-45fd-9ae5-bd1c20dacf4c.png#averageHue=%230a0705&clientId=udeac03aa-2a0e-4&from=paste&height=485&id=u40b7cc2d&originHeight=485&originWidth=936&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17943&status=done&style=none&taskId=u4fddc23c-cfe5-42c3-b6ac-db15f7eec0b&title=&width=936)
参考网上：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657610814150-3a6e061b-1f98-46e3-9e05-25a2cd443711.png#averageHue=%23e2be8a&clientId=udeac03aa-2a0e-4&from=paste&height=163&id=u8ad26654&originHeight=163&originWidth=659&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13584&status=done&style=none&taskId=udbb078dc-8d51-4399-98e1-9b1ad2f98ec&title=&width=659)
于是我把tensorflow版本升级到2.4.0，然后就不会报那个错误了。

在 /home/XiaoLF/hmr2.0-master/src/visualise 中运行以下代码：
```python
python demo.py --image=coco1.png --model=base_model --setting=paired\(joints\) --joint_type=cocoplus --init_toes=false
```
报错：![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657610700305-7cb23202-2ef2-49de-b136-17228b110d35.png#averageHue=%230a0705&clientId=udeac03aa-2a0e-4&from=paste&height=100&id=udd48097d&originHeight=100&originWidth=785&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11166&status=done&style=none&taskId=u25d2fa91-8b39-42b2-9ef4-4f8342f82ba&title=&width=785)
解决：在model.py文件中修改LOG_DIR的位置。如下代码：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657625644400-4e857b38-d6ba-4166-97d6-b76e950b2a1c.png#averageHue=%23fdfcfa&clientId=udeac03aa-2a0e-4&from=paste&height=44&id=ud68bf2f8&originHeight=44&originWidth=812&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6102&status=done&style=none&taskId=u254fbdba-9968-42a0-bad5-e82edf73cba&title=&width=812)
这个时候要记得把下载的预训练模型放到logs文件夹中，如图所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657625696502-be16e9ba-8c6b-4b94-bf24-28eae4fda4d1.png#averageHue=%23edeceb&clientId=udeac03aa-2a0e-4&from=paste&height=217&id=u47c8217a&originHeight=217&originWidth=249&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7838&status=done&style=none&taskId=ud1a25bd4-3e33-47b7-855c-decb87a2565&title=&width=249)
更新：
对于上面的错误，很玄乎，一下报错一下不报错。记住下面的代码以及文件位置：
config.py中
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657693079193-cf0c32ee-d0ba-497f-9daa-961325aa8f1a.png#averageHue=%23fdfbf8&clientId=u031c52f0-d8d7-4&from=paste&height=44&id=ub7c32aec&originHeight=44&originWidth=641&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6311&status=done&style=none&taskId=u4e7ba7e0-01a2-4096-92e9-9e4a4b8e502&title=&width=641)
model.py中：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657693144487-bb7c6c67-f641-4e27-a459-cdf98fce357d.png#averageHue=%23ebf4fa&clientId=u031c52f0-d8d7-4&from=paste&height=44&id=u9227e93d&originHeight=44&originWidth=862&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6326&status=done&style=none&taskId=u8e587b8b-4ba9-4591-8947-8e85d6a174a&title=&width=862)
文件夹位置：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657693210259-479f81ae-d76a-4911-8e90-dcda945cf359.png#averageHue=%23f0efed&clientId=u031c52f0-d8d7-4&from=paste&height=480&id=u306674bb&originHeight=480&originWidth=224&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17647&status=done&style=none&taskId=u90ac17f2-ea0c-4949-9313-9a64563d317&title=&width=224)


base_model就是下载的预训练模型。然后就不会报上面的错了。紧接着有新的错误：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657693227132-b56dbc6d-dbd6-4364-91bb-8971e4796e3b.png#averageHue=%23f8f5f2&clientId=u031c52f0-d8d7-4&from=paste&height=356&id=u35c77331&originHeight=356&originWidth=913&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46205&status=done&style=none&taskId=u5fbf750e-be83-4786-aa6a-78435e59f3a&title=&width=913)

一直找不到这个GLU库，找了网上很多解决办法也没有用，所以我把代码放到云服务器去跑，记得图片的路径要改一下，如下图：
demo.py

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657959352107-69406e1c-640f-4981-9ccc-1b29a2c771f1.png#averageHue=%23fbf9f8&clientId=u47e073ad-4cf8-4&from=paste&height=82&id=u56e7a63b&originHeight=82&originWidth=1382&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10004&status=done&style=none&taskId=u6c776339-7564-4e71-ab7a-c98bd6c2120&title=&width=1382)
运行demo.py之后报如下错误：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657959332044-b2fbbfee-9398-492b-a6cc-b56b959e1e73.png#averageHue=%23f8f5f2&clientId=u47e073ad-4cf8-4&from=paste&height=241&id=u92cc54e8&originHeight=241&originWidth=817&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28286&status=done&style=none&taskId=u840b3f57-e377-4667-b512-730cfd4e08b&title=&width=817)
网上说是因为服务器没有连接到渲染库导致不能进行渲染，从而不能显示出来。
根据一篇博客，看到下面的解决方案：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658044809373-a5fb1cef-2ece-42d1-a882-323266436003.png#averageHue=%23fcfbfa&clientId=udd9f2c5f-e440-4&from=paste&height=162&id=u2133dc16&originHeight=162&originWidth=748&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11263&status=done&style=none&taskId=u39a40952-7232-4d33-8153-747b53421cf&title=&width=748)
然后我先去下载了xvfb库，接着再运行代码：
```python
xvfb-run -s \"-screen 0 1400x900x24\" python demo.py --image=coco1.png --model=base_model --setting=paired\(joints\) --joint_type=cocoplus --init_toes=false
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658044912524-0bc3b9e5-6a98-4315-92ee-d37de4fbfdff.png#averageHue=%23eeeceb&clientId=udd9f2c5f-e440-4&from=paste&height=55&id=uef95a869&originHeight=55&originWidth=1424&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11929&status=done&style=none&taskId=u0e72f74b-017b-498a-95d9-eb4854ab008&title=&width=1424)
再去百度，好像是因为已经打开过xvfb了，所以再次运行就失败了，再次运行以下：
```python
xvfb-run -a python demo.py --image=coco1.png --model=base_model --setting=paired\(joints\) --joint_type=cocoplus --init_toes=false
```
这个时候就陷入了漫长的等待中，也不知道有没有在运行。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658045058899-f95dc8d1-c0e8-4cdd-9c30-d800db2bc251.png#averageHue=%23fdfcfc&clientId=udd9f2c5f-e440-4&from=paste&height=386&id=u99d1398e&originHeight=386&originWidth=1466&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26257&status=done&style=none&taskId=uae6ff8c4-fb1d-4af6-828f-3cf67262a70&title=&width=1466)
原来是因为服务器没有图形显示界面，需要借助Firefox和xvfb，分别下载：
```python
apt install xvfb
apt install firefox
```
# 更新:
总结在demo.py中最后一步出结果：
之前一直报错说连接不上显示设备，然后我猜是不是因为服务器没有渲染设备，因为跑之前的代码都会附带一个渲染设备，所以我猜想把blender软件复制到hmr2.0工程中，最后可以出结果，具体步骤如下：
1、把blender安装包放到HMR-2.0-MASTER文件夹下，然后解压缩。
切换到blender文件夹下，执行以下命令：
```python
export LD_LIBRARY_PATH=hmr2.0-master/blender-2.79/lib:$LD_LIBRARY_PATH
./blender
```
此时会报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658387675909-ba3d548a-af47-4673-96e8-8bfd5e075c88.png#averageHue=%23e6e6e6&clientId=u052b1c73-b460-4&from=paste&height=61&id=u7b4598db&originHeight=61&originWidth=1206&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4137&status=done&style=none&taskId=uc2006a81-1d63-41c5-9851-90ad1a5fcc6&title=&width=1206)
这个时候不用管，好像是在这里的终端运行blender，不能开启那个软件，但是我试过在Xshell中可以。
继续执行以下命令：
```python
xvfb-run -a python demo.py --image=coco1.png --model=base_model --setting=paired\(joints\) --joint_type=cocoplus --init_toes=false

```
就可以看到和作者跑出的一样的代码：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658387830413-0f02b2ef-61ed-478c-85c2-3fb18885f0dd.png#averageHue=%23e3e3e1&clientId=u052b1c73-b460-4&from=paste&height=435&id=u46972587&originHeight=435&originWidth=685&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79651&status=done&style=none&taskId=u7189390e-4248-46e1-a5a0-21cd1338d60&title=&width=685)
这里要强调xvfb这个库很关键。但是demo跑出来的结果和作者跑出来的不一样，看图：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658393130719-be4a11c2-7b21-492b-8239-202ee5e92442.png#averageHue=%23f0f0ee&clientId=u052b1c73-b460-4&from=paste&height=388&id=u6dbab39a&originHeight=388&originWidth=693&originalType=binary&ratio=1&rotation=0&showTitle=false&size=124250&status=done&style=none&taskId=ua15336b4-e8d7-44e3-a37d-0a4d06ded6b&title=&width=693)
2022.7.26更新：
之所以跑出来的结果和作者不一样，是因为在trimesh_render.py代码中需要修改代码;修改成如下：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658837207987-74b38b3a-b263-4387-aa36-209ed04c25a1.png#averageHue=%23fbfaf9&clientId=u38ec54cf-813f-4&from=paste&height=313&id=u9a19e5c8&originHeight=313&originWidth=668&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21000&status=done&style=none&taskId=u7df609b7-05ac-47f9-9ef3-5051abd0f5f&title=&width=668)
最后再次运行可以得到正确结果：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658837255250-f8b82cc3-382c-4aa5-a7bd-b813e620019e.png#averageHue=%23f2f1f0&clientId=u38ec54cf-813f-4&from=paste&height=368&id=ua48f2da2&originHeight=368&originWidth=684&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82213&status=done&style=none&taskId=u263697c8-666b-4279-b7c5-59a1e448d94&title=&width=684)


Xmanager

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1658472972577-489f0d98-f3f7-4534-b190-3cc681197692.png#averageHue=%23f1f4dc&clientId=u4946974d-add1-4&from=paste&height=67&id=u2e55e19f&originHeight=67&originWidth=871&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7302&status=done&style=none&taskId=u8212d340-ee8f-4560-9800-62aecc7cc00&title=&width=871)


# 更新：在AutoDL服务器平台上跑代码，又遇到了下列错误：
![](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657693227132-b56dbc6d-dbd6-4364-91bb-8971e4796e3b.png#averageHue=%23f8f5f2&from=url&id=NjroQ&originHeight=356&originWidth=913&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
但是这一次，按照下面博客的步骤可以解决！
[https://blog.csdn.net/m0_38068876/article/details/125880948](https://blog.csdn.net/m0_38068876/article/details/125880948)
