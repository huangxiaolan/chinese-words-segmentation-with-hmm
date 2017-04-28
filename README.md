# chinese-words-segmentation-with-hmm
a demo project to use python's hmmlearn module in chinese words segmentation

#install

to use this project ,you have to install the follow:

* python
* hmmlearn
the hmmlearn install and turorial url is https://pypi.python.org/pypi/hmmlearn and http://hmmlearn.readthedocs.io/en/latest/tutorial.html

#usage

data.py : 存放训练数据

get_hmm_param: 从训练数据生成hmm算法的参数过程

hmm_start: 训练并且返回短语“我要吃饭”的分词结果， python hmm_start.py
#detail

这个工程其实是介绍如何在中文分词中应用HMM算法的过程，其步骤如下：

1 首先确认HMM算法解决问题的过程，确定参数后，可以通过观察状态推断出隐藏的状态。

适应条件： 观察状态O与隐藏状态S有关，隐藏状态只是跟前一个状态有关，不跟历史状态有关。

中文分词适应该条件。

中文分词过程中观察状态是  “我要吃饭”  -》 “SSBE”（隐藏状态） ，隐藏状态集是 {B（词开头），M（词中），E（词尾），S（独字词）}。

实际上中文分词过程变为判断每个字属于哪个分类（BMES）。这个过程中每个字的类别跟前一个字有关。

2 收集训练数据。

其实是收集 
` 
我要吃饭 -> SSBE
天气不错 -> BEBE
谢天谢地 -> BMME
`           
存在问题，如果找到大量数据，如果标识歧义字符。

3 根据训练数据获取HMM算法的参数。

参数包括：

O： 观察对象的集合， 这里是字的集合，{我要吃饭天气不错谢天地}，并将每个字映射为{0-N-1}的映射。映射值与B参数横轴维度一一对应。

S:  隐藏状态集合，这里是{BMES}，对应为{0,1,2,3},数字表示π，A，B的相关维度，必须与定义一一对应

π:隐藏状态的初始概率，取了自然对数，防止数据量值太小导致问题， 通过收集训练数据可以等到。

A:隐藏状态的转换概率矩阵，取自然对数，通过统计训练数据可以等到

B:给定隐藏状态，每个可观察状态转换等到的概率矩阵。  通过统计训练数据可以等到。

！！！   ！！！

存在问题，如果维度过大，要降维处理？？？？？

4 使用训练结果

每次输入一个短语，如 我要吃饭 ， 需要转为 序列 [0,1,2,3]

利用训练好的模型，用维特比算法，计算出对应的分词结果,第一步出来的结果是{3,3,0,2},转换为[SSBE],进一步可以得到 我/要/吃饭 这样的分词划分。

存在问题：每次计算量都是比较大。

我要吃饭天气不错谢天地 


