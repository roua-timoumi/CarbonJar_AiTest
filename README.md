# CarbonJar_AiTest
****Challenge 1 - Real-Time CO₂ Data Pipeline

In this challenge, I built a real-time data processing pipeline for CO₂ emissions monitoring. The system consists of three main components:

--Data Generation :
A Python script continuously simulates CO₂, temperature, and humidity data, saved to a CSV file every second. The data includes realistic fluctuations and occasional CO₂ spikes to mimic real-world anomalies.
Note: In real projects, data is typically collected from sensors or through web scraping. For this test, we simulate the data ourselves.

--Data Preprocessing :
A real-time processor monitors the CSV file and cleans the data as it arrives. It handles:

Missing values using local temporal neighbors or running averages.

Range validation based on realistic environmental thresholds.

Incremental update of running statistics (mean and standard deviation).


--LSTM Autoencoder for Emissions Anomaly Detection :

The original model had  issues:

Mismatch between encoder and decoder.

No way to reconstruct sequences step-by-step.

Missing output projection layer.

No training loop or data handling.

Fixes Applied:
Added an autoregressive decoder that reconstructs the sequence one step at a time.

Used a Linear layer to project decoder output back to input dimensions.

Included dropout, proper sequence handling, and a full training pipeline (scaling, batching, training, early stopping).

Added reconstruction error-based anomaly detection.

--Debugging Challenges 

Architecture Fixes: The original LSTM Autoencoder had shape mismatches and couldn't reconstruct sequences.

Hidden States Handling: Needed to correctly pass and reshape hidden states between encoder and decoder.

Model Instability: Required tuning hyperparameters (like learning rate) to prevent training failure.

Output Mapping: Added a linear layer to correctly transform decoder output back to input shape.


Overfitting: The model showed overfitting during training, so early stopping was implemented to prevent it and improve generalization.


****Challenge 2 :AI-Based Imputation of Missing Emissions Data

Data Generation and Missing Data Simulation :
I first created a synthetic dataset with 500 samples and four environmental features: CO2, Methane, NOx, and Temperature. To simulate real-world conditions, I randomly removed 10% of the data values, introducing missing entries. This allowed me to work with incomplete data and test imputation methods.

Advanced Data Imputation
To fill the missing values, I applied an advanced imputation technique using IterativeImputer combined with a K-Nearest Neighbors regressor. This iterative method estimates missing entries by modeling each feature as a function of the others, leading to a more accurate and consistent imputation compared to simpler approaches.

Evaluation of Imputation Quality
I evaluated the imputation results by comparing distributions through histograms, basic descriptive statistics (mean, standard deviation, quantiles), and correlation matrices before and after imputation. The close similarity of these metrics confirmed that the imputation preserved the original data distribution and relationships effectively.

Synthetic Data Generation with GAN
I designed a Generative Adversarial Network (GAN) to produce new synthetic samples mimicking the imputed dataset. The generator network transformed random noise vectors into synthetic data points, which I then rescaled back to the original data ranges. This step provided a way to augment data for potential further analysis or model training.

Debugging Challenges and Insights
Throughout the process, I encountered various debugging challenges, such as handling missing data correctly, configuring the imputation pipeline, and scaling GAN outputs accurately. Addressing warnings, verifying data shapes, and visually inspecting results were essential steps. This iterative debugging highlighted the persistence and attention to detail required when working with AI-driven data workflows.

**** challenge 3 :   Feature Engineering Automation  

This challenge involved creating a  pipeline to analyze emissions data and select the most important features for modeling emission intensity. The main steps included:

Feature Engineering:

Converting timestamps to datetime format and extracting temporal features (hour, day of week, month, quarter, weekend indicator).

Calculating emission intensity as emissions normalized by production volume.

Computing rolling statistics (mean, std, max, median, skewness) for emissions and production volume over a 7-day window.

Creating lagged features and change features for emissions and production to capture temporal dependencies.

Handling missing values by forward and backward filling to maintain data continuity.

Feature Selection:

Implementing two methods:

SelectKBest using statistical tests (f_regression) to select top k features.

Recursive Feature Elimination (RFE) with a Random Forest Regressor to iteratively select features based on model importance.

Sorting and returning the selected features along with their importance scores.

Model Validation with SHAP:

Training a Random Forest model on the selected features.

Using SHAP (SHapley Additive exPlanations) to interpret feature contributions and visualize their importance through summary plots.

End-to-End Execution:

Generating synthetic time series data for emissions and production.

Applying feature engineering and selection.

Predicting a target variable based on emission intensity and time features plus noise.

Printing selected features and their importance scores.

Validating and interpreting the model using SHAP plots.

Debugging:
During the challenge, key debugging challenges included handling missing values caused by lag feature creation and correctly interpreting feature importance scores using RFE. Issues such as NaNs from shifting operations were resolved using forward and backward filling. RFE rankings were converted into meaningful scores for better interpretation. Careful alignment of model inputs ensured that SHAP worked correctly. Additionally, synthetic data generation was adjusted to prevent unstable calculations.

Challenge 12 : 
In this challenge, an anomaly detection system was built to handle an imbalanced classification problem. Synthetic data was generated with a large imbalance between normal and anomaly classes. The code applies SMOTE, a technique that artificially balances the dataset by creating synthetic samples of the minority class. Two logistic regression models were trained: one on the original imbalanced data and another on the data after SMOTE balancing. The models were evaluated and compared using multiple metrics and confusion matrices, highlighting the effect of oversampling on detection performance.

Difficulty and debugging:
Debugging this code meant carefully making sure SMOTE was used the right way—only on the training data so the test data stayed untouched. I had to pay close attention to how the data was split and make sure everything matched up correctly after creating new samples. It wasn’t always easy to get the plots and labels right so that the results for both models were easy to understand. I also had to handle warnings and set a fixed random seed to make sure the results were consistent every time I ran the code. Taking the time to carefully check each step—from creating the data, applying SMOTE, training the models, to evaluating them helped me catch small mistakes and finally get the code working smoothly with clear, reliable results.


*****Challenge 4 :  Modular AI-Augmented LCA Model
What was done:
In this project, we refactored a monolithic Life Cycle Assessment (LCA) model into a modular architecture comprising distinct classes: DataIngestor, EmissionFactorMapper, SupplyChainModel, ImpactCalculator, and UncertaintyAnalyzer. Each module encapsulates a clear responsibility, promoting code clarity, reusability, and easier maintenance. The UncertaintyAnalyzer implements a Monte Carlo simulation to quantify uncertainty in emissions estimates, while sensitivity analysis identifies which factors most influence the results.

Why this model and architecture:
This modular design aligns with best practices in software engineering, facilitating independent testing and incremental development. Separating concerns also makes it easier to integrate AI enhancements later—such as NLP for automated data extraction in DataIngestor or graph neural networks for supply chain modeling. The validation strategy includes deterministic calculations verified against expected outputs, Monte Carlo simulations to account for uncertainty, and sensitivity checks to validate model robustness.

Debugging and challenges:
Random sampling logic errors: Early Monte Carlo implementation had inconsistent ranges due to incorrect uncertainty bounds. Debugging with detailed logging and test runs helped ensure uniform sampling within expected bounds.


