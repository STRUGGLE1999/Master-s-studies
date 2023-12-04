# 1. 引用格式
Shen S, Jiang H, Zhang T. Stock market forecasting using machine learning algorithms[J]. Department of Electrical Engineering, Stanford University, Stanford, CA, 2012: 1-5.
#  2.论文解决的问题
机器学习，例如支持向量机和强化学习算法等，因其在金融市场预测方面潜力而被广泛研究。然而现有的文献当中，机器学习算法输入选择的特征大多来自同一市场关注的数据。这种孤立的方法遗漏了其他实体所携带的重要信息，使预测结果更容易受到局部扰动的影响。为了解决这个问题，研究者们提出了情感分析的方法，然而情感分析在某些情况也会失败。本文建议将全球股票数据与其他金融产品的数据相关联，作为机器学习算法的输入特征。
# 3. 采用的思路、技术方法
在本项目中，本文提出了一种新的预测算法，利用全球股市和各种金融产品之间的时间相关性，借助SVM来预测第二天的股票趋势。
## 31.数据收集
本文收集了十六个源数据集，涵盖了2000年1月4日至2012年10月25日的每日价格，本文使用NASDAQ作为数据对齐的基础，其他数据源中缺失的数据用线性值取代。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636937979191-55eabc1c-10f4-429b-9c00-791d15d7874f.png#clientId=u36021f34-0634-4&from=paste&height=185&id=uc24ce165&originHeight=185&originWidth=543&originalType=binary&ratio=1&size=30566&status=done&style=none&taskId=u1ac3af42-aa81-4e33-be3b-93820adefec&width=543)
## 3.2特征选择
本文为了选择具有高时间相关性的输入特征，计算了不同市场趋势（增加或减少）的自相关和互相关，使用NASDAQ作为基准市场。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636940971743-d874a339-bbff-45c4-9bf3-4d4cb55894a0.png#clientId=u36021f34-0634-4&from=paste&height=401&id=ucae43860&originHeight=401&originWidth=577&originalType=binary&ratio=1&size=74942&status=done&style=none&taskId=ue593752a-b3ca-4781-a590-29d9ac7c766&width=577)
由图可知，NASDAQ的自相关仅在原点为非0，近似为一个马尔可夫过程，不能对未来的预测提供太多参考价值。DAX AUD和其他 一些市场等数据源对于我们使用机器学习算法构建的预测器是有前途的特征。除了日常市场走势的相关性，本文还关注长期市场趋势的相关性，这为预测未来价格提供了有价值的信息。
针对所有可能的特征，本文实现了前向特征选择算法，使用不同的机器学习算法来选择对预测精度贡献最大的特征。正如预期的那样，每日市场趋势和长期走势的结合提供了最好的结果。


