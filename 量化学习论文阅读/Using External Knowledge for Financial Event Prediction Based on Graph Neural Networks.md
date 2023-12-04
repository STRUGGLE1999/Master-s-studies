# 1. 引用格式
Yiying Yang1,3 , Zhongyu Wei1,∗ , Qin Chen1 , Libo Wu2 . 2019. Using External 
Knowledge for Financial Event Prediction Based on Graph Neural Networks. 
In _The 28th ACM International Conference on Information and Knowledge _
_Management (CIKM’19), November 3–7, 2019, Beijing, China. _ACM, New 
York, NY, USA, 4 pages. https://doi.org/10.1145/3357384.3358156
#  2.论文解决的问题
金融事件作为最常用的信息之一，可以帮助我们预测市场趋势，为我们提供预测推断，但是尽管金融事件预测很重要，但它并没有引起研究领域足够的重视。本文为了解决这个问题，提出了一种基于历史事件链对目标公司预测下一个财务事件的新任务。
金融事件预测任务存在两方面的挑战：
（1）事件本身包含的信息是有限的，且可以用不同的方式解释。但是外部知识可以缓解这个困难。比如说通过捕捉与一个事件相关的新闻报道可以帮助我们更好预测接下来会发生的事情。
（2）对于金融事件中复杂的关系，本文基于金融事件的历史关系构建了一个事件图谱，利用图神经网络进行事件建模。
# 3. 采用的思路、技术方法
## 3.1思路
提出了一种新的金融事件预测任务，利用新闻和事件图等外部知识进行更好的预测。
## 3.2技术方法
### 3.2.1数据来源
本文从上市公司通过公告向公众公开所发生的事件中构建一个新的数据集，其中包括了2013到2017年间3556个公司发生的1271130个事件。一个单一事件由三部分组成，主体、时间和事件类型。下表是本文列出的360种事件类型中前10名及其数据分布。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1637893293392-a3fbde20-29c4-4927-8821-b2334f38fe2e.png#clientId=u2d5b3205-52fe-4&from=paste&height=342&id=u147811be&originHeight=342&originWidth=622&originalType=binary&ratio=1&size=76739&status=done&style=none&taskId=ufb9fd70d-8d0c-402f-8160-f470828aebb&width=622)
本文从风能信息公司中收集了2013年至2017年的财务新闻，总计1696468条新闻，一条新闻由三部分组成，即主体，时间和标题。事件和新闻与同一公司的同一日期相匹配，然后映射到行业，平均一个财务事件与1.78个新闻节点相关。
### 3.2.2模型结构
本文构建的模型主要有三个部分：事件表征、图神经网络和事件预测。下面是一个模型结构图：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1637894297498-4e26e479-8753-42ea-b8f9-b7f830daf541.png#clientId=u2d5b3205-52fe-4&from=paste&height=527&id=u9bd138be&originHeight=527&originWidth=1208&originalType=binary&ratio=1&size=135879&status=done&style=none&taskId=u28ff168e-fea4-4288-b750-c49ae545818&width=1208)
（1）事件表征
该模块通过结合初始事件嵌入和相关的新闻表示来获得一个事件的表示。初始化事件嵌入中事件嵌入![](https://cdn.nlark.com/yuque/__latex/f9a6812140bf479d2171492563c09cfd.svg#card=math&code=e_%7Bi%7D&id=hLDDX)表示为![](https://cdn.nlark.com/yuque/__latex/15a219ab7594dd228b954eda5d611c6a.svg#card=math&code=h%28e_%7Bi%7D%29%5Cvarepsilon%20R%5E%7Bd%7D&id=cuWpv)，其中d表示维数，候选事件也可以从这些事件类型中选择。与新闻相关的嵌入学习中，由于行业是一个包含28个值的分类变量，本文将28个分类变量转变成一个一维向量，然后使用预先训练好的BERT模型得到动作词嵌入。最后将行业向量和动词向量连接在一起，得到了节点表示![](https://cdn.nlark.com/yuque/__latex/971a697b17e9191e35dbc20c8993ea0d.svg#card=math&code=v_%7Bi%7D%20%20%20%20%5Cvarepsilon%20%20R%5E%7Bm%7D&id=P7hbn)（m是新闻嵌入的维数），本文将相关新闻的最终表征![](https://cdn.nlark.com/yuque/__latex/7077ee6f5102aafeb41675eb433b0a9e.svg#card=math&code=h_%7Bv%7D&id=HCBc3)用所有相关新闻节点的加权和表示，计算结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1637895582586-4ea5df6e-c032-4ef6-acf2-395e25a9ddc2.png#clientId=u2d5b3205-52fe-4&from=paste&height=150&id=u25ca4d04&originHeight=150&originWidth=532&originalType=binary&ratio=1&size=19109&status=done&style=none&taskId=u1a3ef76e-dc69-4aa5-a172-1c4bfddcf3a&width=532)
![](https://cdn.nlark.com/yuque/__latex/bc4a473b73c00467d804ab6edf49b2f1.svg#card=math&code=v_%7Bi%7D%E8%A1%A8%E7%A4%BA%E6%96%B0%E9%97%BB%E5%B5%8C%E5%85%A5%EF%BC%8CR_%7Bi%7D%E8%A1%A8%E7%A4%BA%E7%89%B9%E5%AE%9A%E4%BA%8B%E4%BB%B6%EF%BC%8CW_%7Bv%7D%E6%98%AF%E6%A8%A1%E5%9E%8B%E5%8F%82%E6%95%B0%EF%BC%8C%5Calpha%20_%7BR_%7Bi%7D%7D%E6%98%AF%E6%89%80%E6%9C%89%E8%8A%82%E7%82%B9%E4%B8%ADV_%7Bi%7D%E8%8A%82%E7%82%B9%E7%9A%84%E6%9D%83%E9%87%8D%E3%80%82&id=vqHZu)
事件表征更新中，本文认为有三种方法可以通过映射函数来获得目标事件表征的联合表示，依次为：平均值、非线性变换、连接方法。
（2）门控图神经网络
这个模块使用门控神经网络更新基于事件图的事件表征。首先是事件图构建子模块，本文基于事件链构建事件图，表示为G={E,L},E代表事件节点集，L代表边集。本文统计训练事件链中所有相邻的事件对，将每一对看做一条边![](https://cdn.nlark.com/yuque/__latex/b81530f8c06730856a518a08b1740be9.svg#card=math&code=l_%7Bi%7D&id=ijG2U)，通过下面的式子计算![](https://cdn.nlark.com/yuque/__latex/b81530f8c06730856a518a08b1740be9.svg#card=math&code=l_%7Bi%7D&id=oQd62)：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1637896734234-f050e93b-dfa2-45de-98be-6053c3570450.png#clientId=u2d5b3205-52fe-4&from=paste&height=62&id=u31db5b7b&originHeight=62&originWidth=429&originalType=binary&ratio=1&size=7251&status=done&style=none&taskId=ucb654692-0bb8-4fa7-b71a-2d8b24b88d4&width=429)
count(![](https://cdn.nlark.com/yuque/__latex/510e8ca2b9ca5db02482e36d9f1fe11c.svg#card=math&code=e_%7Bi%7D%2Ce_%7Bj%7D&id=vgwx2))表示事件对的频率，本文构造的事件图有360个节点，有12079条有向边和加权边。
GGNN的基本循环，主要为以下几个公式：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638014606312-69052eba-67a1-4f3b-9279-d9751c6141da.png#clientId=u1fae05c1-5d35-4&from=paste&height=159&id=u3e84da09&originHeight=318&originWidth=997&originalType=binary&ratio=1&size=57995&status=done&style=none&taskId=u0b3abe26-4ff4-4457-b31a-9ac722cffdc&width=498.5)
上面的公式5是通过有向邻接矩阵A在图的不同节点之间传递信息的步骤。剩下的公式可以看成一个循环整体，也就是类似门控循环单元的操作，从上一个时间步长更新每个隐藏层状态。![](https://cdn.nlark.com/yuque/__latex/481ae3eaf20361a0e82b59ebe407bbeb.svg#card=math&code=z%5E%7Bt%7D%E5%92%8Cr%5E%7Bt%7D%E6%98%AF%E6%9B%B4%E6%96%B0%E9%97%A8%E5%92%8C%E9%87%8D%E7%BD%AE%E9%97%A8%EF%BC%8C%5Csigma&id=Asokh)是逻辑符号函数，一个圆圈里的一点表示元素乘法。h^{t}表示更新后的事件表示。
（3）事件预测
该模块分为分数计算和损失函数，分数计算主要是计算候选事件的分数，计算公式如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638015247670-2c62d24e-db4b-4c20-a5c6-b62897ffeb13.png#clientId=u1fae05c1-5d35-4&from=paste&height=77&id=u9ac8dc18&originHeight=154&originWidth=831&originalType=binary&ratio=1&size=16541&status=done&style=none&taskId=ufff213e6-9395-4628-a4ca-fb43f6aa4c8&width=415.5)
候选事件与整个事件链的相关性可以理解为每个事件的关系得分之和。
对于损失函数，本文的训练目标是最大化真实事件的得分，最小化其他候选事件的得分，本文使用margin loss损失函数：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638015673373-aa02761c-cbe8-4767-86a2-474e06062b52.png#clientId=u1fae05c1-5d35-4&from=paste&height=64&id=uf5b19d69&originHeight=127&originWidth=923&originalType=binary&ratio=1&size=20366&status=done&style=none&taskId=uebc6a940-74a6-4f27-b453-a900f64c05e&width=461.5)
![](https://cdn.nlark.com/yuque/__latex/f97559ab8fd1c3d6a011efb4a7ecac45.svg#card=math&code=s_%7Bij%7D&id=ivTid)表示文本事件与候选事件的关系分数，y表示真实事件的索引，m是损失函数参数，![](https://cdn.nlark.com/yuque/__latex/c6a6eb61fd9c6c913da73b3642ca147d.svg#card=math&code=%5Clambda%20&id=mcqBO)是L2正则化参数，模型参数Θ通过RMSprop优化。
# 4. 解决的效果
本文将数据集分为三部分，2013-2016年的数据作为训练集，2017年前半年数据作为验证集，后半年作为测试集。比例为8:1:1，
参数设置：学习率为0.0001，节点嵌入大小为128，隐藏层的单元数位128，GGNN的循环次数为2，训练轮数为1000，参数m为0.015，![](https://cdn.nlark.com/yuque/__latex/c6a6eb61fd9c6c913da73b3642ca147d.svg#card=math&code=%5Clambda%20&id=stIBL)为0.00001，训练过程中丢弃率为0.2。
本文用了目前最先进的基线模型来评估模型的有效性：基线模型分别为：
PMI:基于共现的模型，可以计算基于成对互信息的事件对关系。
Word2Vec:是一种将事件链作为句子的学习事件嵌入的方法。
DeepWalk:是一种使用随机游动来学习事件图嵌入的无监督模型。
LSTM:是将事件顺序信息和成对的事件关系集成在一起以生成事件表示的模型。
GGNN:是本文提出的模型
GGNN+News(mean):是使用外部新闻的GGNN模型，新闻节点嵌入使用平均值池化
GGNN+News（attention）:使用外部新闻的GGNN模型，新闻节点嵌入使用图形注意力机制池化。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638019727954-c99b37c9-8140-4665-87c9-0491b53d55d5.png#clientId=u1fae05c1-5d35-4&from=paste&height=300&id=u2f9fb1a8&originHeight=600&originWidth=1041&originalType=binary&ratio=1&size=135626&status=done&style=none&taskId=udf66c7bc-11e6-49c0-b2b6-e7f51356202&width=520.5)
从表中可以看出，GGNN模型比其他的方法都要更好，说明了通过事件图来模拟金融事件的交互作用的有效性。
DeepWalk和Word2Vec 相较于PMI有了明显的改善，可能是因为这两种方法学习了金融事件的密集特征。由图也可知，新闻事件作为外部信息可以对事件预测作出贡献，此外，图注意力池化效果优于平均池化。
#  5.启示
本文在预测金融事件方面将新闻事件作为外部信息，取得了较好的效果，说明外部信息对于金融事件的预测是不可少的，但是我想可不可以在后续的实验中再加一些其他的影响因子，比如两个或者多个金融公司之间的关系信息。
本文进行了另外两个实验来进一步验证模型有效性
a.用于联合事件表征学习的不同方法性能,实验结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638020796475-b325a5fa-2420-4c41-aaf5-65351fa585fe.png#clientId=u1fae05c1-5d35-4&from=paste&height=158&id=ud08e03fd&originHeight=315&originWidth=1011&originalType=binary&ratio=1&size=51225&status=done&style=none&taskId=u5fda80bc-9d0b-460c-b16a-e22a679ec8c&width=505.5)
作者得出的结论是连接法有着最好的实验结果。
b.所用新闻节点数量对实验的影响

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638020903191-463e59f3-5042-48ff-a91f-16702a5e8667.png#clientId=u1fae05c1-5d35-4&from=paste&height=138&id=u3f464a45&originHeight=276&originWidth=935&originalType=binary&ratio=1&size=58361&status=done&style=none&taskId=ubdeff8d7-0ab3-4bcb-9695-835117848ad&width=467.5)
75.00%的事件对应于新闻节点小于或等于2的数量。4个节点可以覆盖96.62%的数据，6个节点可以覆盖99.80%的数据。（看不懂这个结论）使用的节点越多，得到的实验结果越好，但是会不会也有上限呢？本文没有办法知道使用多少个节点时的结果是最好的。
#  6.有用的论点、句子
1.This paper focuses on a novel financial event prediction task that takes a historical event chain as input and predicts what event
 will happen next.
（本文主要研究一种新的金融事件预测任务，它以历史事件链为输入，预测接下来会发生什么事件）
2.In recent years, machine learning and artificial intelligence have had fruitful applications in the financial domain, such as risk management, marketing, investment prediction and fraud prevention.
（近年来，机器学习和人工智能在风险管理、市场营销、投资预测和欺诈防范等金融领域取得了丰富的应用）
3.By selecting certain length of consecutive events chain from the list as input events,the subsequent one event and the other four selected events are used as candidate events.
（通过从列表中选择一定长度的连续事件链作为输入事件，将随后的一个事件和其他四个选定的事件作为候选事件。）
4.We propose to use external knowledge for better prediction based on a gated Graph Neural Network. Experimental results show the superiority of our
proposed model compared to other baselines.
（我们建议利用外部知识来更好地预测基于门控图神经网络的预测。实验结果表明，我们提出的模型比其他基线具有优越性。）

