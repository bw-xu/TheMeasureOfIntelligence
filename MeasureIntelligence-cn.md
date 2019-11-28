# Copyright Aknowledgement 版权声明

The literature is translated by the students from PKU, UCAS and USTB. The copyright belongs to all the translation contributors:

Bowen Xu* (PKU, xubowen@pku.edu.cn), Bohao Chen (UCAS), Yaling Ma (USTB) and Yuhang Liu (USTB).

Please contact us if you need to reprint.  

**Salute Francois Chollet for his outstanding work!**

**To read and download the original paper, see [arXiv:1911.01547v2](https://arxiv.org/abs/1911.01547v2)**

# The Measure of Intelligence 测量智能

Francois Chollet
Google, Inc.
fchollet@google.com 
November 6, 2019

## Abstract 摘要

To make deliberate progress towards more intelligent and more human-like artificial systems, we need to be following an appropriate feedback signal: we need to be able to define and evaluate intelligence in a way that enables comparisons between two systems, as well as comparisons with humans. Over the past hundred years, there has been an abundance of attempts to define and measure intelligence, across both the fields of psychology and AI. We summarize and critically assess these definitions and evaluation approaches, while making apparent the two historical conceptions of intelligence that have implicitly guided them. We note that in practice, the contemporary AI community still gravitates towards benchmarking intelligence by comparing the skill exhibited by AIs and humans at specific tasks, such as board games and video games. We argue that solely measuring skill at any given task falls short of measuring intelligence, because skill is heavily modulated by prior knowledge and experience: unlimited priors or unlimited training data allow experimenters to “buy” arbitrary levels of skills for a system, in a way that masks the system’s own generalization power. We then articulate a new formal definition of intelligence based on Algorithmic Information Theory, describing intelligence as skill-acquisition efficiency and highlighting the concepts of scope, generalization difficulty, priors, and experience, as critical pieces to be accounted for in characterizing intelligent systems. Using this definition, we propose a set of guidelines for what a general AI benchmark should look like. Finally, we present a new benchmark closely following these guidelines, the Abstraction and Reasoning Corpus (ARC), built upon an explicit set of priors designed to be as close as possible to innate human priors. We argue that ARC can be used to measure a human-like form of general fluid intelligence and that it enables fair general intelligence comparisons between AI systems and humans.

为了在更智能、更像人类的人工系统方面取得缓慢而可衡量的进展，我们需要有一个合适的反馈：我们需要能够定义和评估智能，使两个智能系统以及智能系统与人类能进行比较。在过去的一百年里，在心理学和人工智能领域都有大量的定义智能和测量智能的尝试。我们对这些定义和评估方法进行了总结和批判，同时指明了两个关于智能的历史概念that have implicitly guided them。我们注意到，在实践中，目前AI社区仍然倾向于通过比较AI和人类在特定任务（如棋类游戏和视频游戏）中表现出的技能，来确定基准。我们认为仅仅在特定的任务上评价上述获得的技能，并不能被作为是评价智能的过程，因为技能主要受到先验(prior)和经验的影响：无限的先验或训练数据让实验者“买到”任意级别的技能系统，这在某种程度上掩饰了系统的泛化能力。我们因此以算法信息论(Algorithm Information Theory)为基础，对智能给出了一个新的形式化的定义，将智能描述为技能获取效率，并强调将作用域、泛化难度、先验和经验等概念作为描述智能系统的关键部分。根据这个定义，我们提出了一套制定AGI基准测试的准则。最后，我们提出了一个符合这些准则的新的基准，即抽象-推理语料库(Abstraction and Reasoning Corpus , ARC)，它建立在一组明确的先验之上，这些先验被设计为尽可能接近人类先天的先验。我们认为，ARC也可以用来测量人类这样的通用智能体，并使人工智能系统和人类之间的通用智能的比较成为可能。

Contents
I Context and history
I.1 Need for an actionable definition and measure of intelligence
I.2 Defining intelligence: two divergent visions
I.2.1 Intelligence as a collection of task-specific skills
I.2.2 Intelligence as a general learning ability
I.3 AI evaluation: from measuring skills to measuring broad abilities
I.3.1 Skill-based, narrow AI evaluation
I.3.2 The spectrum of generalization: robustness, flexibility, generality
I.3.3 Measuring broad abilities and general intelligence: the psychometrics perspective
I.3.4 Integrating AI evaluation and psychometrics
I.3.5 Current trends in broad AI evaluation
II A new perspective
II.1 Critical assessment
II.1.1 Measuring the right thing: evaluating skill alone does not move us forward
II.1.2 The meaning of generality: grounding the g factor
II.1.3 Separating the innate from the acquired: insights from developmental psychology
II.2 Defining intelligence: a formal synthesis
II.2.1 Intelligence as skill-acquisition efficiency
II.2.2 Computation efficiency, time efficiency, energy efficiency, and risk efficiency
II.2.3 Practical implications
II.3 Evaluating intelligence in this light
II.3.1 Fair comparisons between intelligent systems
II.3.2 What to expect of an ideal intelligence benchmark
III A benchmark proposal: the ARC dataset
III.1 Description and goals
III.1.1 What is ARC? 
III.1.2 Core Knowledge priors
III.1.3 Key differences with psychometric intelligence tests
III.1.4 What a solution to ARC may look like, and what it would imply for AI applications
III.2 Weaknesses and future refinements 
III.3 Possible alternatives 
III.3.1 Repurposing skill benchmarks to measure broad generalization 
III.3.2 Open-ended adversarial or collaborative approaches

目录
I 背景和历史
I.1需要一个可操作的智能的定义和测量的方法
I.2定义智能：两种不同的视角
I.2.1智能是特定任务技能的集合
I.2.2智力是一种通用的学习能力
I.3人工智能评估：从测量技能到测量广泛的能力
I.3.1基于技能的专用AI的评估
I.3.2泛化spectrum：鲁棒性、适应性、通用性
I.3.3测量广泛的能力和通用智能：心理测量学的观点
I.3.4结合人工智能评估与心理测量
I.3.5 broad AI评估的当前趋势
II新视角
II.1关键评估
II.1.1测量正确的事情：仅仅评估技能并不能推动我们前进
II.1.2泛化的含义：g因子的基础
II.1.3区分先天与后天：发展心理学的见解
II.2定义智能：a formal synthesis 
II.2.1智能作为技能获取效率
II.2.2计算效率、时间效率、能源效率和风险效率
II.2.3现实意义
II.3以此来评估智力
II.3.1智能系统之间的公平比较
II.3.2理想的智能基准是什么
III基准建议：ARC数据集
III.1描述和目标
III.1.1什么是ARC?
III.1.2核心知识先验
III.1.3与心理测量的智力测试的主要区别
III.1.4 ARC的解决方案可能是什么样子的，它对AI应用意味着什么
III.2缺陷和未来的改进
III.3可能的替代方案
III.3.1修订技能基准，以测量广义的泛化能力
III.3.2开放式的对抗或合作方法

## I Context and history 背景和历史
### I.1 Need for an actionable definition and measure of intelligence 需要有智能的可操作的定义和测量

The promise of the field of AI, spelled out explicitly at its inception in the 1950s and repeated countless times since, is to develop machines that possess intelligence comparable to that of humans. But AI has since been falling short of its ideal: although we are able to engineer systems that perform extremely well on specific tasks, they have still stark limitations, being brittle, data-hungry, unable to make sense of situations that deviate slightly from their training data or the assumptions of their creators, and unable to repurpose themselves to deal with novel tasks without significant involvement from human researchers.

人工智能领域的最终目标在上世纪50年代初就已明确阐明，此后又重复了无数次，那就是开发出与人类智能相当的机器。但AI目前远没有达到它的最终目标：尽管我们能够构建执行特定任务非常好的系统，但这些系统仍然有明显的局限性，包括，敏感，极度依赖数据，无法理解稍微偏离他们的训练数据或他们创造者的假设的情形，以及没有人类研究员的参与就无法重新处理新的任务。

If the only successes of AI have been in developing narrow, task-specific systems, it is perhaps because only within a very narrow and grounded context have we been able to define our goal sufficiently precisely, and to measure progress in an actionable way. Goal definitions and evaluation benchmarks are among the most potent drivers of scientific progress. To make progress towards the promise of our field, we need precise, quantitative definitions and measures of intelligence – in particular human-like general intelligence. These would not be merely definitions and measures meant to describe or characterize intelligence, but precise, explanatory definitions meant to serve as a North Star, an objective function showing the way towards a clear target, capable of acting as a reliable measure of our progress and as a way to identify and highlight worthwhile new approaches that may not be immediately applicable, and would otherwise be discounted.	

如果人工智能的唯一成功之处在于开发了专用于特定任务的系统，那可能是因为只有在非常有限的、可知的环境下，我们才能足够准确地定义我们的目标，并以可操作的方式衡量进展。目标定义和评价基准是推动科学进步的最有力动力之一。为了在我们这个领域取得进展，我们需要精确的、定量的智能的定义和测量——特别是像人类一样的通用智能。这将不仅仅是描述智能，更是精确地、可解释地定义智能：而这是将会是这个领域的“北极星”——它指明了通向目标的道路的目标函数，它能够作为取得进展的可靠的衡量，和作为一种识别有价值的新方法的工具。这些新方法可能不会立即有实用价值，如果不加以识别就可能会被忽略。

For instance, common-sense dictionary definitions of intelligence may be useful to make sure we are talking about the same concepts, but they are not useful for our purpose, as they are not actionable, explanatory, or measurable. Similarly, the Turing Test [91] and its many variants (e.g. Total Turing Test and Loebner Prize [75]) are not useful as a driver of progress (and have in fact served as a red herring 1), since such tests completely opt out of objectively defining and measuring intelligence, and instead outsource the task to unreliable human judges who themselves do not have clear definitions or evaluation protocols.

1 Turing’s imitation game was largely meant as an argumentative device in a philosophical discussion, not as a literal test of intelligence. Mistaking it for a test representative of the goal of the field of AI has been an ongoing problem	

例如，词典中关于智能的常识性定义可能有助于确保我们谈论的是相同的概念，但它们对我们的目标没有用处，因为它们没有可操作性、解释性或可测量性。同样,图灵测试[91]和它的许多变体（如Total Turing Test and Loebner Prize [75]）并不能驱动人们取得进展（事实上，它已经成为了一个“红鲱鱼谬误”1），因为这样的测试完全不能客观地定义和测量智能，而将任务推给不可靠的人类判断，而人类本身对智能并没有明确的定义或评价体系。

1图灵的模仿游戏在很大程度上是作为一种哲学讨论中的辩论手段，而不是学术上的智能测试。图灵测试就是一个典型，人们一直误以为，这样的测试是人工智能领域目标。

It is a testimony to the immaturity of our field that the question of what we mean when we talk about intelligence still doesn’t have a satisfying answer. What’s worse, very little attention has been devoted to rigorously defining it or benchmarking our progress towards it. Legg and Hutter noted in a 2007 survey of intelligence definitions and evaluation methods[53]: “to the best of our knowledge, no general survey of tests and definitions has been published”. A decade later, in 2017, Hern´andez-Orallo released an extensive survey of evaluation methods [36] as well as a comprehensive book on AI evaluation [37]. Results and recommendations from both of these efforts have since been largely ignored by the community.	

当我们谈论智能时，我们在谈论什么？这个问题仍然没有一个令人满意的答案，这揭示出我们这个领域的不成熟。更糟糕的是，很少有人关注智能的严格地定义或为制定基准。Legg 和Hutter在2007年对智力定义和评估方法[53]的调研中指出：“就我们所知，还没有关于测试和定义的全面调研发表过。”十年后的2017年，Hern ' andez-Orallo发布了一份关于评估方法[36]的广泛调研，以及一本关于人工智能评估[37]的较全面的书籍。这两项努力的结果和建议后来基本上被社区所忽视。

We believe this lack of attention is a mistake, as the absence of widely-accepted explicit definitions has been substituted with implicit definitions and biases that stretch back decades. Though invisible, these biases are still structuring many research efforts today, as illustrated by our field’s ongoing fascination without performing humans at board games or video games (a trend we discuss in I.3.5 and II.1). The goal of this document is to point out the implicit assumptions our field has been working from, correct some of its most salient biases, and provide an actionable formal definition and measurement benchmark for human-like general intelligence, leveraging modern insight from developmental cognitive psychology.	

我们认为这个问题不该缺乏关注，因为缺乏被广泛接受的明确定义，几十年前来人们对智能只有隐含定义和偏见。这些偏见虽然看不见，但时至今日仍在充斥着许多研究工作，正如我们对棋类博弈或电子竞技表现出持续的迷恋（这是我们在I.3.5和II.1中讨论的趋势）。本文的目的是指出我们的领域一直以来的隐含假设，纠正一些最明显的偏见，并利用发展认知心理学的现代观点，为通用人工智能提供一个可操作的正式定义和测量基准。

### I.2 Defining intelligence: two divergent visions	定义智能：两种不同的观点

Looked at in one way, everyone knows what intelligence is; looked at in another way, no one does.
Robert J. Sternberg, 2000	

从某种角度看，每个人都知道智能是什么；从另一个角度看，没有人知道智能是什么。
Robert J. Sternberg, 2000

Many formal and informal definitions of intelligence have been proposed over the past few decades, although there is no existing scientific consensus around any single definition. Sternberg & Detterman noted in 1986 [87] that when two dozen prominent psychologists were asked to define intelligence, they all gave somewhat divergent answers. In the context of AI research, Legg and Hutter [53] summarized in 2007 no fewer than 70 definitions from the literature into a single statement: “Intelligence measures an agent’s ability to achieve goals in a wide range of environments.”	

在过去的几十年里，人们提出了许多关于智能的正式和非正式的定义，尽管在任何一个单一的定义上都没有科学上的共识。Sternberg和Detterman在1986年指出[87]，当二十多位著名的心理学家被要求定义智能时，他们给出的答案都有些不同。在人工智能研究的背景下，Legg和Hutter[53]在2007年总结了不少于70个定义，从文献变成了一个单一的声明：“智能衡量了智能体在许多不同的(a wide range of)环境中实现目标的能力。”

This summary points to two characterizations, which are nearly universally – but often separately – found in definitions of intelligence: one with an emphasis on task-specific skill (“achieving goals”), and one focused on generality and adaptation (“in a wide range of environments”). In this view, an intelligent agent would achieve high skill across many different tasks (for instance, achieving high scores across many different video games). Implicitly here, the tasks may not necessarily be known in advance: to truly achieve generality, the agent would have to be able to learn to handle new tasks (skill acquisition).	

这一总结指出了在智力的定义中几乎普遍存在的两种特征：一种侧重于特定任务的技能(“实现目标”)，另一种侧重于一般性和适应性(“在广泛的环境中”)。在这个观点中，智能体将在许多不同的任务中获得高技能（例如，在许多不同的电子游戏中获得高分）。这里隐含地说明了，任务不一定是预先知道的：为了真正实现通用性，代理必须能够学习处理新任务（技能获取）。

These two characterizations map to Catell’s 1971 theory of fluid and crystallized intelligence(Gf-Gc)[13], which has become one of the pillars of the dominant theory of human cognitive abilities, the Cattell-Horn-Caroll theory (CHC) [62]. They also relate closely to two opposing views of the nature of the human mind that have been deeply influential in cognitive science since the inception of the field [85]: one view in which the mind is a relatively static assembly of special-purpose mechanisms developed by evolution, only capable of learning what is it programmed to acquire, and another view in which the mind is a general-purpose “blank slate” capable of turning arbitrary experience into knowledge and skills, and that could be directed at any problem.	

这两个特征对应于Catell 1971年提出的流体智能和晶体智能的理论(Gf-Gc)[13]，该理论已成为人类认知能力主流理论的支柱之一，即Cattell-Horn-Caroll理论(CHC)[62]。它们还与关于人类心灵本质的两种对立观点密切相关，这两种观点自该领域诞生以来一直对认知科学产生深远影响[85]：一种观点认为，心灵是一个相对静态的、由进化产生的专用机制的集合，只能够学习一些被预先编写好能够获得的东西；另一种观点认为，心灵是一个通用的“白板”，能够将任意的经验转化为知识和技能，而且可以用于解决任何问题。

A central point of this document is to make explicit and critically assess this dual definition that has been implicitly at the foundation of how we have been conceptualizing and evaluating intelligence in the context of AI research: crystallized skill on one hand, skill acquisition ability on the other. Understanding this intellectual context and its ongoing influence is a necessary step before we can propose a formal definition of intelligence from a modern perspective.	

本文的核心论点是明确和批判这固有技能和习得技能这两个定义，它们已经隐式作为AI研究种构思和评估智能的基础。理解这一智能相关的背景及其后续的影响，是我们从现代视角提出智能的正式定义之前的必要步骤。

### I.2.1 Intelligence as a collection of task-specific skills	智能是特定任务技能的集合

In the distant future I see open fields for far more important researches. Psychology will be based on a new foundation, that of the necessary acquirement of each mental power and capacity by gradation.
Charles Darwin, 1859	

在遥远的将来，我看到了广阔的领域，可在其中进行重要得多的研究。心理学将会建立在一个新的基础上，the necessary acquirement of each mental power and capacity by gradation的基础。
Charles Darwin, 1859

The evolutionary psychology view of human nature is that much of the human cognitive function is the result of special-purpose adaptations that arose to solve specific problems encountered by humans throughout their evolution (see e.g. [19, 74]) – an idea which originated with Darwin [21] and that coalesced in the 1960s and 1970s. Around the same time that these ideas were gaining prominence in cognitive psychology, early AI researchers, perhaps seeing in electronic computers an analogue of the mind, mainly gravitated towards a view of intelligence as a set of static program-like routines, heavily relying on logical operators, and storing learned knowledge in a database-like memory.	

人性的进化心理学的观点认为，人类在进化过程中，遇到的具体问题导致出现了特殊用途的适应性，这种适应性导致了人类的许多认知功能（例如见[19, 74]）——这种想法起源于达尔文[21]，在1970年代和1960年代形成共识。大约在同一时间，这些想法在认知心理学中有很高的地位，也许是因为看到在电子计算机与心智的类似，早期的人工智能研究人员主要倾向于这样一个观点：智能是静态的类似于程序的活动的集合，严重依赖逻辑运算符，并将学到的知识存储在一个类似于数据库的存储器中。

This vision of the mind as a wide collection of vertical, relatively static programs that collectively implement “intelligence”, was most prominently endorsed by influential AI pioneer Marvin Minsky (see e.g. The Society of Mind, 1986 [63]). This view gave rise to definitions of intelligence and evaluation protocols for intelligence that are focused on task-specific performance. This is perhaps best illustrated by Minsky’s 1968 definition of AI: “AI is the science of making machines capable of performing tasks that would require intelligence if done by humans” 2. It was then widely accepted within the AI community that the “problem of intelligence” would be solved if only we could encode human skills into formal rules and encode human knowledge into explicit databases.

2 Note the lingering influence of the Turing Test.	

心智是vertical的、相对静态的程序的wide的集合，它们共同实现“智能”，这一观点得到了有影响力的人工智能先驱Marvin Minsky的大力支持(参见《心智社会》(the Society of mind, 1986 [63])。这种观点引出了智能的定义和智能的评估的协议，这些协议关注于特定任务的性能。Minsky在1968年给出的人工智能的定义或许很好地说明了这一点：“人工智能是一门科学，它使机器能够执行一些任务，这些任务在人类完成时需要用到智能才能完成。”当时，人工智能领域普遍认为，只要我们能将人类技能编码为正式规则，将人类知识编码为明确的数据库，“智能问题”就能得到解决。

2注意图灵测试的持续影响。

This view of intelligence was once so dominant that “learning” (discounted as pure memorization) was often not even mentioned at all in AI textbooks until the mid-1980s. Even McCarthy, a rare advocate for generality in AI, believed that the key to achieving generality was better knowledge bases [60]. This definition and evaluation philosophy focused entirely on skill at narrow tasks normally handled by humans has led to a striking paradox, as pointed out by Hernandez-Orallo [36] in his 2017 survey: the field of artificial intelligence has been very successful in developing artificial systems that perform these tasks without featuring intelligence, a trend that continues to this day.	

这种关于智能的观点曾经占据了主导地位，以至于“学习”（被认为是纯粹的记忆）在20世纪80年代中期之前，人工智能教科书中常常根本没有提及。即使是McCarthy持有罕见的主张是在人工智能泛化能力，他也认为实现泛化能力的关键是更好的知识基础[60]。这个定义和评价哲学完全关注技能通常由人类完成的特定的任务，这导致了一个引人注目的悖论,该悖论由Hernandez-Orallo [36]在他2017年的调研中指出，即，人工智能领域发展出了非常成果的人工系统，它们能执行这些任务，却没有智能的特点，这一趋势一直持续到今天。

### I.2.2 Intelligence as a general learning ability	智能是一种通用的学习能力

Presumably the child brain is something like a notebook as one buys it from the stationer’s. Rather little mechanism, and lots of blank sheets.
Alan Turing, 1950	

大概孩子的大脑就想是从文具店刚买到的笔记本。只有相当少的机制，和大量的空白的工作表。
Alan Turing, 1950

In contrast, a number of researchers have taken the position that intelligence lies in the general ability to acquire new skills through learning; an ability that could be directed to a wide range of previously unknown problems – perhaps even any problem at all. Contrast Minsky’s task-focused definition of AI with the following one, paraphrased from McCarthy [60] by Hernandez-Orallo: “AI is the science and engineering of making machines do tasks they have never seen and have not been prepared for beforehand” [36].	

相反，许多研究人员认为智能是通过学习获得新技能的通用能力；这是一种能力，可以被用到解决一个更大范围的以前未知的问题，甚至可能是任意问题。下述定义与Minsky的以任务为中心对人工智能的定义相反，由后者是Hernandez-Orallo改述自McCarthy [60]：“人工智能是一门科学和工程，它让机器完成他们从未见过、也没有事先准备好面对的任务。”

The notion that machines could acquire new skills through a learning process similar to that of human children was initially laid out by Turing in his 1950 paper [91]. In 1958, Friedberg noted astutely: “If we are ever to make a machine that will speak, understand or translate human languages, solve mathematical problems with imagination, practice a profession or direct an organization, either we must reduce these activities to a science so exact that we can tell a machine precisely how to go about doing them or we must develop a machine that can do things without being told precisely how” [26]. But although the idea of generality through learning was given significant consideration at the birth of the field, and has long been championed by pioneers like McCarthy and Papert, it lay largely dormant until the resurgence of machine learning in the 1980s.	

机器可以通过类似于人类儿童的学习过程获得新技能的概念，最初是由图灵在他1950年的论文[91]中提出的。1958年，Friedberg敏锐地指出：“如果我们要让机器说话，理解或翻译人类语言，用想象力解决数学问题，实践一种职业或领导一个组织，我们要么将这些活动归纳为科学，准确到我们可以确切地告诉机器如何去做；要么开发一种机器，可以在不必精确描述如何做的前提下就能做这些事”[26]。但是，尽管在该领域诞生之初，通过学习实现泛化性的想法就得到了重点考虑，而且一直受到McCarthy和Papert等先驱的拥护，但直到20世纪80年代机器学习的复兴之前这种想法基本上处于静默状态。

This view of intelligence echoes another long-standing conception of human nature that has had a profound influence on the history of cognitive science, contrasting with the evolutionary psychology perspective: Locke’s Tabula Rasa (blank slate), a vision of the mind as a flexible, adaptable, highly general process that turns experience into behavior, knowledge, and skills. This conception of the human mind can be traced back to Aristotle (De Anima, c. 350BC, perhaps the first treatise of psychology [3]), was embraced and popularized by Enlightenment thinkers such as Hobbes [42], Locke [56], and Rousseau [78]. It has more recently found renewed vitality within cognitive psychology (e.g. [79]) and in AI via connectionism (e.g. [41]).	

这一智能视角与另一个长期存在的人性的观念相似，该观念产生了深刻的影响认知科学的历史，与进化心理学的角度对比：洛克的白板，将心智视为的灵活、适应性强、高度通用的过程，该过程将经验转化为行为、知识和技能。这种关于人类心智的概念可以追溯到亚里士多德（De Anima，c. 350BC，这可能是心理学的第一部专著[3]），被Hobbes [42]、Locke [56]和Rousseau等启蒙思想家所接受和推广[78]。它最近在认知心理学（如[79]）和连接主义人工智能(如[41])中有了新的活力。

With the resurgence of machine learning in the 1980s, its rise to intellectual dominance in the 2000s, and its peak as an intellectual quasi-monopoly in AI in the late 2010s via Deep Learning, a connectionist-inspired Tabula Rasa is increasingly becoming the dominant philosophical framework in which AI research is taking place. Many researchers are implicitly conceptualizing the mind via the metaphor of a “randomly initialized neural network” that starts blank and that derives its skills from “training data” – a cognitive fallacy that echoes early AI researchers a few decades prior who conceptualized the mind as a kind of mainframe computer equipped with clever subroutines. We see the world through the lens of the tools we are most familiar with.	

随着机器学习在1980年代的复兴，在2000年代占据主导地位，在2010年代末通过深度学习到达顶峰而占据垄断地位，连接主义驱动的白板正日益成为人工智能研究的主导哲学框架。许多研究人员正在隐式地概念化心智，隐喻“随机神经网络初始化”为的白板，其技能来自“训练数据”—— 这是一种认知谬误，与几十年前早期人工智能研究人员的观点类似，即认为心智作为一种大型计算机，具有一些聪明的子程序。我们总是通过我们最熟悉的工具来观察世界。

Today, it is increasingly apparent that both of these views of the nature of human intelligence – either a collection of special-purpose programs or a general-purpose Tabula Rasa–are likely incorrect, which we discuss in II.1.3, along with implications for artificial intelligence.	
今天，越来越明显的是，这两种关于人类智能本质的观点——要么是一组特殊用途的程序，要么是一张通用的白板——都可能是错误的。

## I.3 AI evaluation: from measuring skills to measuring broad abilities	评估：从测量技能到测量broad abilities

These two conceptualizations of intelligence – along with many other intermediate views combining elements from each side – have influenced a host of approaches for evaluating intelligence in machines, in humans, and more rarely in both at the same time, which we discuss below. Note that this document is not meant as an extensive survey of AI evaluation methods – for such a survey, we recommend Hernandez-Orallo 2017 [37]. Other notable previous surveys include Cohen and Howe 1988 [69] and Legg and Hutter 2007 [53].	

这两种智能概念——以及许多其他结合了双方元素的中间观点——已经影响了许多评估机器、人类智能的方法，并且很少同时在这两种情况下评估，我们将在下面讨论。请注意，本文并不是对人工智能评估方法的广泛调研——对于这样的调研，我们推荐Hernandez-Orallo 2017[37]。其他值得注意的调研包括Cohen和Howe 1988[69]以及Legg和Hutter 2007[53]。

### I.3.1 Skill-based, narrow AI evaluation	基于技能的专用AI的评估

In apparent accordance with Minsky’s goal for AI, the major successes of the field have been in building special-purpose systems capable of handling narrow, well-described tasks, sometimes at above human-level performance. This success has been driven by performance measures quantifying the skill of a system at a given task (e.g. how well an AI plays chess, how well an image classifier recognizes cats from dogs). There is no single, formalized way to do skill-based evaluation. Historically successful approaches include:	

与Minsky的人工智能目标明显一致，该领域的主要成功之处在于建立了特殊用途的系统，这些系统能够处理狭窄的、描述良好的任务，有时还具有高于人类水平的性能。这一成功是由量化系统在特定任务中的技能的性能指标驱动的(例如，人工智能下国际象棋的水平，图像分类器识别猫和狗的能力)。没有一种单一的、正式的方法来进行基于技能的评估。历史上成功的方法包括:

 - Human review: having human judges observe the system’s input-output response and score it. This is the idea behind the Turing test and its variants. This evaluation mode is rarely used in practice, due to being expensive, impossible to automate, and subjective. Some human-facing AI systems (in particular commercial chatbots) use it as one of multiple evaluation mechanics.
 - White-box analysis: inspecting the implementation of the system to determine its input-output response and score it. This is most relevant for algorithms solving a fully-described task in a fully-described environment where all possible inputs can be explicitly enumerated or described analytically (e.g. an algorithm that solves the traveling salesman problem or that plays the game “Connect Four”), and would often take the form of an optimality proof.
 - Peer confrontation: having the system compete against either other AIs or humans. This is the preferred mode of evaluation for player-versus-player games, such as chess.
 - Benchmarks: having the system produce outputs for a “test set” of inputs (or environments) for which the desired outcome is known, and score the response.	

 - 人工评审:由人工评审观察系统的输入-输出响应并进行评分。这就是图灵测试及其变体背后的思想。这种评估模式在实践中很少使用，因为成本高，不可能自动化，而且主观。一些面向人类的人工智能系统（尤其是商业聊天机器人）将其用作多种评估机制之一。
 - 白盒分析：检查系统的实现，确定其输入-输出响应，并对其进行评分。这是最相关的算法解决一个完整的描述的任务在一个完整的描述环境中所有可能的输入都可以显式地枚举或描述分析（例如一个算法解决旅行商问题,或者玩游戏“四子棋”），并常常采取一个最优的形式证明。
 - 同伴对抗：让系统与其他人工智能或人类竞争。这是评估玩家对玩家游戏（如国际象棋）的首选模式。
 - 基准测试:让系统为已知期望结果的输入（或环境）的“测试集”产生输出，并对响应进行评分。

Benchmarks in particular have been a major driver of progress in AI, because they are reproducible (the test set is fixed), fair (the test set is the same for everyone), scalable (it is inexpensive to run the evaluation many times), easy to set up, and flexible enough to be applicable to a wide range of possible tasks. Benchmarks have often been most impactful in the context of a competition between different research teams, such as the ILSVRC challenge for large-scale image recognition (ImageNet) [22] or the DARPA Grand Challenge for autonomous driving [11]. A number of private and community-led initiatives have been started on the premise that such benchmark-based competitions speed up progress (e.g. Kaggle (kaggle.com), as well as academic alternatives such as ChaLearn (chalearn.org), the Hutter prize, etc.), while some government organizations use competitions to deliberately trigger technological breakthroughs (e.g. DARPA, NIST).	

特别是在AI中基准能作为驱动进步的主要因素，是因为他们是可重复的（测试集是固定的）,公平的（测试集对每个人来说都是相同的），可伸缩的（它是廉价的多次运行评价），容易设置，灵活地适用于广泛的可能的任务。在不同研究团队之间的竞争中，基准测试通常是最具影响力的，例如ILSVRC的大规模图像识别（ImageNet）[22]挑战赛或DARPA的自动驾驶[11]挑战赛。许多私人和社区项目已经开始benchmark-based竞赛等的前提下加快进展（例如Kaggle (kaggle.com),以及学术等替代ChaLearn (chalearn.org), Hutter奖,等等），而一些政府组织使用比赛有目的地引导技术突破(例如美国国防部高级研究计划局，NIST)。

These successes demonstrate the importance of setting clear goals and adopting objective measures of performance that are shared across the research community. However, optimizing for a single metric or set of metrics often leads to tradeoffs and shortcuts when it comes to everything that isn’t being measured and optimized for (a well-known effect on Kaggle, where winning models are often overly specialized for the specific benchmark they won and cannot be deployed on real-world versions of the underlying problem). In the case of AI, the focus on achieving task-specific performance while placing no conditions on how the system arrives at this performance has led to systems that, despite performing the target tasks well, largely do not feature the sort of human intelligence that the field of AI set out to build.	

这些成功表明，制定明确的目标和采用客观的绩效衡量方法是非常重要的，整个研究社区都应该分享这些方法。然而，为单一指标或一组指标优化，并非所有东西都被子良和优化，往往会导致权衡(tradeoffs)和捷径(shortcuts) (Kaggle上有一个著名的效应，即赢得模型往往过于专门用于特定的基准，无法部署在依赖此问题的现实版本)。这样的人工智能，着重于实现特定于任务的性能，同时没有条件对系统如何到达这个性能进行限定，这就导致了，尽管执行了目标任务，但很大程度上不具备人类智能的特点，而这正式AI领域力求实现的。

This has been interpreted by McCorduck as an “AI effect” where goal posts move every time progress in AI is made: “every time somebody figured out how to make a computer do something play good checkers, solve simple but relatively informal problems there was a chorus of critics to say, ‘that’s not thinking’ ” [61]. Similarly, Reed notes: “When we know how a machine does something ‘intelligent’, it ceases to be regarded as intelligent. If I beat the world’s chess champion, I’d be regarded as highly bright.” [77]. This interpretation arises from overly anthropocentric assumptions. As humans, we can only display high skill at a specific task if we have the ability to efficiently acquire skills in general, which corresponds to intelligence as characterized in II. No one is born knowing chess, or predisposed specifically for playing chess. Thus, if a human plays chess at a high level, we can safely assume that this person is intelligent, because we implicitly know that they had to use their general intelligence to acquire this specific skill over their lifetime, which reflects their general ability to acquire many other possible skills in the same way. But the same assumption does not apply to an on-human system that does not arrive at competence the way humans do. If intelligence lies in the process of acquiring skills, then there is no task X such that skill at X demonstrates intelligence, unless X is actually a meta-task involving skill-acquisition across a broad range of tasks. The “AI effect” characterization is confusing the process of intelligence (such as the intelligence displayed by researchers creating a chess-playing program) with the artifact produced by this process (the resulting chess-playing program), due to these two concepts being fundamentally intertwined in the case of humans. We discuss this further in II.1.	

这已经被McCorduck解释为一个“人工智能效应”，每当人工智能有所进展，其目标哦都会转移：“每次有人想出如何让计算机做一些玩跳棋，解决简单但相对非正式的问题时，就会有批评者说，这不是思维”[61]。类似地，Reed指出:“当我们知道一台机器如何做到‘智能’的事情时，它就不再被认为是‘智能’了。如果我赢了国际象棋世界冠军，我会被认为非常聪明。”[77]。这种解释源于过度的以人类为中心的假设。作为人类，只有当我们有能力有效地获取一般技能时，我们才能在特定的任务中表现出较高的技能，这与II中所描述的智能相对应。没有人生来就会下棋，也没有人生来就有下棋的天赋。因此，如果一个人的国际象棋水平很高，我们可以比较肯定这个人是聪明的，因为我们知道，在生活中他们一定是使用通用智能来获得这个特定的技能，这反映出他们的通用的能力，用这种能力可以以同样的方式获得许多其他可能的技能。但是，同样的假设不适用于不像人类那样获得能力的系统。如果智能的体现依赖于获取技能的过程，那么，不存在任务X使得获取X的技能就能证明智能，除非X实际上是一个元任务，即该任务能涉及到广泛任务中的技能获取。“人工智能效应”的描述混淆了智能的过程（例如研究人员创造出的下棋程序所显示的智能）和这个过程产生的人工制品（由此产生的下棋程序），因为这两个概念在人类的例子中是基本交织在一起的。我们将在II.1中进一步讨论这个问题。

Task-specific performance is a perfectly appropriate and effective measure of success if and only if handling the task as initially specified is the end goal of the system – in other words, if our measure of performance captures exactly what we expect of the system. However, it is deficient if we need systems that can show autonomy in handling situations that the system creator did not plan for, that can dynamically adapt to changes in the task–or in the context of the task – without further human intervention, or that can be repurposed for other tasks. Meanwhile, robustness and flexibility are increasingly being perceived as important requirements for certain broader subfields of AI, such as L5 self-driving, domestic robotics, or personal assistants; there is even increasing interest in generality itself (e.g. developmental robotics [4], artificial general intelligence [28]). This points to a need to move beyond skill-based evaluation for such endeavours, and to find ways to evaluate robustness and flexibility, especially in a cross-task setting, up to generality. But what do we really mean when we talk about robustness, flexibility, and generality?	

当且仅当处理最初指定的任务是系统的最终目标时（换句话说，如果我们的性能度量准确地迎合了我们对系统的期望），特定于任务的性能才会是一个非常合适和有效的度量。然而，如果我们需要系统能够在处理过程中显示出自主性，即，系统的创造者没有事先计划，而系统可以在没有进一步的人工干预的前提下，动态适应任务中的变化或任务中变化的上下文，或者可以被用来做其他任务，那么，上面的度量就显得不足了。与此同时，鲁棒性和灵活性正日益被视为人工智能某些更广泛子领域的重要要求，如L5无人驾驶、家用机器人或个人助理；甚至对通用本身的兴趣也在增加（例如，开发机器人[4]，通用人工智能[28]）。这表明需要超越基于技能的评估，并找到方法来评估鲁棒性和灵活性，特别是在跨任务设置中，达到通用性。但是当我们谈论鲁棒性、灵活性和通用性时，我们真正的意思是什么呢?

### I.3.2 The spectrum of generalization: robustness, flexibility, generality	泛化的spectrum：鲁棒性、灵活性、泛化性

Even though such machines might do some things as well as we do them, or perhaps even better, they would inevitably fail in others, which would reveal they were acting not through understanding, but only from the disposition of their organs.
Rene Descartes, 1637	

尽管这些机器在某些方面可能和我们做得一样好，甚至更好，但它们在其他方面不可避免地会失败，这将表明它们的行动不是通过理解，而是通过器官的配置。
Rene Descartes, 1637

The resurgence of machine learning in the 1980s has led to an interest in formally defining, measuring, and maximizing generalization. Generalization is a concept that predates machine learning, originally developed to characterize how well a statistical model performs on inputs that were not part of its training data. In recent years, the success of Deep Learning [52], as well as increasingly frequent run-ins with its limitations (see e.g. [51, 16, 59]), have triggered renewed interest in generalization theory in the context of machine learning (see e.g. [102, 67, 45, 70, 17, 49]). The notion of generalization can be formally defined in various contexts (in particular, statistical learning theory [92] provides a widely-used formal definition that is relevant for machine learning, and we provide a more general formalization in II.2). We can informally define “generalization” or“generalization power” for any AI system to broadly mean “the ability to handle situations (or tasks) that differ from previously encountered situations”.	

20世纪80年代机器学习的复兴导致了人们对正式定义、度量和最大化泛化能力的兴趣。泛化是一个先于机器学习的概念，它最初是用来描述统计模型在非训练数据输入上的表现。近年来，深度学习[52]的成功，以及其局限性引发的冲突日益频繁(如[51, 16, 59])，引发了人们对机器学习背景下泛化理论的新兴趣（如[102, 67, 45, 70, 17, 49]）。泛化的概念在不同的语境下的正式定义也不同（特别是，统计学习理论[92]提供了一个广泛使用的与机器学习相关的正式定义，我们在II.2中提供了一个更一般化的概化定义）。我们可以非正式地为任何AI系统定义“泛化”或“泛化能力”，泛化的意思是“处理不同于以前遇到的情况（或任务）的能力”。

The notion of “previously encountered situation” is somewhat ambiguous, so we should distinguish between two types of generalization:
 - System-centric generalization: this is the ability of a learning system to handle situations it has not itself encountered before. The formal notion of generalization error in statistical learning theory would belong here. 
    -	For instance, if an engineer develops a machine learning classification algorithm and fits it on a training set of N samples, the“generalization”of this learning algorithm would refer to its classification error over images not part of the training set. 
    -	Note that the generalization power of this algorithm may be in part due to prior knowledge injected by the developer of the system. This prior knowledge is ignored by this measure of generalization.
 - Developer-aware generalization: this is the ability of a system, either learning or static, to handle situations that neither the system nor the developer of the system have encountered before. 
    -	For instance, if an engineer uses a “development set” of N samples to create a static classification algorithm that uses hard-coded heuristic rules, the “generalization” of this static algorithm would refer to its classification error over images not part of the “development set”. 
    -	Note that “developer-aware generalization” is equivalent to“system-centric generalization” if we include the developer of the system as part of the system. 
    -	Note that “developer-aware generalization” accounts for any prior knowledge that the developer of the system has injected into it. “System-centric generalization” does not.	“以前遇到的情况”的这样的表述有些模糊，所以我们应该区分两种类型的泛化:
 - 以系统为中心的泛化：这是一个学习系统处理它以前没有遇到过的情况的能力。统计学习理论中泛化误差的形式概念就属于这里。
    -	例如，如果一个工程师开发了一个机器学习分类算法，并将其适合于N个样本的训练集，这种学习算法的“泛化”将指的是其对不属于训练集的图像的分类错误。
    -	注意，该算法的泛化能力可能部分是由于系统开发人员注入的先验知识。这种先验知识被这种泛化方法忽略了。
 - 开发人员意识的泛化：这是一个系统的能力，不管是学习还是静态的，来处理系统和系统开发人员以前都没有遇到过的情况。
    -	例如，如果一个工程师使用一个包含N个样本的“开发集”来创建一个使用硬编码启发式规则的静态分类算法，这个静态算法的“泛化”指的是它对不属于“开发集”的图像的分类错误。
    -	注意，如果我们将系统的开发人员作为系统的一部分，那么“开发人员意识的泛化”就等同于“以系统为中心的泛化”。
    -	请注意，“开发人员意识的泛化”说明了系统开发人员已经将先验知识注入到系统中。“以系统为中心的泛化”则不然。
    

In addition, we find it useful to qualitatively define degrees of generalization for information processing systems:
 - Absence of generalization: The notion of generalization as we have informally defined above fundamentally relies on the related notions of novelty and uncertainty: a system can only generalize to novel information that could not be known in advance to either the system or its creator. AI systems in which there is no uncertainty do not display generalization. For instance, a program that plays tic-tac-toe via exhaustive iteration cannot be said to “generalize” to all board configurations. Likewise, a sorting algorithm that is proven to be correct cannot be said to “generalize” to all lists of integers, much like proven mathematical statements cannot be said to“generalize”to all objects that match the assumptions of their proof 3.
 - Local generalization, or “robustness”: This is the ability of a system to handle new points from a known distribution for a single task or a well-scoped set of known tasks, given a sufficiently dense sampling of examples from the distribution (e.g. tolerance to anticipated perturbations within a fixed context). For instance, an image classifier that can distinguish previously unseen 150x150 RGB images containing cats from those containing dogs, after being trained on many such labeled images, can be said to perform local generalization. One could characterize it as “adaptation to known unknowns within a single task or well-defined set of tasks”. This is the form of generalization that machine learning has been concerned with from the 1950s up to this day.
 - Broad generalization, or “flexibility”: This is the ability of a system to handle a broad category of tasks and environments without further human intervention. This includes the ability to handle situations that could not have been foreseen by the creators of the system. This could be considered to reflect human-level ability in a single broad activity domain (e.g. household tasks, driving in the real world), and could be characterized as “adaptation to unknown unknowns across a broad category of related tasks”. For instance, aL5self-drivingvehicle, or a domestic robot capable of passing Wozniak’s coffee cup test (entering a random kitchen and making a cup of coffee) [99] could be said to display broad generalization. Arguably, even the most advanced AI systems today do not belong in this category, although there is increasing research interest in achieving this level.
 - Extreme generalization: This describes open-ended systems with the ability to handle entirely new tasks that only share abstract commonalities with previously encountered situations, applicable to any task and domain within a wide scope. This could be characterized as “adaptation to unknown unknowns across an unknown range of tasks and domains”. Biological forms of intelligence (humans and possibly other intelligent species) are the only example of such a system at this time. A version of extreme generalization that is of particular interest to us throughout this document is human-centric extreme generalization, which is the specific case where the scope considered is the space of tasks and domains that fit within the human experience. We will refer to “human-centric extreme generalization” as “generality”. Importantly, as we deliberately define generality here by using human cognition as a reference frame (which we discuss in II.1.2), it is only “general” in a limited sense. Do note, however, that humans display extreme generalization both in terms of system-centric generalization (quick adaptability to highly novel situations from little experience) and developer-aware generalization (ability of contemporary humans to handle situations that previous humans have never experienced during their evolutionary history).

3 This is a distinct definition from “generalization” in mathematics, where “to generalize” means to extend the scope of application of a statement by weakening its assumptions.	

此外，我们发现定性地定义信息处理系统的泛化程度是有用的：
 - 缺乏泛化：我们在上面非正式地定义的泛化的概念基本上依赖于相关的新颖性和不确定性的概念：一个系统只能泛化到不能提前被系统或其创建者知道的新信息。没有不确定性的人工智能系统不会表现出泛化。例如，一个通过穷举迭代玩井字游戏的程序不能被说成是“泛化”到所有的棋盘配置。同样，一个被证明是正确的排序算法不能被说成是对所有整数列表的“泛化”，就像被证明的数学语句不能被说成是对所有匹配其证明假设的对象的“泛化”一样3。
 - 局部泛化能力，或称“鲁棒性”：这是系统的能力，即给定对分布的足够致密的样本，系统可以处理从单一任务的已知分布中的新的点，或者一系列被充分研究的已知的任务（例如在固定的上下文中的预期扰动）。例如，一个图像分类器可以将之前未见过的包含猫和包含狗的150x150 RGB图像区分开来，在对许多这样的标记图像进行训练之后，就可以进行局部泛化。可以将其描述为“在单个任务或一组定义良好的任务中适应已知的未知”。这就是机器学习从20世纪50年代到今天一直关注的一泛化形式。
 - 全局泛化，或“灵活性”：这是系统在不需要进一步人工干预的情况下处理大量任务和环境的能力。这包括处理系统创建者无法预见的情况的能力。这可以被认为是在一个单一的广泛的活动领域（例如家务，在现实世界中驾驶）中反映人类水平的能力，并可以被描述为“在一个广泛的相关任务类别中对未知的未知的适应性”。例如，L5自动驾驶，或者一个能够通过Wozniak咖啡杯测试的家用机器人（进入一个随机的厨房，制作一杯咖啡）[99]可以说是有泛化性的。可以说，即使是当今最先进的人工智能系统也不属于这一范畴，尽管人们对达到这一水平的研究兴趣日益浓厚。
 - 极限泛化：描述了具有处理全新任务能力的开放式系统，这些新任务只与以前遇到的情况具有抽象的共性，适用于大范围内的任何任务和领域。这可以被描述为“在未知的任务和领域中适应未知的未知”。生物形式的智能（人类和可能的其他智能物种）是目前这种系统的唯一例子。在本文中，我们特别感兴趣的一种极限泛化的版本是以人为中心的极限泛化，这是一种特殊的情况，其中所考虑的范围是适合人类经验的任务和领域的空间。我们将把“以人为中心的极限泛化”称为“通用性(generality)”。重要的是，正如我们在这里使用人类认知作为参考框架（我们将在II.1.2中讨论）来定义通用性，它只是在有限意义上的“普遍性”。但是，请注意，人类在以系统为中心的泛化（从很少的经验快速适应高度新奇的情况）和开发人员意识的泛化（当代人类处理以前人类在其进化史上从未经历过的情况的能力）两方面都表现出极限泛化。

3这与数学中的“泛化”是截然不同的定义，在数学中，“泛化”意味着通过弱化一个命题的假设来扩展其应用范围。

To this list, we could, theoretically, add one more entry: “universality”, which would extend “generality” beyond the scope of task domains relevant to humans, to any task that could be practically tackled within our universe (note that this is different from “any task at all” as understood in the assumptions of the No Free Lunch theorem [98, 97]). We discuss in II.1.2 why we do not consider universality to be a reasonable goal for AI.	

从理论上讲，我们可以往这个列表再添加一个条目：“普适性(universality)”，这将“通用性”扩展到了与人类有关的任务域之外的范围，可以解决在我们的宇宙中的几乎any task（注意，这不同于“any task at all”理解的假设没有免费的午餐定理(98、97)）。我们在II.1.2中讨论了为什么我们不认为普适性(universality)是AI的一个合理目标。

Crucially, the history of AI has been one of slowly climbing up this spectrum, starting with systems that largely did not display generalization (symbolic AI), and evolving towards robust systems (machine learning) capable of local generalization. We are now entering a new stage, where we seek to create flexible systems capable of broad generalization (e.g. hybrid symbolic and machine learning systems such as self-driving vehicles, AI assistants, or cognitive developmental robots). Skill-focused task-specific evaluation has been appropriate for close-ended systems that aim at robustness in environments that only feature known unknowns, but developing systems that are capable of handling unknown unknowns requires evaluating their abilities in a general sense.	

至关重要的是，人工智能的历史是一个缓慢爬升的过程，从基本上没有泛化性（符号人工智能）的系统开始，向能够局部泛化的健壮系统（机器学习）发展。我们现在正进入一个新阶段，我们寻求创造能够广泛推广的灵活系统（例如，混合符号和机器学习系统，如自动驾驶汽车、人工智能助手或认知发展机器人）。以技能为中心的任务特定评估适用于封闭系统，这些系统的目标是在只以已知未知数为特征的环境中保持鲁棒性，但是开发能够处理未知未知数的系统需要从总体上评估它们的能力。

Importantly, the spectrum of generalization outlined above seems to mirror the organization of humans cognitive abilities as laid out by theories of the structure of intelligence in cognitive psychology. Major theories of the structure of human intelligence (CHC [62], g-VPR [48]) all organize cognitive abilities in a hierarchical fashion (figure 1), with three strata (in CHC): general intelligence (g factor) at the top, broad abilities in the middle, and specialized skills or test tasks at the bottom (this extends to 4 strata for g-VPR, which splits broad abilities into two layers), albeit the taxonomy of abilities differs between theories. Here, “extreme generalization” corresponds to the g factor, “broad generalization” across a given domain corresponds to a broad cognitive ability, and “local generalization” (as well as the no-generalization case) corresponds to task-specific skill.	

重要的是，上述概括的spectrum似乎反映了认知心理学中智能结构理论所阐述的人类认知能力的组织。人类智能结构的主要理论(CHC [62], g-VPR[48])都以认知能力分层的方式组织（图1），(CHC中)有三个层：通用智能（g因子），中间的broad abilities，和底部的专业技能或测试任务（g-VPR将其扩展到4层，因为将broad abilities分为两层）,尽管不同理论的能力分类不同。在这里，“极限泛化”对应着g因素，“全局泛化”对应着一个给定领域的广泛认知能力，而“局部泛化”（以及非泛化的情况）对应着任务特定的技能。

Measuring such broad abilities (and possibly generality itself) rather than specific skills has historically been the problematic of the field of psychometrics. Could psychometrics inform the evaluation of abilities in AI systems?	

从历史上看，衡量这样broad abilities（可能还有通用性本身）而不是具体的技能一直是心理测量学领域的难题。心理测量学能否用于人工智能系统的能力评估？

Figure 1: Hierarchical model of cognitive abilities and its mapping to the spectrum of generalization.	
图1：认知能力的层次模型及其到泛化spectrum的映射。

Note that, in what follows:
We use “broad abilities” to refer to cognitive abilities that lead to broad or extreme generalization. Developing such abilities should be the goal of any researcher interested in flexible AI or general AI. “Broad abilities” is often meant in opposition to “local generalization”.
We use “generalization” to refer to the entire spectrum of generalization, starting with local generalization.
Because human general intelligence (the g factor) is itself a very broad cognitive ability (the top of the hierarchy of abilities), we use the term “intelligence” or “general intelligence” to refer to extreme generalization as defined above.	请注意:

我们用“broad abilities”来指导致广义或极限泛化的认知能力。开发这样的能力应该是任何对灵活的人工智能或叫通用人工智能感兴趣的研究人员的目标。“broad abilities”常常与“局部泛化”相对立。
我们使用“泛化”来指代泛化的整个spectrum，从局部泛化到极限泛化。
因为人类的通用智能（g因子）本身是一种非常broad的认知能力（能力层次的顶层），我们使用术语“智能”或“通用智能”来指代上面定义的极限泛化。

### I.3.3 Measuring broad abilities and general intelligence: the psychometrics perspective	测量broad abilities和通用智能：心理测量学的观点

It seems to us that in intelligence there is a fundamental faculty, the alteration or the lack of which, is of the utmost importance for practical life. This faculty is [...] the faculty of adapting one’s self to circumstances.
Alfred Binet, 1916	

在我们看来，在智能中有一种基本的能力，这种能力的改变或缺失，对实际生活会产生极大影响。这种能力就是使自己适应环境的能力。
Alfred Binet, 1916

In the early days of the 20th century, Binet and Simon, looking for a formal way to distinguish children with mental disabilities from those with behavior problems, developed the Binet-Simonscale[8] ,the first test of intelligence, founding the field of psychometrics. Immediately after, Spearman observed that individual results across different, seemingly unrelated types of intelligence tests were correlated, and hypothesized the existence of a single factor of general intelligence, the g factor [83, 84]. Today, psychometrics is a well-established subfield of psychology that has arrived at some of the most reproducible results of the field. Modern intelligence tests are developed by following strict standards regarding reliability (low measurement error, a notion tied to reproducibility), validity (measuring what one purports to be measuring, a notion tied to statistical consistency and predictiveness), standardization, and freedom from bias – see e.g. Classical Test Theory (CTT) [20] and Item Response Theory (IRT) [34].	

在20世纪早期，Binet和Simon为了寻找一种正式的方法来区分有精神障碍的儿童和有行为问题的儿童，开发了Binet- simonscale[8]，这是第一个智力测试，开创了心理测量学的领域。紧接着，Spearman观察到不同的、看似不相关的智力测试的个体结果是相关的，并假设通用智能存在某个单一因素，即g因素[83,84]。今天，心理测量学是心理学中一个公认的分支学科，它已经得出了该领域中一些最具重复性的结果。现代智力测试开发对以下几个方面遵循严格的标准：可靠性（测量误差低、概念与再现性）、有效性（measuring what one purports to be measuring, a notion tied to statistical consistency and predictiveness），标准化，以及偏见无关性——参考如经典测试理论(CTT)[20]和项目反应理论(IRT) [34]。

A fundamental notion in psychometrics is that intelligence tests evaluate broad cognitive abilities as opposed to task-specific skills. Theories of the structure of intelligence (such as CHC, g-VPR), which have co-evolved with psychometric testing (statistical phenomena emerging from test results have informed these theories, and these theories have informed test design) organize these abilities in a hierarchical fashion (figure1), rather similarly to the spectrum of generalization we presented earlier. Importantly, an ability is an abstract construct (based on theory and statistical phenomena) as opposed to a directly measurable, objective property of an individual mind, such as a score on a specific test. Broad abilities in AI, which are also constructs, fall into the exact same evaluation problematics as cognitive abilities from psychometrics. Psychometrics approaches the quantification of abilities by using broad batteries of test tasks rather than any single task, and by analysing test results via probabilistic models. Importantly, the tasks should be previously unknown to the test-taker, i.e., we assume that test-takers do not practice for intelligence tests. This approach is highly relevant to AI evaluation.	

心理测量学的一个基本概念是，智力测试评估的是broad的认知能力，而不是具体任务的技能。与心理测试一起发展起来的智能结构的理论（如CHC,g-VPR）（从测试结果中出现的统计现象为这些理论提供了依据，这些理论也为测试设计提供了依据），以分层的方式组织这些能力（图1）,和我们先前所说的泛化能力谱很相似。重要的是，能力是一种抽象的概念（基于理论和统计现象），而不是一个人头脑中可以直接测量的客观属性，比如在特定测试中的分数。人工智能中broad abilities也就是constructs的评估，与心理测量学中的认知能力的评估属于完全相同的评估问题。心理测量学通过使用大量的测试任务而不是单个任务，并通过概率模型分析测试结果，来实现能力的量化。重要的是，这些任务应该是考生之前不知道的。在美国，我们假设考生不为智力测试而练习。这种方法与人工智能评估高度相关。

Remarkably, in a parallel to psychometrics, there has been recent and increasing interest across the field of AI in using broad batteries of test tasks to evaluate systems that aim at greater flexibility. Examples include the Arcade Learning Environment for Reinforcement Learning agents [6], Project MalmO [71], the Behavior Suite [68], or the GLUE [95] and SuperGLUE [94] benchmarks for natural language processing. The underlying logic of these efforts is to measure something more general than skill at one specific task by broadening the set of target tasks. However, when it comes to assessing flexibility, a critical defect of these multi-task benchmarks is that the set of tasks is still known in advance to the developers of any test-taking system, and it is fully expected that test-taking systems will be able to practice specifically for the target tasks, leverage task-specific built-in prior knowledge inherited from the system developers, leverage external knowledge obtain ed via pre-training, etc. As such, these benchmarks still appear to be highly gameable (see e.g. II.1.1) – merely widening task-specific skill evaluation to more tasks does not produce a qualitatively different kind of evaluation. Such benchmarks are still looking at skills, rather than abilities, in contrast with the psychometrics approach (this is not to say that such benchmarks are not useful; merely that such static multi-task benchmarks do not directly assess flexibility or generality).	

值得注意的是，与心理测量学类似，最近整个人工智能领域对使用大量测试任务来评估具有更大灵活性的系统的兴趣越来越大。例如强化学习智能体[6]的Arcade学习环境，工程 MalmO[71]，行为套件[68]，或者自然语言处理的GLUE[95]和SuperGLUE[94]基准。这些努力的基本逻辑是通过扩大目标任务集来衡量比某一特定任务的技能更一般的东西。然而,当涉及到评估的灵活性，这些多任务基准的关键缺陷的任务仍然是提前知道任何考试系统的开发人员，和它完全预计考试系统能够实践专门为目标任务，利用特定于任务的内置的先验知识继承了系统开发人员，利用外部知识获取通过训练的ed,等等。同样地，这些基准看起来仍然是高度可游戏的(参见例子II.1.1)——仅仅将特定于任务的技能评估扩展到更多的任务并不会产生一种性质不同的评估。与心理测量学方法相比，这样的基准测试仍然着眼于技能，而不是能力（这并不是说这样的基准测试没用；仅仅是这样的静态多任务基准不直接评估灵活性或通用性）。

In addition to these multi-task benchmarks, a number of more ambitious test suites for cognitive abilities of AI have been proposed in the past but have not been implemented in practice: the Newell test by Anderson and Lebiere ([2], named in reference to [66]), the BICA “cognitive decathlon” targeted at developmental robotics [65], the Turing Olympics [27], and the I-Athlon [1]. Lacking concrete implementations, it is difficult to assess whether these projects would have been able to address the ability evaluation problem they set out to solve. On the other hand, two similarly-spirited but more mature test suite have emerged recently, focused on generalization capabilities as opposed to specific tasks: the Animal-AI Olympics [7] (animalaiolympics.com) and the GVGAI competition [72] (gvgai.net). Both take the position that AI agents should be evaluated on an unseen set of tasks or games, in order to test learning or planning abilities rather than special-purpose skill. Both feature a multi-game environment and an ongoing public competition.	

除了这些多任务指标，一些雄心勃勃的人工智能认知能力测试的套件在过去已经呗提出,但尚未实现的实践：Newell测试由Anderson和Lebiere（[2],命名参考[66]）, 针对发展型机器人技术的BICA的“认知十项全能” [65]，图灵奥运会[27]，I-Athlon [1]。由于缺乏具体的实现，很难评估这些项目是否能够解决它们要解决的能力评估问题。另一方面，最近出现了两个类似但更成熟的测试套件，它们专注于泛化能力，而不是特定的任务：Animal-AI Olympics [7] (animalaiolympics.com)和GVGAI competition [72] (gvgai.net)。两者都认为，人工智能代理应该根据一组看不见的任务或游戏进行评估，以测试学习或规划能力，而不是特殊目的的技能。两者都有多游戏环境和持续的公众竞争。

### I.3.4 Integrating AI evaluation and psychometrics	人工智能评估与心理测量相结合

Besides efforts to broaden task-speciﬁc evaluation to batteries of multi-task tests, there have been more direct and explicit attempts to integrate AI evaluation and psychometrics. A ﬁrst approach is to reuse existing psychometric intelligence tests, initially developed for humans, as a way to assess intelligence in AI systems – perhaps an obvious idea if we are to take the term “artiﬁcial intelligence” literally. This idea was ﬁrst proposed by Green in 1964 [29], and was, around the same time, explored by Evans [24], who wrote a LISP program called ANALOGY capable of solving a geometric analogy task of the kind that may be found in a pyschometric intelligence test. Newell suggested the idea again in 1973 [66] in his seminal paper You can’t play 20 questions with Nature and win. It was proposed again and reﬁned by Bringsjord et al. in the 2000s under the name “Psychometric AI” (PAI) [9]. However, it has since become apparent that it is possible for AI system developers to game human intelligence tests, because the tasks used in these tests are available to the system developers, and thus the developers can straightforwardly solve the abstract form of these problems themselves and hard-code the solution in program form (see, for instance, [23, 80, 44]), much like Evans did with in the 1960s with the ANALOGY program. Effectively, in this case, it is the system developers who are solving the test problems, rather than any AI. The implicit assumptions that psychometric test designers make about human test-takers turn out to be difﬁcult to enforce in the case of machines.

除了努力将针对特定任务的评估扩展到一系列多任务测试之外，还进行了更加直接和明确的尝试来整合AI评估和心理测验。 第一种方法是重新使用最初为人类开发的现有的心理测验智力测验，作为评估AI系统中智力的一种方法-如果我们要真正地使用“人工智能”一词，这可能是一个显而易见的想法。 这个想法最初是由格林在1964年提出的[29]，并且在大约在同一时间，由埃文斯 Evans[24]探索，他编写了一个名为ANALOGY的LISP程序，该程序能够解决在心理智力测试中可能发现的那种类比几何任务。纽厄尔（Newell）于1973年在他的开创性论文中再次提出了这个想法[66]，“你不可能在和大自然玩20个问题后就赢”。 在2000年，由Bringsjord等人，以“Psychometric AI”（心理计量AI）”(PAI)[9]的名义，再次提出并对这个观点进行了改进。 但是，此后很明显，人工智能系统开发人员可以进行人类智能测试，因为这些测试中使用的任务可被系统开发人员使用，因此开发人员可以直接解决这些问题的抽象形式，并且，以程序形式对解决方案进行硬编码（例如，参见[23，80，44]），就像埃文斯 Evans在1960年代使用ANALOGY程序所做的那样。 实际上，在这种情况下，解决测试问题的是系统开发人员，而不是任何AI。 心理测试设计师对人类应试者所做的隐含假设对于机器来说很难实施。

An alternative, more promising approach is to leverage what psychometrics can teach us about ability assessment and test design to create new types of benchmarks targeted speciﬁcally at evaluating broad abilities in AI systems. Along these lines, Hern´andezOrallo et al. have proposed extending psychometric evaluation to any intelligent system, including AI agents and animals, in “Universal Psychometrics” [39].

We argue that several important principles of psychometrics can inform intelligence evaluation in AI in the context of the development of broad AI and general AI:

## II A new perspective 新观点

### II.1 Critical assessment	关键的评估

### II.1.1 Measuring the right thing: evaluating skill alone does not move us forward	测量正确的东西：仅仅评估技能并不能推动我们前进

In 1973, psychologist and computer science pioneer Allen Newell, worried that recent advances in cognitive psychology were not bringing the field any closer to a holistic theory of cognition, published his seminal paper You can’t play 20 questions with nature and win [66],which helped focus research efforts on cognitive architecture modelling, and provided new impetus to the longstanding quest to build a chess-playing AI that would outperform any human. Twenty-four years later, In 1997, IBM’s DeepBlue beat Gary Karsparov, the best chess player in the world, bringing this quest to an end [12]. When the dust settled, researchers were left with the realization that building an artificial chess champion had not actually taught them much, if anything, about human cognition. They had learned how to build a chess-playing AI, and neither this knowledge nor the AI they had built could generalize to anything other than similar board games.	

1973年,心理学家和计算机科学先驱Allen Newell，担心在认知心理学领域最新进展不会推进整体的认知理论，他发表了开创性的论文《你不可能在和大自然玩20个问题后就能赢》[66]，这有助于研究工作关注认知体系结构建模，并为建立一个超越人类的国际象棋AI提供了新的动力。24年后的1997年，IBM的“深蓝”击败了世界上最好的棋手加里·卡尔斯波夫，结束了对该目标的追求[12]。尘埃落定后，研究人员意识到，制造一个人工国际象棋冠军，实际上并没有教会他们多少（如果有的话）关于人类认知的东西。他们已经学会了如何建立一个会下国际象棋的人工智能，而这些知识和他们所建立的人工智能都不能泛化到任何类似的棋盘游戏之外。

It may be obvious from a modern perspective that a static chess-playing program based on minimax and tree search would not be informative about human intelligence, nor competitive with humans in anything other than chess. But it was not obvious in the 1970s, when chess-playing was thought by many to capture, and require, the entire scope of rational human thought. Perhaps less obvious in 2019 is that efforts to “solve” complex video games using modern machine learning methods still follow the same pattern. Newell wrote [66]: “we know already from existing work [psychological studies on humans] that the task [chess] involves forms of reasoning and search and complex perceptual and memorial processes. For more general considerations we know that it also involves planning, evaluation, means-ends analysis and redefinition of the situation, as well as several varieties of learning – short-term, post-hoc analysis, preparatory analysis, study from books, etc.”. The assumption was that solving chess would require implementing these general abilities. Chess does indeed involve these abilities – in humans. But while possessing these general abilities makes it possible to solve chess (and many more problems), by going from the general to the specific, inversely, there is no clear path from the specific to the general. Chess does not require any of these abilities, and can be solved by taking radical shortcuts that run orthogonal to human cognition.	

从现代的角度来看，很明显，一个基于极大极小搜索和树搜索的静态国际象棋程序不能提供有关人类智力的信息，除了国际象棋，它也不能与人类竞争。但在上世纪70年代，这一点并不明显，当时许多人认为下棋能够涵盖并要求人类理性思维的全部范围。也许在2019年不那么明显的是，使用现代机器学习方法“解决”复杂电子游戏的努力仍然遵循同样的模式。Newell[66]写道：“我们已经从现有的工作（对人类的心理学研究）中知道，国际象棋的任务包括各种形式的推理和搜索，以及复杂的感知和记忆过程。就更一般的考虑而言，我们知道它还包括规划、评估、方法-结果分析和情景重设，以及若干种类的学习- -短期、事后析因分析、事前分析、书本案例学习等等。”他们的假设是，解决国际象棋需要实现这些通用能力。国际象棋的确涉及这些能力——在人类身上。但是，尽管拥有这些通用的能力使解决国际象棋（以及许多其他问题）成为可能，这是一个通用到具体的过程，但反过来，从具体到通用却没有明确的路径。国际象棋不需要这些能力中的任何一种，可以通过走与人类认知毫无关系的极端捷径来解决。

Optimizing for single-purpose performance is useful and valid if one’s measure of success can capture exactly what one seeks (as we outlined in I.3.1), e.g. if one’s end goal is a chess-playing machine and nothing more. But from the moment the objective is settled, the process of developing a solution will be prone to taking all shortcuts available to satisfy the objective of choice–whether this process is gradient descent or human-driven research. These shortcuts often come with undesirable side-effects when it comes to considerations not incorporated the measure of performance. If the environment in which the system is to operate is too unpredictable for an all-encompassing objective function to be defined beforehand (e.g. most real-world applications of robotics, where systems face unknown unknowns), or if one aims at a general-purpose AI that could be applied to a wide range of problems with no or little human engineering, then one must somehow optimize directly for flexibility and generality, rather than solely for performance on any specific task.	

如果一个人对成功的测量能够准确地抓住他所追求的（如我们在I.3.1中所概述的），例如，一个人的最终目标只是一台下棋的机器，那么对单一目的的性能进行优化就是有用和有效的。但是从目标确定的那一刻起，开发解决方案的过程就会倾向于走所有可能的捷径来达到选择的目标——不管这个过程是梯度下降还是人类驱动的研究。当涉及到没有纳入性能度量的考虑事项时，这些快捷方式通常会带来不良的副作用。如果操作系统的环境太不可预测，预先定义一个无所不包的目标函数（例如大多数真实的机器人应用程序，系统面临未知的未知），或者如果目标是通用人工智能，可以在没有或者很少人类干预的情况下应用于广泛的问题，那么必须以某种方式直接进行优化，以获得灵活性和通用性，而不是仅针对任何特定任务的性能。

This is, perhaps, a widely-accepted view today when it comes to static programs that hard-code a human-designed solution. When a human engineer implements a chatbot by specifying answers for each possible query via if/else statements, we do not assume this chatbot to be intelligent, and we do not expect it to generalize beyond the engineer’s specifications. Likewise, if an engineer looks at a specific IQ test task, comes up with a solution, and write down this solution in program form, we do not expect the program to generalize to new tasks, and we do not believe that the program displays intelligence – the only intelligence at work here is the engineer’s. The program merely encodes the crystallized output of the engineer’s thought process – it is this process, not its output, that implements intelligence. Intelligence is not demonstrated by the performance of the output program (a skill), but by the fact that the same process can be applied to a vast range of previously unknown problems (a general-purpose ability): the engineer’s mind is capable of extreme generalization. Since the resulting program is merely encoding the output of that process, it is no more intelligent than the ink and paper used to write down the proof of a theorem.	

当谈到硬编码人为设计的解决方案的静态程序时，可能是一个被广泛接受的观点。当人类工程师通过if/else语句为每个可能的查询指定答案来实现聊天机器人时，我们不认为这个聊天机器人是智能的，我们也不期望它能泛化。同样地，如果一个工程师看到一个特定的智能测试任务，提出了一个解决方案，以程序的形式写下这个解决方案，我们不能寄希望于这个程序能推广到新的任务，我们也不相信程序显示出了智能——在这里工作是只工程师的智能。这个程序只是对工程师思维过程的具体结果进行编码——然而体现智能的是这个过程，而不是它的结果。智能不是通过输出程序（一种技能）的表现来证明的，而是通过同样的过程可以应用于大量以前未知的问题（一种通用能力）这一事实来证明的：工程师的头脑能够进行极限泛化。由于生成的程序只是对该过程的输出进行编码，因此它并不比用来记录定理证明的墨水和纸张更智能。

However, what of a program that is not hard-coded by humans, but trained from data to perform a task? A learning machine certainly may be intelligent: learning is a necessary condition to adapt to new information and acquire new skills. But being programmed through exposure to data is no guarantee of generalization or intelligence. Hard-coding prior knowledge into an AI is not the only way to artificially “buy” performance on the target task without inducing any generalization power. There is another way: adding more training data, which can augment skill in a specific vertical or task without affecting generalization whatsoever.	

然而，如果一个程序不是由人类硬编码的，而是由数据训练来执行任务的呢？学习机器当然可能是智能的：学习是适应新信息和获得新技能的必要条件。但通过接触数据来编程并不能保证泛化或保证拥有智能。将先验知识硬编码到人工智能中并不是在不产生泛化能力的情况下人为“购买”目标任务性能的唯一方法。还有另一种方法：添加更多的训练数据，可以在不影响泛化的情况下加强对特定任务的技能。

Information processing systems form a spectrum between two extremes: on one end, static systems that consist entirely of hard-coded priors (such as DeepBlue or our if/else chatbot example), and on the opposite end, systems that incorporate very few priors and are almost entirely programmed via exposure to data (such as a hash table or a densely connected neural network). Most intelligent systems, including humans and animals, combine ample amounts of both priors and experience, as we point out in II.1.3. Crucially, the ability to generalize is an axis that runs orthogonal to the prior/experience plane. Given a learning system capable of achieving a certain level of generalization, modifying the system by incorporating more priors or more training data about the task can lead to greater task-specific performance without affecting generalization. In this case, both priors and experience serve as a way to “game” any given test of skill without having to display the sort of general-purpose abilities that humans would rely on to acquire the same skill.	

信息处理系统之间形成一个频谱两个极端：一端是静态系统，完全由硬编码的先验（如深蓝或if/else聊天机器人的例子）；另一端的系统将很少有先验，几乎完全是通过接触数据来编程（如哈希表或紧密连接神经网络）。正如我们在II.1.3中所指出的，大多数智能系统，包括人类和动物，结合了大量的先验和经验。至关重要的是，泛化能力是一个与先前/经验平面正交的轴。如果一个学习系统能够达到一定程度的泛化，那么在不影响泛化的情况下，通过加入更多的先验或更多的关于任务的训练数据来修改系统，可以获得更好的任务特定性能。这样看来，先验和经验都可以作为“玩”任意给定技能测试的方法，而不必显示人类获取相同技能所依赖的那种通用能力。

This can be readily demonstrated with a simple example: consider a hash table that uses a locality-sensitive hash function (e.g. nearest neighbor) to map new inputs to previously seen inputs. Such a system implements a learning algorithm capable of local generalization, the extent of which is fixed (independent of the amount of data seen), determined only by the abstraction capabilities of the hash function. This system, despite only featuring trace amounts of generalization power, is already sufficient to “solve” any task for which unlimited training data can be generated, such as any video game. All that one has to do is obtain a dense sampling of the space of situations that needs to be covered, and associate each situation with an appropriate action vector.	

这可以用一个简单的例子来说明:假设一个哈希表使用了一个位置敏感的哈希函数(例如，最近邻)来将新的输入映射到以前看到的输入。这样的系统实现了一种能够局部泛化的学习算法，其范围是固定的(与所看到的数据量无关)，仅由散列函数的抽象能力决定。这个系统，虽然只有少量的泛化能力，但已经足够“解决”任何可以生成无限训练数据的任务，比如任何视频游戏。你所要做的就是获取需要覆盖的情况空间的密集采样，并将每个情况与适当的行动向量关联起来。

Adding ever more data to a local-generalization learning system is certainly a fair strategy if one’s end goal is skill on the task considered, but it will not lead to generalization beyond the data the system has seen (the resulting system is still very brittle, e.g. Deep Learning models such as OpenAI Five), and crucially, developing such systems does not teach us anything about achieving flexibility and generality. “Solving” any given task with beyond-human level performance by leveraging either unlimited priors or unlimited data does not bring us any closer to broad AI or general AI, whether the task is chess, football, or any e-sport.	

如果一个人的最终目标是学到在考虑范围之内的任务的技能，向局部泛化学习系统添加更多的数据当然是一个合理的，但它不会产生超出了系统所见数据以外的泛化（由此产生的系统仍然非常脆弱，如OpenAI Five等深度学习模型），更为关键的是，开发这样的系统并没有告诉我们任何关于实现灵活性和通用性的东西。利用无限的先验或无限的数据“解决”任何具有超越人类水平性能的给定任务，都不会让我们更接近通用人工智能，无论是国际象棋、足球还是任何电子运动。

Current evidence (e.g. [51, 46, 16, 59, 50]) points to the fact that contemporary Deep Learning models are local-generalization systems, conceptually similar to a locality-sensitive hash table – they may be trained to achieve arbitrary levels of skill at any task, but doing so requires a dense sampling of the input-cross-target space considered (as outlined in [16]), which is impractical to obtain for high-value real-world applications, such as L5 self-driving (e.g. [5] notes that 30 million training situations is not enough for a Deep Learning model to learn to drive a car in a plain supervised setting). Hypothetically, it may be shown in the future that methods derived from Deep Learning could be capable of stronger forms of generalization, but demonstrating this cannot be done merely by achieving high skill, such as beating humans at DotA2 or Starcraft given unlimited data or unlimited engineering; instead, one should seek to precisely establish and quantify the generalization strength of such systems (e.g. by considering prior-efficiency and data-efficiency in skill acquisition, as well as the developer-aware generalization difficulty of the tasks considered). A central point of this document is to provide a formal framework for doing so (II.2andII.3). Failing to account for priors, experience, and generalization difficulty in our evaluation methods will prevent our field from climbing higher along the spectrum of generalization (I.3.2) and from eventually reaching general AI.	

现有的证据(例如[51, 46, 16, 59, 50])指出，当前的深度学习模型是局部泛化的系统，在概念上类似于局部敏感的哈希表(locality-sensitive hash table)——他们可能在任意任务中被训练以实现任意水平的技能，但实现这一点，需要对所关心的输入-目标空间(input-cross-target space)进行密集采样（如[16]中描述），这对于高代价的实际应用来说是不切实际的，比如L5的自动驾驶（例如[5]指出，3000万的训练情况不足以让深度学习模型学习驾驶汽车in a plain supervised setting）。假如，未来或许源自深度学习的方法可具有更强的泛化能力，但证明这样做不能仅仅通过实现高技能，如在给定无限数据或无限工程条件的条件下在DotA2或星际争霸中击败人类；相反，应该设法精确地建立和量化这类系统的泛化强度（例如，通过考虑技能获取中的先验效率和数据效率，以及所考虑任务的开发人员认识到的泛化难度）。本文件的中心要点是提供这样做的正式框架(II.2和II.3)。要沿着泛化谱(I.3.2)爬高，并最终达到一般AI，就要在我们的评估方法中考虑先验、经验和泛化难度。

In summary, the hallmark of broad abilities (including general intelligence, as per II.1.2) is the power to adapt to change, acquire skills, and solve previously unseen problems – not skill itself, which is merely the crystallized output of the process of intelligence. Testing for skill at a task that is known in advance to system developers (as is the current trend in general AI research) can be gamed without displaying intelligence, in two ways: 1) unlimited prior knowledge, 2) unlimited training data. To actually assess broad abilities, and thus make progress toward flexible AI and eventually general AI, it is imperative that we control for priors, experience, and generalization difficulty in our evaluation methods, in a rigorous and quantitative way.	

总之，broad abilities （包括通用智能，如II.1.2）的标志是适应变化，获得技能，并解决以前未见过的问题的能力——而不是技能本身，技能只是智能过程的结晶。在一项系统开发人员事先就知道的任务中测试技能（这是当前人工智能研究的趋势），可以在不显示智能的情况下进行，方法有两种：1)无限的先验知识，2)无限的训练数据。要真正评估广泛的能力，从而在灵活的人工智能和最终的通用人工智能方面取得进展，我们必须在评估方法中严格和定量地控制先验、经验和泛化难度。
