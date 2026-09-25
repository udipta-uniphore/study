# ML/DL Learning Projects — Progress
**Goal:** Small, incremental hands-on projects paired with theory from [[00-esl-study-progress]] and [[00-dl-study-progress]]. Each project should be small enough to finish in a sitting or two — the point is reinforcing one concept at a time, not building something polished. Datasets below are the standard/most-used ones for each task so results are comparable to what's in textbooks/tutorials.

---

## Phase 1: Classical ML Foundations (pairs with ESL Ch. 2–4)
- [ ] Linear regression from scratch (numpy, closed-form + gradient descent)
  - **Dataset:** California Housing (`sklearn.datasets.fetch_california_housing`) — standard regression set; replaced the now-deprecated Boston Housing dataset
- [ ] Ridge & Lasso from scratch — plot coefficient shrinkage paths as regularization strength varies
  - **Dataset:** Diabetes dataset (`sklearn.datasets.load_diabetes`) — small, 10 features, the textbook example for regularization path plots
- [ ] Logistic regression from scratch — binary classifier
  - **Dataset:** Breast Cancer Wisconsin (`sklearn.datasets.load_breast_cancer`) — the standard small binary classification benchmark
- [ ] k-NN classifier from scratch — visualize decision boundary vs. k
  - **Dataset:** Iris (`sklearn.datasets.load_iris`) — the classic 2D-projectable dataset for decision boundary visualization
- [ ] K-fold cross-validation implemented from scratch, compared against sklearn's
  - **Dataset:** Reuse Breast Cancer or Iris above

## Phase 2: Trees, Ensembles, Unsupervised (pairs with ESL Ch. 5–9, 13–15)
- [ ] Decision tree (CART) from scratch — visualize splits
  - **Dataset:** Iris (classification) or Titanic (`seaborn.load_dataset("titanic")`) — both are the standard tree-tutorial sets
- [ ] Bagging ensemble from scratch, built on the tree above
  - **Dataset:** Same as the tree above, to directly compare variance reduction
- [ ] Simple gradient boosting from scratch (boosted stumps)
  - **Dataset:** Diabetes dataset (regression) — same one used in the original Friedman gradient boosting papers' spirit
- [ ] K-means from scratch — visualize cluster assignment over iterations
  - **Dataset:** `sklearn.datasets.make_blobs` (synthetic, so ground-truth clusters are known) — Iris is the standard real-data alternative
- [ ] PCA from scratch (via covariance eigendecomposition)
  - **Dataset:** Iris for a first pass (4D→2D); MNIST (flattened pixels) as the standard higher-dimensional PCA demo

## Phase 3: Into Neural Nets (pairs with Bishop DL Ch. 4–9)
- [ ] Single-layer perceptron from scratch (numpy)
  - **Dataset:** MNIST, restricted to two digits (e.g. 0 vs. 1) — the standard linearly-separable-ish toy case
- [ ] Multi-layer perceptron with manual backprop (no autograd)
  - **Dataset:** MNIST (full 10-class) — the de facto standard first neural net dataset
- [ ] Same MLP rebuilt in PyTorch (autograd) — confirm matching results
  - **Dataset:** MNIST, same split as above for direct comparison
- [ ] Regularization experiment: none vs. weight decay vs. dropout
  - **Dataset:** Fashion-MNIST (`torchvision.datasets.FashionMNIST`) — harder than MNIST, so regularization effects are more visible

## Phase 4: Convolutional Networks (pairs with Bishop DL Ch. 10)
- [ ] Simple CNN
  - **Dataset:** CIFAR-10 (`torchvision.datasets.CIFAR10`) — the standard small-image CNN benchmark, more informative than MNIST for this
- [ ] Data augmentation experiment — measure effect on validation accuracy
  - **Dataset:** CIFAR-10, same as above
