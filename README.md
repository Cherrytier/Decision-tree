原始数据：有如下特点

波士顿房屋这些数据于1978年开始统计，共506个数据点，涵盖了麻省波士顿不同郊区房屋14种特征的信息。本项目对原始数据集做了以下处理：

1，异常的点的清除

2，无关特征的清除

数据操作这块主要通过Numpy和Pandas

数据展示这块主要是matplotlib和后台自定义的包visuals（仍然是基于matplotlib）

 

第一步操作，就是在读入housing.csv文件后，统计一下价格的最小值、最大值、均值、中值和标准差，后期做数据分析的时候，其实用到这几个统计值。

这里实现呢，本身python是自带统计函数的，但是，这里使用numpy相关的函数处理，理论上速度应该更快一些。

可以这样认为，numpy包是基于python做的优化，而pandas是基于numpy做的优化。

其他数据特征含义，可参考下图：
第二步操作，开始建模准备

选用R^2, 也就是决定系数作为模型的评测函数，一看这意思，就是要用回归分析来进行预测啦，因为决定系数，就是回归分析中最经常用的评价指标，

比如，回归系数是否显著，调整后的R平方（调整后的R2平方，及调整后的决定系数，若R2平方为）0.60，可解释为自变量的变异解释了因变量变异的60%，R2平方取值范围为0-1，越大代表回归方程对因变量的解释程度越大）

R^2的0表示从变量不能预测因变量.
R^2的1表示从自变量可以无误差地预测因变量。

所谓的拟合程度，就是指模型本身预测的准确程度，欠拟合，说明预测的太烂，过拟合，说明模型本身对数据太敏感，只是在训练数据上得分高，但是一旦超过那个数据的范围，使用新数据的话，预测准确性就会往下掉。

第三步，做数据切割

主要目的就是为了分为训练数据和测试数据，主要是用到了sklearn中model_selection这个包，然后用到train_test_split，

这个方法主要目的，就是随机生成的参数都比较好理解，

只是，random_state这个参数需要认真学习一下，这个东东叫随机数种子，我们做一个小测试，下面我们在使用train_test_split方法的时候设置这个参数，比如设置为0，当然也可以设置成1，2或者其他整数。



大家看到每次在进行分割的时候，他所分成的数组的结构是一样的。那么如果我不设置这个参数呢，



所以，当你相重复你之前的分割组的时候，你需要设置 random_state,不然它就会随机生成其他的数组结构，不一样的数组结构可能就会引发新的问题。所以舍不设置这个参数主要看你的需求。

把数据分割成训练集和测试集的目的，提升模型的泛化能力是一个目的。泛化能力，跟刚才的过拟合有点观点，就是希望模型本身能同样对新数据有较高的预测能力。

 

第四步，通过学习曲线和验证曲线来分析模型的来对算法进行诊断

下面是不同深度的决策树回归算法生成的学习曲线：

 



学习曲线是用来干嘛的？书上说它是用来判断算法究竟是过拟合还是欠拟合，

过拟合前面提到过，对数据本身依赖行过强，泛化能力较差。可以认为是模型的问题，可能就是模型本身太复杂（相对于训练集），参数太多。

一般说这种模型偏差较高，解决方法是增大训练集，但现实一般不容易做到。

欠拟合是说模型预测的能力太差，评测函数打的分太低，这有可能是数据本身比较复杂，但模型比较简单造成的

一般说这种模型偏差较高，解决方法是构建更多特征，减少正则项。

上面四个图中，从max-depth=6 开始，方差就增大了。而max-depth=1，很显然偏差比较大。

怎么解决呢？或者说怎么找最佳的深度值呢？通过验证曲线：下图



验证曲线，主要展示的是不同深度所对应的分数，这里的分数仍然是决定系数。通过观察，可以发觉在深度为3的时候应该是个最佳点。这个max-depth也可以认为是最优参数

（未完待续）
