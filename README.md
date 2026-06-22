\# Responsible AI for Personal Loan Acceptance Prediction



A university artificial intelligence project exploring how machine learning can support a bank’s personal-loan marketing strategy.



The system predicts whether a customer is likely to \*\*accept a personal loan offer\*\* using demographic, financial, and banking-behaviour data. The project combines exploratory customer segmentation, baseline classification models, Azure AutoML enhancement, performance benchmarking, and responsible AI analysis.



> \*\*Important:\*\* This project predicts customer interest in a loan offer. It does not determine creditworthiness, loan eligibility, interest rates, or whether a loan application should be approved.



\## Project Objectives



The main objectives of this project were to:



\* Explore customer subgroups using K-Means clustering.

\* Develop K-Nearest Neighbours and Artificial Neural Network baseline classifiers.

\* Use Azure Automated Machine Learning to explore an enhanced model.

\* Compare models using multiple classification metrics.

\* Analyse explainability, fairness, privacy, security, monitoring, and legal accountability.

\* Evaluate whether the system is suitable for responsible deployment in a real banking environment.



\## Dataset



The project uses an anonymized personal-loan customer dataset containing \*\*5,000 customer records\*\*.



The dataset includes demographic, financial, and banking-related attributes such as:



\* Age

\* Annual income

\* Family size

\* Average credit-card spending

\* Education level

\* Mortgage value

\* Securities-account ownership

\* Certificate-of-deposit account ownership

\* Online-banking usage

\* Bank credit-card ownership



\### Target Variable



The target variable is `Personal Loan`.



| Value | Meaning                                             |

| ----: | --------------------------------------------------- |

|   `0` | The customer did not accept the personal loan offer |

|   `1` | The customer accepted the personal loan offer       |



The dataset is imbalanced, with substantially more customers in class `0` than in class `1`. Therefore, accuracy alone was not considered sufficient for evaluating the models.



\## Project Workflow



The main implementation stages were:



1\. Load and inspect the dataset.

2\. Check for missing values, duplicate rows, invalid values, and feature relationships.

3\. Remove identifiers and unsuitable features.

4\. Separate the input features from the target variable.

5\. Split the data into training and testing sets.

6\. Scale the input features using `StandardScaler`.

7\. Explore customer groups using K-Means clustering.

8\. Train KNN and ANN baseline classifiers.

9\. Run an Azure AutoML classification experiment.

10\. Compare model performance.

11\. Evaluate technical, ethical, legal, and security considerations.



\## Data Preparation



The following columns were excluded from the classification inputs:



\* `ID`, because it is only a customer identifier.

\* `ZIP Code`, because it is a geographic code and should not be treated as a continuous numerical value.

\* `Experience`, because it contained invalid negative values and was highly correlated with `Age`.



Feature scaling was applied because KNN and K-Means depend directly on distance calculations. Scaling also supports more stable ANN training by placing features on comparable numerical ranges.



\## Customer Segmentation



K-Means clustering was used as an exploratory unsupervised-learning technique rather than as the final prediction model.



The number of clusters was assessed using:



\* The elbow method

\* Silhouette scores

\* PCA visualization

\* Cluster interpretability



The elbow method suggested six clusters, while the silhouette analysis supported two clusters. A final value of \*\*K = 2\*\* was selected because it produced more distinct and interpretable customer groups.



The resulting clusters showed different financial profiles and different personal-loan acceptance patterns.



\## Classification Models



\### K-Nearest Neighbours



KNN predicts a customer’s class by examining similar customers in the training dataset.



The model was trained using scaled data, and different values of `K` were evaluated using cross-validation and F1 score.



\### Artificial Neural Network



The ANN was implemented using TensorFlow and Keras.



Its architecture included:



\* Dense hidden layers

\* ReLU activation functions

\* A sigmoid output layer for binary classification

\* Binary cross-entropy loss

\* The Adam optimizer



\### Azure AutoML VotingEnsemble



Azure Automated Machine Learning was used as the model-enhancement stage.



The Azure experiment tested multiple classification approaches and selected a `VotingEnsemble` as its best-performing model. The experiment used two-fold cross-validation and a limited maximum of five trials.



Azure workspace files, cloud resources, and screenshots are not included in this repository. Their implementation and results are documented here as part of the overall project evaluation.



\## Model Results



| Model                | Accuracy | Precision | Recall | F1 Score |

| -------------------- | -------: | --------: | -----: | -------: |

