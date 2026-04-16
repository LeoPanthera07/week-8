# AI Usage Log — W8 Wednesday Assignment
*(Required by IIT-GN AI Usage Policy)*

---

## Sub-step 1 — Social Media EDA

**Prompt used:**
> "Write a Python function `engineer_hate_and_spam_labels(df)` that derives binary hate_speech and spam labels from a social media DataFrame with columns clean_text and category (-1,0,1). Use a keyword list for hate detection and word-count heuristics for spam. Use named constants for all thresholds."

**Was the AI output correct?**  
Partially. The initial regex pattern used `r'' + kw + r''` which in Python requires raw string concatenation; I changed the pattern construction to use `'|'.join(r'\b' + kw + r'\b' for kw in HATE_KEYWORDS)` to avoid SyntaxWarning on invalid escape sequences.

**What I changed:**  
Added `re` module import; adjusted the hate_pattern regex to be a single joined pattern; added the L2-normalisation step in build_sentence_embeddings after the SVD fallback (originally missing, which would cause dot-product scores > 1).

---

## Sub-step 3 — CNN Architecture

**Prompt used:**
> "Design a PyTorch CNN for MNIST with exactly 2 convolutional blocks using BatchNorm, MaxPool, and Dropout. Include a training loop that returns loss/accuracy history. Use all named constants (CONV1_FILTERS, etc.)."

**Was the AI output correct?**  
Yes, with one modification: I added `optim.lr_scheduler.StepLR` (step_size=5, gamma=0.5) to decay LR after epoch 5 — this was not in the original suggestion but improved convergence from ~98.9% to ~99.1% test accuracy.

**What I changed:**  
Added the LR scheduler. Also fixed the `extract_cnn_features` function in Sub-step 7 which had a logic bug (`x = model.conv_block2(batch if x is None else x)`) — corrected to sequential application `model.conv_block2(model.conv_block1(batch))`.

---

## Sub-step 4 — Semantic Search

**Prompt used:**
> "Write a `semantic_search_top_k` function that takes a query string, a corpus list, and pre-computed L2-normalised embeddings (numpy array) and returns the top-K most similar posts using dot-product cosine similarity. Include try/except defensive handling."

**Was the AI output correct?**  
Yes. I added one enhancement: the function now excludes the query itself from results (scores[idx] = -1.0) in the pipeline version to avoid trivial self-matches.

---

## Sub-step 6 — TF-IDF vs Embeddings

**Prompt used:**
> "Write a `compare_retrieval_methods` function that takes N query posts, runs both TF-IDF cosine search and embedding cosine search on a corpus, computes Jaccard overlap between their top-K result sets, and returns a DataFrame with results. Plot the Jaccard values."

**Was the AI output correct?**  
Mostly. The Jaccard computation had an edge case: `corpus_texts.index(r[0])` raises ValueError if the retrieved text is not in the list (possible due to duplicates). I wrapped it in a try/except and used a set comprehension with error handling.

---

## Sub-step 7 — Transfer Learning

**Prompt used:**
> "Design an experiment to test whether a CNN trained on MNIST can be used as a feature extractor for social media hate speech classification. Convert text posts to 28x28 grayscale images using ASCII character codes. Compare CNN features vs TF-IDF vs random features using Logistic Regression."

**Was the AI output correct?**  
Yes. I added a `np.clip` call in `render_text_as_image` to ensure ASCII values > 127 (unicode characters) don't overflow the [0,1] range. Also added explicit documentation explaining WHY transfer is expected to fail (domain gap, inductive bias mismatch) as this is the key analytical point the Hard sub-step tests.

---

*All AI-assisted code was reviewed, understood, and modified before submission.*
