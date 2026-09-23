# 📱 Personal Behavior & Habit Pattern Mining

A Data Mining project that uses **K-Means clustering** to discover behavioral patterns in smartphone usage data.

The project started with two goals:

1. Learn clustering by building a real project.
2. Explore smartphone usage patterns through data because phone and social-media usage is personally relevant to me.

> **Current status: V1 complete**

---

## 🎯 Project Question

Instead of predicting a predefined target, this project asks:

> **What can data tell us about smartphone behavioral patterns?**

The project uses behavioral features to discover groups of users with similar usage patterns.

---

## 📊 Dataset

The V1 dataset contains:

- **700 users**
- **5 behavioral features**

### Features

| Feature | Description |
|---|---|
| App Usage Time | App usage in minutes per day |
| Screen On Time | Screen-on time in hours per day |
| Battery Drain | Battery drain in mAh per day |
| Number of Apps Installed | Number of installed applications |
| Data Usage | Mobile/data usage in MB per day |

---

## 🧠 Method

The project uses **K-Means clustering**.

Because K-Means is distance-based, the behavioral features are standardized before clustering.

### Pipeline

```text
Raw Dataset
     ↓
Load Data
     ↓
Select Behavioral Features
     ↓
StandardScaler
     ↓
K-Means
     ↓
Evaluate K
     ↓
Interpret Clusters
     ↓
Save Model
     ↓
New User Prediction
     ↓
Streamlit App
````

---

## 🔢 Choosing K

I evaluated values of **K from 2 to 10** using:

* Inertia
* Silhouette Score

|  K | Inertia | Silhouette |
| -: | ------: | ---------: |
|  2 |  909.53 |      0.619 |
|  3 |  433.47 |      0.587 |
|  4 |  243.30 |      0.591 |
|  5 |  158.41 |      0.600 |
|  6 |  136.24 |      0.545 |
|  7 |  126.94 |      0.472 |
|  8 |  118.85 |      0.467 |
|  9 |  112.82 |      0.398 |
| 10 |  106.85 |      0.396 |

The highest silhouette score in this experiment occurs at **K=2 (0.619)**.

For V1, **K=5** was selected to create five behavioral usage profiles for further exploration.

This is a modeling decision rather than a claim that K=5 is the mathematically optimal value according to every metric.

---

## 🧩 V1 Cluster Results

With K=5, the 700 users were distributed as follows:

| Cluster | Users |
| ------: | ----: |
|       0 |   136 |
|       1 |   146 |
|       2 |   143 |
|       3 |   136 |
|       4 |   139 |

### Cluster Profiles

| Cluster | App Usage | Screen Time | Battery Drain |  Apps | Data Usage | Profile                    |
| ------: | --------: | ----------: | ------------: | ----: | ---------: | -------------------------- |
|       0 |    541.42 |       10.11 |       2701.01 | 89.25 |    1974.77 | Very High Behavioral Usage |
|       1 |    131.97 |        3.04 |        883.81 | 30.75 |     451.42 | Low Behavioral Usage       |
|       2 |    235.40 |        4.96 |       1515.06 | 50.00 |     822.01 | Medium Behavioral Usage    |
|       3 |     60.43 |        1.49 |        454.98 | 14.56 |     202.32 | Very Low Behavioral Usage  |
|       4 |    395.75 |        6.91 |       2105.81 | 69.92 |    1232.23 | High Behavioral Usage      |

These labels are **descriptive usage patterns** based on the selected behavioral variables.

They do not represent psychological, medical, or health classifications.

---

## 💾 Model Persistence

The trained components are saved so they can be reused during inference.

```text
models/
├── scaler.pkl
└── kmeans.pkl
```

The scaler stores the preprocessing transformation learned from the training data.

The K-Means model stores the learned cluster structure and centroids.

---

## 👤 New User Prediction

Once the model has been trained, a new user does not require retraining.

### Training

```text
Historical Data
      ↓
Scaler
      ↓
K-Means
      ↓
Learned Clusters
```

### Inference

```text
New User
    ↓
Saved Scaler
    ↓
Saved K-Means Model
    ↓
Existing Cluster
```

The new user's behavioral features are transformed using the same scaler and then assigned to one of the existing clusters.

---

## 📁 Processed Data

The clustered dataset is saved as:

```text
data/processed/clustered_users.csv
```

It contains the original users together with their assigned cluster.

---

## 🖥️ Streamlit Application

V1 includes a Streamlit interface for interacting with the clustering system.

The application allows users to:

* Explore cluster sizes
* Explore cluster profiles
* Enter behavioral information for a new user
* Predict the user's existing cluster
* Display the corresponding behavioral usage profile

Example:

```text
New user assigned to Cluster 4

Behavioral Profile:
High behavioral usage
```

---

## 🗂️ Project Structure

```text
Personal Behavior & Habit Pattern Mining/
│
├── data/
│   ├── raw/
│   │   └── user_behavior_dataset.csv
│   └── processed/
│       └── clustered_users.csv
│
├── models/
│   ├── scaler.pkl
│   └── kmeans.pkl
│
├── notebooks/
│
├── src/
│   ├── preprocess.py
│   ├── clustering.py
│   ├── evaluate.py
│   ├── explore.py
│   └── predict.py
│
├── app.py
├── main.py
├── pyproject.toml
└── README.md
```

---

## ⚙️ Running the Project

Install the project dependencies using `uv`.

Run the main pipeline:

```bash
uv run main.py
```

Run the prediction script:

```bash
uv run src/predict.py
```

Run the Streamlit application:

```bash
uv run streamlit run app.py
```

---

## 📈 V1 Results

V1 successfully implements the complete workflow:

```text
Data
  ↓
Preprocessing
  ↓
Clustering
  ↓
Evaluation
  ↓
Cluster Interpretation
  ↓
Model Persistence
  ↓
New User Inference
  ↓
Interactive Application
```

The system successfully:

* Processes the behavioral dataset
* Creates K-Means clusters
* Evaluates different K values
* Generates cluster profiles
* Saves the preprocessing scaler
* Saves the trained K-Means model
* Assigns new users to existing clusters
* Provides an interactive Streamlit interface

---

## 🧠 What I Learned

This project helped me understand clustering beyond the algorithm itself.

The main lessons were:

* Clustering works without predefined target labels.
* Feature scaling matters for distance-based algorithms.
* Choosing K requires looking at evaluation metrics rather than blindly selecting a value.
* Cluster interpretation requires examining the actual feature values.
* A trained model needs to be saved together with the preprocessing required for inference.
* The training pipeline and inference pipeline must use the same transformation.
* A Data Mining algorithm can be turned into an end-to-end ML workflow.

---

## ⚠️ V1 Limitations

V1 focuses on discovering **smartphone behavioral usage patterns**.

The clusters do **not** determine:

* whether a user's behavior is healthy or unhealthy
* whether a user is addicted
* psychological conditions
* medical conditions
* why a user behaves this way

The model only identifies patterns in the behavioral features provided to it.

---

## 🚀 Future Work

V1 is intentionally the stopping point for now.

Future development may explore:

* Deeper cluster analysis
* Additional behavioral features
* Better cluster interpretation
* Additional validation
* Visualization improvements
* Further analysis of the discovered patterns
* Improvements to the application

---

**V2 will be developed later.**





That's a solid V1 package. 🚀
```
