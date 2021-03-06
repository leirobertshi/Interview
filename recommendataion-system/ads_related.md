# Ads System

[Book](https://dirtysalt.github.io/html/computational-advertising.html#org5c5e5fa) 

## The relationship between different parties

1. [background and dsp, ssp, dmp, adexchange relationship](https://blog.csdn.net/mytestmy/article/details/18987247) 

![dsp and ssp relationship ](https://img-blog.csdn.net/20140208155212531  "relationship")

![ad exchange](https://img-blog.csdn.net/20140208155221296  "ad exchange")

2. CTR introduction

![ctr recommendation system](https://img-blog.csdn.net/20140208162153031)  
CTR system

其中投放平台获取到用户的广告请求后，将请求交给CTR计算与排名模块，这个模块会获取用户数据和广告数据，再根据模型计算CTR。得到ctr后，**有两种方式使用。一种是对CTR进行排名，返回排名ctr最高的广告，然后DSP就可以根据这个CTR出价了；另一种是将广告的出价与ctr相乘，得到每个广告的收益，根据收益排名，返回收益排名最高的广告。**
投放平台获取这个广告后，根据一些条件，会把广告展示给用户，同时产生投放日志。用户会做出相应的反馈（点击或不点击），产生点击日志。
这些日志会产生三个作用。一种是存到日志数据存储工具。第二种是产生实时统计数据，以方便迅速更新特征。第三种是进行在线学习，不断更新线上的模型。
日志存储工具的日志有两种作用，一种是经过ETL，可以生成训练数据，然后用这些训练数据去训练模型，然后更新线上的模型；另一种是利用统计的方式或者其他方式产生特征。
线上模型有两种更新方式，一种是离线训练的更新方式，一种是在线学习的更新方式。
这两种方式分别用的是两种不同的训练算法。离线训练的更新方式比较慢，频率一般比较低，一般都是一天或者一个星期训练一次，用的方法一般是批量学习方式，就是对数据进行多轮的迭代，获取w的尽可能的最优解。在线学习的方式要求能快速更新线上模型，一般是来一个展示记录，就获取这个展示记录的特征，然后利用这个记录去进行只有一个数据的迭代，得到一个新的模型。


广告的有效性：这以有效性模型把广告活动的整个信息接收过程分为三个大阶段:选择(Selection)、解释(Interpretation)与态度(Attitude);或者进一步分解为六个小阶段:曝光、关注、理解、接受、保持与决策,其中每两个小阶段对应一个大阶段。
Exposure, attention,comprehension, acceptance, retension, decision

##计算广告核心问题
计算广告的核心问题,是为一系列用户与环境的组合,找到最合适的广告投放策略以优化整体的投入产出比(ROI)。
对一个广告市场中具体的产品形态而言,我们往往能够主动优化的是产出(return)而非投入(investment)的部分,因此,我们主要关注回报的部分。
μ(a,u,c)表示点击率(Click through Rate, CTR),用ν(a,u)表示点击价值(Click Value)[a = ad, u = user, c = context],而这两部分的乘积,定量地表示了某次或若干次展示的期望CPM值,我们称之为expected CPM(eCPM)。
eCPM 它是计算广告中最常被提及,也最有代表性的定量评估收益的指标,本书中有大量的计算问题都是围绕它展开的。


## 广告系统

广告系统由三个主体部分构成:一个是在线的高并发投放引擎(Ad server),一个是离线的分布式数据处理平台(Grid),另一个是用于在线实时反馈的流式处理平台(Stream computing)。


* 广告投放,机即图中的Ad server。这是接受广告前端Web server发来的请求,完成广告投放决策并返回最后页面片段的主逻辑。
* 广告检索,包括图中的Ad index和Ad retrieval两部分。它主要的功能,是实时接受广告投放信息,建立倒排索引,以及在线时根据用户与上下文标签从索引中查找广告候选。
* 广告排序,包括图中的Ad ranking和Click modeling两部分。
* 用户日志生成,即图中的Session log generation。从各个渠道收集来日志,需要先整理成以用户ID为key的统一存储格式,我们把这样的日志称为用户日志(Session log)。目的是为了让后续的受众定向过与程更加简单高效.
* 商业智能(Business Intelligence,BI)系统,包括ETL(Extract-Transform-Load)过程, Dashboard和Cube。
* 行为定向,包括结构化标签库(Structural label base), Audience targeting, 以及User attributes的cache.
* 定制化用户划分,即图中的Customized audience segmentation:由于广告是媒体替广告主完成用户接触,那么有时需要根据广告主的逻辑来划分用户群,这部分也是具有鲜明广告特色的模块。
* 在线行为反馈:这部分指的是一些需要准实时完成的一些任务,包括短时的用户行为标签和短时用户点击反馈等。
* 当然,在利用日志完成这些逻辑之前,必须要进行的步骤是反作弊(Anti-spam)与计价(Billing)。需要特别指出,这一部分对于在线广告系统的效果提升意义重大: 在很多情形下,把系统信息反馈调整做得更快,比把模型预测做得更准确效果更加显著。
* 广告管理系统:这部分是广告操作者,即客户执行(Account execute, AE)与广告系统的接口,AE通过广告管理系统定制和调整广告投放,并且与数据仓库交互,获得投放统计数据以支持决策。
* 实时竞价接口:这是广告交易市场实时向DSP发起广告询价请求,并根据竞价结果胜出DSP的程序交易接口。它包括作为需求方时使用的RTBS(RTB for Supply),以及作为供给方时使用的RTBD(RTB for Demand)。

## 受众定向

## 竞价广告
在CPC广告网络中,eCPM可以表示成点击率和出价的乘积。即r = μ · ν。但是在有的情况下,我们有动机对此公式做一些微调,把它变成下面的形式: r = μ^κ ·ν. 其中的κ为一个大于0的实数。我们可以考虑两种极端情况来理解κ的作用:当κ → ∞时,相当于只根据点击率来排序,而不考虑出价的作用;反之,当κ → 0时,则相当于只根据出价来排序。因此,随着κ的增大,相当于我们在挤压出价在整个竞价体系中的作用,因此我们把这个因子叫做价格积压(Squashing)因子。


价格积压因子的作用,主要是为了能够根据市场情况,更主动地影响竞价体系向着需要的方向发展。比如说,如果发现市场上存在大量的出价较高但品质不高的广告主,则可以通过调高κ来强调质量和用户反馈的影响;如果发现市场的竞价激烈程度不够,则可以通过降低κ来鼓励竞争;如果存在短期的财务压力,则需要将κ调整到接近于1的范围,往往就可以使得整体营收有所上升。

##＃ 广告检索

解决这一问题的基本思路,是在检索阶段就引入某种评价函数,并按这一函数的评价结果来 决定返回哪些候选。这一评价函数的设计有两个要求:一是合理性,即对最终排序的评价函数有直觉上合理的近似;二是高效性,即需要存在与倒排索引数据结构相契合的快速评价算法,否则就与在排序阶段展开计算没有差别了。 see WAND算法.


### 智能频次控制

在品牌广告中,可以通过EC(expected click)计数上的直接控制来达到一定用户接触程度的目的,由广告主来直接设定;在效果广告中,则可以将EC的计数,或者频次的计数,作为点击率预测模型的特征直接加入训练,靠点击率模型的作用降低出现频次过高的创意的竞争力。



## eCPM

按照点击和转化两个发生在不同阶段的行为,eCPM可以分解 成点击率和点击价值的乘积: (a,u,c) = μ(a,u,c)·ν(a,u). 我们认为点击率μ是广告三个行为主体的函数,而点击价值则是用户u和广告商a的函数。后一点的假设有近似之处,因为实际上媒体的来源会影响用户对广告信息的信任程度,但我们为了概念清楚起见忽略这一影响。

在不同的市场环境下,具体的广告产品可能不需要对这两个量决都进行估计,而且估计要求的准确程度也有所区别:对于按CPC结算的广告网络,需要尽可能准确地估计μ,和粗略地估计ν;对于在广告网络中采买的交易终端,主要需要估计ν;而对于DSP,则需要对两个都有较强的估计能力。


### 点击率(CTR)预测

**LR模型, L-BFGS/ADMM优化, 点击率模型的校正, 点击率模型特征, 点击率预测评测
**
对于一些常用且重要的的偏差特征:
1. ads location, 2. ads size, 3, ads delay 4. date and time 5. web browser


cross feature

* cookie(u) and creatiive(a)
* gender and topic
* location and advertiser
* category(a) and category(u)
* cookie(u)
* creative (a)
* gender(u)

* cold-start, 利用广告层级结构, creative, solution, campaign, advertiser, 以及广告标签对新广告点击率做估计


## 搜索广告

从商业逻辑和产品形态上看,搜索广告可以认为是广告网络的一个特例。它是以上下文查询词为粒度进行受众定向,并按照竞价方式售卖和CPC结算的广告网络。从商业逻辑和产品形态上看,搜索广告可以认为是广告网络的一个特例。它是以上下文查询词为粒度进行受众定向,并按照竞价方式售卖和CPC结算的广告网络。

搜索广告与一般广告网络最主要的区别,是上下文信息非常强,因此用户标签的作用受到很大的限制。因此,关于搜索广告的研究,有两个技术上的重点:

一是查询词的扩展,即如何对 简短的上下文信息做有效的拓展,由于搜索广告的变现水平高,这样的精细加工是值得而且有效的;
二是根据用户同一个搜索session内的行为对广告结果的调整,因为围绕同一个目的一组搜索,往往对于更准确地理解用户意图有很大帮助。


### search extension
* 基于推荐的方法. (session/user, query)矩阵. SVD++在Netflix举办的推荐算法大赛中,以Yehuda Koren为首的小组获得了头名,并得到了100万美元的大奖。他们采用了一种称为SVD++的算法技术,来预测某个用户对某个电影的评分。
* 基于主题模型的方法. 除了利用搜索的日志数据本身,也可以体用一般的文档数据来进行查询词扩展。这类方法实质上就是利用文档主题模型,对某个查询拓展出主题相似的其他查询。
* 基于历史效果的方法. 对搜索广告而言,还有一类方法非常重要,那就是利用广告本身的历史eCPM数据来挖掘变现效果较好的相关查询。由于在广告主选择竞价的查询词时,一般来说都会选择多个查询,如果从历史数据中发现,某些查询对某些特定广告主的eCPM较高,按么我们应该将这些效果较好的查询组记录下来,以后当另一个广告主业选择了某组查询中的一个时,可以根据这些历史记录,自动地扩展出其他效果较好的查询。

## CTR Models

* ffm and fm and poly 2 comparation and solid example [PPT](https://github.com/mJackie/RecSys/blob/master/resource/ffm.pdf) 

### Deep Learning Related CTR Models

[深度学习如何应用在广告、推荐及搜索业务？](https://mp.weixin.qq.com/s/nboZ6p_l30L__FJNyz6Ohw) 
[深度学习在CTR预估中的应用](https://zhuanlan.zhihu.com/p/35484389)
[深度学习在 CTR 中应用](http://www.mamicode.com/info-detail-1990002.html)
[深度学习在美团点评推荐业务中实践](https://gitbook.cn/gitchat/activity/5a9521a4911b964d20d44f1a)


