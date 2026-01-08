# ViDoRe Text-to-Document Image Retrieval Dataset Export

## Dataset Information

- **Dataset**: ViDoRe_arxivqa
- **Source**: vidore/arxivqa_test_subsampled_beir
- **Split**: test
- **Language**: All languages

## Loading the dataset

```python
from datasets import load_from_disk

dataset = load_from_disk("data/examples/arxivqa")

# Access subsets
queries = dataset["queries"]
corpus = dataset["corpus"]
relevant_docs = dataset["relevant_docs"]
```

## Dataset Structure

- **queries**: 2 samples (text queries)
  - query_id: unique query identifier
  - text: text query for document retrieval
  - instruction: retrieval task instruction ("Find a document image that matches the given query.")

- **corpus**: 10 samples (document images)
  - corpus_id: unique corpus identifier
  - image_path: path to document image file (relative to dataset root)

- **relevant_docs**: 2 samples
  - query_id: corresponding query ID
  - corpus_ids: list of relevant document corpus IDs
  - scores: relevance scores for each document (higher is more relevant)

## Task Description

**Task Type**: Text-to-Document Image Retrieval

**Instruction**: Find a document image that matches the given query.

This is a text-to-document image retrieval task where text queries are matched to relevant document images from the corpus. Documents may have varying degrees of relevance indicated by the relevance scores.

## Images

Document images are stored in: `images/`

All images are in PNG format.

## Statistics

- Total document images: 10
- Total text queries: 2
- Average relevant documents per query: 1.00

## Relevance Scores

The `scores` field in `relevant_docs` contains relevance judgments:
- Higher scores indicate higher relevance
- Scores typically range from 0 to 3 or similar scale
- Use these scores for evaluation metrics like NDCG

## Example Usage

```python
from datasets import load_from_disk

# Load dataset
dataset = load_from_disk("data/examples/arxivqa")
queries = dataset["queries"]
corpus = dataset["corpus"]
relevant_docs = dataset["relevant_docs"]

# Get a query and its relevant documents
query = queries[0]
print(f"Query: {query['text']}")

# Find relevant documents
relevant = relevant_docs[0]
print(f"Relevant document IDs: {relevant['corpus_ids']}")
print(f"Relevance scores: {relevant['scores']}")

# Load relevant document images
for corpus_id, score in zip(relevant['corpus_ids'], relevant['scores']):
    doc = corpus.filter(lambda x: x['corpus_id'] == corpus_id)[0]
    print(f"Document {corpus_id} (score={score}): {doc['image_path']}")
```

## Citation

Please cite the original ViDoRe dataset:

```bibtex
@article{faysse2024colpali,
  title={ColPali: Efficient Document Retrieval with Vision Language Models},
  author={Faysse, Manuel and Laurençon, Hugues and others},
  journal={arXiv preprint},
  year={2024}
}
```
