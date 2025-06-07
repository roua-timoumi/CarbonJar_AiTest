# CarbonJar_AiTest
Challenge 1 - Real-Time CO₂ Data Pipeline

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

