[![PyPI
version](https://badge.fury.io/py/fastnode2vec.svg)](https://badge.fury.io/py/fastnode2vec)

# fastnode2vec

*Really* fast implementation of node2vec based on [numba](https://numba.pydata.org/) and [gensim](https://radimrehurek.com/gensim/).

## API

`Node2Vec` inherits from gensim's [`Word2Vec`](https://radimrehurek.com/gensim/models/word2vec.html), all its APi is valid.

```python
from fastnode2vec import Graph, Node2Vec

graph = Graph([("a", "b"), ("b", "c"), ("c", "a"), ("a", "d")],
              directed=False)

n2v = Node2Vec(graph, dim=10, walk_length=100, context=10, p=2.0, q=0.5, workers=2)

n2v.train(epochs=100)

print(n2v.wv["a"])
```

## CLI

Compute embeddings of the [Gnutella peer-to-peer network](https://snap.stanford.edu/data/p2p-Gnutella08.html):

```
wget https://snap.stanford.edu/data/p2p-Gnutella08.txt.gz
fastnode2vec p2p-Gnutella08.txt.gz --dim 16 --walk-length 100 --epochs 10 --workers 2
```

## Load embeddings produced by the CLI

Just use the [`Word2Vec`](https://radimrehurek.com/gensim/models/word2vec.html) API.

```python
from gensim.models import KeyedVectors

wv = KeyedVectors.load("p2p-Gnutella08.txt.gz.wv", mmap='r')
```