# sentencepiece
sentencepiece是Google提出的一种**无监督的文本tokenizer和detokenizer工具**，一般用于**文本生成**的神经网络系统中。  
主要功能是对sub-word unit概念的重新定义实现 有两种模式：BPE和unigram language model  

## token数量确定
大多数的无监督分词算法是采用无限制的词汇表大小，而sentencepiece则采用的是**固定大小的词汇表** 如8k, 16k, 32k  

## 从原始文本中训练
之前的sub-word实现都需要假设输入的句子是pre-tokenized的 这个约束是必须的  
但是这样的模式就会让训练过程变得复杂 实现这个前提很困难  
sentencepiece的实现速度足够快 **直接从原始句子中训练模型**  

## 将字符视为unicode
sentencepiece把**输入文本当做unicode字符进行处理** 空格同样地被认为是文本  
在这个前提下空格会被替换为"_" 如"Hello World." -> "Hello▁World." -> [Hello] [▁Wor] [ld] [.]  
这样的好处是tokenizer和detokenizer十分方便而且完全可逆 完全不依赖具体语言就可以完成  

## sub-word正则化和BPE-dropout
sub-word正则化和BPE-dropout是简单的正则化方法，通过动态子词采样虚拟地增加训练数据，这有助于提高NMT模型的准确性和鲁棒性。  

## 使用
首先通过文本训练一个SentencePieceTrainer 需要显示定义词表大小vocab_size 模式bpe/unigram等参数  
之后会得到一个.model文件和.vocab文件  
.vocab文件中是一个sub-word和id的对应表关系  
.model文件是能够使用的model  
通过这个.model文件就可以对文本进行sub-word处理  