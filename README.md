````
v1.0 update@20181202 梳理框架
v2.1 update@20190102 梳理风控相关各模块的职责
v2.2 update@20190323 
 - 文档结构调整；【所谓风控】部分丰富
 - 加入美团风控架构图
 - TODO：
 	-风控接入+规则引擎 补充，技术落地方案
    -风控行业相关资料搬运到本地
````

# 一、所谓风控-What && Why

TODO 风险目前理解的较为浅显，需要查阅资料补充

所谓风控，我理解可以从2个方面看，即 `风险` `控制`

## 风险

`风险`  这里狭隘的特指互联网产品中存在的风险点，例如

* 账户风险
  * 垃圾注册账号
  * 账号被盗用
* 交易风险
  * 刷单：为提升卖家人气，买卖家串通虚假交易
  * 恶排：恶意买家在卖家处频繁下单不支付；恶意买家通过频繁下单传播垃圾UGC等行为
  * 盗刷：买家卡被盗在平台上消费
  * 套现：伪装买卖家，在平台上信用卡交易，套出现金获利
* UGC风险
  * 在平台上传播垃圾文本、链接、视频等。入口比如商品店铺标题，订单备注等

## 控制

`控制` 即是要 `识别`   `预防`  ` 控制` 这些风险

* 识别：风险行为和一般正常行为的表现形式会有所不同，而对这些异常的识别依赖于行为数据的累计分析，一般采用规则+算法组合的方式，同时规则和算法都依赖于数据特征
* 预防：一般是指在风险事件 事前预判和事中拦截
* 控制：风险事件发生后，可以采取一定措施控制事件影响，例如用户套现，交易已经发生，但是可以通过控制提现时长或者冻结资金等方式来控制影响面

# 二、如何风控- When && How

下图是美团风控架构图

自己想要总结的和这个大同小异，这里借用下。下面会详细描述各模块职责和一些技术细节

![美团风险架构](picture_resource/meituan_riskcontrol_architure.png)

​	

## 风控接入

风控会和很多业务方对接，对接方式随着系统的不断迭代有以下优化

* 原始的方式：每个业务线单独对接，都需要新开接口，单独支持联调。成本高，效率低
* 优化后方式：打造统一接入平台，场景+策略 平台化配置，自动对接调试。同时不断 ***沉淀风控策略*** 作为基础能力，业务对接只需要沟通好风险点，选择对应的策略集，梳理好策略集需要的字段信息对应传入即可

## 规则引擎

规则引擎作为风控的核心，最重要的便是将如何将 **规则算法** 和 **数据特征** 灵活的组装好，将他们的功效输出

直白讲，规则引擎对每次 **事件** 的风险判定，是根据 **规则算法** 综合判定的，而规则算法一般都会需要很多特征数据。左手规则算法，后手特征数据，拼装在一块，变得出风险判定结果

规则引擎系统的打造，一些细节采用如下QA的方式表述

Q1：规则集如何获取

A1：依据风控事件的不同，从规则管理中心获取对应规则。同时规则也应有版本控制



Q2：特征数据如何获取

A2：规则集获取到后，便知道整个规则集需要的特征数据集，直接从数据中心获取



Q3：规则的依赖关系及并行执行

A3：并行；串行；树形等



TODO 更多的细节待思考和补充

Q：规则的动态化发布；规则的表达和可配置；

Q：风险事件的同步、异步接入



## 数据特征

### 基础事件数据

一般针对需要风控介入的场景，称每次场景下的行为为一个事件，例如称一个新用户注册为一个注册事件，一次下单为交易事件，一次支付行为为支付事件等等。

上面描述的事件，称为基本`事件数据`。

针对时间数据，可能做多种维度的统计和分析得到处理后的数据，称为 `特征数据` 

### 特征中心

和数据打交道的一个模块，负责

- 基础事件数据的落地和查询
- 特征数据的统计维护和查询



## 规则算法

### 规则

从B端角度看，规则中心主要负责

- 规则的增删查改的维护
- 规则的测试
- 规则效果分析

从C端角度看

- 各业务线规则集的查询

### 算法



##处罚中心

处罚数据其实也可以看做一种特征数据。负责用户，卡，设备等各种维度的处罚记录和查询。

## 审核中心 && 案例库

针对处罚后需要客服审核及用户申诉的业务线，需要建设案例库及审核中心来支持。



# 三、风控圈子

## 行业参考

小米：https://toutiao.io/posts/c355w5/preview

饿了么：https://blog.csdn.net/gitchat/article/details/78086327

美团：美团博客上的后来被删除，估计是太敏感了。不过网上应该都能搜到。 待补充

携程：https://mp.weixin.qq.com/s/muufqznNNVidPgamlcurCQ

## 风控er

`同盾` `张新波` http://xinbo.me/

`美团` `唐义哲` http://typd.github.io/pages/about-me.ch/

`唯品会` `WalterInSH`  https://github.com/WalterInSH/risk-management-note



## 相关活动及会议

SACC 2016 互联网安全和风控体系 会议分享(京东、爱奇艺、美团等)：http://safe.it168.com/a2016/1028/3000/000003000971_all.shtml



# 四、风控相关招聘

TODO: 分为架构和算法，总结招聘要求所需技能点

拼多多：https://www.lagou.com/jobs/3458578.html?source=position_rec&i=position_rec-2



# 五、参考

支付宝如何识别骗子：https://www.zhihu.com/question/20669015