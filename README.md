# CS5425-Big_Data_System_For_Data_Science

## Project Background
This coursework focuses on handling big data using distributed systems and machine learning frameworks. The assignments utilize different datasets and frameworks to demonstrate proficiency in data processing, analysis, and predictive modeling using industry-standard tools like Hadoop and Spark.

## Assignment 1: Hadoop Streaming for SMS Data Analysis

### Task 1: Identifying Duplicate Messages
**Objective:** Detect duplicate SMS messages sent by the same user in the NUS SMS Corpus dataset.

**Dataset:**
- sms.jsonl (55,835 messages)
- small_sms_dup.jsonl (for testing)

**Approach:**
1. **Mapper:**
   - Extracts the user ID and message text from each JSON object.
   - Computes a hash of the message.
   - Outputs a key-value pair where the key is the combination of user ID and message hash.
2. **Reducer:**
   - Aggregates the counts for each user-message pair.
   - Identifies duplicates (count > 1) and outputs a list of user IDs with duplicate messages.

**Output:**
- A list of user IDs (one per line) who sent duplicate messages.
- Stored in `part-00000` under `/content/output/`.

**Files:**
- `mapper.py`: Contains the mapping logic.
- `reducer.py`: Contains the reducing logic.
- `task1.ipynb`: Jupyter notebook with the complete code and execution steps.

### Task 2: Grammar Correctness Scoring
**Objective:** Evaluate the grammatical correctness of sentences in SMS messages and compute an average score for each user, outputting the top 50 users with the highest scores.

**Dataset:**
- sms.jsonl (55,835 messages)

**Approach:**
1. **Mapper:**
   - Splits each message into sentences using utility functions (`split_into_sentences`).
   - Checks each sentence for grammatical correctness using `check_grammar` (starts with an uppercase letter or number and ends with ., ?, or !).
   - Assigns a score per message:
     - 5 (all sentences correct)
     - 2 (some correct)
     - 0 (none correct)
   - Outputs user ID and score as a key-value pair.
2. **Reducer:**
   - Aggregates scores for each user.
   - Computes the average score.
   - Sorts users by score (descending) and user ID (ascending as strings).
   - Outputs the top 50 users with their average scores (rounded to 3 decimal places).

**Output:**
- A list of the top 50 user IDs and their average grammar correctness scores (e.g., `103\t5.000`).
- Stored in `part-00000` under `/content/output/`.
- Matches the provided `task_2_sample_answer.txt` for the first 5 lines.

**Files:**
- `mapper.py`: Contains the mapping logic with grammar checking.
- `reducer.py`: Contains the reducing logic for averaging and sorting.
- `task2.ipynb`: Jupyter notebook with the complete code and execution steps.

---
## Assignment 2: Machine Learning Pipeline with Spark

**Objective:** Build a machine learning pipeline to predict the status of PPP loans (e.g., "Paid in Full" or "Exemption 4").

**Dataset:** PPP loan data with features such as:
- Borrower details
- Loan amounts
- Jobs reported
- Business type
- Loan status (target variable)

**Approach:**
### Data Preparation
- Load and clean the dataset using PySpark.
- Split into training (70%) and testing (30%) sets.

### Pipeline Stages
1. **StringIndexer**: Encodes categorical features (e.g., `BorrowerState`, `BusinessType`) into numerical indices.
2. **OneHotEncoder**: Converts indexed categorical features into one-hot encoded vectors.
3. **VectorAssembler**: Combines numerical features (e.g., `InitialApprovalAmount`, `JobsReported`) into a single vector.
4. **MinMaxScaler**: Scales numerical features to a range (0,1).
5. **VectorAssembler (Final)**: Combines all processed features (one-hot encoded categorical, scaled numerical, and boolean features like `RuralUrbanIndicator`) into a single feature vector.
6. **RandomForestClassifier**: Trains a model with 150 trees to predict `LoanStatus`.

### Evaluation
- Used `MulticlassClassificationEvaluator` to compute accuracy.
- Achieved approximately **82% accuracy** on both training and testing sets.

**Output:**
- Predictions of loan status and evaluation metrics (training and testing accuracy).

**Files:**
- `assignment_2_student.ipynb`: Databricks notebook containing the complete code, including data preparation, pipeline construction, and evaluation.

