# 复杂爬虫

“复杂爬虫”指需要通过观察网站行为和分析网页源代码理解网站的运行逻辑并在爬虫中模拟的爬虫。

研发要点：查看调用栈找到相关源码，使用AI辅助（推荐通义灵码）分析网页开发者写的源码（分为未混淆和混淆两种情况，知乎混淆过，混淆过的代码，分析十分困难，如何使用AI辅助是个问题）

混淆源码网页爬虫可能的技巧：

1. 尽可能完整模拟请求格式（如果只有这个就可以完成也是简单爬虫）
2. Cookie有特定参数，主要是cookie，cookie可能有其他来源控制，需要全局分析代码，或者是自定义参数，也是类似情况

复杂爬虫的定义，主要是就是为了区分哪些爬虫属于相对比较难的爬虫，然后这类爬虫直接体现在我们的项目周期和报价上的特点，就是项目周期通常情况下会长很长一段时间，然后报价也至少要翻两倍以上。

