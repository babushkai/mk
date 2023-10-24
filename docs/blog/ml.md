## Using LLM for MLOps
Keywords: Python, Cloud, MLOps, ChatGPT

The development pace in the Large Language Model (LLM) space has exploded over the past few months. One of the most intriguing stories is the rapid transition to a new tech stack that supports entirely new engagement patterns with these language models. In this blog post, we'll explore the changes being made in the LLM tech stack and what they mean for developers.

### Existing NLP Tech Stack
Until recently, NLP developers relied on a tech stack optimized for NLP tasks like text classification, named entity recognition, and named entity disambiguation. This tech stack typically comprises data preprocessing pipelines, machine learning pipelines, and various databases for storing embeddings and structured data. This architecture efficiently produced vast triples, word embeddings, sentence embeddings, outputs from sequence to sequence, language model probabilities, and attention weights. Developers typically stored these structured outputs in databases like ElasticSearch, Postgres, or Neo4j, leveraging them as knowledge graphs that users (or services) can query.

This architecture was suitable for generating reliable structured data that could be deployed within enterprise systems to automate major processes, such as classifying documents or searching for relationships between entities. However, its adoption was challenged due to slow startups (requiring large amounts of labeled data and considerable model fine-tuning) and the cost of operation (often incorporating over 30 models in these architectures). Ingestion pipelines and model pipelines were also vulnerable to new document layouts and data types.

### Emerging LLM Tech Stack
Since the fall of 2022, a new tech stack designed to fully harness the potential of LLMs has begun to emerge. Unlike the previous stack, this one aims to enable text generation, a task at which the latest LLMs excel compared to previous machine learning models. This new stack consists of four pillars: data preprocessing pipelines, embedding endpoints + vector stores, LLM endpoints, and LLM programming frameworks. There are several significant differences between the old tech stack and the new one. Firstly, LLMs like ChatGPT, Claude, and Flan T-5 contain much more information encoded than earlier models like GPT-2, so the new tech stack does not rely on knowledge graphs that store structured data (like triples). Secondly, the new tech stack uses ready-made LLM endpoints as models instead of custom-built ML pipelines, at least to start. This substantially reduces the time developers spend today on training specialized information extraction models (like named entity recognition, relation extraction, sentiment, etc.), allowing solutions to be crafted in minimal time (and cost).

Transitioning to this new LLM tech stack is an exciting development that empowers developers to build and deploy more robust NLP applications. The new stack is more efficient, scalable, user-friendly, and maximizes the potential of LLMs. As developers continue to explore new ways to leverage the power of LLMs, we can expect to see even more innovations in this field over the next several months to years.




