---
layout: post
author: Jieying Lan
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background

**Introduction:**
Founded in 1969 in Paris, Sephora is the global leader in beauty retail industry, offering a wide range of luxury skincare, makeup, hair care, and beauty tools. Sephora's success is driven by its commitment to personalized service and expert advice. In a competitive and saturated market, Sephora continues to differentiate through its unique brand identity and innovations in digital tools and personalized recommendations.

**Group effort:**
Our group project aims to derive insights from customer satisfaction and sentiment by analysing reviews and features. Key focus includes identifying best-selling products, evaluating product effectiveness through reviews and ratings, and developing a recommendation system to enhance cross-selling, thereby maximising sales potential and improvng customer satisfaction.

**Individual effort:**
To identify best-selling sunscreen products based on customer reviews and sentiment analysis


## Work Accomplished
Our group start off the project by researching suitable datasets from Kaggle and agreed to adopt Sephora Products and Skincare Reviews (https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews/data).

As part of our group effort, we undertake a general approach for Data understanding and Data Cleaning. We performed EDA (Exploratory Data Analysis) such as data structure, use Word Cloud/ Correlation Matrix to further understand common domain words from customer reviews, Data Cleaning by removing duplicates and special characters, handling missing values, and manage outliers. This is to ensure accuracy for subsequent analysis. Feature engineering was conducted by excluding columns such as 'Unnamed: 0', 'total_feedback_count', 'child_max_price', 'child_min_price', 'child_count', 'value_price_usd' and 'sale_price_usd' to eliminate noise and improve model performance.

In this group project, my focus is  on the Sunscreen category. By recommending both Random Forest and Decision Tree models, we ensure flexibility to address different business needs. With better prediction, the models will aid Sephora in achieving goals such as increasing sales and improving customer satisfaction.


### Data Preparation

**Data Cleaning:**
- Calculate the percentage of missing values for each column in the Sunscreen category.
- Removed columns with more than 50% missing data to.
- Imputed missing categorical values using the mode for columns with 7% to 12% of missing data (occurs most frequent).
- Dropped rows with missing values in columns with less than 5% missing data.

**Feature Engineering:**
- Both `sentiment_score` and `loves_count` were originally considered as key features for predicting best-seller status. During model testing, it was found that the inclusion of loves_count resulted in an accuracy of 1.0, indicating a perfect model which is not realistic. This raised concerns about potential overfitting. After careful consideration, I decided to proceed with only `sentiment_score` as the primary feature for the final model, as it provided more realistic and reliable predictions.

- Convert 'submission_time' column into Date & Time format and extract by year and month.
- Convert text data into TF-IDF vectors to identify importance of a term from customer reviews.
- Apply Singular Value Decomposition (SVD) to reduce dimensionality.
- Train Word2Vec model on text data.
- Sentiment Analysis using VADER to perform sentiment analysis and obtain a sentiment score for each review.
- Created a new binary classification column ‘is_best_seller’ (Target Variable) where Yes = 1 and No = 0.

**Handling Class Imbalance:**
-  Apply SMOTE to balance the training data, prevent overfitting. 

**Generated Target Variable List:**
- Generated the list of ‘is_best_seller’ for Sunscreen products to be used in the modeling phase.


### Modelling

**Model Selection:**
Explored three different modeling techniques: Random Forest, Decision Tree, and Logistic Regression.

**- Random Forest**
Robustness & ability to handle complex interactions i.e. non-linear relationships.
Complex, hard to explain.

**- Decision Tree**
More explainable | Prone to overfitting.

**- Logistics Regression**
Simple and straight-forward, easy to explain. 
Unable to handle non-linear dataset due to linearity of input & output variables.

**Feature Engineering**
I have experimented with both `sentiment_score` and `loves_count` as variable input. The inclusion of `loves_count` led to a perfect accuracy score (AUC = 1.0), which suggested that the model might be overfitting. To avoid this issue and to ensure that the model could generalize better to unseen data, I have decided to exclude 'loves_count' and focus solely on 'sentiment_score' to provide a more balanced and realistic model performance.

**Training and Testing Data**
- Split the data into 80/20 for training and testing for learning effectiveness.
- Class imbalance distribution of training set on best-seller status (Class 1) and non best-seller status (Class 0). To resolve, apply SMOTE (Synthetic Minority Oversampling Technique) to training set for a balanced distribution.
Before: 78 samples (imbalanced)
After: 144 samples (balanced)

