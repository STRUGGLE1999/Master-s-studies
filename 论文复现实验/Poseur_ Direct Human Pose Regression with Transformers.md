![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1677982870039-13409539-12f9-4cd7-841b-e634a263a06e.png#averageHue=%23f7f7f7&clientId=u4bcd6c22-8959-4&from=paste&height=73&id=u6982f7fd&originHeight=146&originWidth=885&originalType=binary&ratio=2&rotation=0&showTitle=false&size=22797&status=done&style=none&taskId=uc1d59a63-7201-4723-9422-aa1fdefee0c&title=&width=442.5)
GitHub地址：[https://github.com/aim-uofa/Poseur](https://github.com/aim-uofa/Poseur)
登录指令：ssh -p 28078 root@region-102.seetacloud.com
密码：oQGv59HKOo

下载项目：
```
git clone https://github.com/aim-uofa/Poseur.git
```
安装相关的包：
```
pip install easydict
pip install einops
```
安装MMPose:
```
conda create --name openmmlab python=3.8 -y
```
激活openmmlab：
```
conda activate openmmlab
```
安装pytorch：
```
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch
```
安装mmcv:
```
pip install -U openmim
mim install mmcv-full
```
Install MMPose:
```
git clone https://github.com/open-mmlab/mmpose.git
cd mmpose
pip install -r requirements.txt
pip install -v -e .
```
安装MMPose时报错：
fatal: unable to access '[https://github.com/svenkreiss/poseval.git/':](https://github.com/svenkreiss/poseval.git/':) Failed to connect to github.co
解决办法：
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
验证MMPose:
```
mim download mmpose --config associative_embedding_hrnet_w32_coco_512x512  --dest .
```
```
python demo/bottom_up_img_demo.py associative_embedding_hrnet_w32_coco_512x512.py hrnet_w32_coco_512x512-bcb8c247_20200816.pth --img-path tests/data/coco/ --out-img-root vis_results
```
最后在Poseur/vis_results文件夹下看到人体姿态检测结果：
![下载.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/22838017/1677985265812-840d8ce1-7bc2-4103-b6dd-5cce0462345f.jpeg#averageHue=%23cbccd1&clientId=u4bcd6c22-8959-4&from=ui&id=u750be98b&originHeight=425&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=105396&status=done&style=none&taskId=u7f9a8469-d374-49ad-861e-8e962027f7c&title=)
下载coco_2017数据集：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1677985619423-0c8d2d8d-c5b5-4478-815d-b9ea69e55e12.png#averageHue=%23f1f4f6&clientId=u4bcd6c22-8959-4&from=paste&height=541&id=u8e6974d9&originHeight=1081&originWidth=1178&originalType=binary&ratio=2&rotation=0&showTitle=false&size=92844&status=done&style=none&taskId=ude5132e0-a664-4fb5-b6a8-d9ec0a4621c&title=&width=589)
下载好了就可以开始训练模型了
```
./tools/dist_train.sh \
configs/poseur/coco/poseur_res50_coco_256x192.py 1 \
--work-dir work_dirs/poseur_res50_coco_256x192
```
报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1678009893770-c5d612a4-0a43-4894-addb-387defd66f3c.png#averageHue=%23070604&clientId=u4bcd6c22-8959-4&from=paste&height=94&id=u479dfcb6&originHeight=187&originWidth=1841&originalType=binary&ratio=2&rotation=0&showTitle=false&size=101748&status=done&style=none&taskId=u5fb6ea53-6ce5-4157-b3d1-e841d7b81c4&title=&width=920.5)
