Dirt 是TensorFlow 的一个库，它提供了渲染 3D 网格的操作。它支持通过几何、光照和其他参数计算导数。DIRT非常快：它使用OpenGL进行光栅化，在GPU上运行，这允许与CUDA进行轻量级互操作。

## 实验环境要求

- an Nvidia GPU; the earliest drivers we have tested with are v367
- Linux; we have only tested on Ubuntu, but other distributions should work
- a GPU-enabled install of TensorFlow, version 1.6 or later
- python 2.7.9 / 3.5 or newer
- cmake 3.8 or newer
- gcc 4.9 or newer

 
源码链接：[https://github.com/pmh47/dirt](https://github.com/pmh47/dirt)
### 跑实验
1.已经创建了一个python 3.5的环境
```python
conda create -n py3 python  
```
2.Python版本和cmake版本都满足了，但是gcc 我没有权限更新，自带的是4.8版本的，还要TensorFlow也下载不成功，一个个来解决。
3.下载TensorFlow：
```python
conda install tensorflow-gpu==1.6    #在py3环境下安装tensorflow
python
>>> import tensorflow
>>> tensorflow.__version__             #进入Python，测试是否安装成功。
```

因为不能安装新版本的gcc，所以我要在docker下跑程序，下面是docker的版本。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1652494505633-94a0a2a8-8eca-4b18-add2-8cb772564a9e.png#clientId=u3f5879a2-825f-4&from=paste&height=158&id=uc2b9ab2c&originHeight=158&originWidth=564&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12933&status=done&style=none&taskId=u07713609-9216-42c5-8fe6-a63b3bc6c1c&title=&width=564)
服务系系统：CentOS 7

但是我目前对docker 不是很熟悉，不会配置docker环境。

第一次运行：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1652506285438-d25357c5-ba95-4f0e-82b7-ab31c7a58a43.png#clientId=ub87928fa-3142-4&from=paste&height=598&id=uae857c36&originHeight=598&originWidth=873&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67062&status=done&style=none&taskId=ua8deb4f7-d7dc-498a-8529-efab2f00025&title=&width=873)
这个错误太多了，我看不懂具体什么意思，后来我去百度了一下 ![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1652506465196-b83ea0c3-e509-4a3f-a9a6-4cb9286cf024.png#clientId=ub87928fa-3142-4&from=paste&height=21&id=u8f58d36c&originHeight=21&originWidth=355&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1884&status=done&style=none&taskId=u79742454-66e9-4261-b72f-3670c7f1856&title=&width=355)这句错误，然后网上说是因为python版本和[第三方库](https://so.csdn.net/so/search?q=%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%93&spm=1001.2101.3001.7020)要求的python版本不一致，所以我使用 “conda install python==3.5”来安装python 。
第二次运行：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1652506578897-7fd5cb97-030c-467e-9fc0-749b587739e1.png#clientId=ub87928fa-3142-4&from=paste&height=171&id=u6b6030a4&originHeight=171&originWidth=706&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19488&status=done&style=none&taskId=u46bd2cf7-133a-41cf-aaba-6c988d42977&title=&width=706)
显示找不到这个包，后面去网上搜显示因为pip版本太高了，所以把pip版本降低。
第三次运行：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1652507828682-13826bf0-f4c5-46cf-87e2-b964e9762c7e.png#clientId=ub87928fa-3142-4&from=paste&height=558&id=ub7704a7e&originHeight=558&originWidth=876&originalType=binary&ratio=1&rotation=0&showTitle=false&size=62084&status=done&style=none&taskId=u9db6b5fd-2b68-4701-aa86-3bdb7fb33d5&title=&width=876)
### 关于在docker 里运行
因为在服务器没有权限，所以我换成在docker里试一下能不能安装dirt库。
第一、首先在docker里拉取一个tensorflow-gpu=1.13.1版本的镜像：
```python
docker pull tensorflow/tensorflow:1.13.1-gpu-py3

```
第二、以这个镜像启动一个docker容器：
```python
docker run -it tensorflow/tensorflow:1.13.1-gpu-py3 /bin/bash
```
第三、安装：
```python
apt update  #更新
apt install cmake   #安装cmake ，最终cmake 版本为3.5.1
apt install git       #安装git,最终git 版本为
apt install python3.5    #安装Python
 

```
第四、安装dirt:
我开始运行以下代码：
```python
cd dirt
pip install .
```
但是报错了，如下：
### ![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653805600431-ddc799c9-a36d-4e26-8e82-631ffd538ed2.png#clientId=u04163b99-d544-4&from=paste&height=130&id=ud0c2b0b2&originHeight=130&originWidth=928&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11961&status=done&style=none&taskId=u7f3835be-182f-4036-9ae6-021b091662b&title=&width=928)
这里的意思应该是 cmake 版本太低了。
安装了高版本的cmake 之后，再次运行安装：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653810818130-2e36301f-cf4d-4680-9c8e-71d34e60c27f.png#clientId=u04163b99-d544-4&from=paste&height=35&id=u8fa75491&originHeight=35&originWidth=839&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2515&status=done&style=none&taskId=udd2e65ec-4210-41cb-98a8-82ad0c400b4&title=&width=839)
找不到opengl,

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653812849793-56746dea-638c-4860-84af-c2648b31b743.png#clientId=u3291260e-4928-4&from=paste&height=1656&id=u57053622&originHeight=1656&originWidth=930&originalType=binary&ratio=1&rotation=0&showTitle=false&size=134338&status=done&style=none&taskId=u6f71b66a-3119-465a-94a7-67327a4b181&title=&width=930)
### 

### 对于apt update更新不成功的问题：
报错信息：
E: Could not get lock /var/lib/apt/lists/lock - open (11: Resource temporarily unavailable)
E: Unable to lock directory /var/lib/apt/lists/
后面上网查资料显示由于前一个进程还没完成就非正常结束了，导致那个进程还保留在那里。所以使用
```python
ps -e | grep apt    #查看进程
```
显示：
1945 pts/0    00:00:00 apt-get
说明进程号为1945的还在运行，所以使用下列语句结束：
```python
kill -9 1945

```
后面再进行apt 更新就可以了。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653792593110-264639ef-0ad0-4233-9fcd-e8509d3cfefe.png#clientId=u04163b99-d544-4&from=paste&height=271&id=u02e813bf&originHeight=271&originWidth=712&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26899&status=done&style=none&taskId=ub9c21772-90b6-4a13-96f7-d20d9714d1b&title=&width=712)

关于git 克隆库失败的问题
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653792972613-6af0f504-c32e-4e6b-8aab-1ad9b0b39b5f.png#clientId=u04163b99-d544-4&from=paste&height=91&id=u45ce0166&originHeight=91&originWidth=907&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10342&status=done&style=none&taskId=u62617bc0-735a-4f1f-ab7b-771c48887ec&title=&width=907)
网上查博客说 将git clone https://[github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020).com/XXXX/XXXX.git改为git clone git://github.com/XXXX/XXXX.git
我试了这种办法还是不可以，报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653793024674-dba10fbe-891b-4ad1-bd49-eb2deff611a4.png#clientId=u04163b99-d544-4&from=paste&height=87&id=u3c82ef38&originHeight=87&originWidth=533&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5273&status=done&style=none&taskId=uf98313d6-0fdb-47b4-9c2d-761fd5f6f7c&title=&width=533)
还有博主说是代理设置问题，之前我可以运行这一句的。
之后我的解决办法是把dirt 文件夹从本地上传到docker中：
```python
docker cp 本地文件路径 ID全称:容器路径
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653805471872-14cab89f-a56e-408f-a8fe-36778ab4cfea.png#clientId=u04163b99-d544-4&from=paste&height=44&id=u7356e479&originHeight=44&originWidth=567&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4474&status=done&style=none&taskId=u2ff56211-0d92-4222-b001-bfce987e446&title=&width=567)
### 关于docker:
以一个镜像启动容器：
docker run -it tensorflow/tensorflow:1.13.1-gpu-py3 /bin/bash

### 文件夹操作
```python
文件夹操作：
cd ..（返回上一级目录）
cd（返回根目录）
cd /...（从跟目录查找文件夹）
cd ...（从当前目录查找文件夹）
mkdir ...（创建文件夹）
cp -ri A/* A1（复制A文件夹中所有的文件至A1）
vim 文件名（编辑文件，:q!退出；:wp!保存并退出；i进入编辑模式）
ls（查看文件夹下所有文件夹）
ll（查看文件夹所有文件）
```
### 安装cmake
我先下载了cmake3.22.1的压缩包到容器里，然后执行在解压后的文件夹下执行：
```python
./bootstrap
```
执行完这一句之后报错，如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653807603000-ff971644-e382-4999-acf9-3a3a20680d96.png#clientId=u04163b99-d544-4&from=paste&height=177&id=ua5934cc6&originHeight=177&originWidth=599&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11999&status=done&style=none&taskId=u7ec568ee-ac84-49c8-a599-fe42b1c0b1f&title=&width=599)
解决办法：安装libssl-dev
apt-get install libssl-dev
然后就可以了。
在接着执行以下语句：
```python
make
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653807763952-509d8c08-37f4-453f-89f4-87f001fdbfff.png#clientId=u04163b99-d544-4&from=paste&height=154&id=u18a511b8&originHeight=154&originWidth=573&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19001&status=done&style=none&taskId=ua18cb4b7-f4e0-49e7-a46f-4f3d97750bc&title=&width=573)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653809731034-9d220437-0257-4967-82ad-94732e2c3976.png#clientId=u04163b99-d544-4&from=paste&height=68&id=u57095611&originHeight=68&originWidth=718&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7590&status=done&style=none&taskId=u45debaf3-dd5a-4e0b-9ba6-908ffffe939&title=&width=718)
执行以下：
```python
make install
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653809753607-01d0373f-fc90-4d9c-93df-3f3fa7543967.png#clientId=u04163b99-d544-4&from=paste&height=145&id=uf6999646&originHeight=145&originWidth=603&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14464&status=done&style=none&taskId=uba3501a6-e094-4275-a477-ebe1c1657cd&title=&width=603)
检查cmake的版本：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653809804854-e24c0db1-ffbe-49c9-9621-724babd1fc7c.png#clientId=u04163b99-d544-4&from=paste&height=51&id=u8c753718&originHeight=51&originWidth=453&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2835&status=done&style=none&taskId=uc8446ac1-5cae-4321-9165-3a352599e36&title=&width=453)
### 找不到opengl
运行以下命令：
```python
apt-get install libgl1-mesa-dev

```
最后还是报错，这个实验没有跑成功，我打算先跑其他的。
我在docker 里面为这个实验配置的容器编号是89616ca7d4d9
