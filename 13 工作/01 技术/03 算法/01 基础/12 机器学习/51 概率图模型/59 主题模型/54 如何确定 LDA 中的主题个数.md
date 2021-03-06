

## 如何确定 LDA 模型中的主题个数？

<span style="color:red;">是呀？</span>

在 LDA 中，主题的个数 $K$ 是一个预先指定的超参数。

对于模型超参数的选择，实践中的做法一般是将全部数据集分成训练集、验证集、和测试集 3 部分，然后利用验证集对超参数进行选择。

例如，在确定 LDA 的主题个数时，我们可以随机选取 60％的文档组成训练集，另外 20％的文档组成验证集，剩下 20％的文档组成测试集。在训练时，尝试多组超参数的取值，并在验证集上检验哪一组超参数所对应的模型取得了最好的效果。最终，在验证集上效果最好的一组超参数和其对应的模型将被选定，并在测试集上进行测试。<span style="color:red;">嗯，好的。</span>

为了衡量 LDA 模型在验证集和测试集上的效果，需要寻找一个合适的评估指标。一个常用的评估指标是困惑度（perplexity）。在文档集合 D 上，模型的困惑度被定义为：

$$
\text{perplexity}(D)=\exp \left\{-\frac{\sum_{d=1}^{M} \log p\left(\boldsymbol{w}_{d}\right)}{\sum_{d=1}^{M} N_{d}}\right\}\tag{6.28}
$$

其中 M 为文档的总数，$w_{d}$ 为文档 d 中单词所组成的词袋向量，$p\left(w_{d}\right)$ 为模型所预测的文档 d 的生成概率，$N_{d}$ 为文档 d 中单词的总数。

一开始，随着主题个数的增多，模型在训练集和验证集的困惑度呈下降趋势，但是当主题数目足够大的时候，会出现过拟合，导致困惑度指标在训练集上继续下降但在验证集上反而增长。这时，可以取验证集的困惑度极小值点所对应的主题个数作为超参数。

在实践中，困惑度的极小值点可能出现在主题数目非常大的时候，然而实际应用并不能承受如此大的主题数目，这时就需要在实际应用中合理的主题数目范围内进行选择，比如选择合理范围内困惑度的下降明显变慢（拐点）的时候。<span style="color:red;">嗯。</span>

另外一种方法是在 LDA 基础之上融入分层狄利克雷过程（Hierarchical Dirichlet Process，HDP），构成一种非参数主题模型 HDP-LDA。<span style="color:red;">要总结下。</span>

非参数主题模型的好处是不需要预先指定主题的个数，模型可以随着文档数目的变化而自动对主题个数进行调整；它的缺点是在 LDA 基础上融入 HDP 之后使得整个概率图模型更加复杂，训练速度也更加缓慢，因此在实际应用中还是经常采用第一种方法确定合适的主题数目。<span style="color:red;">嗯。</span>

<span style="color:red;">对文章进行按主题分类的主题与这个有关系吗？还是说没啥关系？</span>
