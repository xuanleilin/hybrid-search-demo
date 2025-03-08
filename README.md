# **Hybrid Search Demo**

This repository demonstrates **Hybrid Search**, combining **graph-based search** and **vector-based search**, for **Recommendation Systems** and **QA Systems**.

---

## **1. Clone the Repository**
Clone the repository to your local machine:
```bash
git clone git@github.com:xuanleilin/hybrid-search-demo.git
cd hybrid-search-demo
```

---

## **2. Install Dependencies**
Ensure you have **Poetry** installed, then run:
```bash
poetry install --no-root
```
This will create a virtual environment and install all required dependencies.

---

## **3. Activate the Virtual Environment**
Activate the environment using:
```bash
eval $(poetry env activate)
```

---

## **4. Dataset Preparation**
### **4.1 Generate the Similarity Graph**
Use **MinHash (LSH) Approximation** to generate a **similarity graph**:
```bash
cd demo
python3 similarity.py
```
**Expected Output:**
```
Loading dataset...
Computing MinHash signatures...
Generating MinHash: 100%|███████████████████████████| 8640/8640 [00:07<00:00, 1206.81it/s]
Finding similar songs...
Finding similar songs: 100%|███████████████████████| 8640/8640 [00:35<00:00, 244.80it/s]
Saving results...
Saved 4,583,182 similar song pairs to output/similar_songs.csv
```

### **4.2 Generate Embeddings with OpenAI**
Use OpenAI’s embedding model to generate **song embeddings**.

#### **🔹 Step 1: Set up OpenAI API Key**
Replace `<YOUR_OPENAI_KEY>` with your actual **OpenAI API key**:
```bash
export OPENAI_API_KEY="<YOUR_OPENAI_KEY>"
```

#### **🔹 Step 2: Run Embedding Generation**
```bash
python3 embedding_generator.py
```

### **4.3 Copy the Dataset to the TigerGraph Server**
Copy the dataset to your **TigerGraph server** (assuming your data directory is `~/data/KGRec`):
```bash
scp output/* <YOUR_USERNAME>@<YOUR_TIGERGRAPH_SERVER>:~/data/KGRec
```

---

## **5. Create Schema, Load Data, and Install Queries**
There are **two methods** to create the schema and load data:
1. **Using GSQL (Manual)**
2. **Using TigerGraphX (Recommended)**

---

### **5.1 Using GSQL**
To set up the **graph schema** and load data manually:

1. Copy the files from the `demo/gsql/` folder to your **TigerGraph server**.
2. Run the following GSQL scripts in order:
   ```bash
   gsql 1_create_schema.gsql            # Create the schema
   gsql 2_add_vector_attributes.gsql    # Add vector attributes
   gsql 3_load_data.gsql                 # Load the dataset (modify `sys.data_root` if needed)
   gsql 4_install_queries.gsql           # Install graph queries
   ```

---

### **5.2 Using TigerGraphX (Recommended)**
**Step 1:** Launch **Jupyter Notebook**:
```bash
jupyter notebook
```
**Step 2:** Open `hybrid_search_demo.ipynb` and **run the commands** to:
- Create schema
- Load data

**Step 3:** Install Queries in **GraphStudio**:
1. Open **GraphStudio**:  
   **http://<YOUR_TIGERGRAPH_HOST>:14240/studio/#/home**
2. Add and install the queries inside:
   - `get_neighbors.gsql`
   - `graph_based_similarity_search.gsql`

---

## **6. Running Recommendation and QA Systems**
Once the **graph schema and data** are loaded, run the commands in `hybrid_search_demo.ipynb` to perform:

✅ **Graph-Based Similarity Search**  
✅ **Vector-Based Similarity Search**  
✅ **Hybrid Search**  
✅ **Vector-Based Search for QA**  
✅ **Hybrid Search for QA**  

