# ML/DL Learning Projects — Progress

**Goal:** Small, incremental hands-on projects paired with theory from [[00-esl-study-progress]] and [[00-dl-study-progress]]. Each project should be small enough to finish in a sitting or two — the point is reinforcing one concept at a time, not building something polished.

**Constraint:** Everything is written from scratch in Rust ([[~/ml-from-scratch]]) — no Python community libraries (sklearn, PyTorch, HuggingFace `datasets`/`transformers`, FAISS, etc.) and no relying on their dataset loaders. Where a project needs **pretrained weights** (transfer learning, fine-tuning), the weight files themselves (safetensors) are downloaded and loaded directly — all training/fine-tuning/inference code is hand-written. Datasets are referenced by name and raw-file source (CSV/idx/txt/JSON) rather than by library loader calls, since those loaders are Python-only.

---

## Phase 1: Classical ML Foundations (pairs with ESL Ch. 2–4)
- [x] Linear regression from scratch (closed-form + gradient descent)
  - **Dataset:** California Housing — 20,433 samples, 8 features after removing missing values
  - **Result:** Closed-form (R² = 0.692, MSE = 4.47B) outperforms gradient descent (R² = 0.640, MSE = 5.23B) with 1000 iterations at lr=0.01
  - **Code:** `src/lib.rs` (Matrix ops from scratch, linear regression) + `src/main.rs` (CSV loading, data normalization, train-test split)
- [ ] Ridge & Lasso from scratch — plot coefficient shrinkage paths as regularization strength varies
  - **Dataset:** Diabetes dataset — small, 10 features, the textbook example for regularization path plots; available as a plain CSV
- [ ] Logistic regression from scratch — binary classifier
  - **Dataset:** Breast Cancer Wisconsin — the standard small binary classification benchmark; raw CSV from the UCI ML Repository
- [ ] k-NN classifier from scratch — visualize decision boundary vs. k
  - **Dataset:** Iris — classic tiny CSV (150 rows), UCI ML Repository, small enough to embed directly
- [ ] K-fold cross-validation implemented from scratch
  - **Dataset:** Reuse Breast Cancer or Iris above

## Phase 2: Trees, Ensembles, Unsupervised (pairs with ESL Ch. 5–9, 13–15)
- [ ] Decision tree (CART) from scratch — visualize splits
  - **Dataset:** Iris (classification) or Titanic — both standard tree-tutorial sets; Titanic available as a raw CSV (Kaggle competition file or public mirrors)
- [ ] Bagging ensemble from scratch, built on the tree above
  - **Dataset:** Same as the tree above, to directly compare variance reduction
- [ ] Simple gradient boosting from scratch (boosted stumps)
  - **Dataset:** Diabetes dataset (regression), raw CSV
- [ ] K-means from scratch — visualize cluster assignment over iterations
  - **Dataset:** Synthetic Gaussian blobs generated directly in Rust (no library needed — just sample a few 2D Gaussians yourself); Iris as the real-data alternative
- [ ] PCA from scratch (via covariance eigendecomposition)
  - **Dataset:** Iris for a first pass (4D→2D); MNIST (raw idx files) as the standard higher-dimensional PCA demo

