# 🕸️ Multidimensional Data Indexing & Similarity Search Engine

> A comparative study of spatial access methods (k-d Tree, Quadtree, R-tree, Range Tree) combined with Locality Sensitive Hashing (LSH) for efficient hybrid query processing.

![Python](https://img.shields.io/badge/Language-Python-3776AB)
![CS Concepts](https://img.shields.io/badge/Concepts-Spatial%20Indexing%20%7C%20LSH-orange)
![Dataset](https://img.shields.io/badge/Data-Coffee%20Reviews-green)

## 📖 Overview
This project addresses the challenge of performing efficient **k-dimensional range queries** combined with **textual similarity search** on a large dataset. 

Standard database indexing (like B-Trees) struggles with multi-dimensional data. This project implements and evaluates four distinct Multi-dimensional Access Methods (MAMs) to index the [Coffee Reviews Dataset](https://github.com/vasizaf/Multidimensional-Data-Structures/blob/main/simplified_coffee.csv). The goal is to filter records based on numerical attributes (Price, Rating, Year) and then rank them based on textual similarity (Review Comments) using **Locality Sensitive Hashing (LSH)**.

## 🧱 Data Structures Implemented
All data structures were implemented from scratch in Python to evaluate their underlying mechanics and performance trade-offs.

| Structure | Description | Use Case |
| :--- | :--- | :--- |
| **k-d Tree** | A binary tree that partitions space by cycling through dimensions. | Efficient for nearest neighbor search in low dimensions (k ≤ 20). |
| **Quadtree** | A tree data structure in which each internal node has exactly four children. | Ideal for 2D spatial indexing and image processing. |
| **R-tree** | A balanced search tree that groups objects using Minimum Bounding Rectangles (MBR). | The standard for geospatial databases (e.g. PostGIS). |
| **Range Tree** | A tree of trees (multi-level data structure). | Optimized specifically for orthogonal range queries. |
| **LSH (MinHash)** | A probabilistic algorithm for reducing the dimensionality of high-dimensional data. | Used here to find similar text reviews in `O(1)` time (approx). |

## ⚙️ The Query Algorithm
The system executes a **2-Phase Query Processing** pipeline:

1.  **Phase 1: Multidimensional Filtering (The Tree)**
    * The user defines ranges for up to 4 attributes (e.g. *Price between $4-$10, Rating > 94*).
    * The selected spatial tree (e.g. k-d tree) rapidly prunes the search space to find candidate records.
2.  **Phase 2: Similarity Ranking (LSH)**
    * From the filtered candidates, the system applies **LSH** on the text review column.
    * It retrieves the Top-N records that are semantically similar to a query text.

> **Example Query:**
> *"Find the top 3 reviews similar to 'fruity and bright', where the coffee was rated >94, price is $4-$10 and origin is USA."*

## 📊 Performance Evaluation
A core component of this project is the comparative analysis of the four trees.

* **Space Complexity:** The k-d tree proved to be the most memory-efficient.
* **Query Time:** The Range Tree offered the fastest orthogonal range search but suffered from high memory overhead (`O(n log^(k-1) n)`).
* **High Dimensions:** The R-tree performed best when handling clusters of data points compared to the Quadtree which struggled with unbalanced distributions.

## 📂 File Structure
```text
├── kdtree.py / kdtree_gui.py      # k-d Tree logic & Visualization
├── quadtree.py / quadtree_gui.py  # Quadtree logic & Visualization
├── rtree.py / rtree_gui.py        # R-tree logic & Visualization
├── range_tree.py / rangetree_gui.py # Range Tree logic & Visualization
├── lsh.py                         # Locality Sensitive Hashing implementation
├── main.py                        # Entry point for running queries and home GUI
├── simplified_coffee.csv          # Initial dataset
└── queries.txt                    # Batch of test queries
```

## 🚀 How to Run

### Prerequisites
Ensure you have Python 3.x installed. You will also need `pandas` for handling the CSV dataset and `matplotlib` for plotting.

1.  **Install Dependencies:**
    ```bash
    pip install pandas numpy matplotlib
    ```

### Execution
2.  **Launch the Application:**
    Run the main script to open the **Home GUI**. This dashboard allows you to select which Data Structure you want to initialize and visualize.
    ```bash
    python main.py
    ```

3.  **Direct Visualization (Optional):**
    You can also run specific visualization modules directly if you want to bypass the main menu:
    ```bash
    python quadtree_gui.py
    # OR
    python kdtree_gui.py
    ```

> **Note:** Ensure the `simplified_coffee.csv` file is located in the same directory as the scripts before launching the application.
