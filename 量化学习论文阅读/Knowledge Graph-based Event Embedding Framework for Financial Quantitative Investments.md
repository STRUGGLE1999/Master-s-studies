# 1.引用格式
Dawei Cheng, Fangzhou Yang, Xiaoyang Wang, Ying Zhang, and Liqing Zhang. 2020. Knowledge Graph-based Event Embedding Framework for Financial Quantitative Investments. In <i>Proceedings of the 43rd International ACM SIGIR Conference on Research and Development in Information Retrieval</i> (<i>SIGIR '20</i>). Association for Computing Machinery, New York, NY, USA, 2221–2230. DOI:https://doi. org/10.1145/3397271.340142

SIGIR是什么意思
#  2.论文解决的问题
1、现有的工作在事件元组中使用样本特征，例如词袋，命名实体，不能捕捉到他们内在的信息。
2、后来有学者提出运用open IE获取结构化事件特征，但是这种方法增加了事件向量的稀疏性，限制预测能力。
3、再后面，学者提出使用事件嵌入推断结构化事件。
4、为了解决“两个具有相似嵌入的事件可能是不相关的”，研究者提出将知识图谱的外部信息整合到事件嵌入的学习过程。

由于缺乏对金融系统的专家级知识和无法获得带注释的训练数据，从超前滞后关系中有效地学习事件嵌入是具有挑战性的，因此本文在基于这个背景之下，提出将超前滞后关系的知识融入到一个新的基于知识图谱事件嵌入框架（KGEEF）中，以进行量化投资。


#  3.采用的思路、技术方法
首先，从原始数据中抽取结构化事件，同时用上述实体和关系构建知识图谱；
然后，利用一个联合模型将知识图谱信息合并到一个事件嵌入学习模型的目标函数中。
本文提出将超前滞后关系纳入一个新的基于知识图谱的事件嵌入框架中，以进行量化投资。具体步骤为：
从原始新闻文本中抽取结构化关系和事件元组，将复杂的关系和属性知识存储在一个金融知识图谱中，其中分类顶点表示属性实体，边对应于实体之间的关系。利用关系和属性知识理解实体间的超前滞后关系。本文提出了一个连贯的模型将知识图谱信息联合嵌入到事件嵌入学习框架中。
## 3.1框架概述
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634542354698-e731e01d-ab08-4d59-b29e-5294c2207a51.png#clientId=ua6c7f7d7-c75f-4&from=paste&height=472&id=uf86f9779&originHeight=472&originWidth=929&originalType=binary&ratio=1&size=279728&status=done&style=none&taskId=u047dffe8-3b78-4a27-a475-299f884f08b&width=929)
上图是本文提出的用于量化投资的事件嵌入框架整体架构。这个模式包括三个部分：
1）Multi-source input layer  由事件元组、金融知识图谱、关系元组组成。将原始文本转换成事件元组，关系元组和知识图谱。使用OpenIE 5.1版本抽取事件元组，对于关系识别，本文提出了一种顺序学习模型检测新闻文本中的实体关系，关系元组表示为(e1、R、e2)，其中e1表示头实体，R为关系类型，e2表示尾实体，然后利用实体链接在知识图谱中存储关系元组。
2）Event representative learning netwok    将知识图谱中的事件元组，关系元组和节点嵌入进行预训练，然后作为输入，然后利用多源注意网络进行高维表征学习，将学习到的每个原始文本数据的潜在特征作为输出。
3）Detection and optimization module    将前面的实体、事件和图保存到信息作为输入，用来推断它是真实事件或关系的概率。本文将事件和关系损失进行联合优化来训练模型，文中提出的框架保留了事件直接影响和超前滞后效应对股票的影响。
## 3.2关系检测与知识图谱的构建
本文首先将单词表示为低维特征，对于给定的一个带有m个单词的句子x,每个单词wi都由BERT预训练的表示。然后，本文采用双向的长短时记忆网络从两个方向学习单词的信息。双向LSTM包括一个前向的![](https://cdn.nlark.com/yuque/__latex/8e84dbd86c4d56ab0caa6195c7b77544.svg#card=math&code=%5Coverrightarrow%7BLSTM%7D&id=mAH35)和一个后向的![](data:image/svg+xml;utf8,%3Csvg%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20width%3D%227.478ex%22%20height%3D%223.843ex%22%20style%3D%22vertical-align%3A%20-0.338ex%3B%20margin-top%3A%20-0.398ex%3B%22%20viewBox%3D%220%20-1508.9%203219.7%201654.5%22%20role%3D%22img%22%20focusable%3D%22false%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20aria-labelledby%3D%22MathJax-SVG-1-Title%22%3E%0A%3Ctitle%20id%3D%22MathJax-SVG-1-Title%22%3E%5Coverleftarrow%7BLSTM%7D%3C%2Ftitle%3E%0A%3Cdefs%20aria-hidden%3D%22true%22%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-4C%22%20d%3D%22M228%20637Q194%20637%20192%20641Q191%20643%20191%20649Q191%20673%20202%20682Q204%20683%20217%20683Q271%20680%20344%20680Q485%20680%20506%20683H518Q524%20677%20524%20674T522%20656Q517%20641%20513%20637H475Q406%20636%20394%20628Q387%20624%20380%20600T313%20336Q297%20271%20279%20198T252%2088L243%2052Q243%2048%20252%2048T311%2046H328Q360%2046%20379%2047T428%2054T478%2072T522%20106T564%20161Q580%20191%20594%20228T611%20270Q616%20273%20628%20273H641Q647%20264%20647%20262T627%20203T583%2083T557%209Q555%204%20553%203T537%200T494%20-1Q483%20-1%20418%20-1T294%200H116Q32%200%2032%2010Q32%2017%2034%2024Q39%2043%2044%2045Q48%2046%2059%2046H65Q92%2046%20125%2049Q139%2052%20144%2061Q147%2065%20216%20339T285%20628Q285%20635%20228%20637Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-53%22%20d%3D%22M308%2024Q367%2024%20416%2076T466%20197Q466%20260%20414%20284Q308%20311%20278%20321T236%20341Q176%20383%20176%20462Q176%20523%20208%20573T273%20648Q302%20673%20343%20688T407%20704H418H425Q521%20704%20564%20640Q565%20640%20577%20653T603%20682T623%20704Q624%20704%20627%20704T632%20705Q645%20705%20645%20698T617%20577T585%20459T569%20456Q549%20456%20549%20465Q549%20471%20550%20475Q550%20478%20551%20494T553%20520Q553%20554%20544%20579T526%20616T501%20641Q465%20662%20419%20662Q362%20662%20313%20616T263%20510Q263%20480%20278%20458T319%20427Q323%20425%20389%20408T456%20390Q490%20379%20522%20342T554%20242Q554%20216%20546%20186Q541%20164%20528%20137T492%2078T426%2018T332%20-20Q320%20-22%20298%20-22Q199%20-22%20144%2033L134%2044L106%2013Q83%20-14%2078%20-18T65%20-22Q52%20-22%2052%20-14Q52%20-11%20110%20221Q112%20227%20130%20227H143Q149%20221%20149%20216Q149%20214%20148%20207T144%20186T142%20153Q144%20114%20160%2087T203%2047T255%2029T308%2024Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-54%22%20d%3D%22M40%20437Q21%20437%2021%20445Q21%20450%2037%20501T71%20602L88%20651Q93%20669%20101%20677H569H659Q691%20677%20697%20676T704%20667Q704%20661%20687%20553T668%20444Q668%20437%20649%20437Q640%20437%20637%20437T631%20442L629%20445Q629%20451%20635%20490T641%20551Q641%20586%20628%20604T573%20629Q568%20630%20515%20631Q469%20631%20457%20630T439%20622Q438%20621%20368%20343T298%2060Q298%2048%20386%2046Q418%2046%20427%2045T436%2036Q436%2031%20433%2022Q429%204%20424%201L422%200Q419%200%20415%200Q410%200%20363%201T228%202Q99%202%2064%200H49Q43%206%2043%209T45%2027Q49%2040%2055%2046H83H94Q174%2046%20189%2055Q190%2056%20191%2056Q196%2059%20201%2076T241%20233Q258%20301%20269%20344Q339%20619%20339%20625Q339%20630%20310%20630H279Q212%20630%20191%20624Q146%20614%20121%20583T67%20467Q60%20445%2057%20441T43%20437H40Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-4D%22%20d%3D%22M289%20629Q289%20635%20232%20637Q208%20637%20201%20638T194%20648Q194%20649%20196%20659Q197%20662%20198%20666T199%20671T201%20676T203%20679T207%20681T212%20683T220%20683T232%20684Q238%20684%20262%20684T307%20683Q386%20683%20398%20683T414%20678Q415%20674%20451%20396L487%20117L510%20154Q534%20190%20574%20254T662%20394Q837%20673%20839%20675Q840%20676%20842%20678T846%20681L852%20683H948Q965%20683%20988%20683T1017%20684Q1051%20684%201051%20673Q1051%20668%201048%20656T1045%20643Q1041%20637%201008%20637Q968%20636%20957%20634T939%20623Q936%20618%20867%20340T797%2059Q797%2055%20798%2054T805%2050T822%2048T855%2046H886Q892%2037%20892%2035Q892%2019%20885%205Q880%200%20869%200Q864%200%20828%201T736%202Q675%202%20644%202T609%201Q592%201%20592%2011Q592%2013%20594%2025Q598%2041%20602%2043T625%2046Q652%2046%20685%2049Q699%2052%20704%2061Q706%2065%20742%20207T813%20490T848%20631L654%20322Q458%2010%20453%205Q451%204%20449%203Q444%200%20433%200Q418%200%20415%207Q413%2011%20374%20317L335%20624L267%20354Q200%2088%20200%2079Q206%2046%20272%2046H282Q288%2041%20289%2037T286%2019Q282%203%20278%201Q274%200%20267%200Q265%200%20255%200T221%201T157%202Q127%202%2095%201T58%200Q43%200%2039%202T35%2011Q35%2013%2038%2025T43%2040Q45%2046%2065%2046Q135%2046%20154%2086Q158%2092%20223%20354T289%20629Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-2190%22%20d%3D%22M944%20261T944%20250T929%20230H165Q167%20228%20182%20216T211%20189T244%20152T277%2096T303%2025Q308%207%20308%200Q308%20-11%20288%20-11Q281%20-11%20278%20-11T272%20-7T267%202T263%2021Q245%2094%20195%20151T73%20236Q58%20242%2055%20247Q55%20254%2059%20257T73%20264Q121%20283%20158%20314T215%20375T247%20434T264%20480L267%20497Q269%20503%20270%20505T275%20509T288%20511Q308%20511%20308%20500Q308%20493%20303%20475Q293%20438%20278%20406T246%20352T215%20315T185%20287T165%20270H929Q944%20261%20944%20250Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-2212%22%20d%3D%22M84%20237T84%20250T98%20270H679Q694%20262%20694%20250T679%20230H98Q84%20237%2084%20250Z%22%3E%3C%2Fpath%3E%0A%3C%2Fdefs%3E%0A%3Cg%20stroke%3D%22currentColor%22%20fill%3D%22currentColor%22%20stroke-width%3D%220%22%20transform%3D%22matrix(1%200%200%20-1%200%200)%22%20aria-hidden%3D%22true%22%3E%0A%3Cg%20transform%3D%22translate(42%2C0)%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMATHI-4C%22%20x%3D%220%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMATHI-53%22%20x%3D%22681%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMATHI-54%22%20x%3D%221327%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMATHI-4D%22%20x%3D%222031%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%3Cg%20transform%3D%22translate(52%2C825)%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-2190%22%20x%3D%22-56%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3Cg%20transform%3D%22translate(478.01967213114756%2C0)%20scale(3.088524590163934%2C1)%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-2212%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-2212%22%20x%3D%222388%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%3C%2Fg%3E%0A%3C%2Fsvg%3E#card=math&code=%5Coverleftarrow%7BLSTM%7D&id=iYsUD)。通过将正向隐藏状态![](https://cdn.nlark.com/yuque/__latex/b3b7bbb85b937ef9ba1b13c7f526dc2b.svg#card=math&code=%5Coverrightarrow%7Bh_%7Bi%7D%7D&id=Q3Wtu)和和反向隐藏状态![](https://cdn.nlark.com/yuque/__latex/f0f5dcaf1441591eee266574ef66b140.svg#card=math&code=%5Coverleftarrow%7Bh_%7Bi%7D%7D&id=pU7bd)连接，可以从一个给定的![](https://cdn.nlark.com/yuque/__latex/dfc96001568235d60dc12dca4b32d260.svg#card=math&code=w_%7Bi%7D&id=BXcmp)中得到隐藏向量![](https://cdn.nlark.com/yuque/__latex/914767ffc65896751e16a5b9fbf3fc4a.svg#card=math&code=h_%7Bi%7D&id=AajxK)。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634552736511-8ab749a5-3318-46fc-b2e1-daa13a4e98b6.png#clientId=u635f0903-f021-4&from=paste&height=101&id=u17e3459f&originHeight=101&originWidth=291&originalType=binary&ratio=1&size=8780&status=done&style=none&taskId=u9dba9651-97f6-4ac8-8617-2188fc147d5&width=291)
除此之外，并不是所有的单词对关系提取的表示的贡献都是相同的，比如一些定语、状语、补语。本文还引入了多头注意机制学习单词对关系理解的不同重要性，并将信息性单词的表示聚合为：
![](https://cdn.nlark.com/yuque/__latex/202f747ea395286b28e8d0ef1d493d17.svg#card=math&code=%5Calpha%20_%7Bi%7D%3D%5Cfrac%7Bexp%28%5Csigma%20%28W_%7Brh%7Dh_%7Bi%7D%2Bb_%7Brh%7D%29%29%7D%7B%5Csum_%7Bm%7D%5E%7Bj%3D1%7Dexp%28%5Csigma%20%28W_%7Brh%7Dh_%7Bj%7D%2Bb_%7Brh%7D%29%29%7D&id=CQHe6)
其中，
![](https://cdn.nlark.com/yuque/__latex/6d46fae76afc4cc1ddea6b49c51c4df4.svg#card=math&code=W_%7Brh%7D&id=DIruW)和![](https://cdn.nlark.com/yuque/__latex/09e2244d46f0454398f1cd1cd5991e64.svg#card=math&code=b_%7Brh%7D&id=XEltc)代表权重和偏差，![](https://cdn.nlark.com/yuque/__latex/90db017b80d63780533fbc74fb227dba.svg#card=math&code=%5Calpha%20_%7Bi%7D&id=lNtv1)代表![](https://cdn.nlark.com/yuque/__latex/dfc96001568235d60dc12dca4b32d260.svg#card=math&code=w_%7Bi%7D&id=ylfbU)的注意力权重。
最后通过softmax层计算关系预测的条件概率![](https://cdn.nlark.com/yuque/__latex/645fabc4e9b2b8e90d136a3689b0ab13.svg#card=math&code=p%28r%7CS%2C%5Ctheta%20_%7Br%7D%29&id=keTTp)，计算公式为：
![](https://cdn.nlark.com/yuque/__latex/2218c9e14082e8e576642f3d36cd8231.svg#card=math&code=p%28r%7CS%2C%5Ctheta%20_%7Br%7D%29%3D%5Cfrac%7Bexp%28W_%7Bro%7Dh_%7BR%7D%5E%7B%27%7D%2Bb_%7Bro%7D%29%7D%7B%5Csum_%7Bk%3D1%7D%5E%7BnR%7Dexp%28W_%7Bro%7Dh%5E%7B%27%7D_%7Bk%7D%2Bb_%7Bro%7D%29%7D&id=dc0mo)
其中，S表示句子集，![](https://cdn.nlark.com/yuque/__latex/95a7c3749591e6b3b9a3959087614468.svg#card=math&code=%5Ctheta%20_%7BR%7D&id=YAG6A)表示关系抽取网络的参数，![](https://cdn.nlark.com/yuque/__latex/b02afbdb550ede42564f60dddb1eb70e.svg#card=math&code=n_%7BR%7D&id=zjXjc)表示总的关系数量，![](https://cdn.nlark.com/yuque/__latex/9f50b0453c2d13aaf37e6c4105466e1c.svg#card=math&code=W_%7Bro%7D%2Cb_%7Bro%7D&id=umzN8)分别表示输出层的权重和偏差。
下图展示了关系检测网络的的总体架构，通过在设定的水平上引入一个交叉熵目标函数来学习和优化模型。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634554647396-58880d2d-7385-4584-a76c-6104bdb6950c.png#clientId=u635f0903-f021-4&from=paste&height=375&id=u6080572a&originHeight=375&originWidth=423&originalType=binary&ratio=1&size=84028&status=done&style=none&taskId=u85c56a15-d616-4123-bfec-9fe7b36472a&width=423)

在构建网络架构的过程中，本文使用Adam算法作为优化器，并利用输出层上的dropout来防止过拟合。
## 3.3事件表征学习
### 3.3.1事件嵌入
对于给定的事件元组 E=（A,P,O），本文采用神经张量作为事件层，双线性张量被用来模拟主语和谓语之间的关系，以及宾语和谓语之间的关系，神经张量的输入是A,P,O的预训练d维嵌入，输出是潜在事件嵌入。
对于事件学习，本文假设训练语料库中的真实事件得分应该高于损坏事件，
用字典中的随机参数![](https://cdn.nlark.com/yuque/__latex/168d0dd00d21d6b28e0f2e324099dd58.svg#card=math&code=A_%7Bc%7D&id=gnCj1)或![](https://cdn.nlark.com/yuque/__latex/f33bb73cef6499b7756fd4631a23c8ed.svg#card=math&code=O_%7Bc%7D&id=S1ZCZ)替换A或O，构建损坏事件![](https://cdn.nlark.com/yuque/__latex/aa2ad04b74da0a58411672fa526c4aef.svg#card=math&code=E_%7Bc%7D%3D%28A_%7Bc%7D%2CP%2CO%29&id=Z2EhH)或![](https://cdn.nlark.com/yuque/__latex/decc8c68c19ad1f6c246eff8ad3d114d.svg#card=math&code=E_%7Bc%7D%3D%28A%2CP%2CO_%7Bc%7D%29&id=FaEWk)损失函数计算为：![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634556400222-77ca0e36-0d80-43d7-bc05-68516bc5fe75.png#clientId=u635f0903-f021-4&from=paste&height=54&id=u3da78d74&originHeight=54&originWidth=365&originalType=binary&ratio=1&size=7037&status=done&style=none&taskId=ued6bd00b-8840-4de4-8368-fa776b8c86a&width=365)
其中![](https://cdn.nlark.com/yuque/__latex/014779bd27040547475be2a5d4d7af3e.svg#card=math&code=%5Ctheta%20_%7Be%7D&id=jBiTk)是事件神经网络中应该训练的参数，![](https://cdn.nlark.com/yuque/__latex/199ae1e1aa88dcb24534dbbf887fd983.svg#card=math&code=%5Clambda%20_%7Be%7D&id=FICeu)是![](https://cdn.nlark.com/yuque/__latex/014779bd27040547475be2a5d4d7af3e.svg#card=math&code=%5Ctheta%20_%7Be%7D&id=PGWPc)中归一化函数的惩罚参数，通过交叉验证确定。![](https://cdn.nlark.com/yuque/__latex/aa40858b3b39e82656164d7719f9b312.svg#card=math&code=N_%7Be%7D&id=iYpPE)和![](https://cdn.nlark.com/yuque/__latex/bb435c2495b9f176c9271776e905c626.svg#card=math&code=Nce&id=IYsxd)分别是训练事件的数量和损坏事件的数量，![](https://cdn.nlark.com/yuque/__latex/b94b7384d8e7063e90ab37d62e0f5618.svg#card=math&code=f_%7Be%7D&id=hc0g5)表示事件网络的转换。
### 3.3.2 关系嵌入
对于3.2节中模型检测到的关系（e1,R,e2），采用关系模型嵌入来学习两个实体（e1,e2）是否处于一定的关系R之中。本文实验中，关系类型是有限的，因此张量的使用可以提高关系网络的表达能力，该模型通过以下公式推断出两个实体具有一定的可能性：
![](https://cdn.nlark.com/yuque/__latex/68120946701a2f9070ed98f4c2936f47.svg#card=math&code=f_%7Br%7D%3DNN_%7Br%7D%28u_%7Br%7D%5E%7BT%7Dtanh%28e_%7B1%7D%5E%7BT%7DH_%7Br%7D%5E%7B%5B1%3Ak%5D%7De_%7B2%7D%2BW_%7Br%7D%5Cbinom%7Be_%7B1%7D%7D%7Be_%7B2%7D%7D%2Bb_%7Br%7D%29%29&id=GDIHa)
![](https://cdn.nlark.com/yuque/__latex/0bf88e8d88ca8d43d187523d0c666d03.svg#card=math&code=f_%7Br%7D&id=cGYCG)表示关系嵌入神经网络的转换，![](https://cdn.nlark.com/yuque/__latex/a8da103a7dc18fbd7d0cdfd8adabf991.svg#card=math&code=NN_%7Br%7D&id=hOO0c)是将ReLU作为激活函数的前馈神经网络，tanh应用于元素方面，![](https://cdn.nlark.com/yuque/__latex/d3ac03e3d727a217199589ad3c7bb29e.svg#card=math&code=H_%7Br%7D%5E%7B1%3Ak%7D%5Cin%20RR%5E%7Bd%2Ad%2Ad%7D&id=igtV5)是一个包含维度为d*d的k阶矩阵张量，张量积![](https://cdn.nlark.com/yuque/__latex/7b8254277a8c1c744d47d3633dffaf3f.svg#card=math&code=e_%7B1%7D%5E%7BT%7DH_%7Br%7D%5E%7B%5B1%3Ak%5D%7D&id=mBI9q)的输出是![](https://cdn.nlark.com/yuque/__latex/9503880d3140eba78084ba72a7ca208f.svg#card=math&code=R%5E%7Bk%7D&id=g7who)中的一个向量。
接着构造损坏元组的关系，记为![](https://cdn.nlark.com/yuque/__latex/ad975654c1ebbfd41770bcd64832d16b.svg#card=math&code=R_%7Bc%7D&id=mZyCN)=(![](https://cdn.nlark.com/yuque/__latex/dd843363b3b675c5c9aa16c18c3f6cc1.svg#card=math&code=e_%7Bc1%2CR_%7Bc%7D%2Ce_%7Bc2%7D%7D&id=tt2hR)),将字典中的![](https://cdn.nlark.com/yuque/__latex/a0d38953a75669000ec79178d6aa1c92.svg#card=math&code=e_%7B1%7D%2CR%20%2C%E6%88%96%E8%80%85e_%7B2%7D%20&id=QN8UL)用假参数替换。最后，将关系嵌入网络的损失函数定义为：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634570868398-0f4df3df-1a0b-47d0-a019-b853a266655f.png#clientId=ue34f5210-4f3c-4&from=paste&height=67&id=ufc742207&originHeight=134&originWidth=906&originalType=binary&ratio=1&size=22681&status=done&style=none&taskId=u5430a742-5f44-4bd9-9c64-8acb7dfcadf&width=453)
![](https://cdn.nlark.com/yuque/__latex/8bd8b0a7f5bacb20574b8f104f96a2cc.svg#card=math&code=%5Ctheta%20_%7Br%7D&id=psk71)是关系嵌入网络![](https://cdn.nlark.com/yuque/__latex/a85c7dede31f6a33932c60a1e95519ff.svg#card=math&code=f_%7Br%7D%28%29&id=ZEJUA)中应该被训练的参数，![](https://cdn.nlark.com/yuque/__latex/94d022d244740ec77be49e2c893036a6.svg#card=math&code=%5Clambda%20_%7Br%7D%E6%98%AF&id=nlcFf)![](https://cdn.nlark.com/yuque/__latex/8d9fa1c07a4fb22517b5f98fab20b81b.svg#card=math&code=%5Ctheta_%7Br%7D&id=CZ3l3)上![](https://cdn.nlark.com/yuque/__latex/86f7fdde1184ff5735178e4d47f19fc5.svg#card=math&code=L_%7B2%7D%E5%BD%92%E4%B8%80%E5%8C%96%E7%9A%84%E6%83%A9%E7%BD%9A%E5%8F%82%E6%95%B0&id=XTRen)，![](https://cdn.nlark.com/yuque/__latex/4a478c1f04b4cfe83a37a9f48b0ec278.svg#card=math&code=N_%7Br%7D%E5%92%8CN_%7Bcr%7D&id=uVo3R)是训练元组的数量和损坏的关系。
### 3.3.3 图嵌入
已知金融知识图谱G=(V,E)，图嵌入网络学习KG中每个节点的表示，使同一网络中具有相似结构的实体在嵌入空间中离得更近。本文引入了一个具有深度表征学习的模型，将属性、标签和结构信息合并到节点特征中。为了保留嵌入中的网络结构、节点属性和节点标签，本文最大化损失函数为：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634599153540-a69c1ea0-fc45-4170-b1b0-e646ee235c79.png#clientId=u3a539953-9606-4&from=paste&height=145&id=u54d31091&originHeight=290&originWidth=913&originalType=binary&ratio=1&size=40332&status=done&style=none&taskId=u62af57e7-a4d3-45ee-a9c2-66125fa8de8&width=456.5)
![](https://cdn.nlark.com/yuque/__latex/f840ff63c9a712038cd79ed0fddcc2fe.svg#card=math&code=%5Clambda%20_%7Bg%7D&id=pfc7d)表示平衡网络结构、属性和标签信息的参数，b表示序列S的窗口大小，它是由网络上的随机游走产生的，![](https://cdn.nlark.com/yuque/__latex/63d15cf1dbc5dddbb744bda178e25bbb.svg#card=math&code=w_%7Bj%7D&id=Ozc20)表示上下文窗口中的第j个字。可以看出上面的公式主要由三项组成，第一项是根据SkipGram构造的建模网络结构，第二项保留了节点标签信息，第三项为节点内容相关性建模。
## 3.4 多源注意力与优化
对于给定的元组集E,关系集R合知识图谱G，本文提出的方法从3.3节描述的每个模块中生成的隐藏层表征![](https://cdn.nlark.com/yuque/__latex/9a14a4a113040a0f42186c94529bcc76.svg#card=math&code=rep_%7Be%7D%2Crep_%7Br%7D%E5%92%8Crep_%7Bg%7D&id=unqIx)。由于不同的原数据对定量投资任务的事件嵌入影响不同，本文提出了一种新的多源注意网络（MSAN），捕捉数据之间的内在关系。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634604314363-107cdbce-3606-47a3-9893-a2a9719c3fa8.png#clientId=u3a539953-9606-4&from=paste&height=217&id=u9efd338c&originHeight=433&originWidth=882&originalType=binary&ratio=1&size=67647&status=done&style=none&taskId=ua85a6179-0395-4437-977e-1a5c1a9cf5f&width=441)
![](https://cdn.nlark.com/yuque/__latex/7be1574bf86a1c6f34c6a5b3a0ab21ba.svg#card=math&code=%5Calpha%20_%7Be%7D%EF%BC%8C%5Calpha%20_%7Br%7D%E5%92%8C%5Calpha_%7Bg%7D&id=KvAkS)是计算多源向量![](https://cdn.nlark.com/yuque/__latex/cd19e303813094b494430960f609d3f4.svg#card=math&code=rep_%7Bs%7D&id=uPNuQ)的注意力系数，![](https://cdn.nlark.com/yuque/__latex/2b1387bb709e0fc79bfa52e28074b7a5.svg#card=math&code=w_%7Be%7D&id=GX23z)是实体嵌入。![](https://cdn.nlark.com/yuque/__latex/cd19e303813094b494430960f609d3f4.svg#card=math&code=rep_%7Bs%7D&id=UqeSs)是从不同的原始数据学习到的高维表征向量
本文利用sigmod函数输出预测，损失函数定义如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634604671089-6590f43f-51f7-4983-9aa1-d8392c05d37d.png#clientId=u3a539953-9606-4&from=paste&height=61&id=u9256b5ab&originHeight=122&originWidth=669&originalType=binary&ratio=1&size=16492&status=done&style=none&taskId=u22a78708-8656-4cd8-b27e-7b32d76fc03&width=334.5)
其中，![](https://cdn.nlark.com/yuque/__latex/850da9e4ca4680d9a8cc5d007beb0c8b.svg#card=math&code=%5Ctheta%3D%28%7B%5Ctheta%20_%7Be%7D%2C%5Ctheta%20_%7Br%7D%2C%5Ctheta%20_%7Bg%7D%2C%5Ctheta%20_%7B%5Calpha%20%7D%2CW_%7Bs%7D%2Cb_%7Bs%7D%7D%29&id=nZGlT)分别表示事件学习、关系嵌入、图嵌入和多源注意网络中所有学习到的参数，![](https://cdn.nlark.com/yuque/__latex/06f68e30270067759c165c2d8b92bc41.svg#card=math&code=%5Cbeta%20_%7B1%2C2%2C3%7D&id=O1enF)是超参数，![](https://cdn.nlark.com/yuque/__latex/7e41303a794d72d6bd31ad0a4ce5a2d7.svg#card=math&code=W_%7Bs%7D&id=sGI1L)和![](https://cdn.nlark.com/yuque/__latex/4305e134321f2cf92554024062a35a0e.svg#card=math&code=b_%7Bs%7D&id=ozsjO)是输出层的权重和偏差。
## 3.5 量化投资中的工作流程
下图是量化投资中事件嵌入的工作流程，主要分为四个模块：历史语料库知识图谱构建、事件表征学习、新闻事件嵌入和定量投资策略。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634605960503-f447af6d-50ce-4b9e-bf78-6fc9dc2d8522.png#clientId=u3a539953-9606-4&from=paste&height=377&id=ue68c90cf&originHeight=753&originWidth=1010&originalType=binary&ratio=1&size=391696&status=done&style=none&taskId=u128ecb42-5240-464b-b470-a7d866d0150&width=505)
### 3.5.1历史语料库KG构建
模块通过3.2节中提出的关系检测模型从原始新闻语料库中生成实体关系，然后当关系被检测时，在金融知识图谱（FinKG）中创建一条所提及实体的边。
### 3.5.2 事件表征学习
模块通过对事件、关系和图嵌入网络的联合优化来训练上述数据，由此产生训练好的模型和相应的嵌入，该模型应用于嵌入了量化投资的新闻，并通过在线投资模块的结果重新训练。
### 3.5.3 新闻事件获取
模型先通过Open IE 抽取新闻数据的事件元组，接着分别利用构建的金融知识图谱（FinKG）和所学的嵌入模型获取最新的事件和实体嵌入，FinKG会根据新数据进行更新。
### 3.5.4 量化模块
从新的在线决策嵌入输入中动态构建投资策略，这个过程几乎是实时处理的。在线策略依次通过标准化和中和来处理新的嵌入，以进行适当的级别分配，级别包括四个：买进、卖出、满仓、空仓。
# 4. 解决的效果
## 4.1 实验环境
### 4.1.1数据集
本文从中国金融市场上87个主要的网站爬取金融新闻，包括腾讯金融、东方货币、上海和深圳股票交易所等等，时间从2018年1月1日到2019年12月31日。从总数量为5,134，936目录中抽取出5,262,423个实体和6,936,818个关系到金融知识图谱中，其中包括了A股所列的3,268家上市公司。
本文将第一年（从2018年1月1日到12月31日）的数据作为训练集，剩下的作为测试集来观察量化投资嵌入模型的性能。使用THULAC作为中文单词分隔工具将句子分割成短语。
通过OpenIE从新闻文本中抽取结构化事件元组，时间戳也被提取出来，与股票价格信息保持一致。本文从上海和深圳证券交易所收集A股股票市场数据，时间也为2018年1月1日到2019年12月31日，总共有325,786事件元组。下图是详细的训练集和测试
集介绍图：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634610744652-aa022045-1102-462d-a0db-fec4293cb8f5.png#clientId=u3a539953-9606-4&from=paste&height=209&id=ua2c3fb34&originHeight=418&originWidth=981&originalType=binary&ratio=1&size=95155&status=done&style=none&taskId=ua378ee53-f9e9-4932-be31-05d688cd261&width=490.5)
### 4.1.2 算法
本文使用下列方法作为标准来强调本文提出的方法有效性。

- BOW:The Bag-of-word  词袋特征用来表示新闻事件，且用于股票市场预测。
- WB-BERT:单词嵌入由一个事件或股票中每个单词向量的平均值组成，本文使用由语料库预先训练好的BERT模型。
- KDEB:knowledge-driven event embeding 一个整合了知识图谱中的额外信息的事件嵌入模型。
- EREC:Event Representation Learning Enhanced with External Commonsense Knowledge  这个模型利用了关于事件的意图和情绪的外部常识作为股票的特征。

为了评估本文提出的基于知识图谱的事件嵌入框架每个部分的性能，本文主要设置了以下几个变量用于比较：
KGEEF-noGL:表示去除了知识图谱嵌入层
KGEEF-noRL：表示去除了关系表征学习层
KGEEF-noATT:表示去除了多源注意力模型
KGEEF-all：表示包括了本文提出的所有技术
优化器：Adam算法
初始学习率：0.0001
batch size：512
超参数![](https://cdn.nlark.com/yuque/__latex/abae827e1c80df9b7a947c43c39b1d5d.svg#card=math&code=%5Cbeta%20_%7B1%7D%EF%BC%8C%5Cbeta_%7B2%7D%E5%92%8C%5Cbeta_%7B3%7D&id=Bi1Bn)设置为{0.46,0.31,0.23}
### 4.1.3 评估指标
本文通过Micro-F1、Macro-F1和加权F1评分来评估我们提出的在事件相似性测试（多类分类任务）中使用不同基线的KGEEF的性能详细来说，
True Positive （真正, TP）被模型预测为正的正样本；
True Negative（真负 , TN）被模型预测为负的负样本 ；
False Positive （假正, FP）被模型预测为正的负样本；
False Negative（假负 , FN）被模型预测为负的正样本；
Micro-F1和Macro-F1得分的评价指标公式为：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634626338141-7742572f-9547-48bb-b169-64b4fe04c843.png#clientId=u3a539953-9606-4&from=paste&height=140&id=u74145682&originHeight=279&originWidth=830&originalType=binary&ratio=1&size=32371&status=done&style=none&taskId=u65ba7344-27d4-45bf-b6c1-6f653e6ba24&width=415)
l表示事件相似性检验中的级别数，初始化l被设置为5。
加权F1分数的指标公式为：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634626493918-5e1f1a1d-6037-4a40-a1d7-fd30fb5d1cdc.png#clientId=u3a539953-9606-4&from=paste&height=72&id=uf010a2c7&originHeight=144&originWidth=884&originalType=binary&ratio=1&size=20084&status=done&style=none&taskId=uf80f641e-6de0-4ed6-98f2-4b5eb6e74e3&width=442)
![](https://cdn.nlark.com/yuque/__latex/b4ad80684ed94b4380f0d33ba54b91da.svg#card=math&code=N_%7Bi%7D&id=edgwc)表示真实事件样本中第i个标签的数量
本文使用信息系数(IC)来评估事件嵌入框架预测概率的有效性，通过以下公式计算有效性排名：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634626841620-dfa3d82c-ac9f-4dff-b278-d3a18fe84f50.png#clientId=u3a539953-9606-4&from=paste&height=42&id=u5eb7064b&originHeight=84&originWidth=821&originalType=binary&ratio=1&size=17934&status=done&style=none&taskId=u1698d80a-db0d-4c3d-8ed0-c934be3a36c&width=410.5)
     Pred表示预测值，corr表示皮尔森乘积力矩系数，Ret表示利润回报，![](https://cdn.nlark.com/yuque/__latex/22a4a9a7cb289a867f99dc219ad349e8.svg#card=math&code=Order_%7Bt-1%7D%5E%7BPred%7D&id=P9dRx)表示时间t时刻利润回报排名。       
## 4.2 事件相似性    
本文将评估学习到的事件表示的相似性是否与人为标记的事件相似性相一致  。本文手动注释了12,316个事件对，这些事件对来自于emony.cn的呼叫中心财务顾问进行的评估。每个事件对与5个独立的标准相关联，得分从1到5。0分表示事件对之间完全没有关联，5分表示两个事件非常相似。在实验中，通过每个事件对的嵌入向量的余弦相似度来计算相似度得分，然后，我们对余弦相似度得分进行排序，并通过一个简单的分类器将其映射到每个类别（1到5），使用随机森林完成此过程。下图展示了对于不同的基准获得的Macro-F1,Micro-F1和Weighted-F1
         ![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634627975288-af73c2ba-68d4-449a-a706-f6a1402aa49e.png#clientId=u3a539953-9606-4&from=paste&height=229&id=u73b912e9&originHeight=457&originWidth=953&originalType=binary&ratio=1&size=97594&status=done&style=none&taskId=u151f0517-c665-4fb8-992c-a6f7e3c2b94&width=476.5)           
 从图表可以看出，KDEB和EREC很相近，他们都优于最上面两种方法，这说明了整合结构性事件信息和背景知识的重要性。
在一个事件嵌入方案中保留知识图谱嵌入结构是非常重要的，KGEEF-all在Macro-F1、Micro-F1、和Weighted-F1都显著优于其他的方法。
## 4.3 嵌入和投资组合的性能
本文首先计算了本文提出的方法与其他用于比较的方法的 Rank IC，然后使用样本随机森林来预测股价是否会上涨，并将预测分数由高到底排列。买入高分的股票，卖出低分股票。下图是RankIC的结果图：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634629777521-96bb6635-de5b-4ec0-a902-f5c346d60cc5.png#clientId=u3a539953-9606-4&from=paste&height=245&id=ubbe1a54f&originHeight=489&originWidth=2079&originalType=binary&ratio=1&size=231548&status=done&style=none&taskId=u5f591fb6-4051-4926-8375-750279e82c0&width=1039.5)
由图可知，在2019年所有季度中，本文提出的方法显著优于其他方法，特别是在第二季度表明了本文所提方法的有效性。BOW和WB-BRET性能较差，这反映出与回报的相关性较差，通过结合外部知识常识关系，KDEB和EREC有轻微的改善。
为了进一步研究本文提出的事件嵌入框架的价值，作者进行了实验来检查所构建的投资组合的累积回报。下图展示了在不同测试时间戳累积的收益：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634630242695-d9312c19-6148-4335-9947-ed3faedd2372.png#clientId=u3a539953-9606-4&from=paste&height=261&id=u79d5c81d&originHeight=521&originWidth=2056&originalType=binary&ratio=1&size=549185&status=done&style=none&taskId=u5d689879-d789-482b-89ab-6dfe9cb9af4&width=1028)
由图可知，自2019年第二季度以来，KGEEF取得的回报遥遥领先，并扩大了与其他方法的差距。这可能是因为当市场下跌时，本文所提方法的超前滞后关系可以提前发出信号，然后卖出相关股票。所以KGEEF在第二季度开始回报增大，并且在第四季度，获得了超过20%的改善，取得了最佳性能。
## 4.4 实例研究
本文选择了量化投资中的三个流行指标检验所提方法预测股票的有效性。三个指标分别为：ROC、Bias和MACD。ROC衡量的是当前序列与之前序列相比的变幅，Bias表示价格相对于样本移动平均值的偏差水平，MACD表示两个指数移动曲线的差异，MACD称为指数平滑移动平均线，当MACD以大角度变化，表示快的移动平均线和慢的移动平均线的差距非常迅速的拉开，代表了一个市场大趋势的转变。下图是学习嵌入框架用于选定股票的的t-SNE可视化图：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634639695459-0469345b-a807-433c-9b55-fd0e636c5f73.png#clientId=ue8ced8ac-1fff-4&from=paste&height=193&id=ub65ddda5&originHeight=193&originWidth=908&originalType=binary&ratio=1&size=91046&status=done&style=none&taskId=u97d301f1-4067-4cf9-be7c-884b36a787a&width=908)
可以看出具有相同颜色的节点分布更近，两类股票分别分布在低维空间中，而且分的比较散，这在MACD图的布局里更为显著，实验结果表明本文提出的方法可以为优质股和劣质股分配相似的信号。
## 4.5.系统实施
本文基于提出的系统框架，开发了一个该模型在移动迷你应用程序中的使用，还有基于web桌面平台上的应用程序。使用分布式Scrapy网络爬虫框架爬取结构化数据，数据由Kafka和内存中的数据库Redis进行转换，以删除重复的新闻。以Elastic Search 作为原始文本数据库，Neo4j作为图数据库，训练模型由PyTorch实现，里面的代码用Python、Java、和Scala语言编写。
下图展示了本文提出的系统模型应用在移动迷你应用的界面：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634641128491-7b93a150-0cc9-4f11-a41c-2d9a1ac8d719.png#clientId=ue8ced8ac-1fff-4&from=paste&height=352&id=uae1f14bb&originHeight=352&originWidth=554&originalType=binary&ratio=1&size=86954&status=done&style=none&taskId=ud8e74a63-46c1-4237-9e81-5da43c19022&width=554)
（a）图是本文已构建的投资组合产生的热点新闻，事件视图显示在每个股票中；（b）图是中国石油天然气集团公司报道的股票价格和相应的事件。（c）图报道了关于塔里木油田（上游工厂）、中国海洋石油公司（竞争者）的新闻，以及报道了对熔喷布（一种石油化工产的下游产品）的需求不断增加。
下图是基于web的桌面视图界面：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634642044784-09151071-e293-45ca-87df-7b0c684014d2.png#clientId=ue8ced8ac-1fff-4&from=paste&height=486&id=uf1cfee84&originHeight=486&originWidth=1131&originalType=binary&ratio=1&size=299666&status=done&style=none&taskId=ub530b6f6-c9ec-408d-a38a-289377ba1c1&width=1131)
在界面的左上角我们可以看到本文基于事件嵌入构建了一种新的投资组合办法，该图展示了通过事件驱动策略所挑选的股票，界面的右边展示了事件预测股票价格的信号，其中S代表卖出点，B代表买入点。右边界面的下面基于知识图谱的推断详细描述了为什么会有一个卖出或者买入点，该结果为用户提供了一个新的基于金融知识图谱的事件驱动投资建议。
# 5.启示
本文提出了一种新的基于知识图谱的事件嵌入框架，不仅从事件参数中学习信息特征，还从超前滞后关系在金融知识图谱，特别是对产业链上游和下游的影响中学习。本文在使用多源注意力机制和联合优化对事件嵌入框架的学习过程中，集成了图嵌入层和关系学习层。大量的实验表明，本文提出的方法在事件相似性、RankIC和累积回报方面显著优于其他方法，通过从本文构建的大型金融知识图谱中学习到的知识，本文提出的方法输出了有意义的事件嵌入，说明本文的方法在实际的量化投资领域应用中是实用的。
# 6.背景

1. 有研究运用自然语言处理技术学习学习新闻事件的潜在特征，预测市场波动性，以及构建事件驱动交易策略。

Xiao Ding, Yue Zhang, Ting Liu, and Junwen Duan. 2015. Deep learning for 
event-driven stock prediction. In Twenty-fourth international joint conference on 
artificial intelligence.
David Leinweber and Jacob Sisk. 2011. Event-driven trading and the âĂĲnew 
newsâĂİ. The Journal of Portfolio Management 38, 1 (2011), 110–124.
2.现有的表示事件的方法：词袋、命名实体。这些表示方法不能捕捉到内在关系，限制了描述事件的潜能。仅仅使用术语级别的特性，如{“Microsoft”, “buys”, “LinkedIn”}。之后，有学者运用开放的信息提取（Open IE）获取结构化事件特征，这种方法可以更好地捕捉到事件的主体和受体。
3.事件嵌入机制的思想：学习结构化事件的分布式特征。类似的事件也会有相似的特征，尽管他们没有共享共同的单词。传统的事件嵌入方法是基于事件（A,P,O）三元组的单词表示法。
4.金融知识图谱中新闻事件的超前滞后效应（lead-lag effects）背景
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634540995025-30cea850-5bc0-4212-beda-ca1ba9605295.png#clientId=ua6c7f7d7-c75f-4&from=paste&height=463&id=u34dca4ad&originHeight=463&originWidth=689&originalType=binary&ratio=1&size=224636&status=done&style=none&taskId=ue427742f-05fe-492a-8862-a8bdc58e287&width=689)

这幅图说明了当一个新闻事件发生时，超前滞后效应在金融知识图谱的影响。事件对图中的不同实体有不同的影响，而对于一个事件元组，事件不仅影响上述股权，而且还可能以非同步的方式对其他相关公司产生不同的影响。例如，2016年6月13日，“微软收购领英”的一次事件显示，领英的价格上涨了46.81%，微软当天下跌了3.2%。之后，领英的主要竞争对手Salesforce的价格在接下来的两周内下跌了6%以上。此外，还有其他相关公司可能受到该事件的影响，其股票价格是由于超前滞后关系导致的。此外，这种超前滞后关系在制造企业中更为重要。
5.研究现状
在过去的50年里，如何为股票市场提取有用信息的问题已经从金融、数学和计算机科学等领域得到了广泛的研究。在信息检索(IR)中，有两种类型的信息研究最多：
（1）时间序列市场数据
现有的研究在大量机器学习的基础上利用历史时间序列市场数据来预测未来的股票价格，这些机器学习技术包括因子分析、张量理论、指标优化等等。有学者提出了一种注意力加强学习方法学习金融方面的特征，例如平衡风险和制止额外的损失。他们认为，可以通过从价格和成交量波动中挖掘历史市场模式来做出预测。然而，他们的工作忽略了财务新闻信息，这是营销预测的一个关键组成部分。
(2) 非结构化文本数据
随着NLP技术的发展，有研究发现金融新闻可以影响一只股票的市场价格。一些研究者对新闻文档进行语义分析，来检测如何将定性信息归纳到市场估值中；还有学者用主题情绪时间序列倒推股票价格；也有作者利用金融新闻数据上的结构性事件提取方法来学习事件的低维表示。
然而，这些论文主要致力于使用事件的自然信息或外部信息，例如事件的意图和情感，就像在不断扩大因子的选取范围，来增强特征而并没有学习到这些事件中包含的关系。本文提出的基于知识图谱的事件嵌入框架不仅仅学习了知识图谱中的事件参数关系，也保留了超前滞后效应对其他股票的影响。
#  7.有用的论点、句子
1. Effectively learning event embeddings from lead-lag relationship is challenging, due to the lacking of expert level knowledge of financial systems and the inaccessibility of annotated training data.（由于缺乏金融系统的专家级知识和无法获得带注释的训练数据，从领先-滞后关系中有效地学习事件嵌入是具有挑战性的。）
2.Therefore, in this paper, we propose to incorporate the knowledge of lead-lag relationship into a novel knowledge graph-based event embedding framework (KGEEF) for quantitative investment.（因此，在本文中，我们建议将超前滞后关系的知识纳入一个新的基于知识图的事件嵌入框架(KGEEF)中，以进行量化投资。）
3.Large-scale experiments on FinKG and stock market data show that incorporating lead-lag relationships from knowledge graph brings promising improvements for event embeddings. In addition, we achieve better performance in quantitative investment with the developed techniques.（对FinKG和股市数据的大规模实验表明，结合知识图中的超前滞后关系为事件嵌入带来了有希望的改进。此外，我们通过开发的技术取得了更好的量化投资性能。）
4.The bilinear tensors are utilized to model the connection between the actor and the predicate, as well as the object and the predicate.（双线性张量被用来模拟参与者和谓词之间的联系，以及对象和谓词之间的联系。）
4.If a stock is among the top 100 stocks, we allocate more weights to buy them until full. On the contrary, if it is in the bottom 100, we sell them out (short).（如果一只股票是前100只股票之一，我们会分配更多的权重来购买它们，直到买满。相反，如果它在底部100点，我们就会卖出去（卖空））
5.we employ t-SNE [31] to visualize the top 100 weakened and top 100 
strengthened equities into two dimension space and color with blue and red respectively.(我们使用t-SNE[31]将前100弱化和前100强化股票可视化为二维空间，颜色分别为蓝色和红色。)
6.we present a novel knowledge graph-based event embedding framework to learn informative representations from not only the relations of event arguments, but also the lead-lag relations 
across the financial knowledge graphs, especially the effects on upstream and downstream of the industry chain.（我们提出了一种新的基于知识图的事件嵌入框架，不仅从事件参数的关系中学习信息表示，还从金融知识图之间的领先滞后关系，特别是对产业链上游和下游的影响。）
# 8.专有名词
#### Event representative learning:事件特征学习 。 将新闻事件嵌入到连续的空间向量中，以从文本语料库中捕捉语法和语义信息。
#### lead-lag effect：超前滞后效应。
 especially in [economics](https://en.wikipedia.org/wiki/Economics), describes the situation where one (leading) variable is [cross-correlated](https://en.wikipedia.org/wiki/Cross-correlation) with the values of another (lagging) variable at later times.  
表现为不同的证券资产价格之间的交叉相关性, 常常发生于市值不同的股票或股票指数之间。大市值的股票 (指数) 对小市值的股票 (指数) 收益率的变化有预测能力, 大市值股票的收益率的滞后值与小市值股票当前收益率有相关性
#### tuple(A,P,O):三元组，在自然语言处理技术中国。A表示实体，O表示值，P表示属性，代表实体和值之间的关系。这种精确的表示，语义数据可以被明确地查询和推理。这类似于面向对象设计中的实体-属性-值模型的经典表示法，根据这个基本结构，三元组可以组合成更复杂的模型，可以使用三元组作为对象或其他三元组的主语。e.g.  Mike → said → (triples → can be → objects)

- ![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634521662855-b7a33ca5-c16f-45c9-83ad-1edca619a362.png#clientId=u36018d09-76a6-4&from=paste&height=137&id=ue0f2e581&originHeight=137&originWidth=571&originalType=binary&ratio=1&size=12736&status=done&style=none&taskId=u61fe3dca-91a5-432b-a4b1-23b7dd41ba3&width=571)
#### KGEEF:knowledge graph-based event embedding framework 基于事件嵌入框架的知识图谱
#### FinKG:financial knowledge graph 金融知识图谱
#### BERT:Bidirectional Encoder Representations from Transformers  来自变压器提供的双向编码器表示
BERT的全称为Bidirectional Encoder Representation from Transformers，是一个预训练的语言表征模型。它强调了不再像以往一样采用传统的单向语言模型或者把两个单向语言模型进行浅层拼接的方法进行预训练，而是采用新的**masked language model（MLM）**，以致能生成**深度的双向**语言表征。
  该模型有以下主要优点：
1）采用MLM对双向的Transformers进行预训练，以生成深层的双向语言表征。
2）预训练后，只需要添加一个额外的输出层进行fine-tune，就可以在各种各样的下游任务中取得state-of-the-art的表现。在这过程中并不需要对BERT进行任务特定的结构修改。

- NTN：neural tensor network model
- MSAN：multi-source atteintion network   多源注意网络
- Open IE:open information extraction   开放信息提取
- Emoney.cn:是中国领先的金融服务提供商。
- THULAC：（THU Lexical Analyzer for Chinese）由清华大学自然语言处理与社会人文计算实验室研制推出的一套中文词法分析工具包，具有中文分词和词性标注功能。THULAC具有如下几个特点：
1. 能力强。利用我们集成的目前世界上规模最大的人工分词和词性标注中文语料库（约含5800万字）训练而成，模型标注能力强大。
2. 准确率高。该工具包在标准数据集Chinese Treebank（CTB5）上分词的F1值可达97.3％，词性标注的F1值可达到92.9％，与该数据集上最好方法效果相当。
3. 速度较快。同时进行分词和词性标注速度为300KB/s，每秒可处理约15万字。只进行分词速度可达到1.3MB/s。

#### 关系抽取模型

- 传统关系抽取（RE）：
- 目标：给定关系的名称、标注样本，从语料中发现给定类型的关系实例；
- 过程：不需要抽取关系指示文本；
- 特征：常利用实体的类型作为关系抽取的特征；

open关系抽取（open IE）：

- 目标：关系名字未知，输入为语料和少量的独立于关系的经验知识（规则或者实体pair），学习出一个通用的关系抽取模型；
- 过程：需要抽取表明关系的提示文本，之后还要准确地确定关系名称字符串；
- 特征：实体的类型对于抽取不是很有用；不同类型的关系的特征不能通用；
   - POS
   - 形态：大小写、标点符号
   - 上下文word：虚词，如冠词、介词
- 


#### IC、IR、RankIC
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634628670661-7d8c1e58-6785-4b86-8bb2-e89a416cc35e.png#clientId=u3a539953-9606-4&from=paste&height=378&id=u93252543&originHeight=756&originWidth=1485&originalType=binary&ratio=1&size=205313&status=done&style=none&taskId=u8ae9cedf-e4f1-417c-8d17-299e893c1a9&width=742.5)
#### RankIC![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634628716300-d14fe347-423e-48a6-b143-cd88fa1e310c.png#clientId=u3a539953-9606-4&from=paste&height=364&id=u68d980ef&originHeight=727&originWidth=1482&originalType=binary&ratio=1&size=147375&status=done&style=none&taskId=uf37b49d3-3612-40c8-be89-c6e632f1988&width=741)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634628729838-f1d5e480-7394-471d-b635-9a3d8a07b85c.png#clientId=u3a539953-9606-4&from=paste&height=171&id=u9c90eaea&originHeight=342&originWidth=1488&originalType=binary&ratio=1&size=84251&status=done&style=none&taskId=u52e1ca9e-84af-412d-9a4b-b871dd6e0e9&width=744)



![image.png](https://cdn.nlark.com/yuque/0/2021/png/22838017/1634717295092-03e5b050-5806-4eb0-a728-1b6c3ecd8c3d.png#clientId=u0bc9cc4f-900c-4&from=paste&height=156&id=uf7473887&originHeight=156&originWidth=687&originalType=binary&ratio=1&size=48664&status=done&style=none&taskId=uf241c914-c976-48b1-b449-c694d90d3a2&width=687)














