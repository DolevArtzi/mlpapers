### TimeLMs: Diachronic Language Models from Twitter [link](https://arxiv.org/pdf/2202.03829.pdf)
##### **Citation**: Daniel Loureiro,Francesco Barbieri,Leonardo Neves,Luis EspinosaAnke,and José Camacho-Collados. 2022. TimeLMs: Diachronic Language Models from Twitter. CoRR abs/2202.03829 (2022). arXiv:2202.03829 https://arxiv.org/abs/2202. 03829
- Model: [twitter-roberta-base-sentiment-latest](https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment-latest)
- Github: [link](https://github.com/cardiffnlp/timelms)
____

- lack of diachronic specialization in contexts like social media when it comes to LMs
- **time specific LLMs specialized to Twitter data**
- putting time variable at the core of the framework
- similar temporal adaptation showed training LMs with recent data can be beneficial: ([Agarwal and Nenkova, 2021](https://arxiv.org/pdf/2111.12790.pdf); [Jin et al., 2021](https://arxiv.org/pdf/2110.08534.pdf))
- social media specific: start with [BERTweet](https://aclanthology.org/2020.emnlp-demos.2/)
- Twitter Academic API?
- interesting math paper on document representation + hashing [link](https://www.cs.princeton.edu/courses/archive/spring13/cos598C/broder97resemblance.pdf)
- continual training every three months on top of base model
- Masked Language Model is the base (RoBERTa)
- Evaluation: TweetEval, **Pseudo-perplexity**
    [Pseudo-Perplexity Measure](https://aclanthology.org/2020.acl-main.240.pdf) <-- looks pretty interesting 
#### [TweetEval](https://github.com/cardiffnlp/tweeteval)
- emotion recognition, emoji prediction, irony detection, hate speech detection, offensive language detection, sentiment analysis: positive, neutral, negative, and stance detection: favor, neutral against
    - note: sentiment analysis training is focused on abortion, atheism, climate change, feminism, Hillary Clinton. Sounds reasonably diverse though
    - also: minimal training data on stance detection, less recent *opportunity for fine-tuning?*

![tweeteval_leaderboard](https://github.com/DolevArtzi/hellotwitterworld/assets/85849407/4fcfdd49-8d77-4736-935b-fe8e2212e0f3)
- TimeLM-21 is pretty much as good or better than BERTweet, with less data
#### Pseudo Perplexity 
- iterative replacement of tokens with mask, sum corresponding logits 
- observed gradual degradation of older models on newer data
![model_degradation](https://github.com/DolevArtzi/hellotwitterworld/assets/85849407/9a08f809-9a82-4fa6-8055-9de3b177c770)
- also, be aware of covid weirdness, or other major global events being harder to handle
![covid_weirdness](https://github.com/DolevArtzi/hellotwitterworld/assets/85849407/78b680b4-c19f-447b-89b7-622044b21396)
- further model limitations to be aware of [discussion for this model on HF](https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment-latest/discussions/14)
