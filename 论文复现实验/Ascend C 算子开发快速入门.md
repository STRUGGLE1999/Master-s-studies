
以代码仓库的加法为例
代码路径为：/home/HwHiAiUser/samples/cplusplus/level1_single_api/4_op_dev/6_ascendc_custom_op/kernel_invocation/Add

1. **执行run.sh脚本之前，请将run.sh中ASCEND_HOME_DIR环境变量修改为CANN软件包的安装路径。**例如，$HOME/Ascend/ascend-toolkit/latest。

因为我创建环境时使用的是课程提供的镜像所以CANN软件安装包路径改为：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1697693776717-55aae47b-0f3c-4744-96d7-1916b4a41070.png#averageHue=%23fcf9f8&clientId=ub614e9a2-befa-4&from=paste&height=397&id=u49a7fef7&originHeight=793&originWidth=1819&originalType=binary&ratio=2&rotation=0&showTitle=false&size=145043&status=done&style=none&taskId=u9e7afde5-a663-4196-93ad-679c462b7f8&title=&width=909.5)

报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1697693870893-e1c97f3c-77bc-4b3b-84d2-56a6e6392f52.png#averageHue=%2315171a&clientId=ub614e9a2-befa-4&from=paste&height=252&id=u9ea65374&originHeight=503&originWidth=1871&originalType=binary&ratio=2&rotation=0&showTitle=false&size=82304&status=done&style=none&taskId=u95ddb30c-7f2b-47f6-82b1-18e7d2e963b&title=&width=935.5)
需要更新cmake版本 
此时先切换回根用户：
```
sudo -i
```
然后更新cmake
从官网下载cmake:[https://cmake.org/download/](https://cmake.org/download/)
然后解压、运行
```
tar -xzvf cmake-<version>.tar.gz
cd cmake-<version>
./bootstrap
make
sudo make install

```
请将**<version>**替换为你下载的CMake版本号。

**验证新版本的CMake**：
cmake --version
再次执行命令：


报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1697717274309-39ae1551-7b43-412b-a048-c2a3fa09f97e.png#averageHue=%23131516&clientId=ub614e9a2-befa-4&from=paste&height=493&id=uc5d246a8&originHeight=985&originWidth=2000&originalType=binary&ratio=2&rotation=0&showTitle=false&size=160101&status=done&style=none&taskId=u963c2b0b-293b-4421-9212-0f6d63cad50&title=&width=1000)

重新配置Ascend C环境
```
wget https://ascend-repo.obs.cn-east-2.myhuaweicloud.com/Milan-ASL/Milan-ASL%20V100R001C13SPC702/Ascend-cann-toolkit_7.0.RC1.alpha002_linux-x86_64.run -O Ascend-cann-toolkit_7.0.RC1.alpha002_linux-x86_64.run
chmod +x Ascend-cann-toolkit_7.0.RC1.alpha002_linux-x86_64.run
./Ascend-cann-toolkit_7.0.RC1.alpha002_linux-x86_64.run --install --force
wget https://ascend-repo.obs.cn-east-2.myhuaweicloud.com/Milan-ASL/Milan-ASL%20V100R001C13SPC702/Ascend-cann-communitysdk_7.0.RC1.alpha002_linux-x86_64.tar.gz -O Ascend-cann-communitysdk_7.0.RC1.alpha002_linux-x86_64.tar.gz
tar -xf Ascend-cann-communitysdk_7.0.RC1.alpha002_linux-x86_64.tar.gz -C ~/Ascend/ascend-toolkit/latest
wget https://github.com/Kitware/CMake/releases/download/v3.26.4/cmake-3.26.4-linux-x86_64.tar.gz
tar -xf cmake-3.26.4-linux-x86_64.tar.gz
```

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1697887284395-7ac39d53-fe79-4c91-ab56-83920070cae8.png#averageHue=%2313110f&clientId=u53796b64-b711-4&from=paste&height=548&id=u56b01220&originHeight=1096&originWidth=1871&originalType=binary&ratio=2&rotation=0&showTitle=false&size=201044&status=done&style=none&taskId=u9beaeb2b-c0e5-42c0-b41d-dbf14121e24&title=&width=935.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1697887436268-0b2c1245-3123-4ec1-ab8b-9b2640544f16.png#averageHue=%230b0a09&clientId=u53796b64-b711-4&from=paste&height=526&id=u31b0a850&originHeight=1052&originWidth=1861&originalType=binary&ratio=2&rotation=0&showTitle=false&size=163488&status=done&style=none&taskId=uddb0c630-2832-47b1-bd1f-44431f24387&title=&width=930.5)
设置环境变量：
```
source /home/ma-user/Ascend/ascend-toolkit/set_env.sh
export ASCEND_CUSTOM_PATH=$HOME/Ascend/ascend-toolkit/latest
export ASCEND_HOME_DIR=$HOME/Ascend/ascend-toolkit/latest
export PATH=/home/ma-user/work/cmake-3.26.4-linux-x86_64/bin:$PATH
```
下载git仓库
```
git clone https://gitee.com/ascend/samples.git
```
运行加法算子
```
cd ./samples/cplusplus/level1_single_api/4_op_dev/6_ascendc_custom_op/kernel_invocation/Add
bash run.sh add_custom ascend910 AiCore cpu
```
输出如下结果则表示正确
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1697887810300-eca02ac0-c5b5-4a0f-b58c-afbe455866e6.png#averageHue=%230f0d0c&clientId=u53796b64-b711-4&from=paste&height=534&id=u2eec0f50&originHeight=1067&originWidth=1849&originalType=binary&ratio=2&rotation=0&showTitle=false&size=240858&status=done&style=none&taskId=u001e945f-9187-4ccf-b0ac-b77bf5ce777&title=&width=924.5)
