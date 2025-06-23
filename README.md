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

