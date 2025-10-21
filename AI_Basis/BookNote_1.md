# Machine Learning For Absolute Beginners

## <font color=teal>What is Machine learning?</font>

### <font color=teal>1. The concept of Machine learning</font>

<font color=#5F8A8B>**Arthur Samuel** coined the term **"machine learning"** in his 1959 paper, *"Some Studies in Machine Learning Using the Game of Checkers"*.</font>

- The paper states:*"Enough work has been done to verify the fact that a computer can be programmed so that it will learn to play a better game of checkers than can be played by the person who wrote the program."*

- **"machine learning"** is defined as a subfield of computer science that gives computers the ability to learn without being explicitly programmed.

- **"self-learnning"** is a key feature of machine learning. This refers to the application of statistical modeling to detect patterns and improve performance based on data and empirical information; all without direct programming commands.

- Machines don’t require a direct *input command* to perform a set task but rather *input data*.
  
  - input command (Traditional programming): Command > Action
  
  - input data (Machine learning): Data > Model > Action

<font color=#5F8A8B>For students with an interest in **AI**, *machine learning provides an excellent starting point* in that it offers a more narrow and practical lens of study compared to the conceptual ambiguity of AI.</font>

<img src="file:///D:/BonnieHKY.github.io/AI_Basis/Figures/B1_F1.png" title="" alt="B1_F1.png" width="433">

<font color=grey>Visual representation of the relationship between data-related fields</font>

- **Artificial intelligence (AI)**: AI is a **broad field** aiming to *simulate human intelligence*. AI contains numerous subfields, including search and planning, reasoning and knowledge representation, perception, natural language processing (NLP).

- **Machine learning** (data-driven technical path) is currently the most efficient and widely used method to achieve AI.

### <font color=teal>2. Training DATA & Test DATA</font>

<font color=#5F8A8B>In machine learning, **data** is split into *training data* and *test data*.</font>

- **Training data**: the initial reserve of data you use to develop your model

- **Test data**: After you have successfully developed a model based on the training data and are satisfied with its accuracy, you can then test the model on the remaining data, which is test data.



## <font color=teal>Machine learning categories</font>

### <font color=teal>1. Supervised learning: Learning with labeled data</font>

<font color=#5F8A8B>**Supervised learning** is the most mature and widely used category. Its core is that the training data has **explicit "labels"**.</font>

#### Key Characteristics

- Training data includes distinct features (denoted as “X”) and their corresponding correct outputs (denoted as “y”). Since both X and y are known, the dataset is categorized as “labeled.”

- The model’s performance is evaluated by comparing its predictions with the real labels.

#### Subtypes & Typical Scenarios

**1) Predicting "Discrete Categories"**

Labels are discrete values (e.g., "cat/dog", "spam/non-spam"). The goal is to assign new data to a predefined category.

- **Logistic Regression**: Simple and efficient for binary classification (e.g., "whether a patient has a disease").
- **Decision Tree**: Intuitive, with interpretable "if-then" rules (e.g., classifying customer churn risk based on purchase frequency).
- **Random Forest**: An ensemble of decision trees, reducing overfitting (used in credit scoring to judge loan default risk).
- **Support Vector Machine (SVM)**: Excels at high-dimensional data (e.g., text classification, such as judging whether a news article is about "technology" or "sports").
- **Neural Networks (e.g., CNN)**: Used for complex classification (e.g., face recognition, identifying 10,000+ object categories in images).

**2) Predicting "Continuous Values"**

Labels are continuous numerical values (e.g., temperature, sales volume). The goal is to predict a specific number for new data.

- **Linear Regression**: Models the linear relationship between features and labels (e.g., predicting house price based on area).
- **Polynomial Regression**: Handles non-linear relationships (e.g., predicting crop yield based on fertilizer dosage, which is not a straight-line trend).
- **Gradient Boosting Machines (GBM/XGBoost)**: High-precision for complex regression tasks (e.g., predicting a company’s quarterly sales based on historical data and market trends).

### <font color=teal>2. Unsupervised learning: Learning with unlabeled data</font>

<font color=#5F8A8B>Unsupervised learning uses **unlabeled data** (only input features, no "correct answers"). The model’s goal is to automatically discover hidden patterns, structures, or relationships in the data.</font>

#### Key Characteristics

- No labeled data, so the model cannot "verify" correctness during training; it only mines data inherent features.

#### Subtypes & Typical Scenarios

**1) Clustering: "Grouping Similar Data Together"**

Automatically divide data into multiple groups (clusters) where data in the same group are more similar than those in different group.

- **K-Means**: The most classic clustering algorithm; you need to specify the number of clusters (K) in advance (e.g., dividing 10,000 users into 5 consumption groups).
- Hierarchical Clustering: Builds a "tree" of clusters (dendrogram), no need to predefine K (used in biological taxonomy, such as classifying species based on gene sequences).
- DBSCAN: Identifies clusters of arbitrary shapes and filters outliers (e.g., detecting abnormal density in urban traffic flow data).

**2) Dimensionality Reduction: "Simplifying Data Without Losing Key Information"**

Reduces the number of input features (e.g., from 100 dimensions to 10 dimensions) to solve problems like "high-dimensional data sparsity" and "slow computation".

- PCA (Principal Component Analysis): Converts correlated features into uncorrelated "principal components" (e.g., compressing a 1000-pixel image into 100 dimensions while retaining 90% of the image information).
- t-SNE: Excels at visualizing high-dimensional data (e.g., reducing 784-dimensional handwritten digit data (28x28 pixels) to 2 dimensions for scatter plot display).

