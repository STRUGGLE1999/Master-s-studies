# （金融信号表示与交易的深度直接强化学习）
1. 引用格式
Deng, Yue, et al. "Deep direct reinforcement learning for financial signal representation and
 trading." IEEE transactions on neural networks and learning systems 28.3 (2016): 653-664.
#  2.论文解决的问题
本文主要是想尝试解决一个挑战问题：训练计算机来打败有经验的交易员进行金融交易。最广泛的测试条件下，验证了该神经系统在股票和商品未来市场上的鲁棒性。但是要解决这个问题需要面临两个方面的难题：
（1）金融环境总结和表征方面的难题，因为金融数据包含了大量的噪声、跳跃和变动，导致形成了非常不平稳的时间序列。后面有学者提出用技术分析的方法，但是技术分析方法泛化能力差，所以本文提出一个问题：能否直接从数据中学习更健壮的特征表示，而不是利用预定义的手工特征？
（2）金融交易是一种动态行为。下单交易应该考虑一系列的因素，频繁更换交易策略只会增加交易性成本。所以除当前的市场状况外，历史行动和相应的立场也需要在政策学习部分进行明确的建模。这就又产生了一个问题：在不增加额外复杂性的情况下，我们如何将这些外界因素纳入交易系统呢？
# 3. 采用的思路、技术方法
### 3.1思路
通过从两个与生物相关的学习概念受到的启发，搭建了关于深度学习和强化学习的模型，介绍了一种用于实时金融信号表示和交易的循环深度神经网络。在该框架中，深度学习可以自动感知信息特征学习的动态市场条件，增强学习与深度表征进行交互做出交易决策，积累最终奖励。
### 3.2技术方法
本文参考一篇论文中的DRL模型，用TP定义好了奖励函数，为了有效学习价值函数，本文引入了DRL框架，采用非线性函数来近似每个时间点的交易行动。在特征学习方面，本文引入了深度学习技术到直接强化学习中，同时实现特征学习和动态交易过程。
扩展图如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638687493203-032becc6-0694-4fba-b86a-0381e037dfc6.png#clientId=u64dfc177-c330-4&from=paste&height=261&id=ucdf41c36&originHeight=261&originWidth=614&originalType=binary&ratio=1&size=56138&status=done&style=none&taskId=u37cf789c-7fa8-4ab4-8a1c-8a0f5f6af20&width=614)
在DL的框架上，加入了特征学习形成了深度循环神经网络。交易函数变为了：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638687715998-c8f96a92-6d47-4ae0-a42e-22b2f6cc7aa5.png#clientId=u64dfc177-c330-4&from=paste&height=67&id=u1286a2ce&originHeight=67&originWidth=536&originalType=binary&ratio=1&size=7416&status=done&style=none&taskId=u5b0eb60a-d641-471d-b420-3e6dc0f2904&width=536)
![](https://cdn.nlark.com/yuque/__latex/dd85b9ba4cfd256fe1bffec352510fa8.svg#card=math&code=F_%7Bt%7D%3Dg_%7Bd%7D%28f_%7Bt%7D%29&id=iLjQ9)表示通过DNN结构和非线性映射![](https://cdn.nlark.com/yuque/__latex/0f6c6e848c0585ae2fd31ba39f12a366.svg#card=math&code=g_%7Bd%7D%28%29&id=f5obh)将输入向量![](https://cdn.nlark.com/yuque/__latex/93891b8532950353ca2314573419fcaa.svg#card=math&code=f_%7Bt%7D&id=TO75Z)分层变换得到。本文将深度变换部分中的隐藏层设置为4，每个隐藏层的节点数固定为128。
由于金融序列包括大量的不可预测和不确定的数据，而且受很大因素的影响，所以为了减少数据中的不确定性，减少金融信号的鲁棒性，本文利用人工智能领域的模糊学习法
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638688685080-5d9625ab-24bc-42b7-9803-17d3f5c265c0.png#clientId=u64dfc177-c330-4&from=paste&height=323&id=u78ea0f7e&originHeight=323&originWidth=608&originalType=binary&ratio=1&size=106383&status=done&style=none&taskId=ue0f9a240-07b6-49f5-914a-a6e9dcbd333&width=608)
本文为输入向量的每个维度分配k个不同的模糊度，如图所示，由于空间限制。每个输入变量只连接了两个模糊节点，也就是k=2。本文实验中把k设置为3,使用模糊函数的DRNN由模糊表示、深度变换、和DRT三部分组成，这三部分结构分别发挥了数据预处理（减少不确定性）、特征学习（深度转换）和交易政策制定（RL）的作用。
为了更好的训练DNN，本文使用了系统初始化和微调两个步骤：
（1）系统初始化
本文主要有三个学习部分需要初始化，第一是模糊表征部分，需要指定模糊节点中心和宽度。第二是深度转换层，使用自编码来初始化，在每个隐藏层使用自编码技术，直到所有的参数都设置好。第三是DRL部分，可以使用最终的深度表征![](https://cdn.nlark.com/yuque/__latex/d1e61ee20ebe54f5af17ee2e25a5c1e5.svg#card=math&code=F_%7Bt%7D&id=bPBot)作为模型的输入进行初始化。
（2）微调

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638758504028-8b066d33-a5de-449d-8eef-9ac92326a9db.png#clientId=u743aca3c-2e95-4&from=paste&height=341&id=uf3fadcad&originHeight=341&originWidth=625&originalType=binary&ratio=1&size=145572&status=done&style=none&taskId=uf7834387-dc6f-4f26-be5c-a512c4ec843&width=625)
本文作者为了简化梯度推导问题，引入了BPTT方法。用于RDNN微调的任务感知BPTT如上，展示了FRDNN的前两步展开，可以看到，从输出到输入端有链路，即用![](https://cdn.nlark.com/yuque/__latex/863dd6d91b17b46f97cbc6037a613374.svg#card=math&code=%5Cdelta%20_%7Bt-1%7D%E4%BD%9C%E4%B8%BA%E8%BE%93%E5%85%A5%E6%9D%A5%E8%AE%A1%E7%AE%97%5Cdelta_%7Bt%7D&id=svJHZ),每个![](https://cdn.nlark.com/yuque/__latex/a6f317b268ae825d94f832f970af607c.svg#card=math&code=%5Ctau%20&id=McyvV)值表示一个时间堆栈。这样一来，整个系统都不含循环结构，这种设置是为了避免深层上的梯度消失，有了这种设置之后，每个时间栈的反向梯度传播分别来自两个部分：前一个时间栈的信息和奖励函数。下面是作者训练FRDNN的算法：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638760073649-844aef8f-fb78-4c0f-aceb-94e0443b4555.png#clientId=u743aca3c-2e95-4&from=paste&height=631&id=uecb6208c&originHeight=631&originWidth=600&originalType=binary&ratio=1&size=117797&status=done&style=none&taskId=u1bf2206f-6626-4d84-b0f0-d03f2ec375f&width=600)
# 4. 解决的效果
### 4.1实验数据
本文的实验数据测试了股票指数和商品期货合约。对于股票指数数据，作者选择了股票IF合同，是中国首个基于指数的期货合约交易，在商品期货合约上，作者选择了AG和SU合约。本文的输入是近45min的价格变化和前3h、5h、1天、3天、10天的趋势变化，每50个输入节点与3个模糊成员函数连接，获得一级模糊表示，接着模糊层依次通过4个深度转换层，每层有128/128/128/20个隐藏节点，最后的深层表征层和DRL部分相连，用于交易策略的制定。本文在训练RDNN模型时遇到的两个难题是：过度拟合和梯度下降，为了解决过度拟合问题，本文将数据集分为测试集和验证集，然后在验证集实验过程中选出最好的一个；为了解决梯度消失，本文引入了任务感知BPTT方法。
### 4.2模型比较
本文根据实际数据来评估DDR交易系统，除此之外，还比较了其他深度学习的在线交易系统，下图是量化评估，测试性能以TP和SR的形式报告，还有买入和持有的表现：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638862936797-0cfd754e-f020-451b-ba1b-822e8156e3cb.png#clientId=ude210bb8-8f2c-4&from=paste&height=254&id=ued25827c&originHeight=254&originWidth=993&originalType=binary&ratio=1&size=47328&status=done&style=none&taskId=uce75fc0c-7e52-4bd4-98c1-6ba706b776d&width=993)

由表格可以看出，FDDR和DDR在所有市场情况下都比其他两个模型获得的利润和夏普比率好，这一观察结果验证了特征学习确实有助于提高交易性能，可能是因为本文提出的两种模型加了一层额外的模糊表示层。
作者得出的结论是，RL框架可能是一种基于趋势的交易策略，可以在价格波动较大（无论在哪个方向）的市场上获得可靠的利润。
本文还将提出的模型和基于预测的DNN进行比较，三个主要的比较模型是卷积DNN、RNN、LSTM，本文对这些模型采用了在EDDR中相同的训练和测试策略。如果一个方向的预测概率高于0.6,则可以识别出这类信号，下表是不同模型的PR、TT、以及不同交易成本的TPs，
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638932972620-08d17cf7-62db-4ed6-a462-74b2619f696c.png#clientId=u53963c3e-b2b8-4&from=paste&height=199&id=u5013de64&originHeight=199&originWidth=473&originalType=binary&ratio=1&size=26352&status=done&style=none&taskId=uddd0cdc6-5f01-40dd-be90-fc6a8c3ab5b&width=473)
可以看出所有模型的PRs都不是很高，仅仅比50%左右，说明了在高度复杂的金融市场中，很难准确预测价格变动。但是我们可以注意到，尽管只有这么低的PR，在成本为0时，这些模型依然可以获得很高的利润。
### 4.3对全球市的验证
本文从雅虎金融上爬取了1990年1月到2015年9月标准普尔指数的日分辨率历史数据：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1639030625170-994b6cbc-3dc3-4556-a295-326c13bc33d8.png#clientId=u0ee74fc1-cd51-4&from=paste&height=265&id=u496d230a&originHeight=265&originWidth=528&originalType=binary&ratio=1&size=73933&status=done&style=none&taskId=u5dedd337-26bb-4829-9680-d6f9ea7e6b1&width=528)
由表可以看出，本文提出的FDDR适用于每日标准普尔指数，并且与其他两种模型相比，利润可以获得更多。从图还可以看出，在2800天之前，FDDR要优于Multi-FDDR，而在那之后，Multi-FDDR性能更差，这可能是因为在2010年之后，有更多的算法交易公司进入了市场。
本文还对所提模型进行了健壮性检验，实验了深度学习层数为3和5的模型对比，还有每一层的节点数设置了64、128、256的比较，最后，在每个测试级别，样本外性能和相应的训练复杂性如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1639032262035-d69898ac-71b4-4ac0-b808-74a4bb21cc9e.png#clientId=u0ee74fc1-cd51-4&from=paste&height=251&id=ue55e098e&originHeight=251&originWidth=810&originalType=binary&ratio=1&size=43332&status=done&style=none&taskId=ufc19a081-e972-4d73-bd65-0abb8ae9fba&width=810)
由结果可以看出，增加节点数量会导致计算成本增加，而且从128节点数增加到256节点数对于性能提升帮助不大。另一种是通过增加深度神经网络的深度，从实验结果可以看出，神经网络的层数对特征学习很重要。TP和计算成本几乎是成正相关的，本文提出的模型在5层，256个节点时性能最好，但是成本也是最高的。
#  5.启示
本文将深度学习技术运用到直接强化学习框架中，提出了FDDR模型，通过对模型的比较，在全球市场的应用，以及稳健性测试，验证了所提模型的有效性。本文认为所提的模型贡献主要体现在：这是一个无技术指标的交易系统，帮助人们从大量候选人中选择特征，第二，本文还将模糊学习嵌入到深度模型中，减少了原始事件序列的不确定性。最后的股票指数和商品期货合约的实验结果表明作者提出的模型的有效性。本文在以下两个方面还有改进：第一，本文所提的方法只处理了一种股票资产,在以后的工作中，可以扩展DL框架，处理多个资产。第二，金融市场并不是静止不变的，是动态的过程，但是从过去的训练数据中学到的知识不能充分反映后续测试期间的信息，这要求在以后的实验中能够智能选择正确的训练周期方法。
#  6.有用的论点、句子
（1）Hence, we propose a task-aware backpropagation through time method to cope with the gradient vanishing issue -+`		in deep training.
（因此，我们提出了一种基于任务感知的时间反向传播方法来解决深度训练中的梯度消失问题。）
（2）In addressing the aforementioned two questions, in this paper, we introduce a novel RDNN structure for simultaneous environment sensing and recurrent decision making for online fifinancial assert trading.
（为了解决上述两个问题，本文介绍了一种新的RDNN结构，用于网上金融资产交易的同时进行环境感知和循环决策。）
（3）While the DL has shown great promises in many signal processing problems as image and speech recognitions, to the best of our knowledge, this is the fifirst paper to implement DL in designing a real trading system for fifinancial signal representation and self-taught reinforcement trading.
（虽然DL在图像和语音识别等信号处理问题上显示出了巨大的前景，但据我们所知，这是一篇实现DL设计真实的金融信号表示和自学强化交易交易系统的论文。）
（4）In conclusion, the RL framework is perhaps a trend-based trading strategy and could make reliable profifits on the markets with large price movement (no matter in which direction).
（总之，RL框架可能是一种基于趋势的交易策略，可以在价格波动较大（无论在哪个方向）的市场上获得可靠的利润。）
（5）By incorporating the fuzzy learning concept into the system, the FDDR can even generate good results in the nontrending period.
（通过将模糊学习概念纳入系统，FDDR甚至可以在非趋势期内产生良好的结果。）
（6）This paper introduces the contemporary DL into a typical DRL framework for fifinancial signal processing and online trading.
（本文将当代DL引入到一个典型的金融信号处理和在线交易的DRL框架中。）


![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638255735873-ed68f965-e9ba-4576-b048-e628c33d6bd2.png#clientId=ua55de097-7083-4&from=paste&height=407&id=udb560753&originHeight=407&originWidth=739&originalType=binary&ratio=1&size=85167&status=done&style=none&taskId=u36c5d31b-74c2-486b-9873-213fcdcd614&width=739)
DRT:直接增强交易
FRDNN:模糊RDNN
CNY/pnt:China Yuan per point
SCOT:受稀疏编码启发的最优训练
B&H：buying and holding 
P&L：Profit and Loss
SR:夏普比率，可以平衡利润和风险。
PR：profitable rate
TTs：trading times


实时交易决定![](https://cdn.nlark.com/yuque/__latex/5dfe924ad154e9c7b754f269953a20f2.svg#card=math&code=%5Cdelta%20_%7Bt%7D%5Cvarepsilon%20&id=i3ImG){long,neutral,short}={1,0,-1}
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638684881181-5ccbbe0f-8678-4632-8ca6-4bd01602cb83.png#clientId=u64dfc177-c330-4&from=paste&height=64&id=u2ab28663&originHeight=64&originWidth=594&originalType=binary&ratio=1&size=5115&status=done&style=none&taskId=u5491d553-3d15-4e67-9e2c-3b0ef645f3a&width=594)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638686071809-1e0cab55-0f7e-483b-9a4f-ae2861ebbe10.png#clientId=u64dfc177-c330-4&from=paste&height=61&id=u4b35fce0&originHeight=61&originWidth=488&originalType=binary&ratio=1&size=7394&status=done&style=none&taskId=u285b15ae-44e1-4402-a82f-1251675d3c4&width=488)
![](https://cdn.nlark.com/yuque/__latex/93891b8532950353ca2314573419fcaa.svg#card=math&code=f_%7Bt%7D&id=eeJfs)为当前市场决定的特征向量，w和b为特征回归的系数。tanh()函数将价值函数映射到（-1,1）的范围内，以近似于最终的交易决策。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638688278396-3b00ed19-c4d7-4274-8e54-e4ca9e04731e.png#clientId=u64dfc177-c330-4&from=paste&height=88&id=u03c7ee32&originHeight=88&originWidth=480&originalType=binary&ratio=1&size=7274&status=done&style=none&taskId=u48e9f007-d210-4901-9d81-c1561b63b38&width=480)
![](https://cdn.nlark.com/yuque/__latex/5e57efcfa4a338fcdb1e7b1cf132a0d4.svg#card=math&code=a_%7Bi%7D%5E%7Bl%7D&id=jqxvn)为第l层第i个节点的输入，![](https://cdn.nlark.com/yuque/__latex/1bedf09550d2abdfd50350248eabc210.svg#card=math&code=o_%7Bi%7D%5E%7Bl%7D&id=bFueI)为第l层第i个节点 的输出。
优化函数框架：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638689264909-33635cfb-660d-4c55-b922-44980c4f15d5.png#clientId=u64dfc177-c330-4&from=paste&height=173&id=u9783c318&originHeight=173&originWidth=478&originalType=binary&ratio=1&size=19118&status=done&style=none&taskId=uf2520d87-44f4-4764-956d-f61c0cc236d&width=478)
![](https://cdn.nlark.com/yuque/__latex/c0a176b733dacc04a6c548d5111445ea.svg#card=math&code=U_%7Bt%7D%E6%98%AFRL%E5%8A%9F%E8%83%BD%E7%9A%84%E6%9C%80%E7%BB%88%E5%A5%96%E5%8A%B1%E5%87%BD%E6%95%B0%EF%BC%8C%5Cdelta%20_%7Bt%7D%E6%98%AF%E7%94%B1FRDNN%E8%BF%91%E4%BC%BC%E5%8C%96%E7%AD%96%E7%95%A5%EF%BC%8C%E8%80%8CF_%7Bt%7D%E6%98%AF%E7%94%B1DL%E4%BA%A7%E7%94%9F%E7%9A%84%E5%BD%93%E5%89%8D%E5%B8%82%E5%9C%BA%E7%8A%B6%E5%86%B5%E7%9A%84%E9%AB%98%E7%BB%B4%E7%89%B9%E5%BE%81%E8%A1%A8%E7%A4%BA&id=JDe7N)

自编码优化函数最小化损失：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1638757694434-d28fbcc2-7e88-4bbc-8bfd-800f5bb7ee0f.png#clientId=u743aca3c-2e95-4&from=paste&height=82&id=u029591e5&originHeight=82&originWidth=502&originalType=binary&ratio=1&size=8990&status=done&style=none&taskId=u38af73a0-46d7-470b-bee1-110dda85d2b&width=502)
![](https://cdn.nlark.com/yuque/__latex/f9cd7e80e5882a1c60820dfcaab5998a.svg#card=math&code=h_%7B%5CTheta%20%7D%28%29%E5%92%8Ch_%7B%5Cgamma%20%7D%28%29&id=DPS52)是从l层到l+1层或者从l+1到l+2层的前馈变换。![](https://cdn.nlark.com/yuque/__latex/2f26936323c2a215b6e065d1b038b0a2.svg#card=math&code=X_%7Bt%7D%5E%7B%28l%29%7D&id=NiqIU)是第t个输入的训练样本的第l层节点状态。每一项都加了平方是为了避免过拟合现象。


[Deng et al_2017_Deep Direct Reinforcement Learning for Financial Signal Representation and.pdf](https://www.yuque.com/attachments/yuque/0/2021/pdf/22838017/1639185670851-85ea60d1-3d94-492c-8a4a-22e7f3a8bd6d.pdf)

