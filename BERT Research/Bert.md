### [BERT Explained: State of the art language model for NLP](https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270):


Bidirectional Encoder Representations from Transformers: a bidirectional variant of Transformer Networks

The key innovation is applying the bidirectional training of Transformer, a popular attention model.

This is in contrast to previous efforts which looked at a text sequence either from left or right or combined left-to-right and right-to-left training.

The paper shows that a bidirectionally trained model can have a deeper understanding of language context and flow than single-direction language models.

In the original paper (by Google) the researchers detail a novel technique named Masked LM (MLM), which allows bidirectional training in models in which it was previously impossible.

### Background

In the field of computer vision, researchers have repeatedly shown the value of transfer learning - pre-training a neural network model on a known task. for Instance, ImageNet and then fine-tuning using the neural network as the basis of a new purpose-specific model. 
In recent years, researches have been showing that a similar technique can be useful in many natural language tasks.

A different approach, which is also popular in NLP tasks and exemplified in the recent ELMo paper, is feature-based training. In this approach, a pre-trained neural network produces word embeddings, these embeddings are then used as features in NLP models.


### [BERT Explained: A Complete Guide with Theory and Tutorial]https://medium.com/@samia.khalid/bert-explained-a-complete-guide-with-theory-and-tutorial-3ac9ebc8fa7c
[The detailed workings of Transformer are described in a paper by Google](https://arxiv.org/pdf/1706.03762.pdf)
### How BERT works
