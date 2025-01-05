## BioBERT for Biomedical Named Entity Recognition (NER) ##
This repository contains the implementation and experiments related to fine-tuning BioBERT for biomedical Named Entity Recognition (NER). BioBERT is a pre-trained BERT-based model specifically designed for biomedical text mining. This project focuses on fine-tuning BioBERT to identify disease-related entities from biomedical text.


# Introduction #
Biomedical NER is a critical task in healthcare and biomedical research. It helps in automatically extracting important information, such as diseases, chemicals, and genes, from vast amounts of scientific literature. This project focuses on fine-tuning BioBERT for disease entity recognition using two publicly available datasets:

`NCBI Disease Dataset`
`BC5CDR Dataset`
The goal is to improve the performance of BioBERT in biomedical NER tasks by fine-tuning it on these domain-specific datasets.

# Installation #
To get started, clone this repository and install the required dependencies:

Clone the repository:


`git clone https://github.com/your_username/bioBERT-NER.git`
`cd bioBERT-NER`
Create and activate a virtual environment:


`python3 -m venv venv`
`source venv/bin/activate`  # On Windows, use `venv\Scripts\activate`
Install the necessary Python libraries:


`pip install -r requirements.txt`
The requirements.txt file includes:

`transformers `for working with BioBERT.
`datasets` for loading and preprocessing datasets.
`seqeval` for evaluation metrics like precision, recall, and F1-score.
`pandas`, `numpy` for data manipulation.
# Setup #
Datasets
The primary datasets used in this project are:

`NCBI Disease Dataset`: A collection of PubMed abstracts with annotated disease mentions.
`BC5CDR Dataset`: Contains PubMed articles with annotated diseases and chemicals.
You can download these datasets from their respective sources or use the datasets library to load them directly:


`from datasets import load_dataset
dataset = load_dataset('path_to_dataset')`
Model Configuration
The model used in this project is `BioBERT`, which is a BERT-based model pre-trained on large-scale biomedical corpora. The fine-tuning process adjusts the model for the NER task.

Usage
Training
Load and preprocess the datasets:


`from datasets import load_dataset
dataset = load_dataset('path_to_combined_dataset')
Fine-tune the BioBERT model:`


`from transformers import AutoModelForTokenClassification, Trainer, TrainingArguments
from transformers import AutoTokenizer`

`model = AutoModelForTokenClassification.from_pretrained('biobert-v1.1')
tokenizer = AutoTokenizer.from_pretrained('biobert-v1.1')`

`training_args = TrainingArguments(
    output_dir='./results',
    evaluation_strategy="epoch",
    save_strategy="epoch",
    learning_rate=1e-5,
    num_train_epochs=30,
    weight_decay=0.01,
)`

`trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
)`

`trainer.train() `
Evaluation
After training, evaluate the model on the test dataset:


`from transformers import pipeline`

# Load trained model
`model = AutoModelForTokenClassification.from_pretrained('./results')
tokenizer = AutoTokenizer.from_pretrained('biobert-v1.1')`

`nlp = pipeline("ner", model=model, tokenizer=tokenizer)
result = nlp("A sample text to test the model")
print(result)`
# Experiments and Results #
Experiment 1: BioBERT v1.1 on BC5CDR Dataset
Accuracy: 0.969673
F1-Score: 0.878531
Recall: 0.907745
Precision: 0.851139
Experiment 2: BioBERT v1.1 on NCBI Disease Dataset
Accuracy: 0.984875
F1-Score: 0.821752
Recall: 0.864041
Precision: 0.783410
Experiment 3: Fine-Tuning BioBERT on Combined NCBI & BC5CDR Datasets
Accuracy: 0.989194
F1-Score: 0.964904
Recall: 0.967268
Precision: 0.962552
Experiment 4: Fine-Tuning BioBERT Pre-trained on Maccrobat
Accuracy: 0.962223
F1-Score: 0.852991
Recall: 0.859794
Precision: 0.846296
From these results, it is clear that the best performance was achieved in Experiment 3, where BioBERT was fine-tuned on a combined NCBI and BC5CDR dataset.

# Future Work #
To further improve the performance, several areas can be explored:

Improving Contextual Understanding: Enhance the model's ability to understand the nuanced context in longer biomedical documents.
Scaling to Large Datasets: Handle larger datasets efficiently to improve the model's generalization capability.
