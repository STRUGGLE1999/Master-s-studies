REST：关系事件驱动的股票趋势预测

股票预测技术：分为事件嵌入方法和技术分析
技术分析不擅长捕捉由股票外界事件信息引起的价格突变。


# 1.引用格式
Wentao Xu, Weiqing Liu, Chang Xu, Jiang Bian, Jian Yin, and Tie-Yan Liu. 2021. REST: Relational Event-driven Stock Trend
Forecasting. In <i>Proceedings of the Web Conference 2021</i> (<i>WWW ' 21</i>). Association for Computing Machinery, New York, NY, USA, 1–10. DOI:https://doi.org/10. 1145/3442381.3450032
# 2.论文解决的问题
现有的事件抽取技术缺点：
（1）忽略了由股票依赖的属性所区分的事件信息的影响； （Stock-dependent Influence）
相同的事件对不同的股票有不同的影响，也就是说一个事件发生，对有的公司是好的，对有的公司影响是消极的，但是现存的事件驱动方法关注的更多的可能是不同的事件对相同股票的影响。
（2）忽视了来自其他相关联股票的事件信息。(Cross-stock Influence)
针对第一个问题，本文对股票上下文进行建模，了解在不同背景下事件信息对股票的影响。
针对第二个问题，本文构建了一个股票图，设计了一个新的传播层对相关股票的事件信息进行传播。
# 3.采用的思路、技术方法
## 3.1提出了一个新的事件驱动股票趋势预测框架。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636336010356-83cc243c-8f13-4414-902e-0e18fe2e119b.png#clientId=u9add401f-8222-4&from=paste&height=520&id=u41c2554e&originHeight=520&originWidth=688&originalType=binary&ratio=1&size=77811&status=done&style=none&taskId=ubb3800eb-5503-4c42-9145-61659ec7f46&width=688)
本文除了直接从详细的文本信息中对事件特征建模，还同时研究了与股票直接相关的其他股票事件的背景信息。股票图是用来将一个股票直接相关的事件信息的影响传播到其他相互关联的股票。
设计了一个新的传播层模拟交叉股票之间的影响，设计过程基于对现实世界的观察特征：
（1）一个事件对具有不同关系的股票有不同的影响
（2）两只股票之间的事件影响传播强度是动态的
（3）事件信息的影响可能是多跳传播
## 3.2设计了一个传播层来传播事件信息对股票的影响。
关系事件驱动的股票趋势预测框架说明图
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1635154332090-35dc1fc5-c489-45d4-8eca-11929dcb2f96.png#clientId=u8570653b-1c80-4&from=paste&height=479&id=ub2085917&originHeight=479&originWidth=1288&originalType=binary&ratio=1&size=145089&status=done&style=none&taskId=ueb1c1d8b-4152-4dfb-aec2-7573ee9acc4&width=1288)

对于信息处理部分，分为两个小部分，事件信息编码器和股票背景编码器。
### 3.2.1事件信息编码器
主要学习一支股票每天事件信息的特征。这一过程经过两个技术处理：
首先是特定类型的编码器，主要学习事件嵌入，对于每一个事件嵌入，本文都用一个类型嵌入![](https://cdn.nlark.com/yuque/__latex/a578f93d86a9111f5900e4681f14ce16.svg#card=math&code=t_%7Bi%7D&id=Oje4r)和一个符号嵌入序列![](https://cdn.nlark.com/yuque/__latex/bb82cc3fd01ca17fd3ef04e5cfc4162a.svg#card=math&code=%5Cleft%20%5Bw%20_%7Bi1%7D%2Cw_%7Bi2%7D%2C.....w_%7Bin%7D%20%5Cright%20%5D&id=TLh72)来分别表示事件类型和文本中第x个符号。为了知道哪个单词对股票趋势的贡献率最大，本文引入了特定类型的多头注意力机制。接着是经过事件序列编码器，由于股票趋势预测是一个时间序列，所以要将事件嵌入序列输入到单层LSTM网络中，将最后一层隐藏层状态作为股票的事件信息特征。
### 3.2.2股票背景编码器
利用两个LSTM神经网络，一个是用来处理历史事件，另一个处理历史事件的反馈信息。具体步骤为：将历史事件和反馈信息的时间序列输入到两个不同的LSTM中，然后连接LSTM的最后一层隐藏层作为文本的股票背景。
对于学习部分，也分为两小部分，学习单只股票的影响和学习交叉股票的影响。
（1）学习单只股票的影响
从股票背景![](https://cdn.nlark.com/yuque/__latex/3a75ed9663e8daec9ed7f81149a97fea.svg#card=math&code=h_%7Bi%7D%5E%7Bc%7D&id=kKZVV)和事件信息![](https://cdn.nlark.com/yuque/__latex/da0ba27c2ef3fe6cf3d9deed25263fa4.svg#card=math&code=h_%7Bi%7D%5E%7Bt%7D&id=AunLx)中学习事件信息的影响强度，公式为：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636512085381-b947745b-8a4e-4bac-a0e5-b00e4311ef5a.png#clientId=u31767433-80ec-4&from=paste&height=73&id=u4f886e5f&originHeight=73&originWidth=365&originalType=binary&ratio=1&size=7174&status=done&style=none&taskId=u01490edc-0564-4c87-93be-d00d464339a&width=365)
其中![](https://cdn.nlark.com/yuque/__latex/1ffdb438edfe64ab8583c2ef8178cccb.svg#card=math&code=a%5E%7BT%7D&id=FxeKO)是单层前馈神经网络，||表示连接运算，![](https://cdn.nlark.com/yuque/__latex/8415fd0d19cb49187cb738271e960f2a.svg#card=math&code=D_%7Bii%7D%5E%7BT%7D&id=zwdQ5)就表示股票的事件信息影响强度。
接着再将事件信息矩阵和影响强度相乘就得到事件信息的影响，即公式![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636512569917-2f35c892-2ddf-40ee-8faf-2c4c9879fe64.png#clientId=u31767433-80ec-4&from=paste&height=60&id=u8a9f3701&originHeight=60&originWidth=206&originalType=binary&ratio=1&size=2452&status=done&style=none&taskId=u24a42eca-2e14-4ca7-8f0a-ce64719f81f&width=206)
（2）学习交叉股票的影响
为了学习交叉股票的事件影响，本文定义了一个股票图谱，里面的节点表示股票，边表示股票关系。接着，本文设计了一个新的传播层，传输来自股票图中相关股票的事件信息影响，设计的传播层主要关注现实世界的观察特征。主要有三个部分：
a.关系型GCN传播层
为了解决简单的前馈神经网络传播层不能识别不同关系的影响，本文采取了关系型GCN。这里先用一个特定关系映射矩阵映射事件信息的影响。
b.动态传播层
为了模拟现实世界股票市场中，两只股票间的事件影响强度是动态的，本文提出使用动态传播权重来传播事件信息的影响，使用股票背景学习动态权重，如果两只股票之间不相邻，则它们之间的动态权重为0。
c.多跳传播
为了搞清楚多跳路径的事件影响，本文堆叠更多的传播层来收集具有多跳距离的相邻股票的事件效应。本文从l-1跳的相邻股票中传播事件效应，为了从l跳的相邻股票中收集事件影响。
# 4.解决的效果
数据集收集：使用CSI300和CSI500股票指数   2013-2016年的股票数据，这些数据包括：事件数据、股票关系、事件反馈、股票价格趋势标签。2013-2016作为训练集，2017作为验证集，2018作为测试集。
本文为了突出实验效果，与下列几种方法作比较：
ARIMA：使用每只股票的历史趋势序列作为输入来预测股票未来趋势
TGCN：使用图卷积神经网络传播历史价格信息
HAN:对媒体信息使用层注意力机制
Event-driven:直接将原始事件信息输入到一个dense 层，输出股票价格趋势预测
Event-driven_sd:相比于上一种，这种方法可以学习单只股票和交叉股票的事件影响。
GCN:使用GCN传播层传播事件信息的影响
Relational GCN:相比于前一种，该方法可以学习不同关系的不同事件影响
REST（l=1）:使用动态传播层传播事件信息的影响，但是不能通过多跳路径传播事件信息影响。
通过性能比较和投资模拟两方面来比较以上几种方法的性能：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636678765509-edeadb92-fb7c-458f-9c9c-93157cf362a2.png#clientId=u9cc065aa-0f12-4&from=paste&height=324&id=u4f7c6565&originHeight=324&originWidth=613&originalType=binary&ratio=1&size=61054&status=done&style=none&taskId=ubab4dccb-18a5-40b1-bf4f-de1376ab89f&width=613)
性能比较主要这些方法在CSI300和CSI500上的RMSE、MAE、MedAE值，实验结果表明：本文提出的REST框架取得了最低的RMSE、MAE、MedAE值，其他方法之间的比较如下：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636678993147-35826fa7-e344-437a-8b70-b8d1a0bf840c.png#clientId=u9cc065aa-0f12-4&from=paste&height=337&id=u3a32af41&originHeight=337&originWidth=584&originalType=binary&ratio=1&size=96001&status=done&style=none&taskId=uc2314772-6ec2-4378-85e5-86691c33e22&width=584)
投资模拟本文采取一种交易策略模拟股票投资，也就是根据股票趋势预测将股票由高到低排序，使用年回报和夏普比率评估投资模拟结果，实验结果表明，本文提出的REST框架获得了最高的年回报和夏普比率，其他结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636679453822-d6aa28d7-0621-469a-8e53-cbaef935e9ac.png#clientId=u9cc065aa-0f12-4&from=paste&height=339&id=u7bb419eb&originHeight=339&originWidth=581&originalType=binary&ratio=1&size=89001&status=done&style=none&taskId=u82ac2f21-2adf-4df1-8db6-cb787e1b141&width=581)
本文对模型的分析主要体现在三个方面：研究股票背景的影响、事件传播距离的影响、案例研究。
在研究股票背景的影响时，主要对比了三种模型，一种是仅仅使用历史事件的REST框架，另一个是仅使用历史事件反馈的REST框架、最后是使用历史事件和事件反馈的REST框架。实验结果表明，同时使用历史事件和历史事件的反馈可以达到最佳的性能。
在研究事件传播距离的影响时，本文将REST框架的传播层数从1改变到4，并观察在CSI300和CSI500上的结果。实验结果表明，传播层数l是一个敏感的超参数，过多的传播层不能进一步提高种群趋势预测。
在实例研究方面，本文深入研究了三家公司的具体研究案例，分别为：WLY、KCMT、LZLJ。随后比较了股价趋势的真实情况和ARIMA、Event-driven、REST三种模型的预测结果，实验结果表明，ARIMA和Event-driven都不能作出正确预测，而REST模型可以。
# 5.启示
本文提出的关系型事件驱动框架（REST）解决了现存股票预测框架的两个不足点，本文展开的实验结果都表明框架具有有效性，可以取得一个较高的投资回报。在未来的工作中，作者将会从新闻、社交媒体、其他相关股票信息中挖掘更丰富的信息，将不同种类的股票信息融入到股票预测趋势中，还会考虑用无监督学习预测股票趋势。
# 6.有用的论点、句子
stock-dependent influence:the influence of event effect differentiated by the stock dependent properties
好的句子：

1. Stock trend forecasting, aiming at forecasting the future price trends of stocks, plays as one of the fundamental techniques and has attracted soaring attention from human wisdom .（股票趋势预测，旨在预测股票未来的价格趋势，作为基础技术之一，并引起了人类智慧的高度关注。）

2.Therefore, our new propagation layer separately models the effect of event propagation through different relations and combines them for the trend prediction.（因此，我们新的传播层分别模拟了事件通过不同关系传播的影响，并将其结合起来进行趋势预测。）
3. Hence, our new propagation layer models the dynamic propagation strength of influence by taking each stock pair’s dynamic context into account.（因此，我们新的传播层通过考虑每个种群对的动态上下文来模拟影响的动态传播强度。）
专有名词：
hierarchical attention mechanisms(层次化注意力机制)
target-specific abstract-guided news document representation model(特定于目标的抽象引导的新闻文档表示模型)
knowledge-driven temporal convolution network(知识驱动的时间卷积网络)
deep generative model:深度生成模型
Cross-model attention based Hybrid Recurrent Neural Network:基于跨模型注意力的混合递归神经网络。
ARIMA模型
a new State Frequency Memory(SFM)recurrent network    新型状态频率存储器循环网络
Markov random fields(MRFs):马尔可夫随机厂
Open:开盘
Close:收盘
High:最高价格
Low:最低价格
Volume:交易量
VWAP:成交量加权平均价格   volume weighted average price
Annual Return(AR):是金融领域的标准利润指标，即投资提供的一年多的回报。
Sharpe Ratio(SHR):在调整其风险后，衡量投资与无风险资产相比的表现。
股票上下文（Stock Context）：股票历史事件和与之相关的反馈信息的集合
事件反馈：一个事件发生时，对应股票的价格和交易量的相对改变。
介绍某一项方法使用的技术时，可以用“use”,"leverage","A model that utilizes........","feed......"


