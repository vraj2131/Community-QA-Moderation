DescriptionThis repository implements a multi-step text classification pipeline to predict the quality and required action for StackOverflow questions: whether they are high quality, need edits, or should be closed. It combines custom feature engineering (lexical metrics, code-block detection, sentiment), pseudo-subjective indicators from BERT, NER-based counts, and TF-IDF, then trains XGBoost binary classifiers for each task.

🚀 Features

Basic Preprocessing: Lowercases text, removes punctuation, combines Title & Body.

Lexical Features: Counts letters/words, code block stats, sentence-level metrics, sentiment (TextBlob), interrogative title flag.

Subjective Indicators: 20-dimensional pseudo-subjective scores using frozen BERT embeddings + a sigmoid head.

NER Features: Counts of SoftNER labels and relative frequencies.

TF-IDF: 300 most informative tokens from processed_text.

Numeric Transforms: Log, square, and square-root of numeric features.

Feature Scaling: Min-Max scaling of all numeric features.

Low-Correlation Pruning: Drops numeric features whose correlation with the target < 0.01.

Models: Three binary XGBoost classifiers (task_high_quality, task_edit, task_close).

📦 Installation

Clone the repo:

Create a Python environment (recommended):

Install dependencies:

🗂️ Data

train.csv: Training data with columns Id, Title, Body, Tags, CreationDate, Y (label: 0=high-quality, 1=close, 2=edit).

NER model folder: Contains SoftNER tokenizer and model files under drive/MyDrive/Ner.

🔧 Feature Engineering Pipeline

All preprocessing and feature extraction are encapsulated in a single sklearn Pipeline (feature_pipeline.pkl):

AddProcessed: Combine Title & Body → processed_text (cleaned).

SubjectiveTransformer: BERT-based pseudo-subjective indicators.

NERSoftTransformer: SoftNER counts + percentages → vectorized.

LexicalTransformer: Lexical stats → vectorized.

Passthrough: Keep processed_text for TF-IDF.

DropDuplicates: Remove duplicate feature names.

NumericGen: Log, square, sqrt transforms on numeric features.

TFIDF: Append 300 TF-IDF features.

Scale: Min-Max scaling.

DropLowCorrFeatures: Prune low-correlation numeric features.

The fitted pipeline is saved as feature_pipeline.pkl in your Drive.

🏋️ Training XGBoost Classifiers

The script loops over three binary tasks, each with stratified 5-fold CV:



Each pipeline_<task>.pkl bundles both the feature pipeline and its XGBoost classifier.

🎯 Inference

Load any pipeline and predict on new text:



For multi-class decision, compare probabilities across the three pipelines and pick the highest.

📁 Project Structure



🔄 T5-based Rephrasing for LQ_Edit

This repository includes a T5-based rephrasing module specifically used to generate edit suggestions for the low-quality edit (lq_edit) task. It is not applied automatically in the main classification pipeline, but can be invoked on demand to propose clearer, more polished question text.

Input: Raw Title and Body of a question flagged as needing edits.

Processing: The combined text is fed into the fine‑tuned T5 model, which generates a rephrased version preserving the original intent.

Output: Suggested title and body edits that a human reviewer or an automated tool can apply.