![image](https://github.com/user-attachments/assets/a96588cc-512b-44ff-95b3-ce53609567da)

**Model Training**
- All three models were trained on the resampled dataset using key features like 'product ID' and 'sentiment score' as the goals is to predict the binary outcome whether a Sunscreen product is a best-seller or not.

*- Final output ‘is_best_seller’ column:*

![image](https://github.com/user-attachments/assets/e87ced52-7e1a-45e1-9d78-79ba39f39798)

- Monitor the relationship between sentiment_score and the best-seller status to ensure that the model captured the relevant patterns.
![image](https://github.com/user-attachments/assets/e5d2e06e-fecd-42e6-9acb-228e9b26212b)


### Evaluation

**1. Model Performance**
**Random Forest and Decision Tree:**
- Both models demonstrated strong performance with an accuracy of 70%.
- Both struggled with predicting best-seller status due to class imbalance and overfitting issues.
![image](https://github.com/user-attachments/assets/5531eb54-14e6-407e-b895-4db6ac893396)

![image](https://github.com/user-attachments/assets/5b9f4065-c456-4dc7-b4fd-56a8b7ecbd47)

**Logistics Regression:**
- Simple and more explainable, while it has low accuracy of 60%.
- Model struggled to capture non-linear relationships which affects model performance on best-seller prediction.
![image](https://github.com/user-attachments/assets/cd8dfaf7-cca7-4755-af34-4855bf570aeb)

**Model’s performance using key metrics:**
The final model which utilized only `sentiment_score`, achieved an accuracy of 70%. This is in contrast to the initial model that included `loves_count`, which had an AUC of 1.0. The decision to exclude `loves_count` was made to avoid overfitting and ensure the model's predictions were reliable and generalizable.

![image](https://github.com/user-attachments/assets/35266775-5ccb-489f-b226-9b6c075fadc1)

**2. Challenges**
- Overfitting in Random Forest and Decision Tree models due to small dataset.
  
![image](https://github.com/user-attachments/assets/a66d5719-1dfe-46ee-aa55-2c8d76e9a932)

![image](https://github.com/user-attachments/assets/47883b64-68a8-4688-a457-5d9477456575)

- Capture noise instead of meaningful patterns.
- Balancing dataset with SMOTE to help with the imbalance of class issue.

**3. Results**
- Model accuracy: The low precision score from the model performance metric mirrored the realistic imbalance of the best-seller status (Class 1).
- Logical result: Number of best-sellers is naturally fewer as compared to non-best-sellers is logical (based on domain knowledge).
  
![image](https://github.com/user-attachments/assets/b6b6d11b-4210-4b39-9918-f18b7ba8e376)

- Customer Engagement: The model interprets that customer engagement, as reflected in features like 'is_best_seller' is an important driver of product success.
  
![image](https://github.com/user-attachments/assets/175768af-b1bc-4076-b391-077579c9154e)

  
## Recommendation and Analysis

**1. Recommendations for Model Improvement**
- Model Tuning and Advanced Techniques: To better predict best-sellers, fine-tuning the model parameters and exploring advanced techniques like Gradient Boosting or ensemble methods are recommended.

- Handling Class Imbalance: To address class imbalance more effectively, explore different resampling techniques.

- The initial plan included both sentiment_score and loves_count as features. However, the inclusion of 'loves_count' resulted in an AUC of 1.0, indicating potential overfitting. To enhance the model's generalizability, it is recommended to focus solely on optimizing the 'sentiment_score' feature.

**2. Analysis of Business Impact**
- Accurate prediction: The emphasis on importance of predicting best-sellers status for demand forecasting, optimizing inventory and to avoid missed sales opportunities.

- The analysis highlights the importance of customer engagement metrics like 'merged_reviews' in predicting product success.

- The focus on 'sentiment_score' as the primary feature in the final model was a strategic decision aimed at ensuring the reliability and applicability of the predictions.

**3. Final Recommendations**
- Random Forest is recommended as the primary model due to its overall robustness and high accuracy in handling complex data patterns.

- Decision Tree to be considered as an alternative when explainability and transparency are prioritized.

- As part of strategic planning, Sephora can leverage these models to meet objective/KPI set. Thus, improving overall business outcomes.

In conclusion, while `loves_count` was initially considered, the exclusion was necessary to prevent overfitting and to maintain the integrity of the model. For the current analysis, focusing on `sentiment_score` alone has provided a realistic and reliable model that aligns well with our business objectives.


## AI Ethics

**1. Privacy**
The PDPA in Singapore recognizes both the need to protect individuals’ personal data and the need of organisations to collect, use or disclose personal data for legitimate and reasonable purposes.

While working on this project, we must ensure there is no exposure of personal or sensitive data information such as Customer Name or Credit Card Number. Perform data anonymization to erase or encrypt the identifiers if require. 

Our dataset does not contain of personal or sensitive data information. It is represented by the features ‘author_id’ which is the unique identifier for the author of the review on the website.

**2. Fairness**
Fairness is an important consideration when working on predicting the best-seller status. A biased prediction could lead to unfair competition, thus affecting certain products, brands and categories.

As highlighted earlier that one of the challenges on handling imbalance classes, we had applied SMOTE techniques to balance out the classes. The selected models are trained on resampled dataset, ensuring the model did not favour non best-sellers over best-sellers.

**3. Accuracy**
Ensuring the accuracy of predictions is crucial as inaccurate predictions can lead to implication such as forecasting of incorrect best-seller products, inventory overstock, lost sales, and poor customer satisfaction.

Beside the diligent steps on data cleaning process to improve models training accuracy, we used several evaluation metrics i.e. accuracy, precision, recall and Fi-score to assess the models performance thoroughly.

The comprehensive evaluation will help to ensure on model’s prediction to be as accurate as possible, therefore, a low-risk decision making. 

**4. Accountability**
In view of ethical consideration, models need to establish accountability and transparency in algorithms decision-making process.

For this project, we have document clearly on the processes which includes data preparation, model selection and model evaluation. 

In the event when deployment of the model has been buy-in, standard protocols on monitoring model performance and address on issues would need to establish, thus, ensuring on accountability after rollout.

**5. Transparency**
From AI Singapore Community Code of Conduct, transparency is one of the six AI principles. To be transparent about AI processes, we first to understand them and be able to explain the reasoning behind the predictions and decisions.

In this project, Decision Tree has been selected as the alternative model under the overall group conclusion. Reason being of the model interpretability where it allows us to explain how different features i.e. ‘loves_count’ and ‘sentiment_score’ has contribute to the prediction of a best-seller, making the decision-making process more transparent.

**6. Overall Ethical Considerations**
While in this project did not have direct involvement in handling of sensitive personal data, we need to consider AI ethics at every step, from data preparation to modeling / evaluation. The aim is to create a model that not only performs well whilst adhering to ethical standards at a global level.


## Source Codes and Datasets
Source code uploaded: https://github.com/jieyinglan/ITD214_Project.git

Dataset to download: https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews/data
