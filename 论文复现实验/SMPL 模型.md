博客教程：[https://blog.csdn.net/weixin_42145554/article/details/111381447?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-3-111381447.pc_agg_new_rank&utm_term=python3%E5%AE%9E%E7%8E%B0smpl&spm=1000.2123.3001.4430](https://blog.csdn.net/weixin_42145554/article/details/111381447?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-3-111381447.pc_agg_new_rank&utm_term=python3%E5%AE%9E%E7%8E%B0smpl&spm=1000.2123.3001.4430)
论文：


环境配置
```python
conda create -n py2 python   #为程序配置python2.7的环境
```
激活环境
conda activate py2
不激活环境
conda deactivate

XShell 修改编辑文件并保存：
[https://blog.csdn.net/weixin_40873693/article/details/121018630](https://blog.csdn.net/weixin_40873693/article/details/121018630)

### 跑实验：
第一次跑：没有成功，因为没有按照博客要求，第一次我在base 环境下，没有成功创建好python 2.7的环境。
第二次，我在base 环境下另外创建了一个环境，以后运行实验记得另外创建环境，不要在base 环境下。
第三次运行，python2.7配置opendr成功，但是运行SMPL报错了，原因是SMPL自定义的包smpl_webuser导入错误
第四次运行:运行 hello_smpl.py 时报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1652421857533-2b259159-71e1-4f55-b157-aacc07384f8a.png#averageHue=%2317120f&clientId=u1a7d83d9-8ce0-4&from=paste&height=105&id=u76c6422c&originHeight=137&originWidth=767&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21430&status=done&style=none&taskId=uce9c45b1-796d-42ff-ab80-95b5189713e&title=&width=587)，显示没有basicModel 这个文件，于是我在VS Code 上查找了提示行的代码，把basicModel 改为smpl文件夹中对应文件的文件名，为basicmodel。
在接着运行，还是报一样的错误，但是这次我文件名没错，还是报”没有文件名“的错误，我上网搜了一下，发现可能是路径错了，于是我参照网上其他人的做法，把basicmodel_f_lbs_10_207_0_v1.1.0.pkl 文件剪切到hello_world 文件夹下，这样就使得hello_smpl.py和basicmodel_f_lbs_10_207_0_v1.1.0.pkl在同一个文件夹下。然后在使用”m = load_model( './basicmodel_f_lbs_10_207_0_v1.1.0.pkl' )“这一句代码表示文件的正确路径，最后就可以运行hello_smpl.py文件了。在hello_world 文件夹下生成了一个hello_smpl.obj文件。

运行render_smpl.py 时也一样，把m = load_model( './basicmodel_f_lbs_10_207_0_v1.1.0.pkl' ) 这句代码代替render_smpl.py中的，然后运行，会显示![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1652423301115-146a8cc5-a4bb-4596-b7aa-12b3a9e7f7b1.png#averageHue=%23150f0a&clientId=u1a7d83d9-8ce0-4&from=paste&height=49&id=u8910efea&originHeight=49&originWidth=387&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5357&status=done&style=none&taskId=ue98f1e1c-8863-49bd-b82d-05ec10dabb7&title=&width=387)，好像不能看到渲染的smpl 模型，需要下载XManger 。下载好之后，连接一下，就可以运行了。

### 使用docker：
启动docker :docker exec -it ubuntu_xiao /bin/bash
cd5047e375b2c43ac96f2a520cfe98d85793bbeab9610dc92795d9baab935c9f
docker 进入容器命令：docker exec -it 49d38b370641 /bin/bash

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653294494578-3ce04c07-4d8a-429b-bd37-6d4d3ad5aa46.png#averageHue=%23070504&clientId=uf49f8029-00ab-4&from=paste&height=234&id=u2f294b6b&originHeight=234&originWidth=599&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15154&status=done&style=none&taskId=u347246d6-a8fb-423c-9e28-e1c121ab901&title=&width=599)

先启动容器，再进入容器：
启动：$ docker start b750bbbcfd88 
进入容器：docker exec -it 243c32535da7 /bin/bash

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1653963991450-0da9253f-506f-4b51-af91-b0d7530b1096.png#averageHue=%23f6f6f6&clientId=u8ac1841d-ac21-4&from=paste&height=784&id=uf45acbf8&originHeight=784&originWidth=719&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103098&status=done&style=none&taskId=u2f83c5a5-9e01-4f32-b199-266fc7079a9&title=&width=719)

### 重新跑这个模型
运行：
```python
python render_smpl.py
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1656926108468-04902577-bcf9-456f-8845-f65377d7581f.png#averageHue=%23f7f4f1&clientId=u3eb12706-eab4-4&from=paste&height=39&id=u7efe5980&originHeight=39&originWidth=448&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3059&status=done&style=none&taskId=u66e8204e-bba8-4bc1-b9bf-3281c4d78de&title=&width=448)
去百度了一下，解决方案：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1656926136467-8a7896d2-d384-4edd-b0a7-b0862f6b84cf.png#averageHue=%23d9c59a&clientId=u3eb12706-eab4-4&from=paste&height=386&id=u7fdec10e&originHeight=386&originWidth=737&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28984&status=done&style=none&taskId=ub184f92a-2e21-4b12-a723-61f2257a479&title=&width=737)
得到一张渲染图片

![render_SMPL_screenshot_14.05.2022.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1656922905709-bb44acf1-9cea-43bc-8339-48361683cbfe.png#averageHue=%23050505&clientId=u3eb12706-eab4-4&from=drop&id=u1e0b4a6b&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20909&status=done&style=none&taskId=u7c66b8b6-b441-42ec-8506-b338875fe5f&title=)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1656922815269-7c13cbff-5201-4ddf-8607-689c39680f64.png#averageHue=%23fbfaf8&clientId=u3eb12706-eab4-4&from=paste&height=104&id=u625cacc5&originHeight=104&originWidth=472&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8099&status=done&style=none&taskId=u4033faaf-e7db-4eec-93ec-7c6647d378b&title=&width=472)

我试着修改一下参数：
[1]zero pose
设置参数：m.pose[:]=0
m.beat[:]=0
m.pose[0]=np.pi
得到：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22838017/1656926083449-0796fd0c-7d81-4c70-bfcb-12388ee18d92.png#averageHue=%23090909&clientId=u3eb12706-eab4-4&from=paste&height=484&id=u4a0889d8&originHeight=484&originWidth=642&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18920&status=done&style=none&taskId=u661a65c7-e8c0-4a0f-baf4-1befe02f3c4&title=&width=642)
