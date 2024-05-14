---
title: "Neural Information Processing Systems 2023"
summary: My impressions, thoughts, and favourite papers on the 2023 edition of NeurIPS, which I attended in person in New Orleans.
series: ["Conferences", "Mahine Learning", "Travel", "NeurIPS", "USA", "New Orleans"]
weight: 1
aliases: ["/blog/NeurIPS23/"]
tags: ["Conferences", "NeurIPS", "Travel", "Machine Learning", "Deep Learning", "USA", "New Orleans"]
author: ["Lucas Miranda"]
cover:
  image: neurips_logo.png
  alt: "NeurIPS23 logo"
---

### General impressions

### Contrast Everything: A Hierarchical Contrastive Framework for Medical Time-Series

This paper \cite{wang2023contrast} introduces COMET, a contrastive learning framework to learn representations of medical time series in a self-supervised manner. Their main contribution is a hierarchical architecture with four main loss terms, aiming to contrast single observations, trials, samples, and patients, exploiting and learning from different levels of data consistency that they claim may be lost otherwise. For all levels, they use variants of the NCE (Noise Contrastive Estimation loss), where positive and negative pairs are sampled using a masking-based augmentation procedure:


1. *Observation level:* augmented observations of the same time point are treated as positive pairs ($(x_{i,t}, \tilde{x}_{i,t})$), while real and augmented observations of different time points are treated as negative ($(x_{i,t}, x_{i,-t})$ and ($(x_{i,t}, \tilde{x}_{i,-t})$), where $x$ is a given sample, $\tilde{x}$ an augmented sample, $i$ the sample index, and $t$ the time index.

2. *Sample level:* augmented observations of the same sample are treated as positive pairs ($(x_{i}, \tilde{x}_{i})$), while real and augmented observations of different samples are treated as negative ($(x_{i}, x_{j})$ and ($(x_{i}, \tilde{x}_{j})$).


3. *Trial level:* same as for samples, but taking subsets of each time series into account for memory purposes. Of all four, it's the only one that comes as an artefact of hardware limitations, rather than from real consistencies in the data themselves.

4. *Patient level:* same as for samples, but with time series grouped into belonging to the same patient or not. Different time series sampled from the same individuals are regarded as positive pairs, and as negative otherwise.
\end{enumerate}

Their experiments focus on regularly sampled EEG data with little to no missing values, which simplifies the problem significantly when compared to our ICU projects. However, their contrastive framework seems like a great starting point for multi-level learning from ICU time series as well. They beat several time series self-supervised baselines in detecting myocardial infarction and Parkinson's disease.

### Time Series as Images: Vision Transformer for Irregularly Sampled Time Series

This work \cite{li2023time} presents a peculiar way of processing irregularly sampled time series for supervised learning, based on plotting the collected values as images and processing them using a pretrained vision swin transformer \cite{liu2021swin}. The authors run several unorthodox experiments on how different styles of plotting influence performance, such as markers, interpolation, variable order, and colours.
Moreover, they include several experiments on ICU data, and provide direct comparisons with many baselines including SeFT from MLCB \cite{horn2020set}, which they beat in various tasks including sepsis and mortality prediction on physionet 2019 and 2012 challenge data, respectively. They train the models with a simple binary cross-entropy loss, while upsampling the minority class. Interestingly, they also included static information (age, height, weight, sex, demographics) as a paragraph embedded with a language model (RoBERTa-base \cite{liu2019roberta}). Time series and static text embeddings were concatenated prior to classification.

All in all, this is a highly unorthodox approach that clearly demonstrates the generalising capabilities of large vision models, but whether their ideas can be extended to improve irregular time series' representation learning remains unclear.

### An Iterative Self-Learning Framework for Medical Domain Generalization

Finally, the third work in this summary presents an approach to mitigate distribution shift in electronic health record (EHR) data, named SLDG (Self-Learning Domain Generalization) \cite{wu2023an}. In a nutshell, the approach starts by grouping the features semantically in different classes (such as statics, symptoms, treatments, and medical history). Each feature subset is embedded with a trained encoder to an individual latent space, and a series of \textit{latent domains} are retrieved for each modality using hierarchical clustering, with the number of clusters selected automatically based on the silhouette score. This makes it easier to unravel really specific clusters as the intersection of not-so-rare groups of specific features (such as a cluster of \textit{older male patients with a history of smoking and type 2 diabetes}, which can be decomposed in \textit{older}, \textit{male}, \textit{smoking}, and \textit{type 2 diabetes}). Lastly, individual classifiers for a given target variable are trained for each of these feature classes, with clusters recomputed every 20 epochs. 

Their experiments focus on 15-day readmission and mortality prediction on both MIMIC-IV and eICU, with data splits maximising the temporal and spatial gaps between samples, the latter based on the geographical location of the hospitals. They beat several domain generalization baselines on all tasks and metrics, which sounds promising. Interestingly, they do not provide generalization metrics between eICU and MIMIC-IV; only within each dataset individually.

All in all, this seems like a smart approach to efficiently group representations based on prior knowledge about the features, which can have a positive impact on domain shift. Application of these ideas to future iterations of our ICU representation learning projects could be worth a try.


### AmadeusGPT: a natural language interface for interactive animal behavioral analysis