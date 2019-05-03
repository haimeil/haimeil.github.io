
---
layout:     post
title:      Information Extraction
subtitle:   Named Entity Reginition, Relation Extraction
date:       2019-05-03
author:     Haimei
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Web Search and Text Analysis
    - nlp
---




# Information Extraction

##### Goal of IE: 

turns the unstructured information embedded in texts into structure data (like a relational database)

##### The application of IE:

 stock analysis, medical and biological research, rumour dection

##### How to do IE:

Two task: 

### Named Entity Regnition

  To find each eneity in the text and label its type

- Detect the named entities in the text (named entity: organization, location., person, times, money)

- Difficutlies of NER: the ambiguity of segmentation; type ambiguity

- **NER as swquence labelling** word-by-word sequence lableing task, the assigned tag capture both the boundary and the type.

  - IO tagging: **I** represents a token that is inside an entity; **O** all token that are not entities (unable to distinguish between two entities of the same type that are right next to each other)

  - IOB tagging: **B** and **I** for the beginning and inside of each entity type, **O** for tokens outside any entity

    ![image-20190503151601894](/var/folders/xh/t6_np5_s7d5138lh517ct0t40000gn/T/abnerworks.Typora/image-20190503151601894.png)

- Theree satand families of **algorithms for NER tagging** 

  - Feature-based algorithm (MEMM/CRF) 

    ![image-20190503152022971](/var/folders/xh/t6_np5_s7d5138lh517ct0t40000gn/T/abnerworks.Typora/image-20190503152022971.png)

    **word shape**  features are used to represent the abstract letter pattern of the word by mapping lower-case letters to ‘x’, upper-case to ‘X’, numbers to ’d’, and retaining punctuation. This feature is critical for English newswise texts, but may be useless in non-edited or informally-edited sources.

    For example the named entity token L’Occitane will generate the following feature values:

     prefix(wi) = L
     prefix(wi) = L’
     prefix(wi) = L’O
     prefix(wi) = L’Oc word-shape(wi) = X’Xxxxxxxx 

     suffix(wi ) = tane
     suffix(wi ) = ane
     suffix(wi ) = ne
     suffix(wi ) = e short-word-shape(wi ) = X’Xx 

    **gazetteer** is a lit of place names

- **Neural algorithm** bi-LSTM

### Relation Extraction

to find and classify semantic relations among the text entities

**Methods of RE**

If having fixed ralation database:

- Rule-based (hand-written patterns/leico-syntactic pattern)

  *lexico-syntactic pattern*

  $NP_0  such  as  NP_1{,NP_2 ...,(and|or)NP_i},i ≥ 1$	 

  implies the following semantics 

  $∀NP_i,i ≥ 1,hyponym(NP_i,NP_0)$

  For example, given 

  *Agar is a substance prepared from a mixture of red algae, such as Ge- lidium, for laboratory or industrial use*

  allowing us to infer 

  $∀NP_i,i ≥ 1,hyponym(NP_i,NP_0) hyponym(Gelidium,red algae) $

  GOOD: high precision, can be tailored to specific domains

  BAD: low recall, huge manual effort required

- Supervised Learning uses anotated tests to train classifers to annotate an unseen test set.

  - step 1: find pairs of named entities
  - step 2: train a filtering classifier to make a binary decision as to wherether a given pair of entities are related. (Positive means having relation, negative means not)
  - step 3: train another filtering classifer to assign label to the relations found by step 2

  GOOD: can get high accuracies,

  BAD: expensive, brittle( don't generalize well to differerent text genres)

- Semisupervised via Bootstrapping

  Bootstrap a classifer based aon a given high-precision seed patterns (tuples) to learn and discover new patterns.

  ![image-20190503161538242](/var/folders/xh/t6_np5_s7d5138lh517ct0t40000gn/T/abnerworks.Typora/image-20190503161538242.png)

  For example: 

  Given: *Ryanair has a hub at Charleroi*

  Find the set of sentences:

  1. Budget airline Ryanair, which uses Charleroi as a hub, scrapped all weekend flights out of the airport. 

  2.  All flights in and out of Ryanair’s Belgian hub at Charleroi airport were grounded on Friday... 

  3. A spokesman at Charleroi, a main hub for Ryanair, estimated that 8000 

     confidence values passengers had already been affected. 

  Then extract patterns as the following:

  ```
     / [ORG], which uses [LOC] as a hub /
     / [ORG]’s hub at [LOC] /
     / [LOC] a main hub for [ORG] /
  ```

  GOOD: new small anotated patterns

  BAD: semantic drift(an erroneous pattern leads to the introduction of erroneous tuples, can't tackle by using confidence values)

- Distant supervision

  Don't asumme the existence of seed tuples, but mines new tuples. Combines the **combines the advantages of bootstrapping with supervised learning**. It uses a large database to acquire a huge number of seed examples, creates lots of noisy pattern features from all these examples and then combines them in a supervised classifier. 

  GOOD: no semantic drift, share the advantages of the other method mentioned above, can create training tuples to be used with neural classifiers, where features are not required

  BAD: rely on a lare engouth database 

If have no labeled training data, and not even any list of relations:

It is an **Open Information Extraction (OpenIE)** task. The relations are simply strings of words (usually begin with a verb)

- Unsupervied

  For example, given   *United has a hub in Chicago, which is the headquarters of United Continental Holdings*,

  Get the output: 

  ```
  r1: <United, has a hub in, Chicago> 
  r2:  <Chicago, is the headquarters of, United Continental Holdings>
  ```

  GOOD: handle a huge numver of relations without having to specify them in advance

  BAD: map large sets of strings into canonical form, current method heavily on relations expressed with verbs, and so will miss many relations that are expressed nominally. 

  

###Evaluation

#### NER 

computing precision, recall. F1-measure at the entity level

####RE

Supervised: by using test sets with human- annotated, gold-standard relations and computing precision, recall, and F-measure. 

Semi-supervised & unsupervised: need some human evalution, massive datasets used in these settings are impractical to evaluate manually: use a small smple, can only obtain precision, not recall



### Extracting Times

Extract temporal expressions and then normalized it.

Temporal expressions: mostly rule-base approaches

Temporal normalisation: map a temporal expression to either a specific point in time or to a duration

Temporal anchor:  last week, yesterday...

### Event Extraction

similar to RE, including annotation and learning methods.

event ordering: bdetect how a set of events happened in a timeline. 

- involes both event extraction and temporal extraction
- Useful for rumour detection

### Template filling

can recognize stereotypical situations in texts and assign elements formt he text to roles represented as fiexed sets of slots. can help learing algorithms

![image-20190503170753976](/var/folders/xh/t6_np5_s7d5138lh517ct0t40000gn/T/abnerworks.Typora/image-20190503170753976.png)



Reference:

JM3

Jurafsky, Daniel S.; Martin, James H.; *Speech and language processing: an introduction to natural language processing, computational linguistics, and speech recognition*, Third Edition (incomplete draft).
