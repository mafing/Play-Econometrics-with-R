# 熟悉R

关于R的中文入门有很多，在这里不再一一枚举（也不是本册子的职责所在）。但我也不能在这里说，诸位客官回去学完R的基本结构再来吧。一是有点枯燥，二则我个人窃以为学习一个软件最好的办法就是不断的拿例子来磨练，遇到问题去网上搜寻，否则看再多的入门引导也只似过眼烟云，那些命令一会儿就抛到九霄云外了。所以我把会用到的一些东西直接融入在下面这个简单的例子中，有一些必要的说明，希望可以快速的熟悉R。

下面是来自Papke(1995)的一个例子。他研究的是一个退休金计划和计划的慷慨度。

(@401K) 在401K.DTA这个数据集中，我们关心两个变量。prate是在合法的工人中拥有活跃帐户的比例。mrate是用来衡量这个计划的匹配程度（用来代表慷慨度），即如果mrate = 0:5，则表示工人付出了$10，其工作单位相应的付出了$5。接下来，我们需要面对这么几个问题：

    1. 找到prate和mrate这两个变量的平均值。
    2. 对下面这个方程进行最简单的OLS回归：$\hat{prate} = \hat{\beta}_0 + \hat{\beta}_1mrate$，并报告$R^2$。
    3. 找到当mrate = 3.5的时候，prate的预测值。

做计量分析，离不开的就是数据。所以我们第一步先来导入需要的数据。

## 数据的导入

获取数据有很多办法，在R 里面通过Foreign包可以读/写Minitab, S, SAS, SPSS, Stata, Systat等等格式的数据。当然，R本身是支持从文本文件（包括CSV格式）和剪贴板中直接读取数据的。此外，对于R包里面自带的数据集，我们可以直接用`data("name")`来加载数据集。这里我采取的是读取Stata的数据（DTA格式）。

当然，我们首先要加载**foreign**包，可以在R中直接点击“加载程序包”，也可以手动输入：

``` {r load-foreign}
library(foreign)
````

然后就可以使用`read.dta()`命令：

``` {r read-401k}
K = read.dta("data/401K.DTA", convert.dates=TRUE, convert.factors=TRUE, missing.type=TRUE, convert.underscore=TRUE, warn.missing.labels=TRUE)
summary(K)
````

K是我们赋值后在R里使用的数据表的名字。因为R是基于对象(objection)的，所以我们需要在读取数据的时候指定数据存储的对象。同样的，后面会不断的用到对象这一概念。

如果觉得这些东西记起来比较麻烦，一个个字母的打起来也挺麻烦的，怎么办？好在有个包叫做**Rcmdr**。加载这个包之后就会出现图形界面，可以通过点击的方式来操作。

![在R Commander中导入数据](http://i.imgur.com/OvRVp.jpg)
