# Unit 1: Assignment & Project Documentation
## Student Name: Rahul Anand
## SRN: PES2UG23CS461

### Part 1: The Model Benchmark Challenge (Questions_Unit1)
1. Problem Statement
The objective was to evaluate the architectural differences between Encoder-only (BERT, RoBERTa) and Encoder-Decoder (BART) models by forcing them to perform tasks outside their native design, such as text generation and question answering.

2. Solution Implemented
I implemented a benchmarking notebook (Unit1_Benchmark.ipynb) using the Hugging Face transformers library. The experiment tested three models:

- BERT (bert-base-uncased)

- RoBERTa (roberta-base)

- BART (facebook/bart-base)

I ran three specific experiments:

- Text Generation: Tested if models could generate coherent text from a prompt.

- Fill-Mask: Tested the models' ability to predict missing tokens.

Question Answering: Tested extraction of answers from a context passage.

3. Key Observations
Encoders cannot Generate: BERT and RoBERTa failed completely at text generation (producing nonsense or repeating the prompt) because they lack a decoder component to predict the "next token" autoregressively.

Architecture Matters: BERT and RoBERTa excelled at "Fill-Mask" (their native training objective), achieving high confidence scores (>50%).

Base Models vs. Fine-Tuned: All three "base" models performed poorly on Question Answering. This demonstrated that pre-trained weights alone are insufficient for complex downstream tasks without specific fine-tuning (SQuAD, etc.).

### Part 2: Project Implementation (Hands-on)
1. Selected Project
Topic: Zero-Shot Text Classification Goal: Build a model capable of classifying movie plot summaries into genres ("Action", "Comedy", "Horror") without training on a labeled dataset.

2. Solution Implemented
I developed a solution in Unit1_Project_MovieClassifier.ipynb (formerly project_42.ipynb) using the zero-shot-classification pipeline.

Initial Approach (Failure):

- Model: distilbert-base-uncased

    - Result: Random guessing (33% accuracy).

    - Reason: The model was not trained on Natural Language Inference (NLI) data and could not understand the relationship between text and labels.

Optimization (Partial Success):

- Model: MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli

    - Result: The model confused physical keywords (e.g., "explosions" in a comedy) with the "Action" label.

Final Solution (Success):

- Model: facebook/bart-large-mnli

    - Technique: Implemented a custom Hypothesis Template ("The genre of this movie is {}.") and refined labels to be more specific ("Action movie" vs "Action").

    - Result: Achieved accurate classification (>80% confidence) across all three test cases, correctly identifying a Comedy plot despite the presence of "action" keywords.    

### Part 3: Key Concepts Understood

Zero-Shot Learning (NLI): I learned that zero-shot classification works by treating classification as an "entailment" problem (Does sentence A imply label B?). Standard masked models (like DistilBERT) cannot do this; NLI-tuned models (like BART-MNLI) are required.

Prompt Sensitivity: The format of the input matters significantly. Changing labels from single words ("Action") to phrases ("Action movie") and using hypothesis templates guides the model's attention effectively.

Encoder vs. Decoder Limitations: Through the benchmark, I validated that model architecture dictates capability. Encoders are superior for understanding/masking, while Encoder-Decoders are required for generation.