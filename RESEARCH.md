# Research Notes & Literature Review

## Overview

This document contains research notes, paper summaries, and key insights from the dependency parsing literature.

---

## Paper 1: Deep Biaffine Attention for Neural Dependency Parsing (Dozat & Manning, 2017)

### Citation
Dozat, T., & Manning, C. D. (2017). Deep biaffine attention for neural dependency parsing. *ICLR 2017*.

### Core Contributions
- [ ] **Read and understood**
- [ ] **Implemented key concepts**

#### Key Ideas

**1. Biaffine Attention Mechanism**
- *What it is*: 
  - A scoring function that combines two representations (head and dependent)
  - Formula: score(h, d) = h^T U d + W^T [h; d] + b
  - Where U is a bilinear term, W is a linear term
  
- *Why it matters*:
  - Captures rich interactions between potential heads and dependents
  - More expressive than simple dot product or additive attention
  - Separate scorers for arc and label prediction

**2. Model Architecture**
- BiLSTM encoder for contextual representations
- Separate MLPs for arc-head and arc-dependent representations
- Biaffine scorer for both attachment and labeling

**3. Training Strategy**
- Use of pre-trained word embeddings (GloVe, word2vec)
- Character-level representations via CNNs
- Graph-based parsing (finds globally optimal tree)

#### Results & Performance
- State-of-the-art on Penn Treebank (PTB): ~95% UAS, ~94% LAS
- Strong performance across multiple languages
- Faster than previous graph-based parsers

#### Implementation Notes
```
Key equations to implement:
1. Arc scoring: s_arc(i,j) = h_i^T U_arc d_j
2. Label scoring: s_label(i,j,l) = h_i^T U_label^(l) d_j
3. BiLSTM: contextual encoding of input sequence
4. MLP projections: separate for heads and dependents
```

#### Questions & Insights
- *Question*: Why separate representations for heads vs dependents?
- *Answer*: Different syntactic roles require different feature abstractions

- *Question*: How does biaffine compare to other attention mechanisms?
- *Answer*: Adds bilinear interaction term for richer modeling

#### Related Work
- Earlier graph-based parsers (MST-Parser)
- Transition-based neural parsers (Chen & Manning 2014)
- Attention mechanisms in NLP

---

## Paper 2: BERT - Pre-training of Deep Bidirectional Transformers (Devlin et al., 2019)

### Citation
Devlin, J., Chang, M. W., Lee, K., & Toutanova, K. (2019). BERT: Pre-training of deep bidirectional transformers for language understanding. *NAACL 2019*.

### Core Contributions
- [ ] **Read and understood**
- [ ] **Integrated into parser design**

#### Key Ideas

**1. Bidirectional Pre-training**
- *What it is*:
  - Masked Language Model (MLM): randomly mask 15% of tokens, predict them
  - Unlike left-to-right models (GPT), BERT sees full context
  - Next Sentence Prediction (NSP) for sentence relationships

- *Why it matters for parsing*:
  - Rich contextual representations capture syntax implicitly
  - Pre-training on large corpora provides strong initialization
  - Fine-tuning on parsing task improves performance significantly

**2. Transformer Architecture**
- Multi-head self-attention layers
- Position embeddings for sequential information
- Layer normalization and residual connections

**3. Transfer Learning Paradigm**
- Pre-train on unlabeled text (BooksCorpus, Wikipedia)
- Fine-tune on downstream tasks with task-specific layers
- Different layers capture different linguistic properties

#### Using BERT for Dependency Parsing

**Integration Strategies**
1. **Feature-based**: Extract BERT embeddings, freeze weights
2. **Fine-tuning**: Update BERT parameters during parser training
3. **Hybrid**: Fine-tune top layers, freeze bottom layers

**Layer Selection**
- Lower layers: lexical/syntactic information
- Middle layers: syntactic structures (best for parsing)
- Upper layers: semantic information

#### Implementation Notes
```python
# Typical integration approach:
# 1. Load pre-trained BERT model
# 2. Extract contextualized embeddings
# 3. Feed into biaffine parser
# 4. Fine-tune end-to-end or use frozen features

from transformers import BertModel, BertTokenizer

# Extract representations
bert = BertModel.from_pretrained('bert-base-uncased')
embeddings = bert(input_ids)[0]  # Shape: [batch, seq_len, 768]

# Feed to parser
parser_input = embeddings
# ... continue with biaffine scoring
```

#### Questions & Insights
- *Question*: Which BERT layers are most useful for parsing?
- *Research finding*: Middle layers (6-9 for BERT-base) often work best

- *Question*: Should we fine-tune or use frozen features?
- *Trade-off*: Fine-tuning gives better performance but requires more compute

#### Related Work
- ELMo (contextualized embeddings)
- GPT (unidirectional pre-training)
- XLNet (permutation language modeling)

---

## Paper 3: [Recent Paper - 2022+]

