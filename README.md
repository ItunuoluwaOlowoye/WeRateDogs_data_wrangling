# WeRateDogs_data_wrangling
This report documents the data wrangling of tweets by WeRateDogs. This report is to aid reproducibility of the data wrangling section of the analysis.

The data wrangling process was carried out in a Jupyter notebook using Python. All packages needed were imported into the notebook. These packages include pandas (alias pd), numpy (alias np), seaborn, requests, os, json, tweepy, matplotlib, PIL, io, timeit, and dotenv. Any of these packages not initially available in the environment were installed using pip install --packagename.

## Data Gathering
Data for this analysis were gathered from three different sources.

1. Locally available Twitter archive
Udacity provided the Twitter archive and it can be downloaded here. The data is a csv file which was read into a dataframe using pd.read_csv('filepath').

2. Image prediction file stored on the web in Udacity's server
The file was accessed from the web using the requests library. First, a folder to download the file into was created within the same repository as the notebook. Then, the file was downloaded and stored in the folder. Finally, the file (.tsv extension) was read into another dataframe using pd.read_csv('filepath', sep='\t').

3. Data pulled from the Twitter API
Additional data about the tweets provided in the archive was pulled from the Twitter API using the tweepy package. The dotenv package was used to get the keys needed for authentication from a .env file. The .env file was created in the same repository as the notebook using VS Code. The file stores the keys as variables that can be called using os.getenv('key_here').

An array of the tweet ids from the archive was created. Data from every tweet pulled was written and stored into a json file. A try-except code was used to bypass any errors that could result as a result of attempting to pull data from a deleted tweet. This file was created as a JSON lines format file and was read into a third dataframe using pd.read_json('filepath', lines=True).

## Data Assessment
The three dataframes were assessed visually and programmatically to understand the dataset and document data cleaning procedures that should be carried out, if any. The functions called to assess the dataframes include: .info(), .describe(), .duplicated(), and value_counts().

The assessment identified different data quality and tidiness issues that are documented below.

### Quality
#### Completeness:
* No column for the most accurate dog breed prediction.
#### Validity:
* "None" should be default null values.
* Timestamps should be datetime format, not strings.
* Ids should be strings, not numerics.
#### Accuracy:
* Some names are inaccurate.
* Duplicates are present in form of retweets.
#### Consistency:
* Primary and foreign keys shoud have consistent names across dataframes.
* Ratings should be on the same scale.
#### Tidiness
* Dog stage observations should be in one column, not four.
* Certain columns in a dataframe should be merged with another dataframe.

## Data Cleaning
All data quality and tidiness issues identified while assessing the data were cleaned using the Define-Code-Test framework on copies of the original data. The cleaning process involves:

* Creating a new column that selects the dog breed with the maximum prediction confidence using np.select.
* Replacing "None" values with null np.nan values.
* Converting timestamps to datetime format using pd.to_datetime.
* Converting to strings using astype(str).
* Replacing inaccurate values with null values using .replace()
* Removing retweets using .isnull() and .notnull() functions.
* Renaming inconsistent columns using .rename().
* Selecting observations on the same rating scale using .query().
* Joining columns and extracting data needed into one column using .agg() and str.extract() functions.
Merging dataframes using pd.merge().
