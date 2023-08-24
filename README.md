# Big Data Challenge - "Alexa, can you handle big data?"
## Solution
### Key Artifacts
- [Level 1 Jupyter Notebook](level_one_solution.ipynb)
- [Level 2 Jupyter Notebook](level_two_solution.ipynb)

## Background

Many of Amazon's shoppers depend on product reviews to make a purchase. Amazon makes these datasets publicly available. However, they are quite large and can exceed the capacity of local machines to handle. One dataset alone contains over 1.5 million rows; with over 40 datasets, this can be quite taxing on the average local computer. Our first goal for this assignment was to perform the ETL process completely in the cloud and upload a DataFrame to an RDS instance. The second goal was to use PySpark or SQL to perform a statistical analysis of selected data.

There are two levels to this assignment. The second level is optional but highly recommended.

1. Create DataFrames to match production-ready tables from two big Amazon customer review datasets.
2. Analyze whether reviews from Amazon's Vine program are trustworthy.

- - -

## Instructions
### Preparation - Creating RDS Instance on AWS:
* An RDS Instance was created for the PostgreSQL Database in AWS called - `sgbigdatahw`
* ![RDS Instance](Images/Amazon_RDS_Instance.PNG)
* In order to connect to it using PGAdmin tool, the Public Available setting had to be enabled
* ![Public Available](Images/Public_Available.PNG)
* A Server Instance in PGAdmin was then created using the URL associated with the RDS Instance:
* ![Server Instance in PGAdmin](Images/RDS_Server_PGAdmin.PNG)

### Level 1

* Create tables in an RDS database by using the furnished schema.

* Create two separate Google Colab notebooks and **extract** any two datasets from the list at [review dataset](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt), one into each notebook.

  **Note:** It is possible to ETL both data sources in a single notebook, but due to the large data sizes, it will be easier to work with these S3 data sources in two separate Colab notebooks.

* Ensure that the header is handled correctly. If we read the file without the header parameter, we may find that the column headers are included in the table rows.

* For each notebook (one dataset per notebook), to complete the following:

  * Count the number of records (rows) in the dataset.

  * **Transform** the dataset to fit the tables in the [schema file](schema.sql). Be sure the DataFrames match in data type and in column name.

* **Load** the DataFrames that correspond to tables into an RDS instance. **Note:** This process can take up to 10 minutes for each. Be sure that everything is correct before uploading.
  * Once the tables were loaded with the data in the dataframes, the tables were queried to ensure that the data is correctly loaded:
 #### Review_Data Table:
  ![Review_Data Table](Images/Review_ID_Table.PNG)
  
 #### Products Table:
  ![Products Table](Images/Products_Table.PNG)

 #### Customers Table:
  ![Customers Table](Images/Customers_Table.PNG)

 #### Vines Table:
  ![Vines Table](Images/Vines.PNG)

### Level 2

In Amazon's Vine program, reviewers receive free products in exchange for reviews.

![vine01.png](Images/vine01.png)

Amazon has several policies to reduce the bias of its Vine reviews: [https://www.amazon.com/gp/vine/help?ie=UTF8](https://www.amazon.com/gp/vine/help?ie=UTF8).

But are Vine reviews truly trustworthy? Your task is to investigate whether Vine reviews are free of bias. Use either PySpark or—for an extra challenge—SQL to analyze the data.

* If we choose to use SQL, ensure to first use Spark on Colab to extract and transform the data and load it into a SQL table on your RDS account. Perform the analysis with SQL queries on RDS.

* While there are no hard requirements for the analysis, consider steps takes to reduce noisy data, e.g., filtering for reviews that meet a certain number of helpful votes, total votes, or both.

* Submit a summary of your findings and analysis.

- - -

## Hints and Considerations

* Be sure that to start each notebook with following code in the first cell and update the Spark version.

```python
import os
# Find the latest version of spark 3.0  from http://www-us.apache.org/dist/spark/ and enter as the spark version
# For example:
# spark_version = 'spark-3.0.1'
spark_version = 'spark-3.<spark version>'
os.environ['SPARK_VERSION']=spark_version

# Install Spark and Java
!apt-get update
!apt-get install openjdk-11-jdk-headless -qq > /dev/null
!wget -q http://www-us.apache.org/dist/spark/$SPARK_VERSION/$SPARK_VERSION-bin-hadoop2.7.tgz
!tar xf $SPARK_VERSION-bin-hadoop2.7.tgz
!pip install -q findspark

# Set Environment Variables
import os
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"
os.environ["SPARK_HOME"] = f"/content/{spark_version}-bin-hadoop2.7"

# Start a SparkSession
import findspark
findspark.init()
```

* For connection to Postgres run the following code in the next cell.

```python
!wget https://jdbc.postgresql.org/download/postgresql-42.2.9.jar
```

- - -

## Submission

* **Important** be sure to clean up all your AWS resources. Consult the [AWS cleanup guide](../Resources/AWS_cleanup.pdf) and [AWS check billing guide](../Resources/AWS_check_billing.pdf) as reference.

* Download you Google Colab notebooks as `.ipynb` files and upload those to GitHub.

* Copy your SQL queries into `.sql` files and upload to GitHub.

* **Important:** Do not upload notebooks that contain your RDS password and endpoint. Be sure to delete them before making your notebook public!

* Ensure your repository has regular commits and a thorough README.md file

- - -

## References

Amazon customer Reviews Dataset. (n.d.). Retrieved April 08, 2021, from: [https://s3.amazonaws.com/amazon-reviews-pds/readme.html](https://s3.amazonaws.com/amazon-reviews-pds/readme.html)

- - -
