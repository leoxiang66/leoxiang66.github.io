---
title: " Extending a Newton-CG Second-order Optimizer to Natural Language Processing"
publication_types:
  - "7"
authors:
  - admin
abstract: "While Convolutional Neural Networks (CNNs) are a prominent class of
  machine learning models that are mainly applied to analyze visual imagery,
  Recurrent Neural Networks (RNNs) and the cutting-edge Attention Networks,
  Transformer Networks are another significant class of machine learning models
  that are mainly applied to deal with Natural Language Processing problems
  (NLP). Training these networks requires vast computing resources: due to a
  large amount of training data and due to the many training iterations. To
  speed up learning by reducing the necessary number of iterations to
  convergence, many specialized algorithms have been developed. First-order
  methods (using just the gradient) are the most popular, but second-order
  algorithms (using Hessian information) are gaining importance. We have a
  second-order optimizer called Newton-CG that has already shown speed-up and
  accuracy benefits compared with first-order optimizers for image
  classification problems in Mihai Zorca’s bachelor thesis [1]. In this thesis,
  we continue the comparison between Newton-CG and first-order optimizers, but
  we focus on NLP problems or Sentiment Analysis problems more specifically. We
  implemented two models: One is RNN based and the other is Self-Attention
  based. We trained these two models using Newton-CG optimizer and other
  first-order optimizers and recorded their loss and accuracy. We also tried to
  improve Newton-CG’s performance by using Adam to pretrain. In contrast, the
  performance of Newton-CG on sentiment analysis is not as good as on image
  classification. The performance of Newton-CG on the RNN model is very
  unstable, and the accuracy is only higher than that of SGD. On the Attention
  model, the performance of Newton-CG is more stable and the accuracy is
  higher."
draft: false
featured: true
tags:
  - NLP
image:
  filename: featured
  focal_point: Smart
  preview_only: false
date: 2021-09-16T13:58:21.624Z




# url_pdf: ''
url_code: 'https://github.com/leoxiang66/Bachelor-thesis---Extension-of-a-second-order-optimizer-to-NLP'
# url_dataset: 'https://github.com/wowchemy/wowchemy-hugo-themes'
# url_poster: ''
# url_project: ''
# url_slides: ''
# url_source: 'https://github.com/wowchemy/wowchemy-hugo-themes'
# url_video: 'https://youtube.com'
---
---
