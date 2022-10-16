---
title: Learning Rich Representation of Keyphrases from Text
date: 2022-10-16T12:08:22.013Z
draft: false
featured: false
authors:
  - admin
tags:
  - nlp;deeplearning
categories:
  - paper_reading;
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Keyphrase Boundary Infilling with Replacement (KBIR)

A new pre-training objective *Keyphrase Boundary Infilling with Replacement*

*   combines two indivisual tasks

    *   Keyphrase Boundary Infilling (KBI)

    *   Keyphrase Replacement Classification (KRC)

*   jointly learnt in a multi-task learning setup

![](https://i.imgur.com/uisWcIq.png)


*   built this framework on the top of RoBERTa which implements random token Masked Language Modeling

    *   the framework (LM) essentially optimizing MLM along with KBI and KRC objectives.

## Keyphrase Boundary Infilling (KBI)

*   builds upon the Span Boundary Objective (SBO) from SpanBERT

*   the Text Infilling setup from BART

Concret Methods:

1.  Similar to BART, we replace the entire span, in this case a keyphrase, with a single \[MASK] token as shown in Figure 2 and predict the original tokens using positional embeddings in conjunction with boundary tokens.

    ![](https://i.imgur.com/ZAqIXRs.png)


2.  mathematically:

    1.  We denote the output of the transformer encoder for each token $x_l$ in the sequence $x_1, \ldots, x_L$ as $\mathrm{x}_1$.

        $x_l$:  embedding of the $l$th token in the input sequence $x_1, \ldots, x_L$

    2.  However, since the entire span of tokens $\left(x_s, \ldots, x_e\right)$ of a keyphrase $y_m$ is masked with a mask token $x_m$, it is represented with a single vector $\mathbf{x}_{\mathbf{m}}$, where $(s, e)$ indicates its start and end positions and $m$ represents the index of a masked keyphrase span.
        $m \in [1,L]$

    3.  We set a maximum possible number of tokens corresponding to a keyphrase span, $T_{\max }$ such that $i \in\left[1, T_{\max }\right]$.&#x20;
        $T_{\max }$: 关键短语的最大token数, i.e. $T_{\max } = \max{(e - s)}$`由模型学得，see subsection 5`$i$: index of the target token in the keyphrase.&#x20;

    4.  We then predict the sequence of tokens to replace $x_m$ using the encoder embeddings of the external boundary tokens $x_{s-1}$ and $x_{e+1}$, as well as the position embedding $\mathbf{p}_{\mathbf{i}}$ of the target token as shown in Equation 1.

        $$
        \mathbf{y}_{\mathbf{i}}=f\left(\mathbf{x}_{\mathbf{s}-\mathbf{1}}, \mathbf{x}_{\mathbf{e}+\mathbf{1}}, \mathbf{p}_{\mathbf{i}}\right)
        $$

        $\mathbf{y}_{\mathbf{i}}$: vector for 第$i$个token (in the keyphrase)

        *   We use Layer Normalization (Ba et al., 2016) and GeLU (Hendrycks and Gimpel, 2016) activation function to represent $f(\Delta)$

        *   We then use the vector representation $\mathbf{y}_{\mathbf{i}}$ to predict the potential token $x_i$ and compute the cumulative cross-entropy loss for each $i$ present within the unmasked $x_m$ as shown in Equation 2.

            $$
            \mathcal{L}_{\text {Infill }}(\boldsymbol{\theta})=\sum_{i=1}^{T_{\max }} \log p\left(x_i \mid \mathbf{y}_{\mathbf{i}}\right)
            $$

    5.  In addition to predicting the actual tokens, we use a classification head to predict the expected number of tokens corresponding to the $[\mathrm{MASK}]$ in anticipation of providing a stronger learning signal. Each possible length of the \[MASK] is represented as a class and therefore, the number of such classes is equal to the maximum number of possible tokens $\left(T_{\max }\right)$.&#x20;
        The architecture used for classifying the number of tokens is a single linear layer which is trained with cross-entropy loss $\mathcal{L}_{\mathrm{LP}}\left(x_m, z_m\right)$ along with the infilled masked token $x_m$ and the corresponding actual length of the span class $z_m$.&#x20;
        $z_{m}$: ground truth label

3.  The Keyphrase Boundary Infilling (KBI) objective is formally represented as:

    $$
    \mathcal{L}_{\mathrm{KBI}}(\boldsymbol{\theta})=\alpha \mathcal{L}_{\mathrm{MLM}}(\boldsymbol{\theta})+\gamma \mathcal{L}_{\text {Infill }}(\boldsymbol{\theta})+\sigma \mathcal{L}_{\mathrm{LP}}(\boldsymbol{\theta})
    $$

    where $\alpha, \gamma$ and $\sigma$ are co-efficients applied to each loss and are primarily used to normalize the losses across the tasks.

## Keyphrase Replacement Classification (KRC)

Apart from learning representations of keyphrase spans, we wanted our framework to have the ability to identify them within the context of a text input.&#x20;

Motivated by WKLM (Xiong et al., 2019) that explores pretraining a language model through weak supervision by replacing entities with random entities of the same type that belongs to a knowledge base, we adapt it to replace keyphrases by randomly choosing another keyphrase of variable length from the universe of keyphrases identified in a tagged corpus.&#x20;

The KRC task is then modeled as a binary classification task to determine whether a keyphrase is replaced or retained.

To implement this strategy, we construct a keyphrase universe by identifying the set of unique keyphrases tagged across the entire dataset. We then randomly shuffle this keyphrase universe and restrict it to 500,000 keyphrases for computational complexity. We use the concatenated representation of boundary tokens of a keyphrase $x_{s-1}$ and $x_{e+1}$ as input to a linear classifier as shown in Figure 1 . Given the label $y_k$ representing whether a keyphrase was replaced or not, the objective here is to minimize the binary cross-entropy loss $\mathcal{L}_{\mathrm{KRC}}\left(\left(x_{s-1}+x_{e+1}\right), y_k\right)$.

Finally, in order to train a LM with an objective of learning good keyphrase representations we use the KBIR pre-training strategy in which we jointly optimize the KBI loss and the KRC loss along with the already existing $\operatorname{MLM}$ loss $\left(\mathcal{L}_{\mathrm{MLM}}(\boldsymbol{\theta})\right)$ in RoBERTa. This is formally shown in Equation 4.

$$
\begin{aligned}
\mathcal{L}_{\mathrm{KBIR}}(\boldsymbol{\theta})=& \alpha \mathcal{L}_{\mathrm{MLM}}(\boldsymbol{\theta})+\gamma \mathcal{L}_{\text {Infill }}(\boldsymbol{\theta})+\
& \sigma \mathcal{L}_{\mathrm{LP}}(\boldsymbol{\theta})+\delta \mathcal{L}_{\mathrm{KRC}}(\boldsymbol{\theta})
\end{aligned}
$$


# KeyBart
- BART (Lewis et al., 2019) generates sequences of different lengths from the input perturbed with [MASK] tokens along with token addition and deletion. 
- On similar lines, we propose learning rich keyphrase representations by attempting to generate the Original Present Keyphrases in the Catseq format as proposed in (Meng et al., 2017) from an input perturbed with token masking, keyphrase masking, and keyphrase replacement as shown in Figure 2. We call this method **KeyBART**. 
- We maintain the order of occurrence of the keyphrases in the original document and remove duplicate occurrences. 
- Similar to BART, we use a reconstruction loss objective during training which is a cross-entropy loss between the output and set of expected keyphrases.