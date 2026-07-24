# Data Folder

**Dataset:** Credit Card Dataset for Clustering — [Kaggle: arjunbhasin2013/ccdata](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata)

- `DataSet(W4).csv.xls` — the dataset as provided with the task (8,950 credit-card customers × 18 columns; despite the `.xls` extension it is a comma-separated file, and the notebook loads it robustly either way).
- `customers_with_clusters.csv` — output of the notebook: the same customers with two extra columns, `KMeans_Cluster` and `Hierarchical_Cluster`.

The notebook looks for the dataset in this folder or the working directory. If you run it in Colab, upload the dataset to the session first.
