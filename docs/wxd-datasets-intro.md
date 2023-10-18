# Datasets

There are three datasets that have been loaded into watsonx.data system for you to use while exploring the features of the product.

   * [Great Outdoors Company warehouse data](wxd-datasets-gosales.md)
   * [Airline On-Time performance](wxd-datasets-ontime.md)
   * [Taxi fares](wxd-datasets-taxi.md)

## Data Location

The datasets found above have already been preloaded into the system, so there is no need to run the scripts below unless you want to modify the schemas or location of the data.

The data files can be found in the <code style="color:blue;font-size:medium;">/sampledata</code> directory. Underneath this directory you will find datasets in three different formats:

* Parquet - Data that has been formatted in Parquet format that can be loaded directly into Hive and queried by watsonx.data.
* Relational - Data that is in a delimited format that can be loaded into Db2 or PostgreSQL databases.
* CSV - Comma separated values that can be converted to multiple formats or used by watsonx.data.

Within the Parquet and Relational directories are SQL statements that can be used to catalog and load the data into the different systems. 








