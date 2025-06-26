# **From Moments to Meanings: Egocentric Vision**

This repository contains the official implementation for the paper: **"From Moments to Meanings: Egocentric Temporal Localization and Answer Generation with VSLNet and Video-LLaVA on Ego4D"**.  
**Authors:** [Arian Mohammadi](https://github.com/arianmo477), [Hassine El Ghazel](https://github.com/HassineEG), Hadi Abdallah, Ali Ayoub.

## **📜 Abstract**

Egocentric vision focuses on understanding videos captured from a first-person perspective, posing unique challenges such as rapid camera motion and a restricted field of view. The [Ego4D dataset](https://ego4d-data.org/) provides a comprehensive benchmark for this domain through the Natural Language Queries (NLQ) task, which requires retrieving relevant video segments based on textual queries.  
In this work, we evaluate the Video Span Localizing Network (VSLNet) and its simplified variant, VSLBase, under various configurations of video and text feature extractors—including **Omnivore** and **EgoVLP** for visual features, and **GloVe** and **BERT** for language embeddings. To extend egocentric video understanding beyond temporal localization, we propose a two-stage pipeline that combines segment retrieval with **Video-LLaVA** for natural language answer generation, enabling both fine-grained localization and semantic reasoning. We then evaluate how well Video-LLaVA performs compared to other multimodal natural language models.

## **🚀 Project Pipeline**

Our core contribution is a two-stage pipeline that integrates temporal localization with generative question answering:

1. **Stage 1: Temporal Localization with VSLNet**  
   * Given a long, untrimmed egocentric video and a natural language query, we use VSLNet to predict the start and end timestamps of the most relevant segment.  
   * We experiment with different feature extractors to find the optimal combination for this task.  
2. **Stage 2: Answer Generation with Video-LLaVA**  
   * The localized video segment from Stage 1 is fed into Video-LLaVA.  
   * This powerful multimodal model then generates a descriptive, human-like answer to the initial query based on the content of the clip.

A visual representation of our two-stage approach.

## **✨ Key Features**

* **Robust Temporal Localization:** Implementation of VSLNet and VSLBase for the Ego4D NLQ task.  
* **Modular Feature Extraction:** Easily configurable backbones, supporting:  
  * **Visual Features:** Omnivore, EgoVLP  
  * **Text Features:** GloVe, BERT  
* **Generative Q\&A:** Integration with Video-LLaVA to move beyond simple localization and provide semantic understanding.  
* **Comprehensive Evaluation:** Detailed analysis of various model and feature combinations on the Ego4D benchmark.

## **📊 Dataset**

This project uses the **Ego4D** dataset, specifically focusing on the Natural Language Queries (NLQ) challenge.  
You will need to download the dataset, including the videos and annotations, from the official website: [ego4d-data.org](https://ego4d-data.org/).  
Place the downloaded data in a data/ directory or update the paths in the configuration files accordingly.

## **⚙️ Installation**

1. **Clone the repository:**  
   git clone https://github.com/arianmo477/EgoCentricVisionPolito.git  
   cd EgoCentricVisionPolito

2. **Create a virtual environment (recommended):**  
   python3 \-m venv venv  
   source venv/bin/activate

3. **Install the required dependencies:**  
   pip install \-r requirements.txt

## **📈 Results**

Our experiments evaluate the performance of both stages of our pipeline: Temporal Localization and Answer Generation.

### **Temporal Localization Performance**

We tested various combinations of visual (EgoVLP, Omnivore) and textual (BERT, GloVe) feature extractors with VSLNet and its simpler variant, VSLBase.  
**Key Findings:**

* **BERT vs. GloVe:** Across all model configurations, using **BERT** embeddings consistently and significantly outperforms **GloVe**. This highlights the importance of contextualized word embeddings for understanding natural language queries in this task.  
* **EgoVLP vs. Omnivore:** The **EgoVLP** visual features consistently yield better results than Omnivore, demonstrating its superior capability in capturing the nuances of egocentric video.  
* **Best Performing Model:** The combination of **EgoVLP \+ VSLBase with BERT** embeddings achieves the highest scores across all metrics, making it our top-performing model for temporal localization.

Below is a summary of our results:

| Model | Embedding | Rank1@0.3 | Rank1@0.5 | Rank3@0.5 | mIoU |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **EgoVLP \+ VSLBase** | **BERT** | **8.57** | **5.16** | **9.09** | **6.65** |
| EgoVLP \+ VSLBase | GloVe | 5.24 | 3.28 | 6.04 | 4.32 |
| **EgoVLP \+ VSLNet** | **BERT** | **6.12** | **3.87** | **6.50** | **4.98** |
| EgoVLP \+ VSLNet | GloVe | 4.78 | 2.97 | 5.21 | 3.71 |
| **Omnivore \+ VSLNet** | **BERT** | **6.43** | **3.74** | **6.38** | **4.96** |
| Omnivore \+ VSLNet | GloVe | 4.21 | 2.27 | 4.49 | 3.52 |
| **Omnivore \+ VSLBase** | **BERT** | **5.50** | **3.33** | **6.09** | **4.65** |
| Omnivore \+ VSLBase | GloVe | 3.51 | 1.81 | 3.77 | 3.05 |

### **Answer Generation Performance**

For the second stage, we evaluated the quality of answers generated by different models. We compared **Video-LLaVA** against several strong baselines, including variants of Google's Gemma (G1.5F, G1.5P, G2.5F, G2.5P) and GPT-4o.  
**Key Findings:**

* The models show varied performance across different metrics.  
* **G1.5F/P** variants excel in ROUGE scores, indicating better performance in terms of recall and n-gram overlap with ground-truth answers.  
* **G2.5P** achieves the highest BLEU-2 score, suggesting better precision in bigram matching.  
* Our integrated **Video-LLaVA** model demonstrates a strong balance, achieving the highest **BERTScore Precision**, which measures semantic similarity, indicating that its generated answers are contextually very relevant.

| Metric | Variant | LLaVA | G1.5F | G1.5P | G2.5F | G2.5P | GPT-4o |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **BLEU** | BLEU-1 | 0.2558 | 0.2684 | **0.3115** | 0.2889 | 0.3256 | 0.2785 |
|  | BLEU-2 | 0.1607 | 0.1677 | 0.1992 | 0.1741 | **0.2058** | 0.1691 |
|  | BLEU-3 | 0.0901 | 0.1023 | **0.1214** | 0.0995 | 0.1191 | 0.0955 |
|  | BLEU-4 | 0.0333 | 0.0419 | **0.0524** | 0.0449 | 0.0507 | 0.0434 |
| **ROUGE** | R-1 | 0.2825 | 0.2852 | **0.3449** | 0.3097 | 0.3420 | 0.3073 |
|  | R-2 | 0.1280 | 0.1047 | 0.1373 | 0.1065 | **0.1442** | 0.1047 |
|  | R-L | 0.2799 | 0.2819 | **0.3439** | 0.3080 | 0.3359 | 0.3030 |
|  | R-Lsum | 0.2800 | 0.2798 | **0.3456** | 0.3062 | 0.3374 | 0.3031 |
| **BERTScore** | Precision | **0.8964** | 0.8871 | 0.8906 | 0.8859 | 0.8938 | 0.8824 |
|  | Recall | 0.8851 | 0.8923 | **0.8996** | 0.8910 | 0.8975 | 0.8952 |
|  | F1 Score | 0.8904 | 0.8893 | 0.8946 | 0.8880 | **0.8952** | 0.8883 |

## **📜 Citation**

If you find this work useful in your research, please consider citing our paper:  
@inproceedings{mohammadi2024moments,  
  title={From Moments to Meanings: Egocentric Temporal Localization and Answer Generation with VSLNet and Video-LLaVA on Ego4D},  
  author={Arian Mohammadi and Hassine El Ghazel and Hadi Abdallah and Ali Ayoub},  
  year={2024},  
  booktitle={Proceedings of ...},  
  note={GitHub: https://github.com/arianmo477/EgoCentricVisionPolito}  
}

*(Please complete the BibTeX entry once the paper is published).*

## **🙏 Acknowledgments**

This project builds upon the fantastic work of several research teams. We would like to thank:

* The creators of the [Ego4D dataset](https://ego4d-data.org/).  
* The authors of [VSLNet](https://github.com/microsoft/VSLNet).  
* The developers of [Video-LLaVA](https://github.com/PKU-YuanGroup/Video-LLaVA).  
* The teams behind [Omnivore](https://github.com/facebookresearch/omnivore), [EgoVLP](https://github.com/showlab/EgoVLP), [BERT](https://huggingface.co/docs/transformers/model_doc/bert), and [GloVe](https://nlp.stanford.edu/projects/glove/).
