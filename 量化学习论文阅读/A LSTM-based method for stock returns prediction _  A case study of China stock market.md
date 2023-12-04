# 1. 引用格式
K. Chen, Y. Zhou and F. Dai, "A LSTM-based method for stock returns prediction: A case study of China stock market," 2015 IEEE International Conference on Big Data (Big Data), 2015, pp. 2823-2824, doi: 10.1109/BigData.2015.7364089.
#  2.论文解决的问题
与现有的随机预测方法相比，提出一种可以提升股票收益预测的方法。
# 3. 采用的思路、技术方法
## 3.1思路
本文提出了利用LSTM模型预测股票价格，一个序列被定义为在固定时间内一只股票的每日数据集的连续集合。本文利用盈利率标记序列性能，具体来说，就是通过比较序列后3天的平均收盘价和当前序列最后一天的收盘价来计算的。
## 3.2技术方法
搭建LSTM模型，本文搭建的模型包括：具有记忆单元数的输入单元，作为学习特征序列；多个LSTM层和一个dense层；具有记忆单元数的简单输出层，输出序列性能的分类。Keras代码如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1637387944160-a4974cd2-ac10-4dee-b4e0-03db6b10de03.png#clientId=uf628638e-dbf4-4&from=paste&height=325&id=uc8b1179a&originHeight=325&originWidth=580&originalType=binary&ratio=1&size=47085&status=done&style=none&taskId=u3102d9a0-03d2-4ca8-920d-c3a5fd735e5&width=580)
在选择序列学习特征上，本文考虑到现有四种类型股票数据：（1）股票的历史价格数据；（2）通过对历史价格数据计算得来的数据；（3）其他相关股票的历史价格数据；（4）经济基本面数据；本文最后选择了（1）和（3）。
数据来源：本文从雅虎金融上获取了上海和深圳的中国股市历史数据股票，其中包括每天的最高最低价，开盘收盘价，成交量等等，总共从1990年12月19日到2015年9月10日，每天有7767102只股票。
序列数据：一个序列包括30个每日库存数据，本文从2013年6月1日到2015年5月31日获取了1211361个序列，将2013.07.01到2014.11.12的股票数据用作训练集，2014.11.12到2015.05.31的数据用作验证集。
股票收益分类：本文根据收益率将序列分为七类，每个范围区间代表收益率范围。
训练细节:本文通过随机下降算法的RMSprop训练模型，学习率设置为0.001，使用大小为64的小批量数据，通过使用从训练集计算出的每个库存的平均值和标准差，对一个序列的每个向量进行归一化。模型的环境为;CPU:E5-2620,64G的内存，GPU为Tian，选择CentOS7、Theano5和Keras作为深度学习平台。一个时期的持续时间约为1500秒。
本文使用了四种方法进行对比，对比的股票回收预测结果图如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1637390192017-a076a3e3-dfb7-42f8-babb-ce2d5160deca.png#clientId=uf628638e-dbf4-4&from=paste&height=517&id=u5dfc825a&originHeight=517&originWidth=644&originalType=binary&ratio=1&size=80975&status=done&style=none&taskId=u083880e0-95e3-4591-83be-47e3992d945&width=644)
Random代表随机预测，M1表示使用收盘价和交易量作为学习特征LSTM模型；M2表示在M1的基础上进行了正则化处理，M3表示在M2的基础上额外加入最高价、最低价、开盘价的特征，M4表示在M3的基础上额外加入上证指数中的五个特征，M5表示只用上证指数的股票用于培训和验证。
# 4. 解决的效果
结果表明正则化处理对于提高精确度非常有效，上证指数进一步提高了准确性，M5是只使用上证指数选择的股票，具有最高的准确性，这说明不同的股票集会影响预测的准确性，有必要对不同类型的股票进行单独预测。虽然准确度没有很高，但是相比于其他方法提高了一定的准确度，本文认为如果设置正确的阈值，排除极高或极低收益的序列，会有更大的提升。
#  5.启示
本文在股票预测准确率上没有很大的提高，本文的重点在于对比不同特征之间的方法，整体上还是使用的LSTM框架，在往后的研究中，可以对比不同的神经网络结构模型，测试哪一种模型对于股票价格预测有着更好的效果。
除此之外，可以加入更多的特征进行学习，比如国际市场指数，突发金融新闻，情感因素等等外部影响因子。
#  6.有用的论点、句子
Our initial efforts demonstrated the power of LSTM in sequence learning for stock market prediction in China, which is mechanical yet much more unpredictable. 
（我们最初的努力证明了LSTM在中国股市预测方面的力量，这是机械的，但更加不可预测。）
