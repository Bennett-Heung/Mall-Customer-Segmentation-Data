# Mall-Customer-Segmentation-Data

This repository contains: 
* 'Mall_Customer_Segmentation_Data.ipynb' - notebook file 
* 'Data' zip folder - contains the raw data
* 'Images' folder - contains the image outputs of the notebook file
* 'README.md' - summary report for the repository. 

## Objective
You are owning a supermarket mall and want to understand the customers to develop a marketing strategy.

You have information from customer membership cards, such as Customer ID, Age, Gender, Annual Income and a Spending Score, which is a score assigned to the customer based on parameters like customer behaviour and purchasing data. 

The source of the dataset is [here](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python).

## Data Cleaning

The raw data contains each customer's Customer ID, Age, Gender (Male or Female), Annual Income (k$) and a Spending Score (1-100). 

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

|    |   Age |   Income |   Score |   Female |
|---:|------:|---------:|--------:|---------:|
|  0 |    19 |       15 |      39 |        1 |
|  1 |    21 |       15 |      81 |        1 |
|  2 |    20 |       16 |       6 |        0 |
|  3 |    23 |       16 |      77 |        0 |
|  4 |    31 |       17 |      40 |        0 |

## Exploratory Data Analysis

<insert features_ images>

Findings: 
* More females than males in the dataset
* Mainly below 50 years of age 
* Mainly earning less than $90k in annual income
* Greater majority with a spending score between 40 and 60, followed by upper and lower ends of the scoring spectrum. 

<insert Pairplots image>

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
* Gender might not make much impact in clustering.
* Potentially 5 clusters in the Annual Income and Spending Score scatter plots (see middle right and bottom middle plots).
* Between Age and Spending Scores, there could be 3 bins or clusters: (see bottom left plot)
    * 1. Aged between roughly 20 and below 50 with 60+ Spending Scores
    * 2. Majority across all ages between 20 to 60 in Spending Scores
    * 3. Spending Scores below 20. 

**Correlation heatmap** 
Only the negative correlation between age and spending score is significant, as shown earlier.

<insert corr_heatmap image> 

## Modelling
K-Means Clustering, Agglomerative Clustering and DBSCAN were models to cluster the individuals differently. Each model has its own approach to test parameters in selecting an appropriate model.  
* Inertia is calculated to find the elbow point, where the number of clusters is selected
* Dendrograms help with selecting the number of clusters and appropriate linkage method for Agglomerative Clustering
* The optimal epsilon value is  used for DBSCAN is based on finding the maximum curvature on a plot of the closest distances of each point to another point. 

Scatter plots and 3D plots (as 'Females' does not a play a significant factor in clustering) are shown to provide insights of the clustering results from each model. 

### K-Means Clustering

<insert kmeans_inertia image> 

Using the Elbow method, the elbow point appears to be 5 clusters, based on observing the highest negative percentage change in 'inertia2' and the plot above. The inertia decreases by default, but decreases insignificantly as the number of clusters increase after 5 clusters. 

The 5 clusters will be checked across various 2D scatter plots under each combination of variables, along with a 3D plot. 
The combination of scatterplots include the gender variable, 'Female', as below it is evident that scatterplots with 'Female' will not yield informative insights. 

<insert 

The scatterplots and 3D plot show 5 clusters across three key factors: Spending Score, Income and Age. 

**K-Means Clustering shows 5 clusters:**
* Red: High Income and Spending Score, and under 40 years of age
* Orange: High Income and low Spending Score 
* Light Blue: medium Income and Spending Score 
* Blue: Low Income and high Spending Score, and under 40 years of age
* Grey: Low Income and Spending Score.

### Agglomeration Clustering 
Dendrograms were built for each of the four linkage methods ('ward', 'complete', 'average' and 'single') to be considered below. 

<insert Dendrogram_ images> 

The longest vertical distance without any horizontal line passing would help determine the number of selected clusters. From the above dendrograms, this is the most clear using the 'ward' linkage method on the right-hand side. 

By observing for distances below 125 roughly, there are 6 clusters that are shown with 6 vertical lines below the distance of roughly 125. 

<insert agg_scatter images> 

<insert agg_3D image> 

The scatterplots and 3D plot show 6 clusters across three key factors: Spending Score, Income and Age. 

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

<insert dbscan_scatter images> 

<insert dbscan_3D image> 

The scatterplots and 3D plot show 6 clusters across three key factors: Spending Score, Income and Age. 

**DCSCAN shows 4 clusters:**

* Orange: Low Income, high Spending Score under 30 years of Age
* Light Blue: slightly high Income with a high Spending Score, and under 40 years of Age
* Blue: Low Spending Score, slightly higher Income and around 40 years of age
* Grey: Slightly lower Income and moderate Spending Score 
* Red: Outliers.

## Conclusion

The results of each cluster for each model are mentioned in the section above. The selected model is ultimately a matter of choosing either K-Means, Agglomerative Clustering or DBSCAN. 

Despite the different clustering results that come with selecting the model, clustering for all models have a couple of commonalities: 
* Clustering falls under the basis of Age, Spending Score and Income.  
* Each clustering model indicates common clusters following these characteristics: 
    * High Income, high Spending Score, under 40 years of Age
    * High Income, low Spending Score 
    * Low Income, high Spending Score, around the younger half of the Age distribution 
    * The models do show some form of a group roughly in moderate levels of Income and Spending Score.   

Both K-Means and Agglomerative Clustering indicate a low Income and low Spending Score cluster, while DBSCAN treats them as outliers. 

It might be speculative, but it might be a reflection that K-Means and Agglomerative Clustering do not label outliers, while DBSCAN does. 
