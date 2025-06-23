# amazon-esci-product-embeddings

Dataset used: https://github.com/amazon-science/esci-data/tree/main

Phase 1:
Get subset of products, Map all products into a vector collection (FAISS) simple fast and efficient to run locally,
Embedding model, multilingual dataset, 
~~
Choosing parallel trained embedding modles across multiple langualges, such that similar sentences in different languages are embedded close in vecotr space fintuned forsentence similarity.

Some considerations all supports (spanish, english, japanese)
1. LaBSE (Language-agnostic BERT Sentence Embedding)
2.  distiluse-base-multilingual-cased-v1 (smaller)
~~

Seems like there is no cross language product recommendation, 
thus, using separate collections and language-specific encoders is a smart and recommended.
If you're optimizing for speed + similarity search quality, and don't need perfect monolingual tuning:

Use:
jinaai/jina-embedding-v2-small-en for English
intfloat/multilingual-e5-small for Spanish & Japanese

The choice of library huggingface's transformers vs sentece-tranformers, they are not choice
they are for different task, and one will perform well in one area than others

Transformers requires manual pooling and often yields non-ideal sentence embeddings (e.g., using the CLS token out-of-the-box) 

Sentence‑Transformers:
Automatically integrates pooling (mean/max/CLS).
Fine-tunes underlying Transformers with training regimes (e.g., siamese networks and contrastive loss) to optimize embedding quality for similarity tasks 

📌 TL;DR
Use Transformers for broad NLP tasks: token-level outputs, classification, generation.
Use Sentence‑Transformers when you need high-quality sentence embeddings—they’re optimized, easy to use, and efficient for semantic tasks.

The Sentence‑Transformers library uses a flexible Pooling module that supports multiple strategies:
Mean pooling (the default): computes the average of all token embeddings, offering a strong and reliable representation for sentence-level embeddings.

CLS pooling (optional): takes just the [CLS] token embedding. You can enable it via setting pooling_mode="cls" or using pooling_mode_cls_token=True in the Pooling configuration.
Other modes: max pooling, mean_sqrt_len, weighted mean, last token, etc. 

🧠 Why mean pooling is preferred
Empirical results show mean pooling outperforms CLS pooling for semantic similarity and retrieval tasks, since pooling uses context from all tokens, not just one.

CLS was initially trained for classification-related tasks, so without fine-tuning, it may not capture full sentence semantics as effectively.

✅ Summary
By default, Sentence‑Transformers uses mean pooling.
You can switch to CLS pooling (or hybrids) by customizing the Pooling layer.
Mean pooling typically gives better performance for sentence embeddings out of the box.

Why Prefixing Matters in intfloat/multilingual-e5-small
When using the intfloat/multilingual-e5-small model for retrieval tasks, it's essential to prefix inputs with "query: " for search queries and "passage: " for documents or content. This is not optional—it is a crucial design feature of how the model was trained using contrastive learning.

🎯 Key Reasons to Prefix
Role Awareness
Prefixing enables the model to understand the role of each input. During training, the E5 architecture distinguishes between queries and passages. The prefix helps the model adjust its internal attention and pooling strategies based on whether the input is a search query or a document.

Improved Embedding Alignment
Clearly marked inputs ensure that embeddings from queries and passages are more semantically aligned. This alignment increases the chance of successful retrieval since both embeddings are positioned closer in the vector space when they share meaning.

Avoiding Performance Drop
Omitting these prefixes can cause a noticeable decline in accuracy for retrieval or ranking tasks. The model may misinterpret the role of an input, leading to poor embedding quality and reduced effectiveness in downstream tasks like search, recommendation, or ranking.





