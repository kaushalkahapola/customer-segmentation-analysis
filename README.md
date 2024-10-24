# Customer Segmentation using DBSCAN for Targeted Marketing Insights

## Project Overview

This project focuses on **customer segmentation** using the **DBSCAN clustering algorithm**. By analyzing sales data across **luxury**, **fresh**, and **dry goods**, we aim to identify distinct customer segments based on their purchasing behaviors. The insights derived from this clustering will help tailor targeted marketing strategies and product offerings for each group.

A notable insight from this analysis was the discovery of **only two distinct patterns** in sales behavior across the `outlet_city` variable, which led to the creation of a **binary city label**.

## Project Goals

- **Data Preprocessing:** Handle missing values and clean up the dataset.
- **Feature Engineering:** Transform categorical variables, identify patterns in cities, and scale features.
- **Clustering with DBSCAN:** Apply DBSCAN to find customer clusters based on sales patterns.
- **Cluster Analysis:** Examine the resulting clusters to understand customer behavior and suggest marketing strategies.
- **Visualization:** Use various plots to support the analysis and display cluster distribution.

---

## 1. Data Loading

We begin by loading the dataset, which contains sales data from various outlets across different cities. The key sales categories are:

- `luxury_sales`: Sales of high-end or premium goods.
- `fresh_sales`: Sales of fresh products like produce.
- `dry_sales`: Sales of non-perishable, dry goods.

---

## 2. Data Cleaning and Preprocessing

### 2.1 Handling Missing Values
In the first step, we identify and handle missing values. There were a few columns with `null` or `nul` entries, which were replaced by `NaN`, and since the proportion of missing data was minimal, we chose to **drop these rows**.

### 2.2 Converting Data Columns to Numerical
Some columns, especially sales figures, had non-numeric values or were represented as strings with commas. We cleaned these by removing commas and converting the columns into floating-point numbers.

```plaintext
luxury_sales, fresh_sales, and dry_sales columns were successfully converted to numerical values.
```

### 2.3 Encoding Categorical Variables

#### Visualizing Sales Distribution by City

During the exploratory data analysis (EDA), we plotted the sales distributions across different cities using a **boxplot**. This visualization provided a clear overview of the sales variations by city:

![Sales Distribution by City](https://github.com/user-attachments/assets/53f6cdc1-f134-43dd-b755-3400679de760)


**Insight:** We observed that the distribution of sales was not highly varied between the cities, and that there were **only two distinct patterns** based on the `dry_sales` column. Cities with `dry_sales` exceeding 12,000 formed one pattern, while others formed a second pattern. This finding enabled us to simplify the city analysis into **two categories**.

#### Creating Binary City Labels

To reflect this insight, we created a **binary label** for cities:
- Cities where `dry_sales` exceed 12,000 were assigned a label of `1`.
- Cities with lower `dry_sales` were assigned a label of `0`.

This new binary feature, `city_label`, was then used for further analysis.

---

## 3. Data Scaling and Outlier Detection

### 3.1 Scaling the Data
We scaled all numerical features using **StandardScaler** to ensure the features are on the same scale. This step is crucial for distance-based algorithms like DBSCAN, as features with larger ranges can dominate the distance metric.

```plaintext
StandardScaler was used to scale all numeric features.
```

### 3.2 Outlier Detection

Using boxplots, we visualized the distribution of the sales variables (`luxury_sales`, `fresh_sales`, and `dry_sales`) to identify potential **outliers**.

![Outlier Detection](https://github.com/user-attachments/assets/35bab821-6927-41ee-aa8f-86562e59d0aa)



**Insight:** Each sales category showed distinct patterns of outliers, especially in the `fresh_sales` category, where there were many extreme values.

---

## 4. Clustering with DBSCAN

### 4.1 Model Selection: DBSCAN

For this project, we used **DBSCAN (Density-Based Spatial Clustering of Applications with Noise)**, an unsupervised machine learning algorithm particularly effective at identifying **clusters with irregular shapes** and **outliers**.

The parameters we used were:
- `eps`: 0.4
- `min_samples`: 50

After fitting the model on the scaled data, DBSCAN successfully identified 6 clusters. The outliers, which DBSCAN labeled as `-1`, were removed from further analysis.

### 4.2 Visualizing Clusters

We visualized the clusters based on **luxury sales** and **fresh sales** to observe the distribution of customers:

![DBSCAN Clustering](https://github.com/user-attachments/assets/648baa9f-ba87-47bc-89c3-242b311d6f4f)


---

## 5. Cluster Analysis

We derived six clusters, each with distinct sales behaviors. Below is a breakdown of the clusters based on the **mean sales** in each category.

| Cluster | Luxury Sales | Fresh Sales | Dry Sales | Key Insights | Recommendation |
| --- | --- | --- | --- | --- | --- |
| **0** | Higher than average | Very high | Lowest | High demand for luxury and fresh products. | Targeted promotions for luxury and fresh items. Consider increasing the focus on dry goods. |
| **1** | Lower than other clusters | Lowest | High | Strong preference for dry goods, low interest in luxury and fresh products. | Bundle dry goods with luxury/fresh items. Highlight benefits of premium goods. |
| **2** | Highest | Moderate | Moderate | Balanced customers, spending across categories. | Maintain current strategy. Introduce loyalty programs to retain these high-spending customers. |
| **3** | Very low | High | Moderate | Customers prefer fresh products with low luxury purchases. | Market fresh products aggressively and offer affordable luxury options to increase spend. |
| **4** | Moderate | Moderate | Moderate | Balanced across all categories. | Use cross-promotion to encourage customers to try higher-end products. |
| **5** | Very low | Moderate | Highest | Strong preference for dry goods, low interest in luxury products. | Promote luxury products through discounts or bundle deals with dry goods. |

### Visualizing Sales by Cluster

To better understand the behavior of each cluster, we used **boxplots** to visualize the distribution of sales in each cluster:

![Sales by Cluster](https://github.com/user-attachments/assets/b5282f37-57b0-4acc-a877-d87dc4981581)


---

## 6. Evaluation Metrics

To assess the performance of the DBSCAN clustering:

- **Silhouette Score**: A measure of how similar a point is to its own cluster compared to other clusters. A higher score indicates well-defined clusters.
    - **Score**: 0.5101
- **Davies-Bouldin Index**: Measures the average similarity ratio of each cluster with the one that is most similar to it. Lower values indicate better clustering.
    - **Score**: 0.8951

---

## 7. Conclusion and Recommendations

Using DBSCAN, we successfully segmented customers into six distinct groups. These clusters provide valuable insights for targeting customer segments with tailored marketing strategies. The **luxury sales** and **fresh sales** categories revealed clear patterns, with certain clusters heavily favoring specific types of products.

An additional insight was the **binary pattern** found in `outlet_city`, where cities displayed only two distinct sales behaviors. This simplified city-based segmentation significantly.

### Recommendations:
- **Personalized Promotions**: Each cluster has unique preferences. By focusing promotions on the products each group is inclined to purchase (e.g., luxury goods for Cluster 0, fresh products for Cluster 3), companies can increase sales.
- **Bundling Strategies**: For clusters with low luxury sales (Cluster 1, Cluster 5), consider offering bundle deals that include luxury products to encourage trial purchases.
- **Customer Retention**: High-spending clusters (Cluster 2) should be targeted with loyalty programs to maintain their engagement and prevent churn.

---