## Phase 3: Into Neural Nets (pairs with Bishop DL Ch. 4–9)
- [ ] Single-layer perceptron from scratch
  - **Dataset:** MNIST, restricted to two digits (e.g. 0 vs. 1) — standard linearly-separable-ish toy case; raw idx files (Yann LeCun's original ubyte format), parsed directly
- [ ] Multi-layer perceptron with manual backprop
  - **Dataset:** MNIST (full 10-class), same raw idx files
- [ ] Minimal autograd engine from scratch (reverse-mode autodiff, scalar or tensor-based — your own "micrograd"), then rebuild the same MLP on top of it
  - **Dataset:** MNIST, same split — compare results against the fully-manual-backprop version
- [ ] Regularization experiment: none vs. weight decay vs. dropout
  - **Dataset:** Fashion-MNIST — harder than MNIST so regularization effects are more visible; raw idx files from the Zalando research GitHub

## Phase 4: Convolutional Networks (pairs with Bishop DL Ch. 10)
- [ ] Simple CNN
  - **Dataset:** CIFAR-10 — standard small-image CNN benchmark; raw binary format (`cifar-10-binary.tar.gz` from the original site)
- [ ] Data augmentation experiment — measure effect on validation accuracy
  - **Dataset:** CIFAR-10, same as above
- [ ] Transfer learning: load pretrained ResNet18 weights (safetensors export), write the fine-tuning loop yourself
  - **Dataset:** Imagenette or Kaggle "Cats vs. Dogs" — standard lightweight transfer-learning targets; raw image folders

## Phase 5: Sequences & Transformers (pairs with Bishop DL Ch. 11–12)
- [ ] Simple RNN/LSTM for character-level text generation
  - **Dataset:** Tiny Shakespeare (Karpathy's char-rnn corpus) — plain txt file
- [ ] Minimal transformer/attention block from scratch on a toy sequence task
  - **Dataset:** Synthetic copy/reverse-the-sequence task, generated directly in Rust
- [ ] Fine-tune a small pretrained transformer (e.g. DistilBERT): load pretrained weights (safetensors) and its original vocab/merges files (raw, not via a Python tokenizer library), write the fine-tuning loop yourself
  - **Dataset:** IMDb reviews or SST-2 — standard text-classification fine-tuning benchmarks; raw text/TSV files

## Phase 6: Generative Models (pairs with Bishop DL Ch. 14–20)
- [ ] Autoencoder — visualize reconstructions and latent space
  - **Dataset:** MNIST, raw idx files
- [ ] Variational autoencoder (VAE) — compare latent space to plain autoencoder
  - **Dataset:** MNIST — the dataset used in the original Kingma & Welling VAE paper
- [ ] Small GAN — observe training instability firsthand
  - **Dataset:** MNIST first (as in the original GAN paper); CelebA as the standard step-up to face generation, raw image folder
- [ ] Tiny diffusion model — forward/reverse process from scratch
  - **Dataset:** 2D toy distribution (e.g. a Swiss roll or Gaussian mixture, generated directly) for the math; CIFAR-10 as the image-scale follow-up (used in the original DDPM paper)

## Phase 7: Language Models (builds on Bishop DL Ch. 12 Transformers)
- [ ] Byte-pair encoding (BPE) tokenizer from scratch
  - **Dataset:** Tiny Shakespeare (reuse from Phase 5) — small enough to inspect the learned merges by hand
- [ ] Train a tiny GPT-style LM from scratch (nanoGPT-style, char or word level)
  - **Dataset:** Tiny Shakespeare — the exact toy corpus Karpathy's nanoGPT uses for this exercise
- [ ] Evaluate a small pretrained LM's perplexity: load pretrained weights (e.g. GPT-2 small, safetensors) and its original vocab/merges files, write the forward pass and perplexity computation yourself
  - **Dataset:** WikiText-2 — standard small benchmark for LM perplexity comparisons; raw txt
- [ ] Fine-tune a small pretrained LLM (e.g. GPT-2 small) end-to-end — own training loop on top of loaded pretrained weights
  - **Dataset:** Alpaca instruction dataset (52K examples), raw JSON
- [ ] Parameter-efficient fine-tuning experiment (LoRA), implemented yourself, on the same model/task
  - **Dataset:** Same Alpaca subset — compare full fine-tune vs. LoRA on cost and quality

## Phase 8: Retrieval-Augmented Generation (RAG)
- [ ] Embedding + cosine-similarity retrieval from scratch (brute-force, no vector DB library)
  - **Dataset:** SQuAD — question/passage pairs, the standard QA/retrieval benchmark; raw JSON
- [ ] Approximate nearest-neighbor search from scratch (e.g. a simple HNSW implementation) — compare speed/accuracy against the brute-force version
  - **Dataset:** Same SQuAD passages
- [ ] Full RAG pipeline: retrieve top-k passages, feed to your own LLM (Phase 7's tiny GPT, or pretrained weights + own inference code), generate an answer
  - **Dataset:** SQuAD — generated answers can be checked directly against ground-truth answers
- [ ] Chunking strategy experiment (chunk size/overlap vs. retrieval quality)
  - **Dataset:** A longer-document set, e.g. a Wikipedia articles subset — chunking matters more on long documents than SQuAD's short passages
- [ ] Retrieval evaluation (precision@k, recall@k) vs. end-to-end answer accuracy
  - **Dataset:** Natural Questions (NQ) subset — standard benchmark for scoring retrieval quality separately from generation quality

## Phase 9: Agents
- [ ] Basic tool-calling agent (calculator + one lookup tool), driven by your own LLM's output
  - **Dataset:** GSM8K (grade-school math word problems) — standard benchmark for testing whether tool use improves reasoning accuracy; raw JSON
- [ ] ReAct-style agent (interleaved thought/action/observation loop)
  - **Dataset:** HotpotQA (multi-hop QA) — the dataset the original ReAct paper used to demonstrate reasoning+retrieval agents
- [ ] Memory-augmented conversational agent (persist facts across turns)
  - **Dataset:** MultiWOZ (multi-turn dialogue), or a synthetic multi-session conversation you generate yourself if something lighter is preferred
- [ ] Multi-agent collaboration toy (two agents debate/critique each other's answers)
  - **Dataset:** Reuse GSM8K or HotpotQA — measure whether debate/self-critique improves accuracy over a single-agent baseline

---

## Study Sessions Log
**2026-09-25:** Project plan scaffolded. Completed **Phase 1.1 — Linear Regression**: implemented Matrix struct with manual operations, LinearRegression class with closed-form and gradient descent, loaded California Housing (20k samples), trained both methods. Closed-form achieved R² = 0.692, gradient descent R² = 0.640 with default hyperparameters.

---

## Current Focus
**Project:** Phase 1.2 — Ridge & Lasso regression (next)
**Status:** Phase 1.1 complete, code in `~/ml-from-scratch`, notes in `~/study`
**Next:** Implement L2/L1 regularization, plot coefficient paths
