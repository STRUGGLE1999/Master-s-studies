# 基于高频价格-交易量的选股深度学习
# 1. 引用格式
@misc{yang2019deep, 
 title={Deep Learning for Stock Selection Based on High Frequency Price-Volume Data}, 
 author={Junming Yang and Yaoqi Li and Xuanyu Chen and Jiahang Cao and Kangkang Jiang}, 
 year={2019}, 
 eprint={1911.02502}, 
 archivePrefix={arXiv}, 
 primaryClass={q-fin.CP} 
}
#  2.论文解决的问题
现有的一些论文在使用低频数据和特征构建模型时取得了好的成效，但是利用高频股票数据训练出合适的模型仍然是一个问题，加上使用高维信息构造有用的高维特征是一个难题，作者基于这些，想要应用自动构造特征的方法，介绍了LSTM和CNN两种机器学习算法，为了找到中国股票市场最有益的股票交易策略。
# 3. 采用的思路、技术方法
3.1思路
通过上一期（一天或几天）的高频价格数据，本文构建了两种可以预测当天股票预期收益率的模型，并选择开盘时预期收益率最高的股票，使总收益最大化。
3.2技术方法
3.2.1构建LSTM模型
本文先使用![](https://cdn.nlark.com/yuque/__latex/09f3ab605aba2765df89395a1681bddf.svg#card=math&code=x_%7Bit%7D&id=Vchqs)来表示股票i在事件t的特征向量，然后将这些时间序列作为LSTM的输入来预测下一天利润。本文将股票预测问题看作是分类问题，构造了一个依赖于LSTM网络的分类模型：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636874381574-620515cd-f5a9-4cb8-b67b-07c507c23dea.png#clientId=u5c3ddb42-6323-4&from=paste&height=157&id=u23a81ae6&originHeight=157&originWidth=802&originalType=binary&ratio=1&size=27895&status=done&style=none&taskId=u704bbd91-745d-4d0c-9ddb-3e35472e1b8&width=802)
dropout层作用是防止训练过程中出现过拟合，Dense层输出预测的实例属于每个类别的概率分数，这个分数通过softmax层转化为相应的概率。
3.2.2构建CNN模型
卷积神经网络(CNN)可以将每个区域的主要特征转换为更高层次的特征，作为特征提取工具。CNN的这种功能可以用来处理本文的任务。这是因为一天内的数据输入是二维的，一维是特征集，另一维是一天内的时间段。在进行超参数搜索后，本文最终决定使用40个核，这同时考虑了模型的训练成本和预测的有效性。通过该卷积层，本文从11个主要特征中提取了40个高级特征。二维数据结构由16×11（时周期×原始特征）变化到16×40（时期×高级特征）。在完全连接层中，在这些层中，由CNN层生成的16×40数据被扁平为最终的特征向量。然后，将该特征向量通过2个完全连接的层（2个隐藏层）转换为最终的预测。输出层有4个神经元，以SoftMax函数作为激活函数，旨在分别给出大涨、小涨、大跌、小跌的概率，然后计算其今天每日回报的预期，作出股票购买建议。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636875745177-351dd784-6f13-407d-9ab1-e1c5cc0e9cc2.png#clientId=u5c3ddb42-6323-4&from=paste&height=246&id=u4152c22d&originHeight=246&originWidth=856&originalType=binary&ratio=1&size=55114&status=done&style=none&taskId=uf6ea012c-9591-4fcd-a9bc-e0c8dc7eca6&width=856)
3.2.3训练LSTM模型
考虑到本文模型需要使用更长的时间间隔来学习当前数据与几天前数据之间的相关性，本文训练两种预测模型，第一种类型选择输入数据作为前一天每15分钟的价格数据的时间序列，共240分钟，与16步相同。训练模型预测第二天的返回y，第二个模型使用前10天内每120分钟的价格。带有20个步骤的数据预测了第二天的返回 y。
3.2.4训练CNN模型
CNN模型的数据输入和LSTM一样，用跨熵成本函数作为损失函数，Adam作为优化器。在模型设置方面，本文通过对比两个已有的框架进行改进，最初选择在任务中应用类似的框架，分别使用两个卷积层来提取高级特征和持久性特征，然后是一个最大池化层，最后是一个完全连接的层。但是随着实验的进行，作者发现平均池化比最大池化要更好，甚至不要池化层效果更好，最终，在本文框架中，只有一个卷积层和一个全连接层。后面又发现增加全连接层有更好的性能，所以最终决定采用2个全连接层，神经元数量分别是250个和100个，ReLu作为激活函数。在最终，本文决定使用CNN+2Dense作为最终框架，并连续5天提供15分钟的价格-交易数据。
# 4. 解决的效果
4.1数据获取
挑选了中国A股市场的收盘价、开盘价格、最高价格、最低价格、交易量、交易金额、交易次数、佣金比率、交易量比率、佣金购买、佣金销售，这11个特征描述股票状态。本文还挑选了CSI300指数中的数据作为源样本数据集，这样可以减少股票不一致的问题。模型使用每十五分钟和每二十分钟两种类型数据。本文挑选的股票时间段是CSI300指数从2014年6月到2018年12月的数据。但是在后面的研究中发现计算能力不足，最后调整模型时，只使用了2019年1月至2019年5月的数据。
4.2数据处理
本文将日收益率分为四类：大涨、小涨、大跌、小跌。划分范围如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636876952954-c4910862-7bb2-4e00-99f3-77d6f7fb0feb.png#clientId=u5c3ddb42-6323-4&from=paste&height=104&id=u1226c25f&originHeight=104&originWidth=772&originalType=binary&ratio=1&size=21660&status=done&style=none&taskId=u5da53833-8914-460a-8273-092daa47441&width=772)
本文将80%的数据作为训练集，20%作为测试集。LSTM和CNN两种模型共享数据集。LSTM模型使用了两种类型的数据。第一个是前一天每15分钟的价格数据时间序列，共240分钟或16步，第二个是前10天每120分钟的价格数据，即20步的时间序列；CNN模型使用一种数据，即前5天每15分钟的批量数据。
本文使用的损失函数是交叉熵成本函数，在优化器的选择上，本文将Adam，Adadelta，RMSProp三种优化器进行比较，最后选定Adam优化器。
4.3LSTM 模型训练结果
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636885066348-e8176158-c93f-4781-a36c-685988527d72.png#clientId=u0ddd4259-9978-4&from=paste&height=273&id=u7f1d0e24&originHeight=273&originWidth=805&originalType=binary&ratio=1&size=139026&status=done&style=none&taskId=u4e150bb1-bafd-4ef3-beaa-18b902d0eb1&width=805)
从图可以看出，模型L比模型s预测得更好，并且通过两个模型在第二十次迭代的混淆矩阵热力学图可知，较亮的区域集中在对角线附近，尤其是右下角和左上角，表明了该模型对大涨和大跌的情况预测较为准确。但是中间较暗区域表明该模型不能很好区分小涨和小跌。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636885427731-cc4aded5-af89-4456-9ec9-f68a0b0a2650.png#clientId=u0ddd4259-9978-4&from=paste&height=271&id=u12385c8c&originHeight=271&originWidth=843&originalType=binary&ratio=1&size=110720&status=done&style=none&taskId=u63bbbece-0a6f-4a69-885e-4d0ae58a43d&width=843)
4.4CNN实验结果
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636887649359-b1224bfb-b799-4da5-bcb4-1abd7264976e.png#clientId=u0ddd4259-9978-4&from=paste&height=318&id=ua077133a&originHeight=318&originWidth=603&originalType=binary&ratio=1&size=37793&status=done&style=none&taskId=uaaf01b90-8b74-4473-b16a-ddd5343173b&width=603)
从图可以看出，采用CNN的这种模型准确率达到了42%，而如果随机选择的话只有25%。
4.5结果分析
本文以CSI300指数为基线，将LSTM模型和CNN模型连接到回溯测试框架中，并模拟了2019年1月16日至2019年5月31日的交易。具体交易操作如下：以I为股票池，根据模型给出的购买建议进行交易——考虑到交易费用，本文只考虑购买不超过20只股票，预期利润在0.14%或以上。基金平均分配，投资组合每天都在变化。结果如下：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636888276263-6e14e74d-a948-440b-8e9c-d18ef000dafd.png#clientId=u0ddd4259-9978-4&from=paste&height=290&id=u786a40fc&originHeight=290&originWidth=491&originalType=binary&ratio=1&size=36485&status=done&style=none&taskId=u81e2d3ac-0be2-4fe1-b37c-83c9b3d4d37&width=491)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636888286023-f2be5c29-cdbb-45ae-9287-c0c7aa4b6ce9.png#clientId=u0ddd4259-9978-4&from=paste&height=302&id=u89e2d889&originHeight=302&originWidth=498&originalType=binary&ratio=1&size=36890&status=done&style=none&taskId=u3eb77b31-c552-4a77-844e-7c7c96603e0&width=498)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636888296608-5f521e45-c65e-4dbe-85c9-32e043e75a80.png#clientId=u0ddd4259-9978-4&from=paste&height=382&id=u4bb07971&originHeight=382&originWidth=708&originalType=binary&ratio=1&size=66489&status=done&style=none&taskId=ud0b59869-972f-4025-ae19-595c06bafa6&width=708)
由图可知，无论手续费如何，这两种模型都明显优于基线，这从另一个角度解释了它们的有效性。

#  5.启示
（1）本文提出的两种深度学习模型通过实验测试，发现这两种模型具有收敛速度快、泛化能力强、不需要构造因子等优点。然而，预测的准确性与预期之间仍然存在差距。从回测结果图可以看出，CNN和LSTM运用在股票预测方面，不能取得在自然语言处理和图像识别领域那么好的性能。在以后的研究中，可以利用LSTM和CNN的优势来构建一个结合CNN和LSTM的新模型。
（2）对于数据的使用频度方面，本文由于计算能力不足，所以在一个集合中使用的数据是每15分钟和120分钟的价格-交易量特征，而没有使用1分钟或者5分钟一次的高频数据，这会影响到捕捉股市中微小信息和趋势的精确度。
（3）在本文的实例中，每天交易一次股票，这会产生很多不必要的交易费用，在未来的研究中可以利用强化学习来最大化收益。
#  6.有用的论点、句子
M:CSI300指数 2014年7月1日至2018年12月31日，
N:CSI300指数2019年1月至2019年5月，
J:2014年6月至2018年12月期间所有A股的日收益率（收盘价减去开盘价格）
I:CSI指数中的部分

（1）Specififically, we hope to use CNN’s ability to automatically extract high-level features and enhance important features to construct factors. After CNN generating time series about high-level factors, this time series is then used as an input to the LSTM.
（具体来说，我们希望利用CNN自动提取高级特征的能力，增强重要特征来构建因子。在CNN生成关于高级因子的时间序列后，然后将这个时间序列作为LSTM的输入。）
（2）This paper applies neural network of deep learning to construct two models of LSTM and CNN to forecast the expected return rate of the stock today, and to maximize the total return by adapting an appropriate strategy. 
（本文应用深度学习神经网络构建LSTM和CNN两个模型，预测当前股票的预期收益率，并采用适当的策略实现总收益最大化。）
（3）Although, these two models have overcame some diffiffifficulties, there is still a possibility of advancement such as avoiding unnecessary transaction fees. In our later works, we will focus on solving these problems.
（虽然这两种模式已经克服了一些困难，但仍有进步的可能性，如避免不必要的交易费用。在我们以后的工作中，我们将专注于解决这些问题。）

