# Mall-Customer-Segmentation-Data
Customer segmentation for a supermarket mall.

This repository contains: 
* 'Mall_Customer_Segmentation_Data.ipynb' - notebook file 
* 'Data.zip' - zip folder contains the raw data
* 'Images' folder - contains the image outputs of the notebook file
* 'README.md' - summary report for the repository. 

## Objective
One of the key marketing campaign problems involves understanding customers to deliver an effective marketing strategy within business constraints of time and costs. Customer segmentation is a key process that resolves this by targeting specific customers based on common characteristics. In this notebook, the process of customer segmentation is delivered by unsupervised clustering processes on data sourced from [here](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python).


## Data Cleaning

The raw data contains each customer's Customer ID, Age, Gender (Male or Female), 'Annual Income (k$)' (Income) and a 'Spending Score (1-100)' (Score). 

*Preview of Raw Data*

|    |   CustomerID | Gender   |   Age |   Annual Income (k$) |   Spending Score (1-100) |
|---:|-------------:|:---------|------:|---------------------:|-------------------------:|
|  0 |            1 | Male     |    19 |                   15 |                       39 |
|  1 |            2 | Male     |    21 |                   15 |                       81 |
|  2 |            3 | Female   |    20 |                   16 |                        6 |
|  3 |            4 | Female   |    23 |                   16 |                       77 |
|  4 |            5 | Female   |    31 |                   17 |                       40 |

Checks were made as a preprocessing step before exploratory data analysis and modelling. No further actions were needed as results showed that: 
* The datatypes were appropriate; and 
* No missing data, duplicates or invalid data was found. 

Going forward, Customer IDs were dropped as they were not relevant for further analysis and Gender was tranformed into the dummy variable 'Female', where: Female = 1 and Male = 0, as shown below. 

*Preview of Updated Data*

|    |   Age |   Income |   Score |   Female |
|---:|------:|---------:|--------:|---------:|
|  0 |    19 |       15 |      39 |        1 |
|  1 |    21 |       15 |      81 |        1 |
|  2 |    20 |       16 |       6 |        0 |
|  3 |    23 |       16 |      77 |        0 |
|  4 |    31 |       17 |      40 |        0 |

## Exploratory Data Analysis

*Descriptive Statistics* 
|       |     Age |   Income |    Score |
|:------|--------:|---------:|---------:|
| count | 200     | 200      | 200      |
| mean  |  38.85  |  60.56   |  50.2    |
| std   |  13.969 |  26.2647 |  25.8235 |
| min   |  18     |  15      |   1      |
| 25%   |  28.75  |  41.5    |  34.75   |
| 50%   |  36     |  61.5    |  50      |
| 75%   |  49     |  78      |  73      |
| max   |  70     | 137      |  99      |

(Note: 112 females and 88 males were in the data.) 

* Age is positively skewed, as the youngest age to obtain customer data is 18 years of age. 
* Income appears positively skewed, which is plausible given that spending information is more likely obtained from spenders who have more income.  
* (Spending) Scores are roughly normally distributed.

*Pairplots*

![Pairplots.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/Pairplots.png)

**Distributions** 
* Greater kurtosis of Females distributions for Age and Spending Score (1-100) than Males.
* Both Genders have similar Annual Income distributions.
* Both Genders have more spending scores roughly around 50.  
* Both Genders have two peaks in their distributions of Spending Scores: 
    * Males have one peak of low Spending Scores and another slightly above 50;
    * Females mostly have a Spending Score of 50 and another slight peak in the higher end of the Spending Scores. 

**Linear regressions and scatter plots** 
* No significant differences between Genders in terms of the relationships against each feature.
* No evident relationship between Age and Annual Income, but noticed that the younger and older ends of the age distribution have lower distribution and lower spread in Annual Income, while there is a greater spread and broader distribution of Annual Income for more middle-aged individuals. 
* The most significant correlation in terms of magnitude is the negative correlation between Spending Score and Age. 
* Possible clusters appear between Age and Spending Score, and for Spending Score and Annual Income. 

**Hypothesis of clustering**
* Gender is not making much impact to help with clustering so it is dropped.
* Potentially 5 clusters in the Annual Income and Spending Score scatter plots (see middle right and bottom middle plots).
* Between Age and Spending Scores, there could be 3 bins or clusters: (see bottom left plot)
    * 1. Aged between roughly 20 and below 50 with 60+ Spending Scores
    * 2. Majority across all ages between 20 to 60 in Spending Scores
    * 3. Spending Scores below 20. 

### Correlation heatmap 
Only the negative correlation between age and spending score is significant, as shown earlier.

