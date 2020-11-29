# A3-hcds-hcc-bias

## About the project
This project aims to retrieve the quality of English Wikipedia articles of politicians from different countries and uncover possible relations between countries (or regions), their population and the article quality.
The project is also used to discover and get familiar with biases.

### Structure of the project
* data_raw -> contains the raw data
  * export_2019.csv -> data set with pages of politicians, the politician's country and the revision id
  * page_data.csv -> data set with countries, region and population
* data_clean -> contains cleaned up data as well as data that was excluded from the analysis
  * countries-without-match.csv -> cleaned up data used for analysis
  * en-wikipedia-politicians_with_article_quality.csv -> intermediate results containing ORES predictions
  * countries-without-match.csv -> Countries excluded from analysis because they were only in one of the raw data sets
  * ORES_no_scores.csv -> Articles excluded from analysis because no quality prediction could be retrieved from ORES
* results -> contains results of the analysis; 6 csv files (see 'Description of the results' for details)
* src -> contains the source code (jupyter notebook) used for data cleaning and the analysis
* LICENSE -> License information - This project is licensed under the [MIT license](https://mit-license.org/)
* README -> The file you are reading right now :)
* poetry.lock and pyproject.toml -> These files make it easy to run this project (see 'How to run the code')

### How to run the code

#### Requirements
1. Tested with Python version 3.8 (other versions might work too)

#### Steps 
1. Open a shell

1. Clone the repository and go to the root folder of the project (all the following commands have to be run there)

1. Run `poetry install` in the root folder of the project to install all dependencies

1. After the installation is finished, run `poetry shell` to enter a shell in which all dependencies are installed

1. Run `jupyter notebook` to open the project in your browser.


## About the data

### Data sources 

* [Wikipedia article names of politicians with countries](https://figshare.com/articles/Untitled_Item/5513449)
Published by Os Keyes under CC-BY-SA 4.0 license. 

* [Countries with population and region](https://www.prb.org/international/indicator/population/table/)
Published by Population Reference Bureau. License unknown. The data was preprocessed.

* [ORES endpoint doc](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model), [About ORES](https://www.mediawiki.org/wiki/ORES)
The quality of articles was retrieved using the ORES and it's web API. ORES is developed and maintained by the Wikimedia Scoring Platform team. It uses machine learning to predict the quality of Wikipedia articles. License unknown.


### Description of the cleaned data
Header and content of the table that contains the cleaned data:

| article_name | country | region | revision_id | article_quality | population |
|--------------|---------|--------|-------------|-----------------|------------|
| name | lowercase name | uppercase name | ID | ORES prediction | num_inhabitants |

ORES predictions can be in these quality categories:

| ID | Quality Category |  Explanation |
|----|------------------|----------|
| 1 | FA    | Featured article |
| 2 | GA    | Good article |
| 3 | B     | B-class article |
| 4 | C     | C-class article |
| 5 | Start | Start-class article |
| 6 | Stub  | Stub-class article |

For more information see [here](https://www.mediawiki.org/wiki/ORES)

### Description of the results
The results can be found in the folder `results`

* countries-coverage-bottom-10.csv
Ten countries with the lowest ratio of articles (of politicians of that country) to number of inhabitants

| country | articles/population |
|---------|---------------------|
| lowercase name | num_articles/num_inhabitants |

* countries-coverage-top-10.csv
Ten countries with the highest ratio of articles to number of inhabitants

| country | articles/population |
|---------|---------------------|
| lowercase name | num_articles/num_inhabitants |

* countries-relative-quality-bottom-10.csv
Ten countries with the lowest ratio of high quality articles to all articles (of that country)

| country | hq_article_rate |
|---------|---------------------|
| lowercase name | num_hq_articles/num_articles |

* countries-relative-quality-top-10.csv
Ten countries with the highest ratio of high quality articles to all articles (of that country)

| country | hq_article_rate |
|---------|---------------------|
| lowercase name | num_hq_articles/num_articles |

* region_coverage.csv
Regions with the ratio of articles (of politicians of that region) to number of inhabitants

| region | articles/population |
|---------|---------------------|
| uppercase name | num_articles/num_inhabitants |


* region_relative_quality.csv
Regions with and the ratio of high quality articles to all articles (of that country)

| region | hq_article_rate |
|---------|---------------------|
| uppercase name | num_hq_articles/num_articles |



### Important notes 
* Population for some countries are given in millions and some in thousands which was ignored during analysis (-> can negativly impact the results)
* countries-relative-quality-bottom-10.csv contains 10 countries that do not have any English high quality articles of their politicians. These results might be misleading because there are more than 10 countries for which this is true.
