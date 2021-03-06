关键词紧邻距离（Token Proximity）
===

关键词紧邻距离用来衡量多个关键词在同一文档中是否相邻。比如用户搜索“国际足球”这一短语，包含“国际”和“足球”两个关键词，当这两个关键词按照同样顺序前后紧挨着出现在一个文档中时，紧邻距离为零，如果两词中间夹入很多词则紧邻距离较大。紧邻距离是一种衡量文档和多个关键词相关度的方法。紧邻距离虽然不应该作为给文档排序的唯一指标，但在一些情况下通过设定阈值可以过滤掉相当一部分无关的结果。

N 关键词的紧邻距离计算公式如下：

假定第 i 个关键词首字节出现在文本中的位置为 P_i，长度L_i，紧邻距离为

  ArgMin(Sum(Abs(P_(i+1) - P_i - L_i)))

具体计算过程为先取定一个 P_1，计算所有 P_2 的可能值中令 Abs(P_2 - P_1 - L1) 最小，然后固定  P2 后依照同样的方法选择 P3，P4，等等。遍历所有可能的P_1得到最小值。

具体实现见[core/indexer.go](/core/indexer.go) 文件中 computeTokenProximity 函数。

紧邻距离计算需要在索引器中保存每个分词的位置，这需要额外消耗内存，因此是默认关闭的，打开这一功能请在引擎初始化时设定 EngineOpts.IndexerOpts.IndexType 为  LocsIndex。
