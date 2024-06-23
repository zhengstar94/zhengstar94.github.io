---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "System Design学习"
date: "2022-09-02"
categories: 
  - "Interview"
---

## 关于系统设计

**适用人群:** 个人认为我这个复习plan比较适用于mid-career(<5)年经验，面试职位junior or senior。要面system design，但职位本身不是专门做系统不会对系统设计要求很高。因为毕竟是速成。系统设计想成为大神，还是要靠平时的积累一点点搭起来。

鉴于背景介绍里说的，楼主在刷题最头疼的就是系统设计。系统设计小白一开始看大家说grokking system design很有帮助，楼主上来先顺着看了几个题目感觉慢慢读能读明白，但是除非我挨个字句背下来了，否则来个原题我也不一定能答上。所以去请教了身边刚换工作的大神们，得到了一些反馈，下面是我复习的顺序(顺序很重要)，时间和材料如下：

1. DDIA。这本书很早我就买了，奈何一页也没翻过。到我决心跳槽时候也不想拖拖拉拉等看完再说。于是身边一个大神推荐的这个youtube频道DDIA基础概览帮了大忙。[**🔗youtube**](https://www.youtube.com/watch?v=PdtlXdse7pw)（in case链接失效，搜Kunal Cholera这位三哥的DDIA系列）。用12个20分钟左右的视屏把DDIA这本书12章大致内容全概括了一遍。虽然每个视频20分钟左右，但我可能每个都是花视屏本身一倍的时间，边看边在它的slides/图表上记笔记。遇到陌生的词、特别用语会google大概看一下再继续往下看。有些个别重要章节，我在看完以后发现很多资料都会谈到的时候我会回来再看一看（个人认为重要/考点频繁的章节有1,2,5,6,7,9）。这一步视频总长不长，但准备初期我用在刷题时间多，系统设计分配时间少，大概陆陆续续花了两周才完全看完。
2. 理论听了一堆，现实中到底如何设计系统，有一些选择如何取舍。这时候又有大神推荐了这个**[🔗Mideum](https://luanjunyi.medium.com/the-table-of-contents-416d2240fa8e)**。滑动到最后读他写的Technology板块。这里会学到更多面试会常问的一些内容的细节比如cache,sharding,replica等等，以及这些在实际的系统中面临的trade off. （比较松散的看花一周）
3. **Grokking system design** 当这个时候你读了前面两个之后再去看grokking system design会觉得突然醍醐灌顶，很快速的看过去就知道再说什么了，这时候看到有用的东西再快笔记记一下加深影响 （这时候前期电面陆陆续续快结束了，花了一周集中复习system design时候看的）
4. 个别知识点。例如常用的[🔗queue](https://www.freecodecamp.org/news/a-dummys-guide-to-distributed-queues-2cd358d83780/). [🔗CDN, DNS](https://github.com/donnemartin/system-design-primer#system-design-interview-questions-with-solutions) 等在看grokking的时候经常看到，但具体不知道是什么的可以快速读一些文章帮助理解
5. 接下来就是备战了。如果没自信随便来个系统设计题都能有思路的的小伙伴，一定要刷面经，一定要刷面经，一定要刷面经！澄清一下刷面经的心态一定要摆正，不要suppose刷面经一定遇到原题，这样当不是原题时候反而会一下心态慌了。我面所有的system design没有遇到一轮是一模一样的原题，要么稍微要求有变化，要么完全不一样。但面经准备多了，在准备过程中，我会看很多视频，学习他们的思路和表达方式。看多了以后发现很多设计的思路大同小异是通用的。下面几个视频是我在刷面经过程中，帮助我了解系统设计面试到底什么样，怎么和面试官沟通。我把一些我看过的视频list列在这个这个section最后 （system onsite的前几天开始花差不多3-5小时时间准备那家公司的system design面经）
6. **Be Honest** 这一点也是我认为面试最重要的一点！这是罗宾汉的recruiter（我so far遇到最好的recruiter）在打电话帮我prep onsite时候讲的一点，让我顿时深受启发。我实话跟他说我直接intern转正了没有面过system design，所以非常紧张这一轮。他跟我说，给你举个最近面试官反馈的例子，问到DB如何选择的时候，一个fail的回答是“我准备用NoSQL，比如mangoDB, Cassandra，Hbase等等”。然后面试官问为什么的时候，面试的人不能说出很具体为什么。相比较，一个pass的candidate回答是“我选择用MySQL,虽然我知道MySQL可能在scalability和XXXX上有局限，但是因为我没用过NoSQL，我不是特别的熟悉所以我这里选择NoSQL"。这个对我启发很大，我在后面所有onsite面试都是贯彻这一点。例如我记得有一轮我说到partition data，面试官问如果有hot user致使partition不均匀你怎么办。我就记得看的某一个资料里说cassandra本身可以通过config来进行consistent hashing。不用自己手动搞一个consistent hashing logic. 但我当时也就是看我自己笔记里记了这么句话，我也不能确定100%对的。所以我就很实在的说“I didn't use cassandra myself before, but if I remember correctly, I read from a book that cassandra uses consistent hashing to distribute and replica data across nodes. If that is the case, we don't need to implement a separate hashing function to solve that issue.” 能看出面试官对这个回答很满意。假如我这个记错了，因为我很诚实告诉他我也不确定，所以不算是个red flag。所以我一直觉得面试最忌讳的是~~不懂装懂。不会就不会，把会的说出来也体现给面试官you already try your best就行了
7. 最后一点对我很通用就是速成情况下东西太多除非记忆力惊人，否则不可能什么都记得。我加强记忆的方式就是最古老的烂笔头~碰到什么觉得很有用的点就记下来。对我来说非常有帮助！
8. **模拟系统设计面试 （强推）**: [🔗Google Systems Design Interview With An Ex-Googler](https://www.youtube.com/watch?v=q0KGYwNbf-0&feature=youtu.be)
9. **Top K frequent items**: [🔗System Design Interview - Top K Problem (Heavy Hitters)](https://www.youtube.com/watch?v=kx-XDoPjoHw)
10. Distributed Loggin system: [🔗Distributed Logging System Design](https://www.youtube.com/watch?v=WzHgOl3xvu4)
11. Mock interview for design flipkart: [🔗Architecture of Amazon, Flipkart like e-commerce system](https://www.youtube.com/watch?v=2BWr0fsDSs0)
12. Design web crawler: [🔗System Design distributed web crawler to crawl Billions of web pages](https://www.youtube.com/watch?v=BKZxZwUgL3Y)
13. 设计tiny url: [🔗系统设计-短链生成系统-TinyURL](https://www.bilibili.com/video/BV1dy4y1E7A3/?from=search&seid=16388328340966394325&spm_id_from=333.337.0.0&vd_source=5b034aa798984f12216c91f6be1f3e32)



## 关于刷题

刷题我不是从零开始。3年前面实习机会的时候，那会转专业没别的优势只能好好刷题，于是很认真刷过很长一段时间。所以现在基本是要捡起三年前的手感。在九月底开始刷题时候我是先花两周把几大知识点的经典题先做一遍，熟悉了一些常考的算法。然后开始集中刷领英tag题。一是我个人感觉linkedin拿offer概率比其他公司更大些，tag题也就140左右想刷完压力也不大。其次是我个人感觉linkedin tag题大多题都很经典，也涵盖了所有的考点算法。能理解并熟练做大多数linkedin的tag题那面试随意什么题临场发挥也不怕。所以我基本除了罗宾汉，刀大师和红色卖房网的tag题我面试前有刷一刷之外，基本上一直反复刷linkedin题。到我最后一个onsite面完那一天我再也没有打开利口.题量最终停留在了265。下面几点是我特别想分享的，不同意也求轻喷：

- 认真面对每一个OA。我刷题的第3-4周做了5个公司OA。一个例子是，我99%不会去热带雨林 （因为wlb是换工作主要考量）。但recruiter联系我说给我oa做的时候我想了想还是接受了。当时队友各种说我浪费时间。但我的看法是OA本身也是在刷题，而且给你限时做题，这不是一个很好的模拟面试的机会么。我觉得这些OA就在各种电面以前已经帮我慢慢建立了面试不紧张的心态
- 拿不想去的公司先练练手。把不是很想去的公司电面都排到最前面，报着挂了也无所谓的心态开心面。过了的看看onsite如果好几轮瞎聊天的就cancel。如果是算法+system design的又可以onstie也排前面一点练手
- 关于面经。跟系统设计刚好相反，对于大多数公司我不是很喜欢算法题刷面基。比如狗，领英，软这一类公司就实在没必要看了。题多到怀疑人生。刷了没面到原题也许也会紧张。还不如掌握了各种算法之后相信自己临场应变就行。对于很小的公司，面经找也是找不到的。罗宾汉这样规模不大不小，当下比较hot的公司刷刷面经还是挺有用的。因为题还不多，面试也的确多按面经来
- 合理安排时间，不要burn out自己。按自己的速度定一个合理计划。我因为有一些实习时候刷题的基础，我给自己大概前两周订的一天5题。后面慢慢增加到一天10题差不多。等专门复习system design那一周又会相应减少量。所以因为有计划，只要每天达成甚至超过了计划，我就会特别开心觉得自信满满。如果没达成，就隔一天或者几天慢慢补回来就行。因为家有小娃，我每天5-9点期间一定是我带娃的时候，也是我换换脑子的时候。周末一定有一天我们会一家人出去玩，晚上回来做个两题保持一下手感就行。所以整个跳槽感受虽然也有压力，面试很累。但是有好好吃饭，好好睡觉，周末也放松了所以整个过程心态是特别放松的

## 写在最后

还有一个想要在职跳槽速成system design然后2月内结束复习+面试的一个必要因素就是：可以花多长时间来准备。对于有身份问题的，以及只是想看看外面有没有什么合适机会的小伙伴，资料都可以借鉴，但速成7周结束跳槽面试还是过于辛苦。因为毕竟还有本职工作要做好，如果没有合适机会之前还是要保住现在的饭碗。对于我，我是破罐子破摔一定要走人。在我开始认真准备的第一天我就计划好什么时候请两周假，什么时候辞职，不管找没找到工作我都一定要那个时候走人，然后感恩节出去玩休息~。就因为这样，我可以那7个星期几乎每天花<30分钟时间在工作上，其他时间都在复习准备



