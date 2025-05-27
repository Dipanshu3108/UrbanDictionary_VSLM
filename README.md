# Urban Dictionary ML Pipeline

> End-to-End Language Model Training Workflow

## 📊 Data Retrieval

Accessing Urban Dictionary dataset from Kaggle containing word definitions and metadata.

- **Source:** Kaggle Dataset
- **Format:** CSV File  
- **Storage:** Google Drive

## 🔧 Data Filtering & Preprocessing

Cleaning and normalizing raw data to ensure quality input for the model.

| Process | Description |
|---------|-------------|
| **Symbol Filtering** | Remove obscure characters - Keeps only standard punctuation and alphanumeric characters, removing emojis and special symbols |
| **Text Normalization** | Lowercase conversion - Converts all text to lowercase to standardize input |
| **Whitespace Cleanup** | Standardize spacing - Replaces multiple spaces with single spaces and trims leading/trailing whitespace |
| **Duplicate Removal** | Ensure unique entries - Removes duplicate word-definition pairs from the dataset |

**Dataset Metrics:**
- **50K** Rows Processed
- **2** Columns (word, definition)

## 🔤 Feature Extraction & Tokenization

Converting text into numerical representations using Byte Pair Encoding (BPE) tokenization.

- **Tokenizer:** Custom BPE - Byte Pair Encoding learns subword units by iteratively merging the most frequent character pairs
- **Vocab Size:** 2,500 tokens
- **Special Tokens:** [SEP], [PAD], [BOS], [EOS]
  - `[SEP]`: Separates word from definition
  - `[PAD]`: Padding token
  - `[BOS]`: Beginning of sequence
  - `[EOS]`: End of sequence

**Format:** `"word [SEP] definition"`

## 🧠 Model Architecture

Transformer-based language model with attention mechanisms for understanding context.

| Component | Specification |
|-----------|---------------|
| **Type** | Transformer LM |
| **Layers** | 6 Transformer blocks |
| **Attention Heads** | 8 |
| **Hidden Dimension** | 256 |
| **Feed-Forward** | 1024 |
| **Max Length** | 512 tokens |

**Technology Stack:**
- PyTorch
- Multi-Head Attention  
- Positional Encoding
- Layer Normalization

### Single Transformer Block Architecture

```
┌─────────────────────────────────────┐
│        Multi-Head Self-Attention    │
│   Captures relationships between    │
│        words in context            │
└─────────────────┬───────────────────┘
                  ↓
┌─────────────────────────────────────┐
│        Layer Normalization 1       │
│    Normalizes features after       │
│           attention                │
└─────────────────┬───────────────────┘
                  ↓
┌─────────────────────────────────────┐
│        Feed-Forward Network        │
│   GELU activation with 1024        │
│          hidden units              │
└─────────────────┬───────────────────┘
                  ↓
┌─────────────────────────────────────┐
│        Layer Normalization 2       │
│    Normalizes features after       │
│         feed-forward               │
└─────────────────────────────────────┘
```

## ⚡ Model Training

Training the model on processed data with optimization and validation.

| Parameter | Value |
|-----------|-------|
| **Optimizer** | AdamW - Adam optimizer with weight decay for better regularization |
| **Learning Rate** | 3e-4 |
| **Batch Size** | 16 |
| **Sequence Length** | 64 |

**Data Split:**
- **70%** Training Data
- **15%** Validation Data  
- **15%** Test Data

**Training Results:**
- Training Progress: ![85%]
- Validation Progress: ![78%]
- Test Progress: ![92%]

## 📈 Model Evaluation

Assessing model performance using various metrics and validation techniques.

| Metric | Performance |
|--------|-------------|
| **Perplexity Score** | Low |
| **Validation Loss** | Good |
| **Generation Quality** | High |

**Additional Features:**
- **Checkpoint:** Best model saved
- **Scheduler:** Cosine Annealing - Learning rate scheduler that reduces learning rate following a cosine curve

## ✨ Output Generation

Generating new definitions for words using the trained model with controlled sampling.

| Parameter | Value | Description |
|-----------|-------|-------------|
| **Sampling** | Temperature-based | Controls randomness in generation. Higher values (>1.0) increase diversity, lower values (<1.0) increase determinism |
| **Top-K** | 50 tokens | Limits selection to the top 50 most likely next tokens |
| **Top-P** | 0.95 nucleus | Selects from the smallest set of tokens whose cumulative probability exceeds 0.95 |
| **Max Length** | 100 tokens | - |

### EXPECTED Example Outputs 

| Input | Output |
|-------|--------|
| **"lit"** | *"something that is really cool or exciting"* |
| **"savage"** | *"when someone does something bold without caring"* |
