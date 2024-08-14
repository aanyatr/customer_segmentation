# Customer Segmentation
This project was done on e-commerce fashion brand's website data.
Such data can be segmented based on demographic, location, behavioural and technographic metrics.
The steps involved in the project where as follows:
1. Data Retrieval
2. Data Cleaning
3. Data Imputation
4. Data Analysis
   
## Data Retrieval: 
Using BigQuery table retrieved the following columns:
            
1. user_pseudo_id
2. latest_activity_date
   
Activity Insight Columns:              
1. total_purchase_count       
2. purchase_days              
3. total_add_to_cart
4. total_purchase_revenue         
5. total_begin_checkout     
6. total_product_views
7. total_visits          
8. event_types        
9. total_events

Location columns:
1. city                   
2. region     
3. country                    
4. continent                 
5. sub_continent          
6. metro  

## Data Cleaning:
Using Jupyter Notebook we understand the data in detail and clean it.

We first import all the required libraries for this task.
Then loading the csv file from our local machine via pandas is done and the data frame is named df.
The results from df.info() show us that there are 5,56,693 rows in the frame along with 10 columns.

1. When all location columns have null or ‘not set’ value. Dump them.
Result: 4385 rows dropped, leaving no null values in country, sub_continent and continent columns.
city                    18281
country                  0
region                  11499
continent                0
sub_continent            0
metro                   449210

2. City and region is set to null, metro is not null. 
Check and impute values for the city and region according to metro.
Then metro values changed to match existing values in the city table.

Code:
# change region to Washington, and city to washington
# Replace 'New York, NY' with 'New York' in the 'city' column & region as 'New York' too
df['city'] = df['city'].replace('New York, NY', 'New York')
df['region'] = df['region'].replace('New York, NY', 'New York')

3. City is null, metro is not null.
Check and impute values for the city according to metro.
rows3.loc[:,'city']=rows3['metro']
Then change those values to match city values available. (84 unique values were there)
df['city'] = df['city'].replace('West New York', 'New York')
df['city'] = df['city'].replace('Atlanta GA', 'Atlanta')
df['city'] = df['city'].replace('New York, NY', 'New York')
df['city']=df['city'].replace('Washington DC (Hagerstown MD)','Washington')
df['city'] = df['city'].replace('Los Angeles CA', 'Los Angeles')

4. When the city is not null but the region is null. 

Using the available values for city and country impute region.

Number of selected rows with CITY not null but REGION null: 3439
73 unique rows were present 
Code: 
df.loc[(df['city'] == 'Singapore') & (df['country'] == 'Singapore'), 'region'] = 'Central Region'
df.loc[(df['city'] == 'Kathmandu') & (df['country'] == 'Nepal'), 'region'] = 'Bagmati Pradesh'

Region not found for:
San Fernando City Corporation
Umka
Goma
Gibraltar
Vaduz
Sofia Province
Andijan
Douala
Urgench
Serrekunda

Missing values available in the data: 
city                    13077
region                   3699
metro                  444825


4. Where city, metro and region are all null, drop these. 
RESULT: (3671 rows dropped)
After running this the missing values are:
city                    9406
region                   28
metro                441154

As Region only has 28 empty values we can drop these.

We drop city values as well after examining to see if any way to impute as there wasn’t, we can drop these as well.
Resulting in the missing values column now being:
metro              431720

Total rows: 462297 (now)

## Analyzing the data: 
Deriving the column ‘Customer Behavior’.
Values: Below Average Viewer, Purchaser, and Viewer, 
Frequent Purchaser & Viewer

## Verifying the columns:
Double check the  total_purchase_count & purchase_days column.
Checked the total_purchase_days and total_purchase_count columns to see if their values are correct.
Created another table from BigQuery which only calculated the number of purchases, and then tallied it to the
pre-existing segmented_data table to ensure the table we have is correct.

## Clustering the data: 
    1. Convert categorical data into numeric data through encoding techniques.
