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

