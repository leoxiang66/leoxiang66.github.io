---
title: Knowledge-driven Spoken Dialogue
date: 2022-8-16T13:10:22.369Z
summary: A﻿ dialogue system that can conduct a conversation using information
  from the provided knowledge graph.
draft: false
featured: false
tags:
  - Knowledge Graph
  - Conversational AI
  - Dialogue
links:
  - url: https://github.com/NLP-Guild/Knowledge-driven-spoken-dialogue
    name: GitHub
    icon_pack: fab
    icon: github
image:
  filename: featured.png
  focal_point: Smart
  preview_only: false
---
# Knowledge-driven-spoken-dialogue
[Official Web](https://developer.huawei.com/consumer/en/activity/starAI2022/algo/)
## 1 Introduction
Chatbots are often let down by their passiveness, which often leads to meaningless responses to user requests and limited information. That is why a dialogue system that is capable of interpreting messages and being informative is needed. However, required knowledge may come from different domains, and the samples are not evenly distributed across these domains. Worse still, a dialogue system may need to make complex queries or inferences to fetch the correct information from the knowledge graph. Another challenge is when users verbally make requests, which are converted to text using the automatic speech recognition (ASR) technology, the converted text often contains errors. Therefore, a dialogue model is expected to be more robust when encountering errors caused by colloquial expressions, as well as word and syntax errors in text converted using ASR, thus giving users standard and expected responses.

Evaluation Metric：the sum of their weighted scores in indicators for automatic scoring

### 1.1 Competition Description

The data of multi-round dialogues and knowledge graphs will be provided for you to set up knowledge-driven dialogue models. The tasks are as follows:
1. Knowledge selection
    - Objective: Interpret user questions and select the correct knowledge triples to answer the questions.
    - Input: dialogue history and knowledge base
    - Output: knowledge tripless
    - Scoring indicators: precision, recall, and F1 score
2. Response generation
      - Objective: Generate responses based on the selected knowledge triples.
      - Input: dialogue history, knowledge base, and knowledge triples
      - Output: natural, smooth, and reasonable responses
      - Scoring indicators: BLEU-1/2, DISTINCT-1/2, and generation_F1, all of which are calculated based on Chinese characters, for automatic scoring; informativeness (0-2), coherence (0-2), and factual accuracy $(0-2)$ for evaluation by judges

## 2 Dataset

1. raw dataset: [link](https://huggingface.co/datasets/Adapting/2022-GLOBAL-AI-CHALLENGE)
3. modified dataset: [link](https://huggingface.co/datasets/Adapting/Knowledge-Driven-Dialogues)

## 3 Methods
### Model Architecture
<!-- ![Model Architecture](https://img.kookapp.cn/assets/2022-08/MN7r0IPgTh10s1du.png) -->
<img src="https://img.kookapp.cn/assets/2022-08/MN7r0IPgTh10s1du.png" alt="Model Architecture" width="1000"/>

1. **Dialogue history examples**: `[usr] 你知道刘德华吗 [sys] 知道， 他是一名演员。 [usr] 那你看过他演的电影吗？`
2. **Entity Detection Model**: we implemented 1) rule-based model 2) lucene-based model
3. **External Knowledge Graph**: [link](https://huggingface.co/datasets/Adapting/2022-GLOBAL-AI-CHALLENGE/blob/main/kg.json)
4. **Knowledge examples**: 
    `[ett]` refers to `尼古拉斯·凯奇`
    *[ett] 的星座是摩羯座。[ett] 的Information是尼古拉斯·凯奇（Nicolas Cage），1964年1月7日出生于美国加利福利亚州，美国演员。1982年，17岁的尼古拉斯·凯奇进入电影行业，出演影片《开放的美国学府》。1984年，凯奇主演了影片《鸟人》。1988年，他出演了《吸血鬼之吻》。1992年，他凭借影片《我心狂野》中的表演，获得了第43届戛纳电影节金棕榈大奖。1996年，他主演的动作片《勇闯夺命岛》，并凭借《离开拉斯维加斯》中的酒鬼一角获得当年奥斯卡最佳男主角奖。而后出演《变脸》、《空中监狱》等动作片。2002年，凯奇执导了自己的导演处女作《Sonny》。2004年，尼古拉斯·凯奇主演的《国家宝藏》上映。2007年，尼古拉斯·凯奇凭借其主演的两部电影《灵魂战车》及《国家宝藏2》成为年度票房冠军。2010年，尼古拉斯·凯奇主演了电影《魔法师的学徒》。。[ett] 的主要成就是奥斯卡金像奖最佳男主角。[ett] 的职业是导演。[ett] 的毕业院校是加利福尼亚大学洛杉矶分校影剧系。[ett] 的主要成就是英国学院奖最佳男主角提名。[ett] 的身高是185cm。[ett] 的代表作品是离开拉斯维加斯。[ett] 的中文名是尼古拉斯·凯奇。[ett] 的代表作品是变脸。[ett] 的代表作品是战争之王。[ett] 的出生地是美国 加利福利亚州 长滩。[ett] 的国籍是美国。[ett] 的代表作品是国家宝藏。[ett] 的代表作品是勇闯夺命岛。[ett] 的代表作品是魔法师的学徒。[ett] 的主要成就是金球奖最佳男演员。[ett] 的主要成就是华鼎奖全球最佳男演员。[ett] 的外文名称是Nicolas Cage。[ett] 的职业是演员。[ett] 的职业是制片人。[ett] 的出生日期是1964年01月07日。[ett] 的代表作品是空中监*
5. **Dialogue Generator Input**: `[klg] {knowledge} {chat_history}`
6. **Dialogue Generator**: we fine-tuned [`uer/bart-base-chinese-cluecorpussmall`](https://huggingface.co/uer/bart-base-chinese-cluecorpussmall) on the training dataset

## 4 Result
see [result.json](https://github.com/NLP-Guild/Knowledge-driven-spoken-dialogue/blob/main/result/result.json)

## 5 Usage
see [result_generation.ipynb](https://github.com/NLP-Guild/Knowledge-driven-spoken-dialogue/blob/main/methods/result_generation.ipynb)

## 6 Roadmap
- [x] annotate the domains of entities [link](https://huggingface.co/datasets/Adapting/2022-GLOBAL-AI-CHALLENGE/blob/main/knowledge_data.csv)
- [ ] train a classifier on domains of the entities
- [ ] use IR models to match entities and attributes
- [ ] retrain the dialogue generator with more specific knowledge
