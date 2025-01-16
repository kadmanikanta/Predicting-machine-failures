# Machine Failure Prediction - README

## Dataset Description

### Introduction
This dataset contains various features related to a manufacturing process, aimed at predicting machine failures. The target variable, **Machine Failure**, indicates whether the machine has failed based on several failure modes.

### Features

- **UID**: A unique identifier ranging from 1 to 10,000.
  
- **Product ID**: Consists of a letter (L, M, H) for product quality variants:
  - **L (Low Quality)**: 50% of all products
  - **M (Medium Quality)**: 30% of all products
  - **H (High Quality)**: 20% of all products

- **Type**: The product quality type, directly derived from the **Product ID**:
  - **L**: Low Quality
  - **M**: Medium Quality
  - **H**: High Quality

- **Air Temperature [K]**: Air temperature generated using a random walk process, normalized with a standard deviation of 2 K around 300 K.

- **Process Temperature [K]**: Process temperature generated with a random walk process normalized to a standard deviation of 1 K, plus 10 K added to the air temperature.

- **Rotational Speed [rpm]**: Rotational speed is calculated from a power of 2860 W, overlaid with normally distributed noise.

- **Torque [Nm]**: Torque values are normally distributed around 40 Nm, with a standard deviation of 10 Nm. No negative torque values are allowed.

- **Tool Wear [min]**: The product quality variants influence tool wear time:
  - **H**: Adds 5 minutes of tool wear
  - **M**: Adds 3 minutes of tool wear
  - **L**: Adds 2 minutes of tool wear

- **Machine Failure (Target Label)**: A binary label indicating whether the machine has failed (1) or not (0). This label is set to 1 if any of the following failure modes occur:
  - **Tool Wear Failure (TWF)**
  - **Heat Dissipation Failure (HDF)**
  - **Power Failure (PWF)**
  - **Overstrain Failure (OSF)**
  - **Random Failure (RNF)**

### Failure Modes

The "Machine Failure" label is set to 1 if any of the following failure modes are true:

- **Tool Wear Failure (TWF)**:
  - The tool is replaced or fails at a randomly selected tool wear time between 200 and 240 minutes. Occurs 120 times, with 69 tool replacements and 51 failures (randomly assigned).

- **Heat Dissipation Failure (HDF)**:
  - Heat dissipation causes a process failure if the difference between air temperature and process temperature is below 8.6 K, and the rotational speed is below 1380 rpm. Occurs in 115 data points.

- **Power Failure (PWF)**:
  - The product of torque and rotational speed (in rad/s) calculates the required power for the process. If the power is below 3500 W or above 9000 W, the process fails. Occurs in 95 data points.

- **Overstrain Failure (OSF)**:
  - The process fails if the product of tool wear and torque exceeds a threshold:
    - L product variant: 11,000 minNm
    - M product variant: 12,000 minNm
    - H product variant: 13,000 minNm
  - Occurs in 98 data points.

- **Random Failure (RNF)**:
  - Each process has a 0.1% chance of failure, regardless of process parameters. This failure mode is true for only 5 data points.

### Conclusion

This dataset is designed to model and predict machine failures in a manufacturing process. It can be used for failure prediction, which is crucial for minimizing costly downtime and ensuring efficient operations.

---

## Business Case: Predicting Machine Failures

In the context of predicting machine failures, we aim to identify failures early to avoid costly downtime and repairs. We evaluated multiple machine learning models to predict failures:

### Model Performance:

1. **Random Forest Model**:
   - **Accuracy**: 97.47%
   - **Recall**: 38.46%
   - Although Random Forest provides high accuracy, it performs poorly in terms of **recall**. This means it only identifies 38.46% of actual machine failures, which is a significant problem, as failures lead to expensive downtime and repairs.

2. **Logistic Regression**:
   - **Accuracy**: 81.20%
   - **Recall**: 88.46%
   - Logistic Regression has a lower accuracy but an excellent **recall** score of 88.46%. This is crucial because our primary goal is to capture as many failures as possible to minimize downtime, even at the cost of some false positives.

3. **Decision Tree Model**:
   - **Accuracy**: 90.73%
   - **Recall**: 80.77%
   - The Decision Tree model performs well with a good **accuracy** and decent **recall**. While not as good as Logistic Regression in terms of recall, it still provides a solid balance between predicting failures and maintaining accuracy.

### Why Did the Random Forest Model Perform Poorly?

Despite being a more advanced model, **Random Forest** performed poorly due to inadequate preprocessing. While it achieved high accuracy, the low recall indicates that it struggled to capture most machine failures effectively.


### Conclusion

- The **Logistic Regression** model achieves **88% recall**, making it the best for predicting machine failures.
- The **Random Forest** model, despite its high accuracy, performed poorly with **38% recall** due to insufficient preprocessing.
- **Advanced preprocessing** steps like univariate tests and backward regression can significantly improve Random Forest's performance and recall.

