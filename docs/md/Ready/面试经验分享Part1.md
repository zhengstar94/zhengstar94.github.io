# 面试经验分享Part1

## 简历

简历当然要好好打磨，增加面试机会，帮助咱们“以量取胜”。除了**linkedin**，还有一些诸如**indeed, hired**都很好用，每一个平台我都收到不少interview request。

## Coding

coding不只是考察coding，同样也考察沟通能力，闷头写code是不行的。对于咱们非母语选手，沟通能力恰恰需要着重锻炼。个人推荐两个方法：

1. 套模版。这里不是指算法模版，是指答题模版：比如先问clarification+edge cases，然后沟通approach，最后oral test... 个人推荐这个[REACTO模版](https://www.youtube.com/watch?v=DIR_rxusO8Q)

2. 国人不能只看leetcode，因为这玩意儿不锻炼英语表达。这里强烈推荐**AlgoExpert**，集练习、编译、讲解于一体。它价格和leetcode差不多，题目量精简到了150+题，但优点是每一题都录了讲解视频，看讲解视频能够很好的锻炼英语能力和think out loud，国人选手我强烈推荐。(同公司下的SystemExpert不推荐，错误很多，而且讲师貌似只会用SQL解决所有问题。)同样，对于刚刚起步的初学者 ，如果你连leetcode的答案都看不懂的话，那我同样建议你从AlgoExpert入手。当然leetcode里也有很不错的教学专栏（很容易被忽略），里面总结了算法题型和code template，哪怕是有经验的选手，也非常推荐重新过一遍，查缺补漏。
3. 面试中，如果面试官质疑你的方法，千万不要害怕承认错误，不要下意识地否定和抬杠。虽然不绝对，但面试官对这道题目的经验应该比你多，如果下意识地否定面试官的hint，绝对是一个red flag，毕竟coachable也是面试中最重要的一个标准。
4. 另外一定要准备**low level design**，比如设计一个**RSU**、设计一个**Hash Table**。我这次每一个大厂面试，都遇到了这方面的问题。**准备方法是在leetcode里面输入design关键词，然后把里面的所有案例全都通读**。很多设计凭空想是很难的，但只要看一遍有概念就行。

## System Design

我同意一个说法，SystemDesign就是搭积木，你先要熟悉每一块积木的特性，然后再研究具体的拼装方案。如果你不知道什么是cache,什么是kafka,那么你去读设计方案的时候一定是云里雾里。所以我建议先学习知识性的内容，再看具体的案例分析。

1. DDIA毫无疑问是神书。我感觉这本书就像内功心法，每次读都能增进一丝修为。内功深厚了，面对任何情况都能底气十足。但这本书确实读起来很累，尤其对初学者，书中讨论的corner cases太多，初学者读的时候很容易迷失方向。所以我强烈推荐从[Scott Shi的视频](https://www.youtube.com/watch?v=th_73AVA4dY&list=PLAd5bt5mn3V3TrrJFBpnu4PH9e8KZMvNA)开始，视频提供了一个总结性的脉络，读起来就会轻松很多。

2. 吐血推荐[俄罗斯大叔](https://www.youtube.com/watch?v=bUHFg8CZFws&t=4228s)这个视频简直是字字玑珠，读个3-5遍真不嫌多。
3. 很多人推荐的[Grokking System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview?affiliate_id=5749180081373184), 我个人认为并不适合初学者。主要原因是结构性太差，case by case的学习方式会增加初学者的负担。这玩意儿更适合把DDIA读完之后，再查缺补漏，增加自己对每一种系统的理解。
4. 然后，最重要的是，一定要学习real world solutions。方法就是InfoQ上面的tech talk. 你看了InfoQ之后，就会发现网上那些视频制作者都是纸上谈兵、一家之言。很多复杂产品的复杂需求，是只有在实践中才会遇到的，所以一定要学习各大公司的实际产品架构。比如以下两个方案让我印象极深，我在网课中从来没见过，但却出现在实际中运用中：
    - 如果搜索需求大，其实可以用ElasticSearch来当作一个中转的搜索点，找到了id之后再从database里提取数据。
    - 一般观点是不能把cache当作主要存储。但实际上Lyft就是用Redis来记录“所有的”实时车辆位置。仔细想想的话很有道理，用Cache才能满足这种快速的读写要求。

> 如果你在面试中说出“**as far as I know, this is the real world solution from Company XXX”，(据我所知，这是XXX公司的真实解决方案)**这样不仅展示了知识广度，也增加了答题的底气。就算面试官和你的观点不同，他也要掂量一下是不是自己的问题。

## Behavior

这个推荐亚麻leadership princles，建议针对每一点都准备一个故事，这样对于其他面试确实是降维打击。

同时推荐这个视频：**[Most Tech Interview Prep is GARBAGE](https://www.youtube.com/watch?v=0Z9RW_hhUT4)**：，简述了junior/mid-level/senior/staff 的区别，同一个问题不同的答案就能体现出级别的区分度。

**另外，现在很多公司会有一个Project Deep Dive的环节，就是考察过去做过的项目。就算不叫这个名字，一般也会在BQ里面问。所以至少要准备一个过去的项目方案，直接画出infrastructure保存好，要用到时候随时share screen.**







