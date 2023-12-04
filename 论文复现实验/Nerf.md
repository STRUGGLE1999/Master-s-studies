[https://github.com/bmild/nerf](https://github.com/bmild/nerf)


## 跑实验：
在进行到![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657591717963-d871001b-9c50-4197-99df-3e51e4c67b96.png#averageHue=%238fcbed&clientId=u3d390205-0f99-4&from=paste&height=62&id=u23318776&originHeight=62&originWidth=482&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2426&status=done&style=none&taskId=ufc51845c-d39a-457e-b7ae-016bccf94c7&title=&width=482)这一步骤时，发现报以下错误，后面去google一下，有网友说在Tensorflow2.1.0能跑起来，所以去下载了对应的版本。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657591675471-a1c3ec9b-2c8e-4f19-af56-1a47fc470eeb.png#averageHue=%230c0906&clientId=u3d390205-0f99-4&from=paste&height=269&id=u0a0ed71c&originHeight=269&originWidth=940&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34921&status=done&style=none&taskId=u5ac18238-689a-48b7-af6c-2d753884a29&title=&width=940)
安装成功之后，再次运行  python run_nerf.py --config config_fern.txt   报以下错误：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657591873960-837fe473-1893-4f86-8f0e-617197ef0c83.png#averageHue=%230a0806&clientId=u3d390205-0f99-4&from=paste&height=94&id=uf1346741&originHeight=94&originWidth=719&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7980&status=done&style=none&taskId=ub60c7384-1928-4bd6-bdfd-fcb6a3c0347&title=&width=719)
将代码改为以下，删除contribute
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657593346500-8f07c232-0621-48fc-9368-79cf16c127c1.png#averageHue=%23faf9f7&clientId=u3d390205-0f99-4&from=paste&height=35&id=u66ade0de&originHeight=35&originWidth=561&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4290&status=done&style=none&taskId=u835f6d29-95a4-4c94-80a4-0a1c8eda53f&title=&width=561)
接着运行，报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657593832122-5449679a-4be5-43ce-b725-bacfec3e2f04.png#averageHue=%230f0c0a&clientId=u3d390205-0f99-4&from=paste&height=129&id=uf23de06c&originHeight=129&originWidth=637&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11406&status=done&style=none&taskId=uce31a092-c976-45cf-b6bd-313f9f4f010&title=&width=637)
解决：在run_nerf_helpers.py中修改以下代码：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1657593889046-19d8f3bb-dd57-4003-8f75-fdca2c9eb9ea.png#averageHue=%23fbfbfa&clientId=u3d390205-0f99-4&from=paste&height=48&id=u6617a509&originHeight=48&originWidth=623&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5239&status=done&style=none&taskId=u79cf22ed-f687-4da6-b490-019903a3833&title=&width=623)
