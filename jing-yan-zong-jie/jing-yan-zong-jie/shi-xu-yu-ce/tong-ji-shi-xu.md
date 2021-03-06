# 统计时序

统计模型和时间序列模型是分析时间序列并进行预测的一种最通用的方法。在金融领域中，该方法得到了最广泛的应用。它主要通过对历史序列的分析和建模，将序列中的“趋势”和“白噪声”分离出来，最后通过对趋势的预测得到未来一段时间需求的变化。一些典型的时间序列模型有自回归滑动平均模型\(ARMA\)，差分整合移动平均自回归模型\(ARIMA\)，自回归条件异方差模型\(GARCH\)；还有一系列指数平滑模型，比如三次指数平滑模型\(Holt-Winters\)等。

![](../../../.gitbook/assets/tong-ji-shi-xu.png)

均值模型顾名思义，对历史一段时间求均值，并作为未来每个时刻的需求预测：

                                                              $$X_t=\frac{1}{T}\sum\limits_{i=1}^TX_i$$

均值模型有很多变化，滑动均值，窗口可以开7天，14天...去峰谷平均，近5天值去掉一个最大值一个最小值，剩下3个值取平均作为预测...显然，均值模型过于简单，并且在实际应用中往往无法达到较好的效果。但是，构建均值模型具有以下两个重要意义：

1. 均值模型为需求预测提供了准确率的基线。如果要从 0 到 1 构建一个预测模型，第一步不是去构建一个复杂的模型，而是先看看简单的预测能达到什么样的准确率，作为后续模型迭代评估的标准。比如对于两个时间序列，机器学习模型预测的准确率分别是 70%和 80%，但是均值模型给出的的基线准确率分别是 50%和 75%，说明前者使用机器学习模型得到的提升更大。
2. 均值模型为预测值提供了合理的尺度。一般来说，某个单品的需求都在某个范围之内波动\(促销和缺货除外\)。需求不可能短时间激增，也不可能短时间骤减。因此这个波动的范围就是该单品需求的“尺度”，它可以通过均值模型简单得出。在机器学习回归模型中，某些单品销量的的预测值和真实值有时候会差一个量级。\(比如真实销量 1000 个，但模型预测 100 个。\)如果在模型中加入均值模型作为校准，就可以避免这样的偏差发生。

![](../../../.gitbook/assets/liang-ji-wen-ding.png)

