# python数据分析攻略

说道python,大家恐怕首先想到的就是数据科学.`python`有着一系列完善配套的数据科学相关工具.
包括通用的数据结构工具`numpy`,结构化数据分析表格工具`pandas`,科学计算算法包`scipy`以及数据可视化工具`matplotlib`


数据科学总体上来讲就这么几个步骤:

+ 数据获取

    最典型也是最为人所知的就是爬虫技术,说白了爬虫就是http技术的衍生,因此不会在这边讲,同时提供几个常用的数据来源用于学习

+ 数据存储

    数据存数涉及数据库技术,这个题目太大了本文也无法全讲,只会涉及一些最基本的操作

+ 数据清洗

    一般来说数据并不是拿来就可以用的,会有很多的噪音缺值异常值等,这就需要数据清洗.通常pythoner使用`pandas`来处理

+ 数据分析

    数据分析一般就是将清洗好的数据按一定的算法计算得出一定的结论.这一步一般使用
    `pandas`,`numpy`,`scipy`等工具提供的算法计算.也可以使用比如`sklearn`,`Statsmodels`等黑箱算法包.
    本文只会讲到`pandas`,`numpy`,`scipy`,其他工具则会在相关的文章中结合例子讲解.

+ 数据可视化

    最常见的python可视化工具就是`matplotlib`.数据可视化的作用一来是辅助数据分析,
    根据图形判断数据的特征用来挑选合适的分析方式.另一个就是用来让一般客户看懂自己的分析结果.