**3) Anomaly Detection: "Finding Data That ‘Doesn’t Fit’"**

Identifies data points that deviate significantly from normal patterns (also called "outlier detection").

- Isolation Forest: Efficiently isolates outliers by randomly splitting features (used in financial fraud detection, such as identifying a $1M transaction in a user’s usual $100 monthly spending).
- Autoencoder (a type of neural network): Reconstructs normal data; abnormal data have higher reconstruction errors (e.g., detecting faulty products in factory production line sensor data).

### <font color=teal>3. Reinforcement learning: Learning through trial and error (rewards & punishments)</font>

<font color=#5F8A8B>**Reinforcement learning (RL)** is the third and most advanced algorithm category in machine learning. It simulates how humans/animals learn: an "agent" (e.g., a robot, an AI chess player) interacts with the "environment" (e.g., a room, a chessboard) by taking actions. It receives **reward signals** (positive for good actions, negative for bad actions) and learns to maximize the total long-term reward</font>

#### Key Characteristics

- Reinforcement learning is set to train the model through continuous learning.

- A standard reinforcement learning model has measurable performance criteria where outputs are not tagged—instead, they are graded.

#### Subtypes & Typical Scenarios

- **Q-Learning**: A classic model-free RL algorithm, learning a "Q-table" to judge the value of each (state, action) pair (e.g., training a robot to navigate a maze to reach the goal).
- **Deep Q-Network (DQN)**: Combines Q-Learning with deep neural networks, handling high-dimensional states (e.g., training an AI to play Atari games like "Breakout" by inputting screen pixels).
- **Policy Gradient**: Directly optimizes the "policy" (the strategy of choosing actions) to maximize rewards (used in continuous action scenarios, such as controlling a drone’s flight altitude and speed).



## <font color=teal>The machine learning toolbox</font>

### <font color=teal>1. Compartment 1: Data</font>

> #### Structured data v.s. Unstructured data
> 
> **1) Structured data has a fixed, organized format (like tables)**
> 
> Structured data is the main input for **traditional ML tasks** (no need for complex preprocessing like feature extraction).
> 
> - It follows a fixed schema and is usually stored in tabular formats.
> 
> - Each piece of data belongs to a specific "field" (column), with clear data types (e.g., numbers, dates, text strings).
> 
> - Quantitative or categorical: Most fields are numerical (e.g., age, price) or discrete categories (e.g., "gender: male/female").
> 
> **2) Unstructured data has no predefined structure (like free text or images)**
> 
> Unstructured data has no fixed format, is often generated by humans (or sensors) in a natural way, and accounts for over 80% of all real-world data. **ML models cannot directly use it**. Unstructured data requires specialized models to extract features before use, which converts it into "structured feature vectors" (a process called **feature engineering** or **representation learning**).
> 
> - **Rich information, captures implicit and multi-dimensional insights**: It includes text, images, audio, etc., which can reflect subjective emotions, visual features, or contextual details that structured data cannot..
> - **Wide application scope, supports cutting-edge ML tasks**: It enables high-value scenarios that structured data cannot cover, such as *computer vision (image recognition)* and *natural language processing (NLP)*—these are the **core of modern AI**.
> - **High preprocessing complexity, long development cycles**: **Unstructured data needs to be converted into "structured feature vectors** (e.g., text → word embeddings via BERT, images → pixel feature maps via CNN) before ML models can use it.
> - **Poor interpretability, hard to trust and debug**: Deep learning models for unstructured data (e.g., CNN, Transformer) are "**black boxes**." It’s hard to explain why a model made a decision. For example, "why did the model classify this CT scan as ‘having cancer’?" If a model misclassifies a tumor as benign, doctors can’t trace the reason—this may lead to misdiagnosis.

#### Basic building blocks of structured data

**1) Variable: a specific attribute that describes the data (= a "column" in table)**

Each variable captures one type of information, and all entries in that column follow the same data format.

**2) Feature: a subset of variables that help the model to learn**

Features are "useful variables" for the training the model. 

**NOTE**: In most structured data scenarios, people use "feature" and "variable" interchangeably.

**3) Vector: A 'Numerical List' that represents a single data sample. (= a "row" in table)**

- A "vector" is a single row of structured data (a **sample) into a format that machines can process. It’s a list of numbers where each number corresponds to a feature of the sample.

- Vectors only contain numbers. Categorical features need to be converted to numbers first (this is called "**categorical encoding**").

**NOTE**: In **mathematical definition of linear algebra**, vector can be a row vector (horizontal) or a column vector (vertical). In **machine learning**, *feature vectors* usually refers to the **row vector**, because this is how models process data: ML models process data **one sample at a time**, a row vector directly corresponds to "all features of one sample," which is the unit the model needs to learn from.

![B1_F2.png](D:\BonnieHKY.github.io\AI_Basis\Figures\B1_F2.png)

### <font color=teal>2. Compartment 2: Infrastructure</font>

### <font color=teal>3. Compartment 3: Algorithms</font>

- Beginners will typically start off by using simple supervised learning algorithms such as linear regression, logistic regression, decision trees, and k-nearest neighbors.

- Beginners are also likely to apply unsupervised learning in the form of k-means clustering and descending dimension algorithms.

### <font color=teal>4. Visualization</font>
