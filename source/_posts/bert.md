---
title: bert
date: 2019-06-26 15:22:21
tags:
---

## What is `bert`?
![](/images/bert/figure1.png)


## Bert training
The rough process of training Bert is like: randomly mask some words in a sentence, then


## Data preparation
### Step 1
Collect Corpus and arrange them by rules: one sentence per line and Each document are separated by a blank line.
```
Bidirectional Encoder Representations from Transformers (BERT) has shown marvelous improvements across various NLP tasks. 
Recently, an upgraded version of BERT has been released with Whole Word Masking (WWM), which mitigate the drawbacks of masking partial WordPiece tokens in pre-training BERT. 
In this technical report, we adapt whole word masking in Chinese text, that masking the whole word instead of masking Chinese characters, which could bring another challenge in Masked Language Model (MLM) pre-training task. 
The model was trained on the latest Chinese Wikipedia dump. 

We aim to provide easy extensibility and better performance for Chinese BERT without changing any neural architecture or even hyper-parameters. 
The model is verified on various NLP tasks, across sentence-level to document-level, including sentiment classification (ChnSentiCorp, Sina Weibo), named entity recognition (People Daily, MSRA-NER), natural language inference (XNLI), sentence pair matching (LCQMC, BQ Corpus), and machine reading comprehension (CMRC 2018, DRCD, CAIL RC). 
Experimental results on these datasets show that the whole word masking could bring another significant gain. Moreover, we also examine the effectiveness of Chinese pre-trained models: BERT, ERNIE, BERT-wwm. 
```


## Source Code Anatomy

### `TrainingInstance`
`tokens`: tokens (words) after being masked
`segment_ids`: [0,0,0,0,1,1,1]
`is_random_next`: If the second segment is the next sentence of the first one
`masked_lm_positions`: [5, 10, 34, 63], masked tokens' index
`masked_lm_labels`: ['we', 'some', 'body', 'good'], the acctual token befor being replaced.

