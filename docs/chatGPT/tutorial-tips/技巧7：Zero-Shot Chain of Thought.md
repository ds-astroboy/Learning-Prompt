---
sidebar_position: 7
---
<head>
  <script defer="defer" src="https://embed.trydyno.com/embedder.js"></script>
  <link href="https://embed.trydyno.com/embedder.css" rel="stylesheet" />
</head>


基于上述的第三点缺点，研究人员就找到了一个叫 Chain of Thought 的技巧。

这个技巧使用起来非常简单，只需要在问题的结尾里放一句 `Let‘s think step by step` （让我们一步步地思考），模型输出的答案会更加准确。

这个技巧来自于 Kojima 等人 2022 年的论文 [Large Language Models are Zero-Shot Reasoners](https://arxiv.org/abs/2205.11916)。在论文里提到，当我们向模型提一个逻辑推理问题时，模型返回了一个错误的答案，但如果我们在问题最后加入 `Let‘s think step by step` 这句话之后，模型就生成了正确的答案：

![ZeroShotChainOfThought001.png](./assets/ZeroShotChainOfThought001.png)

论文里有讲到原因，感兴趣的朋友可以去看看，我简单解释下为什么（🆘 如果你有更好的解释，不妨反馈给我）：

1. 首先各位要清楚像 ChatGPT 这类产品，它是一个统计语言模型，本质上是基于过去看到过的所有数据，用统计学意义上的预测结果进行下一步的输出（这也就是为什么你在使用 ChatGPT 的时候，它的答案是一个字一个字地吐出来，而不是直接给你的原因，因为答案是一个字一个字算出来的）。
2. 当它拿到的数据里有逻辑，它就会通过统计学的方法将这些逻辑找出来，并将这些逻辑呈现给你，让你感觉到它的回答很有逻辑。
3. 在计算的过程中，模型会进行很多假设运算（不过暂时不知道它是怎么算的）。比如解决某个问题是从 A 到 B 再到 C，中间有很多假设。
4. 它第一次算出来的答案错误的原因，只是因为它在中间跳过了一些步骤（B）。而让模型一步步地思考，则有助于其按照完整的逻辑链（A > B > C）去运算，而不会跳过某些假设，最后算出正确的答案。

按照论文里的解释，零样本思维链涉及两个补全结果，左侧气泡表示基于提示输出的第一次的结果，右侧气泡表示其收到了第一次结果后，将最开始的提示一起拿去运算，最后得出了正确的答案：

![ZeroShotChainOfThought002.png](./assets/ZeroShotChainOfThought002.png)

这个技巧，用于解复杂问题有用外，还适合生成一些连贯主题的内容，比如写长篇文章、电影剧本等。

但需要注意其缺点，连贯不代表，它就一定不会算错，如果其中某一步骤算错了，错误会因为逻辑链，逐步将错误积累，导致生成的文本可能出现与预期不符的内容。

另外，根据 Wei 等人在 [2022 年的论文](https://arxiv.org/pdf/2201.11903.pdf)表明，还有它仅在大于等于 100B 参数的模型中使用才会有效。如果你使用的是小样本模型，这个方法不会生效。

---

2023-04-12 更新（感谢[qq-740943515](https://github.com/qq-740943515)分享）：
根据 Yongchao Zhou 等人的[最新论文](https://sites.google.com/view/automatic-prompt-engineer)，更好的 prompt 是:
```
Let's work this out in a step by step way to be sure we have the right answer.
```