| KNN                  |   96.60% |    93.06% | 69.79% |   79.76% |

| ANN                  |   98.40% |    91.67% | 91.67% |   91.67% |

| Azure VotingEnsemble |   98.26% |    95.38% | 86.04% |   90.46% |



\## Results Interpretation



The ANN produced the highest recall and F1 score. It identified a larger proportion of customers who actually accepted the personal loan offer.



The Azure VotingEnsemble produced the highest precision. When it predicted that a customer would accept the offer, that prediction was more likely to be correct.



The KNN model achieved strong accuracy and precision but had noticeably lower recall. This means it missed more actual loan acceptors than the other models.



The final model choice therefore depends on the bank’s objective:



\* The \*\*ANN\*\* is more suitable when identifying as many potential acceptors as possible is the priority.

\* The \*\*Azure VotingEnsemble\*\* is more suitable when reducing false-positive marketing targets and wasted marketing resources is the priority.



\### Evaluation Limitation



The model comparison is informative but not perfectly controlled.



The KNN and ANN models were evaluated using an 80/20 train-test split in the notebook, while Azure AutoML used cross-validation. The Azure model also received a slightly different preprocessing pipeline.



A stronger future comparison would evaluate all models using the same dataset version, preprocessing pipeline, cross-validation folds, and evaluation metrics.



\## Responsible AI Considerations



\### Fairness and Bias



Historical customer behaviour may contain biased patterns. The model could reproduce these patterns and disproportionately exclude certain customer groups from marketing opportunities.



Before deployment, the bank should compare error rates across relevant customer groups and investigate whether false-negative rates are uneven.



\### Explainability



The ANN and VotingEnsemble models are more difficult to interpret than simpler classification models.



Explainability techniques should be used to determine:



\* Which features influence predictions most strongly.

\* Whether the model relies excessively on individual variables.

\* Why a particular customer received a positive or negative prediction.



\### Privacy and Data Protection



Although the dataset is anonymized, it still contains financial and behavioural information.



A real implementation should apply:



\* Access controls

\* Encryption

\* Data minimization

\* Secure storage

\* Audit logging

\* Clear data-retention rules



\### Security



The system may be exposed to threats such as:



\* Unauthorized access to customer data

\* Training-data poisoning

\* Manipulated retraining records

\* Model theft

\* Insecure cloud configuration



The bank should validate the origin and integrity of new training data before retraining the model.



\### Monitoring and Model Drift



Customer behaviour can change because of economic conditions, interest rates, changing income patterns, or changing demand for personal loans.



After deployment, the bank should monitor:



\* Precision

\* Recall

\* F1 score

\* Input-feature distributions

\* Prediction distributions

\* System reliability

\* Data quality



Retraining should only occur using validated data, and every replacement model should be benchmarked against the existing production model.



\### Human Oversight and Accountability



The model should support staff rather than independently control customer-related decisions.



The bank remains responsible for:



\* The data used to train the system.

\* The model selected for deployment.

\* How predictions are interpreted.

\* How customers are targeted.

\* Any harm caused by incorrect or unfair use.



\## Deployment Assessment



The system should \*\*not be deployed in its current form as a fully automated production system\*\*.



It may be used as a controlled decision-support prototype, provided that:



\* Humans review the model’s recommendations.

\* Fairness testing is completed.

\* Explainability tools are introduced.

\* Production monitoring is established.

\* Security controls are implemented.

\* Data usage and accountability responsibilities are documented.

\* The final model is selected according to clearly defined business objectives.



\## Current Limitations



\* The dataset contains only 5,000 records.

\* The target classes are imbalanced.

\* The baseline and Azure evaluations used different testing procedures.

\* Azure AutoML was limited to five trials.

\* Fairness was discussed but not experimentally measured.

\* No production monitoring pipeline was implemented.

\* No live banking system was integrated.

\* The results should not be generalized to real customers without additional validation.



\## Repository Contents



```text

responsible-ai-loan-prediction/

├── FoAI.ipynb

├── bank\_personal\_loan\_data.csv

├── Dataset Metadata.pdf

└── README.md

```



\## Intended Use



This repository is intended for:



\* Academic learning

\* Machine-learning experimentation

\* Portfolio demonstration

\* Responsible AI analysis

\* Model-comparison practice



It is not intended for real loan approval, credit scoring, customer eligibility decisions, or uncontrolled production deployment.





