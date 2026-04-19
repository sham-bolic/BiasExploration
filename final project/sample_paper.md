THINGS TO REFERENCE:  
[https://www.cis.upenn.edu/\~eliors/freng\_corpus.html](https://www.cis.upenn.edu/~eliors/freng_corpus.html)  
[https://huggingface.co/google-bert/bert-base-multilingual-cased](https://huggingface.co/google-bert/bert-base-multilingual-cased)  
[https://aclanthology.org/attachments/P17-1104.Notes.pdf](https://aclanthology.org/attachments/P17-1104.Notes.pdf)  
[https://arxiv.org/pdf/1810.04805](https://arxiv.org/pdf/1810.04805)  
[https://universalconceptualcognitiveannotation.github.io/](https://universalconceptualcognitiveannotation.github.io/)

Articles to look into  
[https://clic2024.ilc.cnr.it/wp-content/uploads/2024/12/87\_main\_long.pdf](https://clic2024.ilc.cnr.it/wp-content/uploads/2024/12/87_main_long.pdf)  
[https://aclanthology.org/2021.eacl-main.189.pdf](https://aclanthology.org/2021.eacl-main.189.pdf)  
[https://arxiv.org/pdf/1911.03310](https://arxiv.org/pdf/1911.03310)

[https://aclanthology.org/2020.findings-emnlp.389.pdf](https://aclanthology.org/2020.findings-emnlp.389.pdf)

- Also about BERT layers  
- Notes:  
- BERT acquires linguistic information at roughly the same progression as a traditional language processing pipeline (I can explain it in more detail, if either of you are interested)--basically, BERT’s early layers express surface features (sentence structure, morphological features, etc), middle layers; syntactic structure, and higher layers express semantic features(\!\!).  
- Got tired, no more tonight  
- 

[https://aclanthology.org/P19-1356/](https://aclanthology.org/P19-1356/) ← banger alert.

[https://arxiv.org/pdf/1906.04341](https://arxiv.org/pdf/1906.04341)

- Also about BERT layers  
- Notes:  
- Studying attention maps of a pre-trained model to determine learned linguistic structure.  
- Attention maps can be studied by looking at the weights of each word when computing the next word.  
- Paper focuses on BERT’s 144 attention heads and their behavior (esp. Around \[SEP\] token)  
- Some heads have 75+% accuracy on direct object relations.  
- Attention heads attend to specific tokens more than others; Early heads: \[CLS\], middle heads: \[SEP\], and deep heads: periods and commas. (they think this may just be due to the fact that these tokens appear the most and don’t really get masked out).   
- Believe \[SEP\] is actually just a no-op for the attention heads.   
- No head ‘gets’ all features of syntax very well. However, many heads do some aspect of syntax very well.   
- Head 8-10 had 86.8% accuracy on direct objects, head 8-11 had 94.3% accuracy on determiners (noun modifiers, generally), head 7-6 had an 80.5% accuracy on possessive relations, head 4-10 had 82.5% accuracy on passive auxiliary verbs, head 9-6 had a 76.3% accuracy on determining the object that a preposition modifies, and head 5-4  had a 65.1% accuracy for coreference relations. 

Other paper I found (the first paper references the second one) \- mostly focused on BERT layers:  
[https://arxiv.org/pdf/1905.05950](https://arxiv.org/pdf/1905.05950)

- BERT represents the steps of the traditional NLP piepone lin an interpretable and localized way  
- Regions responsible for each step appear in the expected sequence: POS tagging, parsing, NER, semantic roles, then conference  
  - Basically talks about how these steps are from the shallowest layer to the deepest layer  
- This paper probes the BERT model to quantify where specific types of linguistic information are encoded  
- Results on BERT-large \- 24 layers  
  - POS tags processed earliest, then constituents, dependencies, semantic roles, and coreference  
  - Basic syntactic information appears earlier in the network  
  - High-level semantic information appears at higher layers  
  - Constituents are represented earlier than coreference  
  - Syntactic information is more localizable, and weights related to syntactic tasks tend to be concentrated on a few layers  
  - Information related to semantic tasks is generally spread across the entire network  
- They observe the same general ordering on the 12-layer BERT-base model 

[https://arxiv.org/pdf/1911.03310](https://arxiv.org/pdf/1911.03310)

[https://clic2024.ilc.cnr.it/wp-content/uploads/2024/12/87\_main\_long.pdf](https://clic2024.ilc.cnr.it/wp-content/uploads/2024/12/87_main_long.pdf)

**TITLE: mBERT and the Encoding of Semantic Roles: Insights from UCCA Annotations in Parallel Corpora**

**Abstract**  
	This study explores the capabilities of mBERT in encoding semantic categories as established by the UCCA framework. We examine the model’s performance in mapping word-level embeddings to the UCCA semantic roles in mono-lingual settings, as well as within a cross-linguistic context. To this end, we focus our investigation on English and French, employing a UCCA-annotated parallel corpus. To identify which layers of mBERT are most effective when it comes to this encoding, we analyze each layer-specific output embedding and from these results, we observe that the intermediate to deeper layers of the model are generally the best-performing for encoding the semantic categories. This appears to be true both within a mono-lingual and a cross-lingual setting. The results have been evaluated based on accuracy. 

* Investigate the ability of mBERT to encode UCCA framework defined semantic roles  
* Evaluated mBERT’s abilities to map word-level embeddings to UCCA roles in monolingual settings as well as between languages  
* The languages we used were english and french  
* We analyzed mBERT using layer-wise embeddings, using the data generated from only 1 layer at a time to determine which layer best encodes UCCA roles  
* Indirectly, this allows us to see how well mBERT encodes semantics into their embeddings  
* Results are evaluated using the measure of accuracy  
* Results show that middle layers best encode the UCCA roles for mono-lingual settings as well as cross-lingual settings

**Introduction**  
Semantic role annotation is critical in understanding the linguistic meaning of a sentence or paragraph by assigning roles to words and characters in a sentence, including identifying actions, or recipients. To retrieve the word-level semantic roles, this paper uses a corpus annotated with the UCCA framework. The UCCA framework developed by Abend et al. (2017) is mainly used for grammatical representation, specifically, analyzing and annotating natural languages using semantic categories and graph structures. The ability to capture semantic roles is important in various NLP tasks such as translation and general semantic understanding. The model we focus on is the multilingual BERT (mBERT) model, which is the BERT model trained on multiple different languages. This paper investigates how well mBERT encodes semantic roles defined by the UCCA framework in both monolingual (English and French) as well as cross-lingual settings, specifically, aiming to identify which layers of mBERT best capture these roles.  
Previous works have also analyzed mBERT’s abilities to perform in multilingual settings, including the study by Muller et al. (2021) in which they explored how mBERT’s layers contribute to cross-lingual tasks, particularly focusing on syntactic tasks such as POS tagging, dependency parsing, and named-entity recognition. Their results demonstrated that the deeper part of the model is most critical for cross-language transfer while the upper layers have weaker cross-lingual abilities. Our work shifts the focus of mBERT’s performance within their layers to cross-lingual semantic abilities by analyzing the encoding of high-level semantic representations within the model’s word-level embeddings.   
Libovicky et al. (2019) also presents a study that focuses on analyzing the cross-lingual representations for semantic tasks to analyze mBERT’s language neutrality. Their paper uses a parallel self-annotated corpus with a trained classifier to probe mBERT to determine if it is able to identify the language of a sentence from its representation. Their study also evaluates how well mBERT represents word-level semantics for bilingual alignment. Our study builds off of their research by emphasizing on the encoding of UCCA semantic roles, which represent high-level semantic abstractions in a language. Additionally, we evaluate mBERT’s semantic encoding using a logistic regression as a classifier, focusing specifically on whether semantic roles are encoded in mBERT embeddings.  
Our paper also aims to evaluate each of mBERT’s layers in the semantic encoding task, which aims to add evidence to the paper by Tenney et al. (2019). Their study performs evaluation on both the layers of BERT (12 layers) and BERT large (24 layers) by probing the models to quantify where specific types of linguistic information are encoded. They discovered that the region's responsibility for each traditional NLP step appears in the expected sequence: POS tagging, parsing, NER, semantic roles, and finally, coreference. The study by Jawahar et al. (2019) also explores BERT’s encoding of linguistic information using probing as well. They discovered that the initial layers of BERT encode surface features, middle layers encode syntactic features, and final layers encode semantic features. Though our paper also performs an evaluation on mBERT’s layers, it specifically focuses on each layer’s ability to encode semantic roles, which can provide results that support previous discoveries.    
Our study first parses a UCCA annotated corpus to extract the word to role mappings before using a specific transformer layer of mBERT to generate embeddings. A logistic regression model is then trained on the embeddings with the UCCA roles as labels first in monolingual then in cross-lingual settings to evaluate accuracy. We hypothesize the mBERT’s embeddings will most effectively encode semantic roles in monolingual settings within deeper layers as the embeddings may encode language-specific information. For cross-lingual settings, we predict that the deeper layers will also be most effective, but the overall classifier accuracy will be lower compared to the monolingual experiments. 

**Method**  
**Dataset:**

The dataset used for this study is a parallel French-English corpus consisting of passages from the novel “20,000 Leagues Under the Sea”. The corpus contains a total of 308 passages, 154 passages in English, and 154 parallel passages in French, with each passage consisting of multiple sentences. This dataset is taken from the study by Sulem et al. (2015), where they explored the UCCA framework in a case study focusing on ensuring semantic stability between translations. The original dataset is provided in XML format, with the UCCA annotations represented as hierarchical nodes connected by edges. The terminal nodes within the UCCA annotations represent words and punctuations, and the edges connecting the nodes define their semantic role within the sentence. This representation allows for a detailed and comprehensive understanding of the sentence semantics of the corpus.   
We chose this dataset because the original study focused on demonstrating how UCCA semantic annotations remain highly consistent between English and French translations, and this consistency allows isolated analysis of whether mBERT embeddings can capture and align semantic roles across parallel texts in the two languages.

**Data Preprocessing:**

	Our goal is to extract each word of each of the passages as well as its corresponding UCCA annotated role within the sentence. To extract these roles, we used the lxml library to parse the XML files. The parsing process involved traversing the XML structure to retrieve the semantic role from the edge connecting each terminal node (containing the word or punctuation) to its immediate parent in the UCCA hierarchical graph.  
	To preserve the integrity of the dataset, no further preprocessing (other than extraction of the words and UCCA roles) was performed. Since the focus of this paper is semantic roles, which are context-dependent, it was important not to disrupt the alignment between each word and its semantic roles. If excessive preprocessing, including removing or modifying words were performed, this could potentially impact the ability to compare and evaluate role alignment across the parallel English-French corpus.  
	After preprocessing the data, all sentences are split into their individual components (words/punctuations) and are stored with their position within the sentence, and their semantic role(s) within the sentence ({(word, position): \[role(s)\]}). The example below shows the preprocessed data for one sentence within one passage in both English and French:

ENGLISH: \[{('The', '1'): \['E'\], ('year', '2'): \['C'\], ('1866', '3'): \['C'\], ('was', '4'): \['F'\], ('marked', '5'): \['C'\], ('by', '6'): \['R'\], ('a', '7'): \['E'\], ('bizarre', '8'): \['E'\], ('development', '9'): \['C'\], (',', '10'): \['U'\], ('an', '11'): \['E'\], ('unexplained', '12'): \['C'\], ('and', '13'): \['N'\], ('downright', '14'): \['E'\], ('inexplicable', '15'): \['C'\], ('phenomenon', '16'): \['A', 'C'\], ('that', '17'): \['F'\], ('surely', '18'): \['G'\], ('no', '19'): \['E'\], ('one', '20'): \['C'\], ('has', '21'): \['F'\], ('forgotten', '22'): \['C'\], ('.', '23'): \['U'\]....}, …\]

FRENCH: \[{('L’année', '1'): \[\], ('1866', '2'): \['C'\], ('fut', '3'): \['F'\], ('marquée', '4'): \['C'\], ('par', '5'): \['R'\], ('un', '6'): \['E'\], ('événement', '7'): \['C'\], ('bizarre', '8'): \['E'\], (',', '9'): \['U'\], ('un', '10'): \['E'\], ('phénomène', '11'): \['A', 'C'\], ('inexpliqué', '12'): \['C'\], ('et', '13'): \['N'\], ('inexplicable', '14'): \['C'\], ('que', '15'): \['R'\], ('personne', '16'): \['A'\], ('n’', '17'): \[\], ('a', '18'): \['F'\], ('sans', '19'): \['E'\], ('doute', '20'): \['C'\], ('oublié', '21'): \['C'\], ('.', '22'): \['U'\]...}, …\]

**Generating Probe Input:**

A linear classifier is used in the form of a logistic regression model to probe the semantic encoding of mBERT. The input to the classifier is the word-level embeddings for each word and their corresponding UCCA-annotated roles as labels.  
To generate the word-level embeddings used for the linear probe, representations were extracted from a specific transformer layer of mBERT. The single-layer specification allows analysis of how semantic information is captured across different stages of the model’s processing. To generate the embeddings, the sentences were first tokenized, with words split into subwords, and then mapped back to the original words after. The embeddings are created for each subword and then aggregated together to create the word-level representation using mean pooling, ensuring that the final word-level embeddings align with the original sentence structure. The hidden states from the specified mBERT layer are used to get the contextualized word embeddings. For words with multiple UCCA roles, it is split into multiple separate inputs of the same embedding but different role labels. 

**Probe Training:**  
A linear probe was used to evaluate mBERT’s ability to encode UCCA semantic roles because of its simplicity, and role as a baseline method. By fixing the model complexity, the difference in performance in the model can be more directly related to the quality of semantic encoding within the embeddings at each transformer layer, rather than a result of variations in the complexity or capacity of the classifier. This was primarily chosen for the layer-wise analysis, where the goal is to identify which layer(s) best captures semantic role information. The linear probe does not modify the embeddings from mBERT, which means that it can perform its analysis on the raw mBERT outputs. This isolation ensures that the results of the model reflect mBERT’s ability to encode semantic roles.   
	The next step is to train the linear probe to evaluate the extent to which semantic roles are encoded within the embeddings generated by the mBERT model. For this task, a logistic regression model from the scikit-learn library was used and 2000 max iterations were specified. The data was split into training (80%) and test (20%) test sets, with both English and French corpora split independently to evaluate the model’s performance in each individual language. The balanced\_weight parameter in LogisticRegression is also set to True to account for the imbalance of role frequencies within the corpus and improve the model’s sensitivity to less frequently represented elements. 

**Testing and Evaluation:**

The trained linear classifier is evaluated on the test set using accuracy. Four experiments were conducted:   
1\. The linear probe was trained on English data, then evaluated on the English test set  
2\. Trained on French data, and then evaluated on the French test set  
3\. Trained on English data, and evaluated on the French test set  
4\. Trained on French data, and evaluated on the English test set.   
Since this paper also aims to explore the layers of mBERT which best encode semantics, for each of mBERT’s 12 transformer layers, the 4 experiments above were repeated, allowing for evaluation of each layer’s embedding ability in monolingual and cross-lingual settings.

**Results:**

	The accuracies for each experiment conducted using each layer of mBERT to generate embeddings are formatted in tables shown in Figure 1 as well as plots shown in Figure 2\.

**Figure 1: Accuracy Table With Balancing**

| Layer | Train: En, Test: En | Train: Fr, Test: Fr | Train: En, Test: Fr | Train: Fr, Test: En |
| :---- | :---- | :---- | :---- | :---- |
| 1 | 0.674345958609918 | 0.7219679633867276 | 0.4906164174549893 | 0.4149160484185865 |
| 2 | 0.7094884810620852 | 0.7341723874904653 | 0.5511901129081477 | 0.474345958609918 |
| 3 | 0.7278406872315502 | 0.7498093058733791 | 0.5678974671956057 | 0.5756345177664974 |
| 4 | 0.737212026552128 | 0.7555301296720061 | 0.6091699725358559 | 0.6228816868410777 |
| 5 | 0.7575165950800469 | 0.7601067887109078 | 0.654485810192249 | 0.648184303006638 |
| 6 | 0.7567356501366653 | 0.7551487414187643 | 0.6827128471162649 | 0.690589613432253 |
| 7 | 0.7602499023818821 | 0.7593440122044242 | 0.6832468721391517 | 0.7070675517376025 |
| 8 | 0.7493166731745412 | 0.7578184591914569 | 0.7108635947512969 | 0.6992581023037876 |
| 9 | 0.7489262007028504 | 0.7559115179252479 | 0.7181110772047604 | 0.7090199140960562 |
| 10 | 0.7469738383443967 | 0.7559115179252479 | 0.7030057979859627 | 0.692151503319016 |
| 11 | 0.7376024990238188 | 0.7540045766590389 | 0.6692859322551113 | 0.646622413119875 |
| 12 | 0.7473643108160875 | 0.7620137299771167 | 0.5988709185230394 | 0.6073408824677861 |

**Figure 2: Accuracy Plots**

- **Linked in the github under plots**

	In terms of monolingual performance, training and testing within the same language consistently achieved the highest accuracy across all layers. For English (Train: En, Test: En), the peak accuracy of \~0.760 occurs with embeddings from layer 7, followed closely by layer 7 with an accuracy of \~0.757. For French (Train: Fr, Test: Fr), the embeddings from layer 12 have the highest accuracy of \~0.762, followed closely by layer 5 with an accuracy of \~0.760. These results suggest that mBERT captures information regarding semantic roles most effectively around the middle layers as well as the final layer. This result matches that of Tenney et al. (2019) which state that the middle to deeper layers of mBERT can best encode semantic information.   
	For cross-lingual performance, there is an expected drop in accuracy compared to the monolingual experiments, which reinforces the challenges of aligning semantic roles across different languages. For the experiment that is trained on English and tested on French, the maximum accuracy of \~0.718 is achieved with the embeddings from layer 9, followed by an accuracy of \~0.712 from layer 8\. For the experiment that is trained on French and tested on English, the maximum accuracy of \~0.709 is also achieved with layer 9, followed by layer 7 with an accuracy of \~0.707. The cross-lingual results show that semantic information can be cross-lingual applicable within the deeper layers of mBERT. This corresponds with the results by Jawahar et al. (2019) who states that the final layers of BERT models best encode semantics. This gap between the monolingual and cross-lingual performance shows that while mBERT embeddings are able to capture semantic information well, the embeddings still contain language-specific characteristics.  
	The lowest accuracies for both the monolingual experiments and the cross-lingual experiments were achieved in the most shallow layers of mBERT, which is consistent with previous papers which state that the earlier layers of the model encode surface-level linguistic features.   
	Figure 2 shows the plots for each experimentation for each layer of mBERT and it is clear that for most cases, the accuracy reaches a peak around the middle to deeper layers of mBERT, before plateauing and decreasing near the deepest layer(s) of the model. This result suggests that the deepest layers of mBERT are more tuned towards encoding other information other than general semantic roles. This could be because later layers of the model tend to contain language-specific features and become increasingly specialized in representing these features. 

**Limitations**  
While this study provides insight into mBERT’s ability to encode UCCA semantic roles, one limitation of the experimentation is the dataset used in this study. The French-English parallel corpora used in this study to train and test the models are relatively small, as well as limited to only 2 languages. The small dataset restricts the generalization of this paper’s findings to other languages, especially those with large typological differences from English or French. Expanding the dataset to include other languages within the parallel corpus (especially languages that are widely different from French or English) would allow for a more comprehensive analysis of mBERT’s cross-lingual capabilities.  
Additionally, because the corpus used in the paper is from a real novel, the distribution of UCCA roles across the dataset is uneven. Although this is a reflection of the inevitable variability of role occurrence in real-world language use, this imbalance may bias the classifier’s performance towards UCCA roles that appear more often, leaving less common roles insufficiently modelled. To address this, we attempted to balance the LogisticRegression model, which however, is an artificial solution that has its downsides, as it may lead to overfitting on less frequent roles or misrepresenting the true distribution of roles in the data.

**Conclusion**  
This paper evaluated the ability of mBERT to encode semantic roles defined by the UCCA framework in both monolingual and cross-lingual settings using layer-specific analysis. Results of the experimentation conducted show that mBERT effectively captures semantic role information, with the middle layers performing the best for both English and French in monolingual experiments. In terms of cross-lingual experiments, it is shown that semantic role assignment between languages is more difficult. Although for cross-lingual studies, deeper mBERT layers showed improved performance, it still performs worse than monolingual accuracy.  
The results obtained from this study show the ability of mBERT to encode rich semantic information while also highlighting the presence of features specific to certain languages which is the limitation behind cross-lingual alignment. By using a parallel corpus annotated with the UCCA framework, insights were provided on how semantic roles are represented in multilingual word embeddings, as well as the importance of certain mBERT layers in capturing general semantics.  
Future work could expand this analysis to more languages, especially performing cross-lingual analysis on languages that are semantically and even syntactically more different than English and French. Additional probing techniques could also be implemented, including non-linear probes to further understand the cross-lingual potential of semantic annotations in multilingual models, and be able to capture the non-linear relationships between embeddings and semantic roles.  

**Statement of Contribution**  
The entire group contributed to researching topics for the paper. Initially, we worked individually to test various methods and experiments before agreeing on a valid approach that aligned with our interests. We collaborated on all aspects of the project, in the code, specifically, XML parsing, embedding creation, linear classifier training, analysis, as well as the final write-up.
