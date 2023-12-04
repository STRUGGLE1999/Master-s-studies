论文：
[Mir_Learning_to_Transfer_CVPR_2020_supplemental.pdf](https://www.yuque.com/attachments/yuque/0/2022/pdf/22838017/1654500334502-d47e3002-55a7-4928-9e52-928626a7790e.pdf?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2022%2Fpdf%2F22838017%2F1654500334502-d47e3002-55a7-4928-9e52-928626a7790e.pdf%22%2C%22name%22%3A%22Mir_Learning_to_Transfer_CVPR_2020_supplemental.pdf%22%2C%22size%22%3A7164194%2C%22type%22%3A%22application%2Fpdf%22%2C%22ext%22%3A%22pdf%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22mode%22%3A%22title%22%2C%22download%22%3Atrue%2C%22taskId%22%3A%22u39fd1ac4-cb1a-4ed8-8006-c9fb4810079%22%2C%22taskType%22%3A%22upload%22%2C%22__spacing%22%3A%22both%22%2C%22id%22%3A%22ud083d7a4%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22file%22%7D)
源代码链接：[https://github.com/aymenmir1/pix2surf](https://github.com/aymenmir1/pix2surf)

按照教程：

1. 先创建一个虚拟环境:
```python
conda creative -n py_pix2surf python
```

2. 进入虚拟环境中：
```python
conda activate py_pix2surf
```

3. Clone the repo:git clone https://github.com/aymenmir1/pix2surf

运行着一步显示不能连接到github，所以我把pix2surf这个文件下载下来上传到Xftp上。还有把 blender2.79也上传上去，放到一个文件夹里。
4.进入对应的文件夹路径下：
```python
cd /home/XiaoLF/Pix2Surf/pix2surf-master/pix2surf-master
```
5.安装实验所需的包  Install the requirements using conda:
```python
source scripts/install_conda.sh
```
```python
source scripts/prepare_data.sh
```
#### 报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654302068621-714535ca-a1d4-4f9b-a891-da40a43114b2.png#averageHue=%23130e0a&clientId=ucf917e8c-a332-4&from=paste&height=76&id=u4cf92a05&originHeight=76&originWidth=634&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11147&status=done&style=none&taskId=ua672ad73-5be0-4280-a0f4-92e324fd623&title=&width=634)
是因为下载预训练权重文件夹一直报错，后面我尝试自己去网上下载这个文件夹。找到相应的参考博客：[https://github.com/pbelevich/virtual-try-on/blob/main/pix2surf_demo.ipynb](https://github.com/pbelevich/virtual-try-on/blob/main/pix2surf_demo.ipynb)
里面提及了一个网址：[https://drive.google.com/uc?id=1ULtdEXRrxH9_CtTrWensIbwybeWKz8Dj](https://drive.google.com/uc?id=1ULtdEXRrxH9_CtTrWensIbwybeWKz8Dj)
但是这个网址打不开，所以推断是因为网址的问题导致实验失败：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654310253704-33488b2d-01b6-4f33-8a04-4f74a435f3e1.png#averageHue=%23fefefe&clientId=ucf917e8c-a332-4&from=paste&height=516&id=ua81cf027&originHeight=516&originWidth=1446&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39765&status=done&style=none&taskId=u7b92d06c-3186-4179-9aae-312b0013290&title=&width=1446)

#### 更新：
后面我去github网站找到了prepare_data.sh文件，发现里面有下载地址：[https://nextcloud.mpi-klsb.mpg.de/index.php/s/ozRcGdGwAJ3tBns/download](https://nextcloud.mpi-klsb.mpg.de/index.php/s/ozRcGdGwAJ3tBns/download)
所以我把这个文件夹下载下来，按照他的.sh代码：
```python
wget -O pix2surf.zip https://nextcloud.mpi-klsb.mpg.de/index.php/s/ozRcGdGwAJ3tBns/download
unzip pix2surf.zip
rm pix2surf.zip
mv data ./train/
```
运行：source scripts/prepare_data.sh  竟然可以了，很奇怪。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654414509733-9f33c36d-95cf-4727-9a46-b6e1fadd544f.png#averageHue=%23fbfaf9&clientId=u974bd4f9-8132-4&from=paste&height=99&id=u54101da8&originHeight=99&originWidth=738&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13471&status=done&style=none&taskId=ucf17e92d-3e68-4470-9715-905c409d547&title=&width=738)

运行 python demo.py时报以下错误：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654417009702-4d7f09b3-67f4-44f3-8020-2cb755aa6d1e.png#averageHue=%230a0705&clientId=u974bd4f9-8132-4&from=paste&height=347&id=u495edda3&originHeight=347&originWidth=942&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44001&status=done&style=none&taskId=u31e7473f-0205-4129-9156-b54fad00089&title=&width=942)
百度了一下解决方案为：在加载模型的语句上，增加**map_location='cuda:0’**
```python
 loaded_state = torch.load(model_path+seq_to_seq_test_model_fname,map_location='cuda:0')

```
但是我依旧报错，后面我把demo.py文件的torch.load加载语句改成：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654417495740-16ac0c1f-e18d-4ac4-99ec-5e3dba031cc7.png#averageHue=%23f1ecea&clientId=u974bd4f9-8132-4&from=paste&height=194&id=u50bc636d&originHeight=194&originWidth=751&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25874&status=done&style=none&taskId=u8374c955-e5e5-4a6e-ad80-538015eafc7&title=&width=751)
就报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654417564287-6e2cb7a8-3d52-4e34-9ddc-44298e680241.png#averageHue=%23c9c3c1&clientId=u974bd4f9-8132-4&from=paste&height=258&id=u01683b4f&originHeight=258&originWidth=973&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38315&status=done&style=none&taskId=u8e7b94ee-38d7-45f8-8a12-d4d9a4a4098&title=&width=973)
显示cuDNN错误，后面参考同门的解决方案：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654417758360-7245aebe-1c13-4c2d-b5c9-6d71803690a1.png#averageHue=%23d6ebfb&clientId=u974bd4f9-8132-4&from=paste&height=226&id=u94f054c4&originHeight=226&originWidth=1305&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38432&status=done&style=none&taskId=u2a220a2c-dc64-4fc5-9a30-b3b4c7f7e73&title=&width=1305)
改了demo.py文件中的 def get_gpus(self)方法：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654418216996-e4234933-776c-485c-ac55-fe66f2392b09.png#averageHue=%23fcf5f4&clientId=u974bd4f9-8132-4&from=paste&height=365&id=ud3c0e8da&originHeight=365&originWidth=540&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35961&status=done&style=none&taskId=uda45766b-feee-4124-b26d-3c4d241c8c2&title=&width=540)

运行之后，再次报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654418332578-dce56ddc-8e54-4a8b-8bae-ffc3507148e3.png#averageHue=%23f8f6f4&clientId=u974bd4f9-8132-4&from=paste&height=121&id=udc0b9678&originHeight=121&originWidth=338&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3517&status=done&style=none&taskId=u5c462690-3e7a-4f28-a34d-a50a031ab62&title=&width=338)
这个错是因为没有运行blender，从[https://download.blender.org/release/Blender2.79/](https://download.blender.org/release/Blender2.79/) 上下载linux64位的blender压缩包。通过Xftp上传到根目录下：
执行以下代码：
```python
tar xvf blender-2.79-linux-glibc219-x86_64.tar.bz2    #解压缩包
mv blender-2.79-linux-glibc219-x86_64 blender279     #更改文件名
mv blender279 /home/XiaoLF/Pix2Surf/pix2surf-master/pix2surf-master
# 移动文件位置
 
```
然后执行以下代码，运行blender
```python
./blender

```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654422586170-ef50a8f6-8962-480c-9ae0-7b77e54c6aa1.png#averageHue=%23100c09&clientId=u974bd4f9-8132-4&from=paste&height=39&id=ub15fbcb1&originHeight=39&originWidth=860&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5719&status=done&style=none&taskId=u72decaa1-2699-4ffd-a7fc-967c0cf4590&title=&width=860)
提示找不到共享库：
参考链接：[https://blog.csdn.net/sahusoft/article/details/7388617](https://blog.csdn.net/sahusoft/article/details/7388617)
使用里面的方法（3）,在命令行输入：export LD_LIBRARY_PATH=/usr/local/mysql/lib:$LD_LIBRARY_PATH
这里的路径要换成libGL.so.1的文件路径，在blender文件夹下的bin文件夹下。（注意：这个只能在没有权限或者临时需要的时候使用，也就是说下一次跑这个程序又要重新输入一遍）：
```python
export LD_LIBRARY_PATH=/home/XiaoLF/Pix2Surf/pix2surf-master/pix2surf-master/blender279/lib:$LD_LIBRARY_PATH
```
这样在blender目录下运行./blender就可以看到blender程序跑起来了。但是要在demo.py代码的run(self)方法中改变blender的位置，如图：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654423801253-7685c4b2-702d-41a4-9cf3-887414a4e19d.png#averageHue=%23fcf6f5&clientId=u974bd4f9-8132-4&from=paste&height=144&id=ufc1837ab&originHeight=144&originWidth=656&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15208&status=done&style=none&taskId=u8def2453-d695-4b29-b118-b147e5b4b34&title=&width=656)
最后运行demo程序后，就可以一直到最后一步了，但是跑完之后还报了一个错误：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1654423869518-8897e8e6-f48b-4324-a7e2-c64af5fe02fa.png#averageHue=%23f8f6f4&clientId=u974bd4f9-8132-4&from=paste&height=35&id=ucf2bc499&originHeight=35&originWidth=583&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3886&status=done&style=none&taskId=u1055f72e-1eac-44ea-afe7-fcd1b39c2e7&title=&width=583)
但是最后可以生成一个mp4文件：