# 4. 解决的效果
## 4.1单一特征预测
本文使用互相关来估计每个特征的重要性。为了验证相关分析给出的信息，本文使用个体特征预测NASDAQ指数趋势。预测精度图如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636965004001-56a5e1d3-f96a-4522-9fd9-5e2ce334c30d.png#clientId=u36021f34-0634-4&from=paste&height=213&id=u2e805738&originHeight=213&originWidth=573&originalType=binary&ratio=1&size=18773&status=done&style=none&taskId=u7aacd91e-dd6d-41f7-8879-56b75a7e14c&width=573)
## 4.2长期预测
本文将预测问题定义为预测明天指数值与某几天指数值之间的差值。本文使用SVM作为训练模型，在不同时间跨度下的预测精度如图：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636965353143-493c243e-509c-4cd0-be35-a0acbba8dd33.png#clientId=u36021f34-0634-4&from=paste&height=269&id=ud4a983dd&originHeight=269&originWidth=566&originalType=binary&ratio=1&size=26007&status=done&style=none&taskId=u30bcdd72-b092-465a-bdad-6adefe20538&width=566)
由图可知，随着时间跨度的延长，预测精度会增加。当时间跨度超过30天时，准确率达到85%。
## 4.3多维特征预测
本文比较了SVM算法和MART算法的预测精度，结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636965915358-2a23c24d-1cd3-4ec4-b63d-5ed4ad37d1ee.png#clientId=u36021f34-0634-4&from=paste&height=166&id=u4358241f&originHeight=166&originWidth=586&originalType=binary&ratio=1&size=20679&status=done&style=none&taskId=ue509fc81-eb0d-4063-8515-2d37ab74481&width=586)
## 4.4预测选择
本文使用均方误差的平方根(RMSE)作为标准，使用线性回归、广义线性模型（GLM）和SVM算法来预测NASDAQ每日变动精确值，不同算法的RMSE值如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1636966365954-87b1bbcc-d5e1-46d5-b5b1-3faba1647ec7.png#clientId=u36021f34-0634-4&from=paste&height=146&id=u60a6bfa1&originHeight=146&originWidth=601&originalType=binary&ratio=1&size=17841&status=done&style=none&taskId=u2dde7471-4291-4c65-8ff1-94346db6e19&width=601)
## 4.5多级分类
为了最小化交易风险，本文使用SVM回归模型算法，将原始数据分为三类，负、中性、正，运用了精度和召回率的公式。召回率直接影响了交易频率，而精度影响了每个时间点的利益和损失。
## 4.5交易模拟
本文将提出的模型模拟结果与两个选定的基准测试进行了比较。对于基准模型1，本文在第一天用所有的钱买股票，最后一天卖出股票，因此利润取决于测试期间的趋势。对于基准模型2，本文假设如果今天的指数高于昨天，那么明天的收盘价指数将高于今天。当预测值上升时，我们就会购买大部分股票。否则，我们就会卖掉我们所有的股票。当股市顺利进行时，这种模型表现良好。但当市场频繁波动或遭受冲击时，它会损失很大。对于本文所提出的模型，作者使用了SVM学习获得的预测结果，交易本金与基准模型2相同，当预测为正时买入，预测为负时卖出。实验结果图如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1637055342781-4461b287-a9a0-48b2-8bf2-ad187bf20d79.png#clientId=u47fe1beb-3eea-4&from=paste&height=327&id=u012dd72e&originHeight=327&originWidth=569&originalType=binary&ratio=1&size=46089&status=done&style=none&taskId=u8142ad38-50a9-4b19-afca-bd6bea436fd&width=569)
由图可知，在大多数时候，本文所提的模型都可以赢得最多利润。平均而言，我们的模型每隔50天就可以获得814.6美元的利润。这就是50天内8%的回报率。因此，我们的年利率可以达到30%左右。除了获得最高的利润，本文提出的模型风险也很低，损失少。
#  5.启示
在该项目中，本文提出使用机器学习算法从全球不同金融市场收集的数据来预测股票指数的走势。本文的发现可以归纳为三个方面：
1.相关性分析表明，美国股市指数与在美国股市开盘前或开盘初收盘的全球市场之间有强大的相互联系。
2.本文提出了各种基于机器学习的模型来预测美国股票的日趋势。数值计算结果表明，计算精度较高
3.一个实际的交易模型建立在我们训练有素的预测器之上。与选定的基准相比，该模型产生了更高的利润。
在以后的研究中，可以重点关注以下几个方面：
第一个方法是探索其他创造性和有效的方法，可能在股市预测方面产生更好的表现。第二，可以修改模型，以处理交易过程中的税费。最后，我们可以研究卖空机制，使市场看涨时利润最大化。
#  6.有用的论点、句子
NASDAQ： <美>全国证券交易商自动报价系统协会
DJIA：<美国>道·琼斯工业指数
MART:Multiple Additive Regression Trees algorithm
（1）In this project, we propose a new prediction algorithm that exploits the temporal correlation among global stock markets and various financial products to predict the next-day stock trend with the aid of SVM.
（在本项目中，我们提出了一种新的预测算法，利用全球股市和各种金融产品之间的时间相关性，借助SVM来预测第二天的股票趋势。）
（2）Popular algorithms, including support vector machine (SVM) and reinforcement learning, have been reported to be quite effective in tracing the stock market and help maximizing the profit of stock option 
purchase while keep the risk low.
（据报道，包括支持向量机(SVM)和强化学习在内的流行算法在跟踪股票市场方面相当有效，并帮助实现股票期权购买的利润最大化，同时保持低风险）
（3）It can be seen from the graph that the autocorrelation of NASDAQ daily trend is only non-zero at the origin based on which we may conclude that the trend of NASDAQ daily index is approximately a Markov process.
（从图中可以看出，纳斯达克日趋势的自相关仅在原点为非零，由此我们可以得出纳斯达克日指数的趋势近似为一个马尔可夫过程。）
