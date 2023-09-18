# LeadSquared Project - nlp-1.2-context-lebeling

## Extracting context from customer reviews
15th September 2023
### SUMMARY
The purpose of the project is to extract meaningful labels from customer reviews, which shall help in gathering insights from them accurately and efficiently from a large scale of data. I shall be taking it forward as a topic-labeling problem.  

### ASSUMPTIONS
1. **Adjective - head based tag**: Based on the idea that our context should be a feature of the clothing product and describe the clothing product. 
An Important assumption taken is our meaningful context will be 2 worded with the first word, being an adjective and the second being a noun (feature of cloth), where our adjective shall be describing the noun. (eg. Perfect dress, Tight fit, etc). They will be called label adjective and label topic in the procedure.
1. **Review Independence** : A major assumption taken here is the review text field (‘’ and ‘’) , should be enough to accurately find context in the review, and other columns. 
1. **Semantically similar** :All the tags relevant to dress feedback should be semantically similar to our target words :  ‘dress’ ,‘product’ or ‘e-commerce’. And this will be judged cosine similarity index with suitable threshold.(Mentioned in Assumption #4) 
1. **Threshold confidence** : 
 - cosine similarity > 0.35 for the label to be semantically similar to any of our target words in order to be relevant to  customer review on the item.
 - cosine similarity > 0.4 between label and feedback (review title + review sentence) to map context with the feedback.


Other important standard assumptions taken, which are usually taken in NLP modeling - 
1. **Data completeness** : Here the assumptions are that the data provided to us covers all significant labels. Any new dataset on this shall give no effect.
1. **Language independence**: Since the entire training data set is in English language, we assume that all other and coming reviews will be in English as well. 



### PROCEDURE
1. Data exploration: learn about dataset. It’s fields, presence of null values, etc.
1. Extract labels using dependency parsing. The labels will be of format -> “{adjective} {adj-head}”. For example - “pretty dress”,”strange fit”.
1. These labels will now be filtered out -> 
  - First, we will filter labels by count. Data with very low count (<10 values and present in <2 ids).
  - Filter label by semantics:
     - Should be semantically similar to chosen target words (dress, product, ecommerce) - _words decided via hit and trial._
      - Eliminate synonymous pairs (when 2 labels mean synonymous things).
1. Manual elimination of irrelevant pairs.
1. Finding pairs by searching for synonym of topics (noun in the label) in sentence, in that case, we will run all the labes with noun in it and find best match to review by cosine similarity.  - _Using sentence_transformers for encoding._
1. Using cosine similarity and threshold confidence between encoded labels and feedbacks to map labels to feedbacks.
1. Handle trivial cases where no text feedback is there or none of the labels passed the threshold.
