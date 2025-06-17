<img src="assets/system-overview.png" alt="System Diagram" style="width: 60%;">

## Project Information

### Project Description
A hybrid patent‐search system was developed that combines:
- **Semantic Query Expansion**  
  Uses domain-specific embeddings (trained on patent text) to identify and add relevant synonyms, acronyms, and related terms to the user’s original query. This yields broader recall without sacrificing precision.
- **Vector-Based Retrieval**  
  Leverages FAISS to perform approximate nearest-neighbor search over high-dimensional sentence embeddings of patent abstracts and claims, enabling fast look-up of semantically similar documents.
- **BM25 Ranking**  
  Applies Elasticsearch’s BM25 algorithm to compute lexical relevance scores; these scores are then combined with vector-similarity scores in a weighted fusion layer to balance exact-term matches and semantic matches.
- **CPC Classification**  
  A RoBERTa transformer fine-tuned on 100 000 labeled patent abstracts predicts Cooperative Patent Classification (CPC) codes, allowing users to filter search results by technological domain (e.g., “Aerospace,” “Propulsion,” “Materials”).
- **CLIP-Driven Visual Search**  
  Processes user-supplied images (diagrams, drawings) through OpenAI’s CLIP model to generate image embeddings. These are compared to embeddings of figure images extracted from patents, enabling “find patents with similar diagrams” functionality.

Together, these components deliver a **precision of 87%** and **recall of 82%** on benchmark patent-retrieval tasks, reducing manual review time by 45%.

### Course Code
- **BBM480**

### Objectives
1. Accelerate patent prior-art search for aerospace engineers  
2. Improve retrieval accuracy across multilingual patent filings  
3. Integrate visual and text modalities for richer search experiences  

### Methodology
- **Data Collection  
A corpus of **15 000 English-language** and **5 000 Turkish-language** patents (filed between 2010 and 2023) was assembled. Metadata (titles, abstracts, claims) and full-text PDFs were sourced via public patent offices and curated for redundancy. This bilingual dataset ensures robust evaluation across both global and local (Turkish) contexts.

- **Preprocessing  
All PDF documents underwent **OCR cleaning** to correct recognition errors and remove artifacts. Metadata fields were **normalized** (e.g., date formats, inventor names) and standardized. Sentence-Transformers were then used to generate **768-dimensional embeddings** for each abstract and claim, producing a unified vector representation for semantic search.

- **Indexing  
Two complementary indices were built:  
- An **Elasticsearch BM25 index** for fast lexical retrieval and relevance scoring.  
- A **FAISS vector index** (IVF-PQ) for approximate nearest-neighbor search over the embedding space.  
At query time, results from both indices are merged via a weighted fusion algorithm.

- **Classification  
A **RoBERTa transformer** was fine-tuned on **100 000** manually labeled patent abstracts to predict Cooperative Patent Classification (CPC) codes. The classifier achieves **92% top-3 accuracy**, enabling users to restrict searches to specific technological fields (e.g., A—“Human Necessities,” B—“Operations & Transport”).

- **Visual Search  
Patent figures were extracted and passed through **OpenAI’s CLIP** to produce image embeddings. At search time, user-supplied diagrams are encoded and compared against this gallery, allowing retrieval of patents with visually similar figures (e.g., mechanical drawings, flowcharts).

### Key Findings
- **Semantic expansion** increased recall by 12% over BM25 alone  
- **Multimodal fusion** (text + images) improved mean average precision (mAP) by 9%  
- **User study (n=12 engineers):** 92% rated the system “very helpful” in identifying relevant prior art  

### Supporting Institutions
- **TUSAŞ** (Turkish Aerospace Industries)  
- **TÜBİTAK 2209-B** (University Students’ Industry-Oriented Research Projects)  
- **SIU 2025** (IEEE Signal Processing and Communication Applications Conference)

---

## Poster

<div style="width:100%; height:600px; margin: 1em 0;">
  <iframe
    src="assets/480_poster.pdf"
    width="100%"
    height="100%"
    style="border:1px solid #ccc;"
    title="Project Poster PDF">
    Your browser does not support embedded PDFs.
    <a href="/poster.pdf">Download the PDF</a>.
  </iframe>
</div>
