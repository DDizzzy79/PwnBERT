# PwnBERT

A project based on BERT to detect GLIBC vulnerabilities.

## What is PwnBERT

PwnBERT is a BERT-based vulnerability detection tool designed to identify and analyze Pwn-related vulnerabilities (e.g. UAF, heap overflow, etc.) in C language. By combining natural language processing techniques and security domain knowledge, this project aims to provide an efficient and reliable solution to help developers and security researchers identify potential security risks and thus strengthen code security. **In future, we will try to apply `PwnBERT` on actual open-source and kernels and see if it can detect vulnerabilities in real-world programs.**
### What is PwnBERT (In the way that human can understand)

Generally speaking, PwnBERT is a tool that helps find and analyze vulnerabilities in computer programs written in C language that could be exploited by attackers. It uses a technique called natural language processing and combines it with security expertise to make it easier for developers and security researchers to identify potential security risks and make code more secure. The goal is to create a reliable and efficient solution for identifying and preventing potential security threats.

Why you should Pay attention on this project?

* it used OpenAI API (ChatGPT) for acquiring training set
* it used AI training to detect complex vulns in codes, instead of identifying them via structure analyzing.

* it's made by me, ALL BY MY SELF! (i know it does not sound like a good reason but i will put it up here anyway:) )

## Where are we currently?

Currently, we have finished our main fine-tuning of our BERT module, which enable PwnBERT to indentify `Off-by-one` vulns ( after all we still decided to use DistilBert :sadfaceemoji  ) and the accuracy is pretty ideable. Optimization will be done.

### What is CodeBERT

CodeBERT is a state-of-the-art neural model for code representation learning. It is based on the Transformer architecture and is pre-trained on a large corpus of code. CodeBERT can be fine-tuned on various downstream tasks such as code classification, code retrieval, and code generation. By leveraging the pre-trained model, CodeBERT can effectively capture the semantic and syntactic information of code, which makes it a powerful tool for code analysis and understanding. In PwnBERT, we use CodeBERT to assist in identifying and analyzing Pwn-related vulnerabilities in C language.

## Plan introduction

In our project, we generally seperated our plan into few stages;
### Stage one
  * Make the trainset from a large amount of code ( generated by openAI API )

      * using elaborately designed prompt to generate specific codes section

      * fine-tuning using those samples & CodeBERT

### Stage two
  * Using data sample collected from websites like `CVE` to make a training-set
    * collect vuln list
    * retrive real vuln codes that are related to the vuln in vuln list
    * .....



# Stage one: First trial
In this part we will use `OpenAI API` to generate our training set, then fine-tune our BERT model using those samples. We select this appoarch first due to the fact that we are not sure if we can collect enough data for training, and we are not sure if we can collect data that are related to the vulns we want to detect.

## How to use?
### `generate_code_segments`
In this part, what we basically did is use `OpenAI API` 's `ChatGPT` to generate our prompt, then extract the code in `collect_generated_code(amount_of_time):`. You can test our code by following these steps:

1. `$ touch config.py` This will create a config file that will be used later for the `main.py` file

2. `$ echo "OPEN_AI_KEY = #YOUR_API_KEY"` Change `#YOUR_API_KEY` to your OpenAI API KEY

3. `$ python3 main.py` This will run the python file.

### `PwnBERT.py`

From executing this file, you can acquire your training sets and eval sets, remember to modifiy     `generate_tokens()` function in `main` function.

### `train.py` and `train_v2.py`

Due to some problem we have not solve yet, we decided to create `train_v2.py` for trainning. 

Generally speaking: This is a Python script for fine-tuning the DistilBert model for sequence classification using PyTorch and the transformers library. The script defines a CodeDataset class that inherits from PyTorch's Dataset class, which represents a dataset of code files. The CodeDataset class loads code files from two directories, one containing vulnerable code and the other containing non-vulnerable code, and preprocesses the code using the DistilBertTokenizer to generate token IDs, attention masks, and labels.

The script then defines a compute_metrics function that calculates the accuracy of the model on the evaluation dataset. The main function of the script, finetune_pwnbert, loads the DistilBertForSequenceClassification model from the transformers library, initializes the model with a specified number of output labels, and fine-tunes the model on the training dataset. The function takes as input four directory paths containing the training and evaluation datasets of vulnerable and non-vulnerable code, respectively, and saves the finetuned model and tokenizer in the specified output directory.

## Result
According to our test result, `PwnBERT` can identify relatively effective, average eval_loss=`MISSING DATA HERE`

<img width="825" alt="Xnip2023-04-05_10-02-03" src="https://user-images.githubusercontent.com/72267897/229962055-85ae8e41-136c-403c-b542-8c963ace824e.png">


However, due to the fact that we are using `BERT`, PwnBERT can only identify `Off-by-one` vuln that looks like the sample from trainset *(not sure if it comprehens it or not)*

# Stage two: More diverse and more realistic sample
## Our idea?
**In this particular stage, we will generally focus on collecting real and more diverse training samples.** For instance. We will use `CVE` as our main source of data, and we will try to collect as much data as possible. We will also try to collect data from other sources, such as `Github` and `Stackoverflow`.

However, with tons of researchs and tries, we found this abosolutely amazing repo called: `
juliet-test-suite-c` Which basically saved our life.

Basically, The Juliet Test Suite for C is a comprehensive suite of test cases designed to help developers and researchers identify and evaluate the effectiveness of static analysis and other security-related tools for detecting, diagnosing, and mitigating software vulnerabilities in C programs. Developed by the National Security Agency (NSA) Center for Assured Software (CAS) and the U.S. Department of Homeland Security (DHS), the suite contains over 81,000 test cases covering a wide range of CWEs (Common Weakness Enumerations).



# Update Logs
**Mainly updates after Mar 22, 2023:**

* v1.1, Mar 20: Started to use `concurrent.futures` for acceleration purposes.

* v1.2, Mar 22: Created `PwnBERT.py`, major adjust the structure of directories (because I need to import them), fix minor bugs and added new stuff on `generate_code_segments/` and `tokenize_codes`.

* v1.2.1: Fix bugs that might effect significantly on the codes

* v1.2.5(idk): Fix Mega bug
