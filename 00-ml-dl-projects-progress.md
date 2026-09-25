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

---

## Study Sessions Log
**2026-09-25:** Project plan scaffolded, tied to ESL and Bishop DL trackers. Added standard dataset per project.

---

## Current Focus
**Project:** Not yet started
**Status:** Plan set up with datasets
**Next:** Pick a Phase 1 project once ready to pair with early ESL notes
