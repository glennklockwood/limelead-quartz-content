The datasets used in [[LLM training]] exist in several formats:

**Raw training data** are text, images, audio files, that have not been wedged into a shape that the model training framework can accept. For text-based data, this is often raw HTML exactly as it was scraped from the Internet.

**Tokenized training data** is significantly smaller than raw training data and is ready to consume by the model training process.  For large language models, this means tokenized text, tokenized images, and tokenized audio.

As data is converted from raw to tokenized, it exists in various intermediate formats; for example, most open training datasets are distributed as collections of documents encoded, with metadata, in json or jsonl.

## Storage requirements

### Raw data

The process of converting raw text-based training data to a high-quality collection of text documents has been document as a 67&times;.[^wanjuan-cc]

### Tokenized data

The average size of a single token (in bytes) is variable:

- [According to OpenAI][what are tokens], a token in a typical English-language dataset is about four bytes.
- [The Pile][] paper has both tokens and words for different datasets and found an average 3.41 bytes per token.
- WanJuan-CC reported an average of 4.37 to 4.45 bytes per token.[^wanjuan-cc]

Assuming:

- OPT-3 was 4.44 bytes/token per the [OPT-175][] paper appendix C.2
- The Pile dataset is 3.41 bytes/token
- Everything else is OpenAI's 4 bytes/token

We can estimate the size (in GB) of various LLM training datasets:

| Dataset                 | Training tokens  | Training Bytes (est)             |
| ----------------------- | ---------------- | -------------------------------- |
| [Llama-3][]             | &gt; 15 trillion | 60 TB (54.6 TiB)                 |
| [LLaMa-2][] 70B         | 2.0 trillion     | 8 TB (7.3 TiB)                   |
| [OpenELM][]             | 1.5 trillion     | 6 TB (5.5 TiB)                   |
| [OPT-175][]             | 180 billion      | 800 GB (475 GiB)                 |
| [GPT-3][]               | 300 billion      | 1.2 TB (1.1 TiB)                 |
| [The Pile][]            | 260 billion      | 890 GB (830 GiB)                 |
| [ROOTS][]/[BLOOM][]     | 341 billion      | 1.6 TB (1.5 TiB)                 |
| [C4.en.noBlocklist][C4] | 156 billion      | 1,077 GB (1,003 GiB)<sup>1</sup> |

<sup>1</sup> C4's size is the size of the download (in TFDS format), not the size of the training tokens. I don't know how much TFDS overhead is included here, but the bytes per token for C4 comes out very high (6.9) which indicates TFDS is very inefficient.

[what are tokens]: https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them

[Llama-3]: https://github.com/meta-llama/llama3/blob/main/MODEL_CARD.md
[LLaMa-2]: https://arxiv.org/pdf/2307.09288.pdf
[OpenELM]: https://arxiv.org/pdf/2404.14619
[OPT-175]: https://arxiv.org/pdf/2205.01068.pdf
[GPT-3]: https://arxiv.org/pdf/2005.14165.pdf
[The Pile]: https://arxiv.org/pdf/2101.00027.pdf
[BLOOM]: https://arxiv.org/pdf/2211.05100.pdf
[ROOTS]: https://arxiv.org/pdf/2302.14035.pdf
[C4]: https://arxiv.org/pdf/2104.08758v1.pdf
[^wanjuan-cc]: [[2402.19282] WanJuan-CC: A Safe and High-Quality Open-sourced English Webtext Dataset (arxiv.org)](https://arxiv.org/abs/2402.19282)
## Computational requirements

### Text processing

Training large language models requires a significant amount of text data, and these data are often derived from massive amounts of html scraped from the Internet. The process of converting these web scrapes into tokenized datasets of high quality requires extensive data preprocessing which typically happens on CPUs that are good at processing large amounts of uneven, messy data in memory.

The process of converting web scrapes into clean, tokenized data is described in the following resources:

- The [Dolma][] dataset used the [CCNet][] processing pipeline
- [Deduplicating Training Data Makes Language Models Better](https://arxiv.org/abs/2107.06499)
- [RedPajama-Data: The RedPajama-Data repository](https://github.com/togethercomputer/RedPajama-Data) contains code for preparing large datasets for training large language models
- The [WanJuan-CC](https://arxiv.org/abs/2402.19282) subset of the Common Crawl dataset

[Dolma]: https://blog.allenai.org/dolma-3-trillion-tokens-open-llm-corpus-9a0ff4b8da64
[CCNet]: https://arxiv.org/abs/1911.00359

The [GPT-3 paper][] describes a very specific approach to data processing that relies on a combination of a few Apache Spark built-in tools:

- [Tokenizer](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.ml.feature.MinHashLSH.html) 
- [HashingTF](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.ml.feature.HashingTF.html)
- [Spark's MinHashLSH](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.ml.feature.MinHashLSH.html) with ten hashes

To identify overlaps between the training dataset and benchmark datasets, they also identified exact overlaps based on documents that had overlapping N-grams that ranged from 8-grams to 13-grams.

The [MM1 paper][] cited both the [GPT-3 paper][] and [CCNet][] as representative of their text processing pipeline.

[GPT-3 paper]: https://arxiv.org/abs/2005.14165
[MM1 paper]: https://arxiv.org/abs/2403.09611

### Multimodal dataset creation

The [MM1 paper][] cites the [OBELICS][] paper as a representation of how they constructed datasets that interleaved text and images for multimodal training. They specifically filter images based on aspect ratio, size, and URI contents. They deduplicate based on URL and MD5 across documents (images appearing more than ten times) and only retain the first copy of an image within each document that replicates an image.

[OBELICS]: https://arxiv.org/abs/2306.16527