Normalize or standardize the numerical data after that (standard scalar, min-max scalar).
Split the user_pseudo column and activity date in another dataframe df2 (redundant for clustering).
Cluster the data to check how many clusters through both silhouette score and elbow score, though silhouette score is a better method. 
Visualize the data and clusters, and graphs.
After clustering is done, join the dataframe with df2 based on index. 
Change the derived columns logic don’t use numbers using dynamic logic such as percentile. 
Understand the micro behaviors from the clusters then you make the problem supervised.
Need to convert categorical data?
In its original form categorical data cannot be utilized by machine learning algorithms to derive valuable insights as algorithms don’t know how to deal with categories but numeric values on which they are formed. Thus leading to flawed results if computation were to occur in their original form. 
Understanding the nature of categorical data is imperative in choosing the method to convert them. 
To first classify the variables as nominal and ordinal.
All variables can be classified as nominal variables 
but we can classify Customer_Behaviour as ordinal. 
Techniques to convert categorical variables into numerical variables:
Suitable for nominal variables:
 SNO
Encoding Technique
Points to remember:
Implementation

One-hot Encoding
To use where unique cases of data are less for columns i.e. up to 10 should be fine.
It increases data dimensionality immensely as the data contains many sparse matrices. 
Col1 values: red, blue, green, orange
Now these values will form 4 new columns: 
col1values_red, col1values_blue,col1values_green  col1values_orange. 
With 0 indicating they don’t have a particular value and 1 indicating it does.

Binary Encoding
Mid-ground between label and one-hot encoding.
It is better suited for nominal variables with many categories, as lesser data dimensionality increases as compared to one-hot-encoding. 
Converts the nominal variables to integers and then to their binary format. 

Frequency Encoding
Computes the frequency of each categorical nominal variable, not suitable in cases where the frequency is the same for values in a particular column.
The data will not be able to show that difference properly, thus loss of categorical info. 
Doesn’t increase dimensionality of data.

BaseN encoding

Suitable for ordinal variables:

S.NO
Encoding Techniques
Points to remember
Implementation

Label Encoding
Label Encoding is best suited to ordinal categorical variables, where the categories have a logical order or progression. 

Ordinal Encoding
Preserves the order of the categories in ordinal variables, though the degree of difference cannot be determined exactly and thus it treats the degree of difference as the same. Can lead to misleading results with nominal variables.

Other encoding techniques:
Mean Encoding(encode based on mean of target variable, can lead to overfitting, but doesn’t increase dimensionality of data)
Helmert Encoding
Hash Encoding
Target Encoding (applied for supervised problems can’t be utilized here)
Nominal variables in our data:
City
Country
Continent
Sub_continent
Metro
Region 
Customer-Behaviour
As the number of values for most of these columns are very high, we decide to use binary encoding on these columns above.
Resources:
Converting Categorical Data into Numerical Form: A Practical Guide for Data Science | by Brandon Wohlwend | Medium
How to Convert Categorical Data in Pandas and Scikit-learn
What are Categorical Data Encoding Methods | Binary Encoding
Types of Categorical Data Encoding Schemes | by Lakshay Arora | Analytics Vidhya | Medium
Standardization of Data:
Apart from the columns, user_pseudo_id and lastest_activity_date all other columns are standardized using Standard Scalar. 
As these columns are split and stored in another column.
Applying K-Means:
Resources:
2.3. Clustering — scikit-learn 1.5.0 documentation
Silhouette Coefficient. This is my first medium story, so… | by Ashutosh Bhardwaj | Towards Data Science.
Introduction to K-Means Clustering Algorithm
K-Mean: Getting the Optimal Number of Clusters
Things to know:
A Voronoi Diagram can be used to better understand the implementation of K-Means.
In k-means 
Initialize the K-means algorithm multiple times as it takes time to converge. 
Converging of K-means is highly dependent on initialisation of the centroids.

Bucketing columns with high cardinality. 
Coming up with logic for categorical columns to reduce computation complexity.
Ensuring RFM is there.
Grouping columns together, PCA.
