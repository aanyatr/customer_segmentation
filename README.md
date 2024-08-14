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

Then we start finding the number of missing values in the rows for each column.