*Correlation Heatmap*

![corr_heatmap.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/corr_heatmap.png)

## Modelling
K-Means Clustering, Agglomerative Clustering and DBSCAN were models to cluster the individuals differently. Each model has its own approach to test parameters in selecting an appropriate model.  
* Inertia is calculated to find the elbow point, where the number of clusters is selected
* Dendrograms help with selecting the number of clusters and appropriate linkage method for Agglomerative Clustering
* The optimal epsilon value is  used for DBSCAN is based on finding the maximum curvature on a plot of the closest distances of each point to another point. 

Scatter plots and 3D plots (as 'Females' does not a play a significant factor in clustering) are shown to provide insights of the clustering results from each model. 

### K-Means Clustering
*K-Means Inertia for each number of clusters*

![kmeans_inertia.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/kmeans_inertia.png)

Using the Elbow method, the elbow point appears to be 5 clusters, based on observing the highest negative percentage change in 'inertia2' and the plot above. The inertia decreases by default, but decreases insignificantly as the number of clusters increase after 5 clusters. 

The 5 clusters will be checked across various 2D scatter plots under each combination of variables, along with a 3D plot. 
The combination of scatterplots include the gender variable, 'Female', as below it is evident that scatterplots with 'Female' will not yield informative insights. 

*K-Means Scatterplots*

![kmeans_scatter.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/kmeans_scatter.png)

*K-Means 3D Plot*

![kmeans_3D.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/kmeans_3D.png)

**K-Means Clustering shows 5 clusters:**
* Red: High Income and Spending Score, and under 40 years of age
* Orange: High Income and low Spending Score 
* Light Blue: medium Income and Spending Score 
* Blue: Low Income and high Spending Score, and under 40 years of age
* Grey: Low Income and Spending Score.

### Agglomeration Clustering 
Dendrograms were built for each of the four linkage methods ('ward', 'complete', 'average' and 'single') to be considered for agglomeration clustering. The 'ward' linkage method and number of clusters were determined on the basis of creating the cut-off from the longest vertical distance without any horizontal line passing. This is shown below from observing for distances below 125, where there are 6 clusters shown with 6 vertical lines below the distance of 125. 

![Dendrogram__Ward.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/Dendrogram__Ward.png)

*Agglomerative Clustering Scatterplots*

![agg_scatter.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/agg_scatter.png)

*Agglomerative Clustering 3D Plot*

![agg_3D.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/agg_3D.png)

**Agglomerative Clustering shows 6 clusters:**

* Red: High Income, low Spending Score 
* Orange: medium Income and Spending Score, and over 30 years of Age
* Light Orange: high Income and Spending Score, and under 40 years of Age
* Light Blue: low Income, high Spending Score and under 30 years of Age
* Blue: Low Income and Spending Score
* Dark Blue: medium Income and Spending Score, and under 30 years of Age.

### DBSCAN
The optimal value for epsilon is be found at the point of maximum curvature of each point's closest distance from another point. 
From the below, the maximum curvature is where epsilon is 10.

*DBCSAN Scatterplots*

![dbscan_scatter.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/dbscan_scatter.png)

*DBCSAN 3D Plot*

![dbscan_3D.png](https://github.com/Bennett-Heung/Mall-Customer-Segmentation-Data/blob/main/images/agg_3D.png)

**DCSCAN shows 4 clusters:**

* Orange: Low Income, high Spending Score under 30 years of Age
* Light Blue: slightly high Income with a high Spending Score, and under 40 years of Age
* Blue: Low Spending Score, slightly higher Income and around 40 years of age
* Grey: Slightly lower Income and moderate Spending Score 
* Red: Outliers.

## Deployed Solution

**Agglomerative Clustering** results show that is the deployable solution producing the most clear-cut clusters. K-Means Clustering and Agglomerative Clustering models produce similar results. However, Agglomerative Clustering shows more distinguishable clusters across the three key characteristics - income, age and spending score. DBSCAN distinguishes from these techniques with the ability to show outliers, but does not produce clusters that are as distinguishable for customer segmentation here. 

To deliver an effective marketing campaign, different approaches could be catered to each of the following customer segmentations, as split by Age, Income and Spending Score. 

* Two high Income groups of customers: 
    * i) one with low Spending Scores 
    * ii) another with high Spending Scores and under 40 years of Age
    
    
* Two medium Income and Spending Score customer groups: 
    * iii) one over 30 years of Age
    * iv) another under 30 years of Age
    
    
* Two low Income groups of customers: 
    * v) one with low Spending Scores
    * vi) Another with high Spending Scores and under 30 years of Age.
