---
title: Hello World
tags: [ge2e]
categories:
    - [DL, LSTM]
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

```python
def get_cossim_my(embeddings):
    b, n, v = embeddings.shape
    left = embeddings.repeat(1, 1, embeddings.size(0)).view(b, n, b, v)
    centroids = embeddings.mean(dim=1)
    right = centroids.repeat(b, n, 1, 1)
    s = embeddings.sum(dim=-2, keepdim=True) - embeddings
    s = s / (embeddings.shape[1] - 1)
    for b_idx in range(b):
        for utt_idx in range(n):
            right[b_idx][utt_idx][b_idx] = s[b_idx][utt_idx]
    sim = F.cosine_similarity(left, right, dim=-1)
    return sim 
```