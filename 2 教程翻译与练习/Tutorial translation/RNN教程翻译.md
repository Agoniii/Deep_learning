# RNN教程 Part1 - 介绍RNN

[教程来源](http://www.wildml.com/2015/09/recurrent-neural-networks-tutorial-part-1-introduction-to-rnns/)

递归神经网络(RNNs)是非常流行的模型，它在许多自然语言处理(NLP)的任务上取得了非常好的效果。然而，尽管RNNs近期十分火热，我只发现非常有限的一些资源来系统解释RNNs如何工作，以及如何使用它们。而这就是这篇教程所要讲述的内容。我将这个系列分成多个部分，并打算覆盖以下内容：

1. 介绍RNNs(本篇)
2. [使用Python和Theano来实现一个RNN](http://www.wildml.com/2015/09/recurrent-neural-networks-tutorial-part-2-implementing-a-language-model-rnn-with-python-numpy-and-theano/)
3. [理解Backpropagation Through Time(BPTT)算法和vanishing gradient problem(梯度消失问题)](http://www.wildml.com/2015/10/recurrent-neural-networks-tutorial-part-3-backpropagation-through-time-and-vanishing-gradients/)
4. [实现一个GRU/LSTM RNN](http://www.wildml.com/2015/10/recurrent-neural-network-tutorial-part-4-implementing-a-grulstm-rnn-with-python-and-theano/)

作为教程的一部分，我们将实现一个[基于迭代神经网络的语言模型](http://www.fit.vutbr.cz/research/groups/speech/publi/2010/mikolov_interspeech2010_IS100722.pdf)。语言模型的应用有两个方面：其一，它允许我们基于它们在现实世界中出现的可能性为任意语句打分。这给了我们关于语法和语义正确性的度量方法。这种模型通常作为Machine Translation systems(机器翻译系统)的一部分来使用。其二，语言模型能够生成新的文本(我认为这是更屌的应用之处)。在莎士比亚的文集上训练语言模型可以使其生成具有莎士比亚文风的文章。在[这篇有趣的公开文章](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)中，Andrej Karpathy( CS231n前半部分的讲解老师，非常帅且有激情)阐述了基于RNNs的字符级语言模型可以做什么。

这里假设你至少有些熟悉基本的神经网络。如果很遗憾你对这方面没有什么了解，~~请你关掉教程然后该干嘛干嘛去~~你也许希望去看看[Implementing A Neural Network From Scratch](http://www.wildml.com/2015/09/implementing-a-neural-network-from-scratch/)，这会引导你去了解非迭代的神经网络背后的思想和实现。

##什么是RNN？

RNNs背后蕴含的思想是利用序列信息。在传统的神经网络中，我们假设所有输入(以及输出)都是彼此独立的。但这对于许多任务来说是个非常糟糕想法。如果我们想预测语句中的下一个单词，知道在此之前有什么单词肯定是比较好的。RNNs之所以被称为*recurrent*(迭代)是因为它们会对语句中的每个元素执行相同的任务，并且输出都依赖于之前的计算。另一种理解RNNs的方式是它们拥有"memory"，它会捕捉迄今为止的计算信息。理论上讲，RNNs可以利用任意长语句的信息，但实际上，它们被限制以至于只能往回看很少的步数(看不懂没事后面会再说，这边我也不知道咋翻译比较好了)。这里是一个典型的RNN的样子：

![Nature上的RNN图](./img/rnn.jpg)
```
    RNN和在进行前向计算时的展开形式.来源：Nature
```

上图展示了RNN展开成一个完整网络的样子。这里我们所说的“展开”只是指一个表达完整的语句的网络。例如，我们所关心的语句具有5个单词，那么网络将会展开成一个5层的神经网络，一层表示一个单词。RNN的计算所遵循的公式如下所示：

* $x_{t}$是时间步数$t$时的输入。
* 