### Citation
*[Fill in after selecting a paper]*

### Selection Criteria
Looking for papers that cover:
- Transformer-based dependency parsing
- Multilingual parsing advances
- Enhanced representations (e.g., RoBERTa, XLM-R for parsing)
- Recent benchmarks and datasets

### Recommended Recent Papers
1. "Structured Prediction as Translation between Augmented Natural Languages" (Paolini et al., 2021)
2. "Cross-Lingual BERT Contextual Embedding Space Mapping with Isotropic and Isometric Conditions" (Wang et al., 2022)
3. "Transition-based Neural Dependency Parsing with Mixed Attention" (recent ACL/EMNLP)

### Core Contributions
- [ ] **Read and understood**
- [ ] **Evaluated relevance to project**

#### Key Ideas
*[To be filled after reading]*

#### Novel Techniques
*[To be filled after reading]*

#### Comparison to Dozat & Manning
*[To be filled after reading]*

#### Implementation Considerations
*[To be filled after reading]*

---

## Key Concepts in Dependency Parsing

### Fundamental Definitions

**Dependency Tree Properties**
- Single root node
- Each word has exactly one head (except root)
- No cycles (acyclic graph)
- Connected (all words reachable from root)

**Parsing Approaches**
1. **Graph-based**: Score all possible arcs, find optimal tree
2. **Transition-based**: Build tree incrementally using actions
3. **Sequence-to-sequence**: Treat as translation problem

### Evaluation Metrics

**Unlabeled Attachment Score (UAS)**
- Percentage of words with correct head
- Measures structural accuracy

**Labeled Attachment Score (LAS)**
- Percentage of words with correct head AND label
- Stricter metric, measures both structure and relations

**Formula**
```
UAS = (# correct head assignments) / (# total words)
LAS = (# correct head + label assignments) / (# total words)
```

### Common Datasets

**English**
- Penn Treebank (PTB): ~50k sentences, news text
- Universal Dependencies (UD): cross-lingual, standardized

**Multilingual**
- Universal Dependencies (UD): 100+ languages
- CoNLL shared tasks

---

## Implementation Roadmap

### Phase 1: Understanding (Week 1)
- ✅ Read key papers
- ✅ Understand biaffine attention
- ✅ Understand BERT integration
- ⏳ Implement basic biaffine scorer

### Phase 2: Baseline (Week 2)
- ⏳ Data loading and preprocessing
- ⏳ Simple BiLSTM baseline
- ⏳ Training pipeline
- ⏳ Evaluation metrics

### Phase 3: Advanced (Week 3-4)
- ⏳ Full biaffine parser
- ⏳ BERT integration
- ⏳ Optimization and tuning
- ⏳ Error analysis

---

## Open Questions & Research Directions

### Technical Questions
1. How to handle non-projective trees?
2. What's the trade-off between model size and accuracy?
3. How to optimize for low-resource languages?

### Architectural Choices
1. BiLSTM vs Transformer encoder?
2. Fine-tune BERT or use frozen features?
3. Separate vs joint arc/label prediction?

### Practical Considerations
1. How much training data is needed?
2. What computational resources are required?
3. How to handle long sentences efficiently?

---

## References & Resources

### Papers
1. Dozat & Manning (2017) - Deep Biaffine Attention
2. Devlin et al. (2019) - BERT
3. [Additional paper TBD]

### Codebases
- [AllenNLP Dependency Parser](https://github.com/allenai/allennlp-models)
- [Stanza](https://github.com/stanfordnlp/stanza)
- [Hugging Face Transformers](https://github.com/huggingface/transformers)

### Tutorials & Blogs
- Stanford CS224N Lecture Notes
- The Gradient: "Dependency Parsing" series
- Jay Alammar's illustrated guides

### Datasets
- [Universal Dependencies](https://universaldependencies.org/)
- [Penn Treebank](https://catalog.ldc.upenn.edu/LDC99T42)

---

## Daily Progress Log

### Day 1 
- Created project structure
- Read Dozat & Manning paper
- Key insights: [fill in]
- Questions raised: [fill in]
- Next steps: [fill in]

### Day 2 
....

---

## Glossary

**Dependency Parsing**: Task of analyzing grammatical structure by identifying head-dependent relationships

**Biaffine Attention**: Attention mechanism using bilinear transformation to score head-dependent pairs

**Universal Dependency (UD)**: Cross-lingual syntactic annotation framework

**UAS/LAS**: Unlabeled/Labeled Attachment Score - standard parsing evaluation metrics

**Projectivity**: Property where dependency arcs don't cross when drawn above the sentence

**Graph-based Parsing**: Finding optimal tree from all possible arcs using global scoring

**Transition-based Parsing**: Building tree incrementally through sequence of actions

**Arc**: Directed edge from head word to dependent word

**Root**: Special token that serves as head of the main verb

---
