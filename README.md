# Similar but Unique: An NLP Model to Predict a Subreddit Post as Nike or Adidas

This repository is for a project I worked on as a Data Science Immersive student at General Assembly. 

## Executive Summary

- [Problem Statement](#Problem-Statement)
- [Description of Data](#Description-of-Data)
- [Methodology](#Methodology)
- [Conclusions & Recommendations](#Conclusions-&-Recommendations)
- [Sources](#Sources)


---

## Problem Statement

If you remove the brand name and product names from the text, can we predict the brand that an online post is discussing using a Natural Language Processing (NLP) model? 

Does marketing language influence online conversations about products? I was curious about two things:
* If you remove the brand name and product names from the text, can we predict the brand that an online post is discussing using an NLP model?
* How does the language that is used in a marketing campaign influence the way a brand’s critics and supporters choose to talk about that brand in an online setting?

---

## Description of Data 

The data included in this analysis was pulled from https://www.reddit.com and includes posts from two subreddit pages:

/r/Nike
/r/adidas
The data collection process was completed in another Jupyter Notebook.

There were 4120 total posts pulled from the two subreddit pages, including 1992 from the Nike subreddit and 2128 from the Adidas subreddit. The posts are from December 20, 2018 to October 15, 2019.

---

## Methodology

1. Collect data
2. Import & clean data
3. Exploratory data analysis
4. Modeling
5. Conclusions & Recommendations

### Collection & Cleaning of Data

### Modeling


---

## Conclusions & Recommendations

I applied different transformers (Count Vectorizer and TFIDF Vectorizer) to different models (Logistic Regression, Support Vector Classifier, and Naive Bayes Multinomial), and incorporated different stop word exclusions (none, English, brand names, product names). Overall, I found the highest test accuracy scores and least amount of overfitting in the Naive Bayes model using either transformer and excluding either English or product names as stopwords.

Removing brand names was not enough to prevent overfitting, however removing product names saw better improvement on the overfitting because they occurred more often. When people are talking about either brand online, they are often referring to a specific product from that brand. When using a popular product name, the brand is often left out because it is intuitively implied by using the product name. This is particularly the case in this instance of Nike and Adidas, which both have excellent brand recognition.

As next steps and further iterations on this project and model, I would get more historical data. The data I included ranged from December 2018 to October 2019. I think it would add more value to include more historical data, which would ultimately make my model stronger and potentially better at predicting new posts.

Furthermore, the topics of conversation around these products on reddit were largely centered around selling/buying shoes, customer service related questions, searching for promo codes, etc. While my problem statement and initial questions about the data centered around relating the conversations taking place online to the marketing campaigns of each respective company, I learned throughout my EDA process that these perhaps were not the right questions for this particular set of data. The reddit posts are more "business as usual" than "on brand" conversations. In future iterations or deveopment of my model, I would also compare these posts to each respective brand's marketing campaigns. I think this could introduce more of a real-world business application for this project. If a marketing department can understand if the online reddit community is talking about their products in the way they intended (i.e. using the messaging they have used in their campaigns), they can use that information to fuel future campaigns.

Additionally, with more historic data, I would conduct sentiment analysis on each brand and compare this against any PR 'wins or losses' for that brand. For example, Nike's ad featuring Colin Kaepernick had very mixed feedback from the public depeding on where a particular person stands on some political issues. Likewise, Adidas released a sneaker for Black History Month that was entirely white in color, which also had varying feedback. Comparing the sentiment of posts with timestamps that coincide with these events could help the company analyze the full impact that each event had on their brand. I also would look at each brand compared to Under Armour, which has much less international recognition and less history.

---

## Sources

- www.reddit.com
- Josh Robin - shared the function that was used to pull the data from Reddit
- https://blog.insightdatascience.com/how-to-solve-90-of-nlp-problems-a-step-by-step-guide-fda605278e4e

---