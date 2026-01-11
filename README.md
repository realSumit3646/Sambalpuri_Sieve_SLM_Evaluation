# Sambalpuri_Sieve_SLM_LLM_Evaluation
Probing Dialectal Drift and Cultural Grounding in Language Models: A Case Study on Standard Odia vs. Sambalpuri language. 
# Dialectal Robustness Evaluation

## Description
Sambalpuri is a dialect of Odia Language with approximately 10 million speakers. Despite having such a large speaker base, it remains a low-resource language in digital ecosystem. Being a native speaker, I found that even the most used LLMs like Google's Gemini and openAI's GPT fail to translate to Sambalpuri Language and often resort to the use of parent language (Odia) for certain parts. Here I am studying this Standard Language Bias by quantifying the failure. I will quantify the Lexical, Morphological and Syntactic Drift where models incorrectly defaults to Standard Odia (the high-resource parent language) or Hindi/Bengali in some cases during the translation tasks. By making a 100-sentence expert-annotated parallel corpus, I establish a benchmark to measure the fidelity of dialectal nuances—such as unique verb conjugations and pronominal systems—that are currently lost in mainstream multilingual representations. This wwork would help to identify limitations in claimed "multilingual support" and help to increase accessibility of the models. 

## Project Objective
This project evaluates the performance of Small Language Models (SLMs) like Microsoft Phi-3.5 & Phi 4 Mini and Large Language Models like Gemini on Sambalpuri, a low-resource dialect of Odia. The goal is to quantify "Standard Language Drift" and assess cultural grounding.

## Methodology
- **Anchor Language:** English
- **Gold Standard:** Expert-annotated (Standard Odia via Odia Lecturer and NET(JRF) qualifier; Sambalpuri via Native Speaker).
- **Target Models:** Microsoft Phi-3.5-mini, Gemini 1.5 Flash.

## Current Status
- [x] Project Setup
- [x] Data Collection (100/100 sentences annotated with linguistic features)
- [ ] Model Inference (In Progress: Running Phi-3.5, Phi 4 and Gemini)
- [ ] Error Analysis & Visualization

## Critical Findings
### 🚨 Script Contamination in Phi-3.5
During the evaluation of **Microsoft Phi-3.5**, I came across a significant **Language Identification (LID) failure**. 
- **The Issue:** The model incorrectly classified Odia as a "Bengali-Assamese" language on prompting it about Odia knowledge.
- **The Result:** When prompted for Standard Odia or Sambalpuri, the model hallucinates translations using **Bengali script and grammar**.
- **Impact:** This suggests a significant "representation gap" in the model's pre-training data for the Odia language family, leading to high-resource language (Bengali) interference.

### 🚨 Semantic Hallucination, Meta Hallucination and Script Contamination in Phi 4
While evaluating **Microsoft Phi 4-mini**, I found the following gaps - 
- **Semantic Hallucination:** I asked the meaning of ଉଠ ଓ ମୁହଁ ଧୁଅ (Wake up and wash your face). The model replied - ଉଠ means "Yes" and ମୁହଁ means "Agreement.". This is a Total Semantic Failure. The model recognizes the script but has lost the mapping to the actual Odia dictionary. It is making guesses based on common conversation patterns (like affirmation) rather than translation.
- **Meta Hallucination:** The model confidently claims it can assist with "cultural nuances associated with Odia" right after failing a 1st-grade level translation.This is an Alignment Issue. The model's "confidence" is not calibrated with its "competence."
- **Script Contamination(Nagari/Bengali Interference):** When asked for Odia, it gave me: তোমার কেমন আছেন? (Bengali script) and तुम्हारी किमकम आश्वस्त? (Devanagari/Hindi script).
Complete conversation can be found in Results/phi4_mini_hallucination_log.txt
graph TD;
    A[English Input] --> B{LLM Processor};
    B -->|High Confidence| C[High-Resource: Bengali/Hindi];
    B -->|Low Confidence| D[Low-Resource: Sambalpuri/Odia];
    C --> E[Script Contamination & Hallucination];
    D --> F[Accurate Dialectal Output];
    style E fill:#f96,stroke:#333,stroke-width:2px;
