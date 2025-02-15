# Document Similarity using MapReduce

## Objective
This project computes the Jaccard Similarity between multiple text documents using Hadoop MapReduce.

## Problem Statement
Given a set of documents, the task is to determine the similarity between each pair of documents based on the words they contain. The Jaccard Similarity is calculated as follows:

**Jaccard Similarity (A, B) = |A ∩ B| / |A ∪ B|**

## Input Format
The input consists of multiple lines, where each line represents a document. Each line follows this format:

```
cloud computing is scalable and efficient
cloud storage is used for big data processing
big data analytics is crucial for insights
```

## Expected Output
The output contains the Jaccard Similarity between document pairs:

```
(document3, document2)	Similarity: 0.33
(document3, document1)	Similarity: 0.11
(document2, document3)	Similarity: 0.33
(document2, document1)	Similarity: 0.20
(document1, document3)	Similarity: 0.11
(document1, document2)	Similarity: 0.20
```

## Approach and Implementation
This project is implemented using **Java and Hadoop MapReduce**. The program consists of the following components:

### **Mapper Class**
- Reads each line of the input and extracts the document name and its content.
- Tokenizes the document content into words.
- Emits **(word, document ID)** key-value pairs.

### **Reducer Class**
- Receives a word and a list of documents in which it appears.
- Builds an **inverted index** where each document is mapped to its word occurrences.
- Computes the **Jaccard Similarity** for each document pair by:
  - Counting the intersection (common words between two documents).
  - Counting the union (total unique words in both documents).
  - Applying the Jaccard formula.
- Outputs the document pairs and their computed similarity scores.

## Steps to Run the Project

### **Prerequisites**
- **Hadoop 3.2.1**
- **Java 8 or higher**
- **Maven**
- **Git**
- **Docker (optional, for running Hadoop in a container)**

### **Build and Run Steps**
1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd assignment-1-mapreduce-document-similarity
   ```

2. Build the project using Maven:
   ```bash
   mvn clean package
   ```

3. Start Hadoop (if running locally, ensure Hadoop services are active):
   ```bash
   start-dfs.sh
   start-yarn.sh
   ```
   If using Docker:
   ```bash
   docker exec namenode hadoop namenode -format
   docker-compose up -d
   ```

4. Upload input data to HDFS:
   ```bash
   hdfs dfs -mkdir -p /input
   hdfs dfs -put input/*.txt /input
   ```

5. Run the Hadoop MapReduce job:
   ```bash
   hadoop jar target/DocumentSimilarity-0.0.1-SNAPSHOT.jar com.mapreduce.DocumentSimilarityDriver /input /output
   ```

6. Retrieve the output:
   ```bash
   hdfs dfs -cat /output/part-r-00000
   ```

7. Copy output to local filesystem:
   ```bash
   hdfs dfs -get /output /local-output
   ```

## Challenges Faced
1. **Tokenization Issues:** Initially, tokenization inconsistencies led to incorrect document-word mapping. Resolved by using regex-based tokenization.
2. **Handling Large Input Files:** Used Hadoop’s efficient **combiner** to optimize the shuffle phase and reduce network overhead.
3. **Debugging MapReduce Jobs:** Used Hadoop logs and local testing with small datasets before deploying on HDFS.
4. **Duplicate Pairs:** Ensured **unordered document pairs** were processed uniquely to avoid duplicate similarity calculations.

## Conclusion
This project successfully calculates document similarity using the **Jaccard Similarity measure** in a **distributed MapReduce framework**. The implementation ensures scalability and efficiency when processing large datasets.

