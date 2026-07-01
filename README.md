# Intrusion Detection System (IDS) using Metaheuristic Feature Selection

This repository details a machine learning project focused on building an Intrusion Detection System (IDS) for network traffic analysis. The project explores the effectiveness of various metaheuristic optimization algorithms for feature selection combined with different classification models to identify network intrusions.

## Table of Contents
- [Introduction](#introduction)
- [Dataset](#dataset)
- [Feature Engineering](#feature-engineering)
- [Feature Selection Methods](#feature-selection-methods)
  - [Cuckoo Catfish Optimization (CCO)](#cuckoo-catfish-optimization-cco)
  - [Chaotic Tent Puma Optimization (CTPO)](#chaotic-tent-puma-optimization-ctpo)
  - [Binary Artificial Hummingbird Optimizer (AHO)](#binary-artificial-hummingbird-optimizer-aho)
- [Model Training and Evaluation](#model-training-and-evaluation)
  - [Random Forest Classifier](#random-forest-classifier)
  - [Logistic Regression](#logistic-regression)
  - [Long Short-Term Memory (LSTM) Network](#long-short-term-memory-lstm-network)
  - [Multi-Layer Perceptron (MLP) Network](#multi-layer-perceptron-mlp-network)
- [Setup and Usage](#setup-and-usage)

## Introduction
An Intrusion Detection System (IDS) is crucial for cybersecurity, aiming to detect malicious activities or policy violations in a network. This project investigates the use of advanced feature engineering and metaheuristic feature selection techniques to enhance the accuracy and efficiency of IDS models. We compare the performance of traditional machine learning (Random Forest, Logistic Regression) and deep learning models (LSTM, MLP) on feature subsets optimized by Cuckoo Catfish Optimization (CCO), Chaotic Tent Puma Optimization (CTPO), and Binary Artificial Hummingbird Optimizer (AHO).

## Dataset
The dataset used for this project is `MQTTEEB-D_dataset_All_Processed.csv`, which contains network traffic data with various features related to TCP and MQTT protocols. The `Target` column represents the class labels indicating different types of network behavior (e.g., normal traffic, various attack types).

**Initial Data Overview:**
- **Shape:** 222,813 rows, 13 columns
- **Columns:** `timestamp`, `tcp_flags`, `tcp_time_delta`, `tcp_len`, `mqtt_conack_flags`, `mqtt_conflag_cleansess`, `mqtt_conflags`, `mqtt_dupflag`, `mqtt_hdrflags`, `mqtt_kalive`, `mqtt_msg`, `mqtt_qos`, `Target`.

## Feature Engineering
To enrich the dataset and provide more discriminative features for the models, we performed extensive feature engineering on the `df_engineered` DataFrame. Key engineered features include:
- `tcp_len_category`: Categorical binning of `tcp_len` (small, medium, large).
- `mqtt_msg_category`: Categorical binning of `mqtt_msg` (none, low, moderate, high).
- `mqtt_msg_per_tcp_len`: Ratio of MQTT messages to TCP length.
- `tcp_len_mqtt_msg_product`: Product of TCP length and MQTT messages.
- `unusual_tcp_flags`: Binary flag for TCP flags outside the top 3 most frequent.
- `mqtt_conflags_anomaly`: Binary flag for MQTT connection flags outside the top 3 most frequent.
- `mqtt_kalive_deviation`: Binary flag for non-standard MQTT keep-alive values.
- `high_mqtt_qos`: Binary flag for high MQTT Quality of Service (QoS) level 2.
- `inter_arrival_time`: Time difference between consecutive packets.
- `time_of_day_seconds`: Time of day in seconds.
- `day_of_week`: Day of the week.

After engineering and one-hot encoding categorical features, the dataset expanded to **24 features**.

## Feature Selection Methods
We utilized three different metaheuristic optimization algorithms to select optimal subsets of features, aiming to reduce dimensionality while preserving or enhancing model performance.

### Cuckoo Catfish Optimization (CCO)
- **Algorithm:** A hybrid metaheuristic inspired by cuckoo search and catfish behavior.
- **Objective:** To find a subset of features that maximizes a fitness function (balancing model accuracy and feature count).
- **Features Selected:** 10 features.
  - Example: `tcp_time_delta`, `mqtt_conack_flags`, `mqtt_hdrflags`, `mqtt_msg`, `mqtt_msg_per_tcp_len`, `tcp_len_mqtt_msg_product`, `mqtt_kalive_deviation`, `high_mqtt_qos`, `time_of_day_seconds`, `tcp_len_category_medium`.

### Chaotic Tent Puma Optimization (CTPO)
- **Algorithm:** A metaheuristic algorithm that incorporates chaotic tent maps for enhanced exploration and exploitation in the search space.
- **Objective:** Similar to CCO, to identify a concise and effective feature subset.
- **Features Selected:** 9 features.
  - Example: `tcp_time_delta`, `tcp_len`, `mqtt_conflags`, `mqtt_msg`, `unusual_tcp_flags`, `mqtt_conflags_anomaly`, `mqtt_kalive_deviation`, `tcp_len_category_medium`, `tcp_len_category_large`.

### Binary Artificial Hummingbird Optimizer (AHO)
- **Algorithm:** A nature-inspired optimization algorithm simulating the foraging strategies of hummingbirds, adapted for binary feature selection.
- **Objective:** To select features that contribute most to classification accuracy with minimal redundancy.
- **Features Selected:** 10 features.
  - Example: `tcp_len`, `mqtt_conack_flags`, `mqtt_qos`, `mqtt_msg_per_tcp_len`, `unusual_tcp_flags`, `high_mqtt_qos`, `inter_arrival_time`, `day_of_week`, `tcp_len_category_medium`, `tcp_len_category_large`.

## Model Training and Evaluation
For each set of selected features, four different classification models were trained and evaluated on a 70/30 train-test split (stratified by target variable).

### Random Forest Classifier
- **Type:** Ensemble Learning (tree-based).
- **Characteristics:** Robust to overfitting, handles non-linear relationships well, less sensitive to feature scaling.

### Logistic Regression
- **Type:** Linear Model.
- **Characteristics:** Simple, interpretable, requires feature scaling for optimal performance, assumes linear separability.

### Long Short-Term Memory (LSTM) Network
- **Type:** Recurrent Neural Network (RNN).
- **Characteristics:** Designed for sequential data, capable of learning temporal dependencies. In this project, each sample was treated as a single timestep.

### Multi-Layer Perceptron (MLP) Network
- **Type:** Feedforward Neural Network.
- **Characteristics:** Capable of learning complex non-linear relationships, requires feature scaling and one-hot encoding of target for multi-class problems.

## Setup and Usage
To run this project, you will need a Python environment with the following libraries:

- `pandas`
- `numpy`
- `scikit-learn`
- `tensorflow` / `keras`
- `matplotlib`
- `seaborn`

```bash
pip install pandas numpy scikit-learn tensorflow matplotlib seaborn
```

Ensure your dataset `MQTTEEB-D_dataset_All_Processed.csv` is accessible (e.g., mounted from Google Drive if running in Colab) at `/content/drive/My Drive/MQTTEEB-D_dataset_All_Processed.csv`.

The notebook can be run sequentially to replicate the feature engineering, feature selection, model training, and evaluation steps.
