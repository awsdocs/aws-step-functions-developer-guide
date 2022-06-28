# Machine learning<a name="use-cases-machine-learning"></a>

 Machine learning enables organizations to quickly analyze collected data to identify patterns, then make decisions with minimal human intervention\. Machine learning starts with an initial set of data, known as training data\. This training data helps to increase a machine learning model’s prediction accuracy, and serves as the foundation through which this model learns\. Once the model is considered accurate enough to meet business needs, it’s deployed to production\. The [AWS Step Functions Data Science Software Development Kit \(SDK\)](concepts-python-sdk.md) is an open\-source library that allows you to easily create workflows that preprocess data, train and then publish your models using Amazon SageMaker and Step Functions\.

Preprocessing existing data sets is how an organization often creates training data\. This method adds information, such as by labeling objects in an image, annotating text or processing audio\. To preprocess data you can use AWS Glue, or you can create an SageMaker notebook instance that runs the Jupyter Notebook app\. Once your data is ready, it can be uploaded to Amazon S3 for easy access\. As machine learning models are trained, you can make adjustments to each model’s parameters to improve accuracy until it’s ready for deployment\. 

Step Functions lets you to orchestrate end\-to\-end machine learning workflows on SageMaker\. These workflows can include data preprocessing, post\-processing, feature engineering, data validation, and model evaluation\. Once the model has been deployed to production, you can refine and test new approaches to continually improve business outcomes\. You can create production\-ready workflows directly in Python, or you can use the Step Functions Data Science SDK to copy that workflow, experiment with new options, and place the refined workflow in production\. 

Some types of machine learning workflows that customers use Step Functions for include:

**Fraud Detection**
+ Identify and prevent fraudulent transactions, such as credit fraud, from occurring\. 
+ Detect and prevent account takeovers using trained machine learning models\.
+ Identify promotional abuse, including the creation of fake accounts, so you can quickly take action\. 

**Personalization and Recommendations**
+ Recommend products to targeted customers based upon what is predicted to attract their interest\.
+ Predict whether a customer will upgrade their account from a free tier to a paid subscription\.

**Data Enrichment**
+ Use data enrichment as part of preprocessing to provide better training data for more accurate machine learning models\.
+ Annotate text and audio excerpts to add syntactical information, such as sarcasm and slang\.
+ Label additional objects in images to provide critical information for the model to learn from, such as whether an object is an apple, a basketball, a rock, or an animal\. 