### Recursive Deep Models for Semantic Compositionality Over a Sentiment Treebank [link](https://aclanthology.org/D13-1170.pdf)
##### **Citation**: Richard Socher, Alex Perelygin, Jean Wu, Jason Chuang, Christopher D. Manning, Andrew Ng, and Christopher Potts. Recursive deep models for semantic compositionality over a sentiment treebank. In David Yarowsky, Timothy Baldwin, Anna Korhonen, Karen Livescu, and Steven Bethard, editors, Proceedings of the 2013 Conference on Empirical Methods in Natural Language Processing, pages 1631–1642, Seattle, Washington, USA, October 2013. Association for Computational Linguistics.

- Code stuff: [link](https://paperswithcode.com/paper/recursive-deep-models-for-semantic)
____
- Significant Contributions (at the time):
    - semantic word spaces never before good at encoding meaning of longer sentences
    - introduce sentiment treebank
    - Recursive Neural Tensor Network
    - Outperforms 2013 SOTA, and adds 9.7% improvement in one sentence +/- sentiment analysis over baseline bag of features models
    - captures effects and scope of negation
- the treebank is significant and allows many models to be trained from it, and the phrases were vetted by multiple humans judges
- aim to obtain better sentiment data from short comments e.g. Twitter data
- address compositional effect of semantic vector spaces (hard to compose long phrases, not encoded well enough)
- Foundational RNN Papers: (Socher et al., 2011a-b) [link1](https://icml.cc/2011/papers/125_icmlpaper.pdf), [link2](https://aclanthology.org/D11-1014.pdf) 