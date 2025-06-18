<img src="assets/system-overview.png" alt="System Diagram" style="width: 60%;">

## Project Information

### Project Description
A hybrid patent‐search system was developed that combines:
- **Semantic Query Expansion**  
  Uses domain-specific embeddings (trained on patent text) to identify and add relevant synonyms, acronyms, and related terms to the user’s original query. This yields broader recall without sacrificing precision.
- **Vector-Based Retrieval**  
  Leverages FAISS to perform approximate nearest-neighbor search over high-dimensional sentence embeddings of patent titles, abstracts and claims, enabling fast look-up of semantically similar documents.
- **BM25 Ranking**  
  Applies Elasticsearch’s BM25 algorithm to compute lexical relevance scores; these scores are then combined with vector-similarity scores in a weighted fusion layer to balance exact-term matches and semantic matches.
- **CPC Classification**  
  BERT-based classificiation models fine-tuned on 90k labeled patent abstracts predicts Cooperative Patent Classification (CPC) codes of user queries, which are then use to provide another channel of relevance to patents, other than the sentence embeddings, which can often be highly noisy.
- **Image-to-document search**  
  Processes user-supplied images (diagrams, drawings) through OpenAI’s CLIP model to generate image embeddings in a shared embedding space between texts and images. This enables multimodal search without storing images and diagrams for individual patents.

Together, these components deliver a highly-accurate patent search system that reduces manual query crafting and filtering, significantly streamlining patent search processes.

### Course Code
- **BBM480**

### Objectives
1. Accelerate patent prior-art search for aerospace engineers  
2. Enable natural-language based search
2. Improve retrieval accuracy by applying techniques like Query Expansion and CPC Re-ranking   
3. Integrate visual and text modalities for richer search experiences  

### Methodology
- **Data Collection**  
The patent corpus was assembled in collaboration with TUSAŞ, with a large portion of records provided directly by the company. Consequently, the dataset is heavily weighted toward aerospace and defense–related patents.

- **Preprocessing**  
Since BERT-based models (e.g., PatentSBERTa) were used downstream, traditional text-cleaning steps like stop-word removal or punctuation stripping were unnecessary. Preprocessing was therefore limited to identifying and removing corrupted or incomplete records.

- **Indexing**  
Each patent’s title, abstract, and claims were converted into sentence embeddings via PatentSBERTa. These 768-dimensional vectors were then indexed in Elasticsearch to enable semantic (vector) searches alongside standard keyword lookups.

- **CPC Classification and Patent Re-ranking**  
A BERT-based classifier was trained to predict CPC subclasses, enabling automatic filtering and re-ranking of search results. Three model variants (single-layer, multi-task, and hierarchical-loss) were compared, and their predictions were combined in an ensemble “voting” mechanism to boost accuracy. 

- **Visual Search**  
For multimodal support, patent figures were extracted and passed through a pre-trained CLIP model to generate image embeddings. At query time, user-supplied diagrams are encoded and matched against this gallery, allowing retrieval of patents with visually similar illustrations.

### Key Findings
- Hybrid search consisting of semantic similarity and BM25 caused significant increase in the rankings of relevant patents in the search queries, with 98% Hit@1 rate for the most relevant patent, compared to the 55% Hit@1 rate for a baseline semantic search system.
- Query Expansion reduced the need for careful query crafting for the users by producing high-quality and context-aware query expansions thanks to powerful prompting techniques such as Pseudo-Relevance Feedback (PRF).
- Relatively large LLMs were able to run locally thanks to highly optimized LLM-inference library **Llama.cpp** and local LLM server **LM Studio**.

### Publications
- A research paper, titled "Comparative Study of Different Approaches in Hierarchical Multilabel CPC Classification" was accepted to SIU2025 conference.

### Supporting Institutions
- **TUSAŞ** (Turkish Aerospace Industries)  
- **SIU 2025** (IEEE Signal Processing and Communication Applications Conference)
- **TÜBİTAK**: The project was awarded **TÜBİTAK 2209-B** (University Students’ Industry-Oriented Research Projects) funding for promising undergraduate academic research.
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

## Demo Video

<div style="max-width:800px; margin:1em auto;">
  <video controls width="100%">
    <source src="assets/demo.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</div>