- [ ] Transfer learning: fine-tune a small pretrained CNN (e.g. ResNet18)
  - **Dataset:** Imagenette (`fastai`'s 10-class ImageNet subset) or the Kaggle "Cats vs. Dogs" set — both are the standard lightweight transfer-learning targets

## Phase 5: Sequences & Transformers (pairs with Bishop DL Ch. 11–12)
- [ ] Simple RNN/LSTM for character-level text generation
  - **Dataset:** Tiny Shakespeare (the corpus made popular by Karpathy's char-rnn) — the standard toy text-generation dataset
- [ ] Minimal transformer/attention block from scratch on a toy sequence task
  - **Dataset:** Synthetic copy/reverse-the-sequence task (generated on the fly) — standard sanity-check task for verifying attention implementations
- [ ] Fine-tune a small pretrained transformer (e.g. DistilBERT)
  - **Dataset:** IMDb reviews (`datasets.load_dataset("imdb")`) or SST-2 from GLUE — both are the standard text-classification fine-tuning benchmarks

## Phase 6: Generative Models (pairs with Bishop DL Ch. 14–20)
- [ ] Autoencoder — visualize reconstructions and latent space
  - **Dataset:** MNIST — standard for autoencoder demos
- [ ] Variational autoencoder (VAE) — compare latent space to plain autoencoder
  - **Dataset:** MNIST — the dataset used in the original Kingma & Welling VAE paper
- [ ] Small GAN — observe training instability firsthand
  - **Dataset:** MNIST first (as in the original GAN paper); CelebA (`torchvision.datasets.CelebA`) as the standard step-up to face generation
- [ ] Tiny diffusion model — forward/reverse process from scratch
  - **Dataset:** 2D toy distribution (e.g. a Swiss roll or Gaussian mixture) for the from-scratch math; CIFAR-10 as the standard image-scale follow-up (used in the original DDPM paper)

## Phase 7: Language Models (builds on Bishop DL Ch. 12 Transformers)
- [ ] Byte-pair encoding (BPE) tokenizer from scratch
  - **Dataset:** Tiny Shakespeare (reuse from Phase 5) — small enough to inspect the learned merges by hand
- [ ] Train a tiny GPT-style LM from scratch (nanoGPT-style, char or word level)
  - **Dataset:** Tiny Shakespeare — the exact toy corpus Karpathy's nanoGPT uses for this exercise
- [ ] Evaluate a small pretrained LM's perplexity
  - **Dataset:** WikiText-2 (`datasets.load_dataset("wikitext", "wikitext-2-raw-v1")`) — the standard small benchmark for LM perplexity comparisons
- [ ] Fine-tune a small pretrained LLM (e.g. GPT-2 small) end-to-end
  - **Dataset:** Alpaca instruction dataset (52K examples) — the standard small instruction-tuning set
- [ ] Parameter-efficient fine-tuning experiment (LoRA) on the same model/task
  - **Dataset:** Same Alpaca subset — compare full fine-tune vs. LoRA on cost and quality

## Phase 8: Retrieval-Augmented Generation (RAG)
- [ ] Embedding + cosine-similarity retrieval from scratch (numpy, no vector DB)
  - **Dataset:** SQuAD (`datasets.load_dataset("squad")`) — question/passage pairs, the standard QA/retrieval benchmark
- [ ] Swap in a real vector DB (FAISS or Chroma) for the same retrieval task
  - **Dataset:** Same SQuAD passages, to directly compare against the from-scratch version
- [ ] Full RAG pipeline: retrieve top-k passages, feed to an LLM, generate an answer
  - **Dataset:** SQuAD — generated answers can be checked directly against ground-truth answers
- [ ] Chunking strategy experiment (chunk size/overlap vs. retrieval quality)
  - **Dataset:** A longer-document set, e.g. a Wikipedia articles subset — chunking matters more on long documents than SQuAD's short passages
- [ ] Retrieval evaluation (precision@k, recall@k) vs. end-to-end answer accuracy
  - **Dataset:** Natural Questions (NQ) subset — the standard benchmark for scoring retrieval quality separately from generation quality

## Phase 9: Agents
- [ ] Basic tool-calling agent (calculator + one lookup tool)
  - **Dataset:** GSM8K (grade-school math word problems) — the standard benchmark for testing whether tool use improves reasoning accuracy
- [ ] ReAct-style agent (interleaved thought/action/observation loop)
  - **Dataset:** HotpotQA (multi-hop QA) — the dataset the original ReAct paper used to demonstrate reasoning+retrieval agents
- [ ] Memory-augmented conversational agent (persist facts across turns)
  - **Dataset:** MultiWOZ (multi-turn dialogue) — the standard multi-turn dialogue benchmark, or a synthetic multi-session conversation if something lighter is preferred
- [ ] Multi-agent collaboration toy (two agents debate/critique each other's answers)
  - **Dataset:** Reuse GSM8K or HotpotQA — measure whether debate/self-critique improves accuracy over a single-agent baseline

---

## Study Sessions Log
**2026-09-25:** Project plan scaffolded, tied to ESL and Bishop DL trackers. Added standard dataset per project. Added Phases 7–9 covering LLMs, RAG, and agents.

---

## Current Focus
**Project:** Not yet started
**Status:** Plan set up with datasets
**Next:** Pick a Phase 1 project once ready to pair with early ESL notes
